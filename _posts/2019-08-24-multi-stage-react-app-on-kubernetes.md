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



### Skaffold
https://skaffold.dev/docs/how-tos/profiles/
https://skaffold.dev/docs/references/yaml/
