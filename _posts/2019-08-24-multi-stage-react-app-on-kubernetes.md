---
layout: post
title: A cleaner multi-stage continuous deployment on Kubernetes for a Create React App
description: "This blog posts presents a clean way to make a multi-stage deployment of a Create React App on a Kubernetes Cluster.
Deployments with kubectl, helm charts, kustomize and skaffold are presented."
author: ama
permalink: /ama/a-cleaner-multi-stage-deployment-on-kubernetes-for-a-create-react-app
published: false
categories: [devops]
tags: [reactjs, kubernetes, environment-variables, docker, dockerfile, continuous-deployment, kustomize, helm, skaffold]
---

Most of applications depend on external factors that have different values depending on the environment where they are 
deployed. We mostly use for that [environment variables](https://en.wikipedia.org/wiki/Environment_variable).
This blog posts presents a clean way to make a multi-stage deployment of a Create React App on a Kubernetes Cluster. You can use
 this approach for seamless integration into your continuous deployment pipeline. 
 
In the beginning it will you show how to set up the React App and the guide you through several deployment possibilities on Kubernetes. You will deploy with 
native `kubectl` command, with [helm](https://helm.sh/), with [kustomize](https://kustomize.io/) and at the end use [skaffold](https://skaffold.dev/).  

We use a simple app that displays the latest public bookmarks published on [www.bookmarks.dev](https://www.bookmarks.dev).
Depending on the environment the apps runs on, it will display the environment name and the navigation bar's color is different

The source code is available on [Github](https://github.com/CodepediaOrg/multi-stage-react-app-example)

## TLDR; 
Create a _config.js_ file where you inject the environment variables in the `window` object (e.g. _window.REACT_APP_API_URL='https://www.bookmarks.dev/api/public/bookmarks'_).
Add this file to the _public_ folder of your react application. Dockerize your react application and at deployment time to kubernetes overwrite the config.js file in the 
container - you can do that with Kubernetes configMaps via native kubectl commands, kustomize or helm. 

## Prerequisites
To run this application on Kubernetes locally make sure you have [Docker Desktop](https://github.com/kubernetes/minikube) (what I used for testing)
 or [minikube](https://github.com/kubernetes/minikube) installed. You can also deploy it directly in the cloud if you have an account. 

<!--more-->

## React App Setup

The react application presented in this tutorial is build with [create-react-app](https://github.com/facebook/create-react-app).

### The `public` folder

You need to add a [config.js](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/public/config.js) in the [public folder](https://create-react-app.dev/docs/using-the-public-folder).
This will not be processed by webpack. Instead it will be copied into the _build_ folder untouched. To reference the file in the `public` folder, you need to use the special
variable called `PUBLIC_URL`:

```  
    <head>
       .....
       <title>React App</title>
       <script src="%PUBLIC_URL%/config.js"></script>
     </head>
```

What does the file contain:
```javascript
window.REACT_APP_API_URL='https://www.bookmarks.dev/api/public/bookmarks'
window.REACT_APP_ENVIRONMENT='LOCAL'
window.REACT_APP_NAVBAR_COLOR='LightBlue'
```

> Usually the API_URL is also pointing to a different URL depending on the environment, but here it is the same overall.  

With that you set your environment variables on the `window` object. These are the properties mentioned above. Make sure they are unique,
 so a good practice is to add the `REACT_APP` prefix as in [Adding Custom Environment Variables](https://create-react-app.dev/docs/adding-custom-environment-variables).
 
 
> WARNING: Do not store any secrets (such as private API keys) in your React app! Environment variables are embedded into the build, meaning anyone can view them by inspecting your app's files.


At this point you can run and build the app locally the way you know it:  
```bash
npm install 
npm start
```

> I recommend using [nvm](https://github.com/nvm-sh/nvm) to run NodeJS locally

and then access it at [http://localhost:3000](http://localhost:3000)

### Why not use the `process.env` approach
 
At runtime (in the browser for web apps) we don't have access `process.env`, so the values that are dependent on the environment have to be set prior to that, namely at **build time**. 
If you do the deployment from your local machine, you can easily control the environment-variables - build the app for the environment you need it for and then deploy it. Tools like kustomize and skaffold, makes 
this feel like a breeze in the Kubernetes world as you'll later find out in the article.
 
But if you follow a continuous deployment approach, you'd usually have several steps, which form a so called **pipeline**:
1. commit your code to a repository, hosted somewhere like [GitHub](https://github.com/)
2. your build system gets notified
3. build system compiles the code and runs unit tests
4. create image and push it to a registry, such as [Docker Hub](https://hub.docker.com/).
5. from there you can deploy the image


The idea is to repeat these steps as little as possible. With the approach presented in this blog post it will be only number five (deployment) where
we have environment specific setups. 


## Containerize the application  

But first things first, let's build a docker container to use for the deployment on Kubernetes. Containerizing the application requires a base image to create 
an instance of the container. 

### Create the Dockerfile

The [Dockerfile](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/Dockerfile) contains the steps needed to build the Docker image:

```
# build environment
FROM node:12.9.0-alpine as build
WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH
COPY package.json /app/package.json
RUN npm install --silent
RUN npm config set unsafe-perm true #https://stackoverflow.com/questions/52196518/could-not-get-uid-gid-when-building-node-docker
RUN npm install react-scripts@3.0.1 -g --silent
COPY . /app
RUN npm run build

# production environment
FROM nginx:1.17.3-alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

We use a [multi-stage build](https://docs.docker.com/develop/develop-images/multistage-build/)  to build the docker image.
In the first step we build the React APP on a [node alpine image](https://hub.docker.com/_/node/) and in the second step
we deploy it to an [nginx-alpine image](https://hub.docker.com/_/nginx). 


### Build the docker image
To build the docker image run the following command in the project's root directory:

```bash 
docker build --tag multi-stage-react-app-example:latest .
```

At this point you can run the application in docker by issuing the following command:

```bash 
docker run -p 3001:80 multi-stage-react-app-example:latest
```

We forward port `80` of nginx to `3001. Now you can access the application at [http://localhost:3001](http://localhost:3001)

> Note that the environment is LOCAL, as it picks 

### Push to docker repository
You can also push the image to adocker repository. Here is an example pushing it to the codepediaorg organisation on dockerhub:
```
docker tag multi-stage-react-app-example codepediaorg/multi-stage-react-app-example:latest
docker push codepediaorg/multi-stage-react-app-example:latest
```


## Deployment to Kubernetes

We can now take a docker container based on the image we created and deploy it to kubernetes. 

For that all we need to do is create a Kubernetes service and deployment:

```
apiVersion: v1
kind: Service
metadata:
  labels:
    app.kubernetes.io/component: service
  name: multi-stage-react-app-example
spec:
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 80
  selector:
    app: multi-stage-react-app-example
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/component: service
  name: multi-stage-react-app-example
spec:
  replicas: 2
  selector:
    matchLabels:
      app: multi-stage-react-app-example
  template:
    metadata:
      labels:
        app.kubernetes.io/component: service
        app: multi-stage-react-app-example
    spec:
      containers:
        - name: multi-stage-react-app-example
          image: multi-stage-react-app-example:latest
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
```


### Kubernetes context and namespace

Before you run a kubectl apply command it is good to know what [context](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/#context)
 and [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) you are applying your command against. 

The easiest way to do this is to install [kubectx](https://github.com/ahmetb/kubectx)  and then issue `kubectx` to get 
the current context and `kubens` for the current namespace (by default is `default`).
 
Now that you know where your kubernetes objects will be applied you can add them to a file, like 
[deploy-to-kubernetes.yaml](TODO add link) and apply the command:

```bash
kubectl apply -f deploy-to-kubernetes.yaml
```

This will create the `multi-stage-react-app-example` service of type [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types).
 You can verify its presence by listing all services

```bash
kubeclt get svc
```

or grep it with `kubectl get svc | grep multi-stage-react-app-example`

### Port forward
To access the application inside the cluster we can use [port-forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/).
The command to forward the service created before is
```bash
kubectl port-forward svc/multi-stage-react-app-example 3001:80
```

> Note `svc` before the service name 

This commands forwards the local port 3001 to the container port 80 specified in the deployment file. 
Now you can access the application inside the container at [http://localhost:3001](http://localhost:3001) and 
it uses the **LOCAL** environment. 

### Tear down
To delete the service and deployment issue the following command
```bash
kubectl delete -f deploy-to-kubernetes.yaml
```

### Make application deployment environment aware
Remember our purpose for continuous delivery pipeline to make the application "aware" of the environment at deployment. 

#### Create configMap
Well we can do that by creating a [configMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/). 
Let's create a configMap for the `dev` environment:

```bash
kubectl create configmap multi-stage-react-app-example-config --from-file=config.js=environment/dev.properties
```
This will create a configMap which we can reference by the `config.js` key and the content are the environment values.

We can check this by issuing the following kubectl command:
```bash
kubectl get configmaps multi-stage-react-app-example-config -o yaml
```

The result should look something like the following:

```yaml
apiVersion: v1
data:
  config.js: |
    window.REACT_APP_API_URL='https://www.bookmarks.dev/api/public/bookmarks'
    window.REACT_APP_ENVIRONMENT='DEV'
    window.REACT_APP_NAVBAR_COLOR='LightGreen'
kind: ConfigMap
metadata:
  creationTimestamp: "2019-08-25T05:20:17Z"
  name: multi-stage-react-app-example-config
  namespace: default
  resourceVersion: "13382"
  selfLink: /api/v1/namespaces/default/configmaps/multi-stage-react-app-example-config
  uid: 06664d35-c6f8-11e9-8287-025000000001
```

#### Mount the configMap in the container
Now the trick is to mount the config map into the container via a volume and overwrite the config.js file with the 
values from the config map. We moved the configuration of the service and deployment resources in separate files in the [kubernetes](https://github.com/CodepediaOrg/multi-stage-react-app-example/tree/master/kubernetes) folder.
See out the new configuration of the pods in the [kubernetes deployment](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/kubernetes/deployment.yaml):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/component: service
  name: multi-stage-react-app-example
spec:
  replicas: 1
  selector:
    matchLabels:
      app: multi-stage-react-app-example
  template:
    metadata:
      labels:
        app.kubernetes.io/component: service
        app: multi-stage-react-app-example
    spec:
      containers:
        - name: multi-stage-react-app-example
          image: multi-stage-react-app-example:latest
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 80
          volumeMounts:
            - name:  multi-stage-react-app-example-config-volume
              mountPath: /usr/share/nginx/html/config.js
              subPath: config.js
              readOnly: true
      volumes:
        - name: multi-stage-react-app-example-config-volume
          configMap:
            name: multi-stage-react-app-example-config
```


In the `volumes` section of the specification we defined a volume based on the configMap we've just created:

```yaml
      volumes:
        - name: multi-stage-react-app-example-config-volume
          configMap:
            name: multi-stage-react-app-example-config
``` 

and then we mount it in the container in the folder where nginx delivers its files:

```yaml
spec:
  ...
  template:
  ...
    metadata:
      labels:
        app.kubernetes.io/component: service
        app: multi-stage-react-app-example
    spec:
      containers:
        ...
          volumeMounts:
            - name:  multi-stage-react-app-example-config-volume
              mountPath: /usr/share/nginx/html/config.js
              subPath: config.js
              readOnly: true
```

> Note: we need to use `subpath` to only overwrite the _config.js_ file, otherwise the content of the folder is replaced with this file


#### Deploy on kubernetes cluster
We will deploy now on the same local cluster, but this would be for your **dev** cluster to deploy to, by applying 
all the files from the `kubernetes` directory:

```bash
kubectl apply -f kubernetes
```

Verify the config.js file has been replaced and connecting to the pod.

```bash
#first export list the pod holding our application
export MY_POD=`kubectl get pods | grep multi-stage-react-app-example | cut -f1 -d ' '`
kubectl exec -it $MY_POD -- /bin/sh # connect to shell in alpine image
less /usr/share/nginx/html/config.js # display content of the config.js file
```

It should have the variables for the **dev** environment:
```bash
window.REACT_APP_API_URL='https://www.bookmarks.dev/api/public/bookmarks'
window.REACT_APP_ENVIRONMENT='DEV'
window.REACT_APP_NAVBAR_COLOR='LightGreen'
```

But better you can see it in action by port forwarding the application. You know now how it goes:
```bash
kubectl port-forward svc/multi-stage-react-app-example 3001:80
```

Navigate to [http://localhost:3001](http://localhost:3001) and now you should see the **DEV** environment on the navigation bar.

In continuous delivery pipeline you could have two steps:
1. create the configMap based on the dev.properties file
2. deploy on the target cluster with `kubectl` specified above

#### Tear down 

```bash
kubectl delete -f kubernetes
```

//TODO - mention problem with the update - https://github.com/kubernetes/kubernetes/issues/50345

// TODO maybe remove - We will create the configMap `multi-stage-react-app-example-config`. [Here](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/kubernetes/config-dev.yaml) it is the content `dev` environment:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: multi-stage-react-app-example-config
data:
  config.js: |
    window.REACT_APP_API_URL='https://www.bookmarks.dev/api/public/bookmarks'
    window.REACT_APP_ENVIRONMENT='DEV'
    window.REACT_APP_NAVBAR_COLOR='LightGreen'
```


## Deploy on Kubernetes with Kustomize
What if now when deployment into the **prod** cluster you want to have two pods, instead of one serving the web app. Of course 
you could modify the _deployment.yaml_ file, specify there 2 replicas instead of one and deploy. You can solve this in an elegant
matter by using Kustomize. 

>[Kustomize](https://github.com/kubernetes-sigs/kustomize) is a standalone tool to customize Kubernetes objects through
a [kustomization file](https://github.com/kubernetes-sigs/kustomize/blob/master/docs/glossary.md#kustomization). Since 1.14, Kubectl
also supports the management of Kubernetes objects using a kustomization file.

With Kustomize we will define base resources in the so called **bases** (cross cutting concerns available in environments) and in the **overlays** the properties that are specific for the different deployments. We place the files in the [kustomize](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/) folder - `tree kustomize`:
```
kustomize/
├── base
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── overlays
    ├── dev
    │   ├── dev.properties
    │   └── kustomization.yaml
    ├── local
    │   ├── kustomization.yaml
    │   └── local.properties
    └── prod
        ├── deployment-prod.yaml
        ├── kustomization.yaml
        └── prod.properties
```

In the base folder we define the service and deployment, because in this case they are overall the same (except the 2 replicas for prod, but we'll deal
with that later).

### Deploy to **dev** cluster with Kustomize
Let's say we watn to deploy to our **dev** cluster with kustomize. For that we will use the `dev` overlays.
In the dev [kustomization file](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/kustomize/overlays/dev/kustomization.yaml):
```
bases:
  - ../../base

configMapGenerator:
  - name: multi-stage-react-app-example-config
    files:
      - dev.properties
```

we point to the `bases` defined before and use the _dev.properties_ file to [generate the configMap](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/#generating-resources). 

Before we apply the `dev` overlay to the cluster we can check what it generates by issuing the following command:
```bash
kubectl kustomize kustomize/overlays/dev
```

> Note that the generated configMap name has a suffix (something like - `multi-stage-react-app-example-config-gdgg4f85bt`), which
is appended by hashing the contents This ensures that a new ConfigMap is generated when the content is changed. In the initial 
deploymant.yaml file is still referenced by `multi-stage-react-app-example-config`, but in the generated Deployment object it has
the generated name. 

To apply the "dev kustomization" use the following command:
```bash
kubectl apply -k kustomize/overlays/dev # <kustomization directory>
```

Now port forward (`kubectl port-forward svc/multi-stage-react-app-example 3001:80`) and go to [http://localhost:3001](http://localhost:3001)

#### Update an environment variable value
If you for example would like to update the value of an environment variable say, `window.REACT_APP_NAVBAR_COLOR='Blue'`, what you need to
do is apply gain the **dev** overlay:
```bash
kubectl apply -k kustomize/overlays/dev
#result similar to the following
configmap/multi-stage-react-app-example-config-dg44f5bkhh created
service/multi-stage-react-app-example unchanged
deployment.apps/multi-stage-react-app-example configured
```

Note the a new configMap is created and is applied with the deployment. Reload and now the navigation bar is blue. 

#### Tear down
```
kubectl delete -k kustomize/overlays/dev
```

#### Deploy to production with kustomize
As mentioned before, maybe for production you would like to have two replicas delivering our application. For that you 
can create an **prod** overlay that derives from that common **base** as the **dev** overlay.

It defines extra an [deployment-prod.yaml](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/kustomize/overlays/prod/deployment-prod.yaml) file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: multi-stage-react-app-example
spec:
  replicas: 2
```
which is a partial Deployment resource and we reference in the prod [kustomization.yaml](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/kustomize/overlays/prod/kustomization.yaml) file under [`patchesStrategicMerge`](https://github.com/kubernetes-sigs/kustomize/blob/master/docs/fields.md#patchesstrategicmerge):
```yaml
bases:
  - ../../base

patchesStrategicMerge:
  - deployment-prod.yaml

configMapGenerator:
  - name: multi-stage-react-app-example-config
    files:
      - config.js=prod.properties
```
 
You can see it's being modified by running:
```bash
kubectl kustomize kustomize/overlays/prod
```
and then apply it:
```bash
kubectl apply -k kustomize/overlays/prod
```

If you run `kubectl get pods` you should now see two entries, something liks:
```bash
NAME                                             READY   STATUS    RESTARTS   AGE
multi-stage-react-app-example-59c5486dc4-2mjvw   1/1     Running   0          112s
multi-stage-react-app-example-59c5486dc4-s88ms   1/1     Running   0          112s
```

> Now you can port forward and access the application the way you know it
##### Tear down
```bash
kubectl delete -k kustomize/overlays/prod
```

## Deploy on Kubernetes with [Helm](https://helm.sh/)

What is Helm? According to the documentation:
> [Helm](https://github.com/helm/helm#kubernetes-helm) is a tool that streamlines installing and managing Kubernetes applications. Think of it like apt/yum/homebrew for
Kubernetes. 

Helm uses the so called Kubernetes charts. Charts are packages of pre-configured Kubernetes resources. If you want to learn
more about Helm read the [docs](https://helm.sh/docs/), we won't go into much details here, only punctual where it is needed. 

At the moment Helm has a client (`helm`) and a server (`tiller`). Tiller runs inside of your Kubernetes cluster, and manages releases (installations)
of your charts. 

### Helm installation
On MacOS you can install the client with homebrew:
```bash
brew install kubernetes-helm
```
For other platforms see [Installing the Helm Client](https://helm.sh/docs/using_helm/#installing-helm).

To install Tiller on your local Kubernetes cluster for testing just call the following command:
```bash
helm init

#result should something similar to the following:
Creating /Users/ama/.helm 
Creating /Users/ama/.helm/repository 
Creating /Users/ama/.helm/repository/cache 
Creating /Users/ama/.helm/repository/local 
Creating /Users/ama/.helm/plugins 
Creating /Users/ama/.helm/starters 
Creating /Users/ama/.helm/cache/archive 
Creating /Users/ama/.helm/repository/repositories.yaml 
Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com 
Adding local repo with URL: http://127.0.0.1:8879/charts 
$HELM_HOME has been configured at /Users/ama/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation

``` 
 
To check the helm version you can run then the following command:
```bash
$ helm version
Client: &version.Version{SemVer:"v2.14.3", GitCommit:"0e7f3b6637f7af8fcfddb3d2941fcc7cbebb0085", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.14.3", GitCommit:"0e7f3b6637f7af8fcfddb3d2941fcc7cbebb0085", GitTreeState:"clean"}
```

### Helm setup in project
The helm configuration is present in the [helm-chart](https://github.com/CodepediaOrg/multi-stage-react-app-example/tree/master/helm-chart)
which was initially created via the `helm create helm-chart` command and adjusted for this app's needs.

#### Templates
The most important piece of the puzzle is the templates/ directory. This where Helm finds the YAML definitions for  your
Services, Deployments and other Kubernetes resources. 
Let's take a look at the [service](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/helm-chart/templates/service.yaml) definition:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "helm-chart.fullname" . }}
  labels:
    app.kubernetes.io/name: {{ include "helm-chart.name" . }}
    helm.sh/chart: {{ include "helm-chart.chart" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app.kubernetes.io/name: {{ include "helm-chart.name" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
```

It looks similar to the one used when installing with Kubectl or Kustomize, only that the values are substituted by Helm
at deployment with the ones from Helm-specific objects. 

#### Values
Values provide a way to override template defaults with your own configuration. They are present in the template
via the `.Values` object as seen above. 

Values can be set during `helm install` and `helm upgrade` operations, either by passing them in directly, or by uploading a [`values.yaml`](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/helm-chart/values.yaml) file.

#### The configMap
This time we will create the [configMap](https://github.com/CodepediaOrg/multi-stage-react-app-example/blob/master/helm-chart/templates/configMap.yaml) as Kubernetes object:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: multi-stage-react-app-example-config
  annotations:
    # https://github.com/helm/helm/blob/master/docs/charts_hooks.md
    "helm.sh/hook-delete-policy": "before-hook-creation"
    "helm.sh/hook": pre-install, pre-upgrade
data:
  config.js: {{ toYaml .Values.configValues | indent 4 }}
```

### Deploy to local cluster with helm
Before deploying with helm you might want to examine the chart for possible issues and do a `helm lint`:
```bash
helm lint helm-chart
```
and execute a dry-run
```bash

```


 

### Skaffold
https://skaffold.dev/docs/how-tos/profiles/
https://skaffold.dev/docs/references/yaml/
