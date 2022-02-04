---
layout: post
title: Delete kubernetes deployment with kubectl
description: "Delete kubernetes deployment with kubectl code snippet"
author: ama
permalink: /snippets/61fa84d488422d447cdeb1e4/delete-kubernetes-deployment-with-kubectl
published: true
snippetId: 61fa84d488422d447cdeb1e4
categories: [snippets]
tags: [kubernetes, kubectl, codever-snippets]
---

If you don't know exactly the deployment name in the current context you can use the following command to list all deployments:

```shell
kubectl get deploy
```

You should get a list of deployments similar to the following:

```
NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
pdv-location-batch-test                    0/1     1            0           2m12s
connector-partner-service                  1/1     1            1           330d
```

The use `kubectl delete deploy <deployment name>` to delete the deployment , in our case `pdv-location-batch-test`

```shell
kubectl delete deploy pdv-location-batch-test
```

You should get something similar to the following:

```
deployment.apps "pdv-lokation-batch-test" deleted
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="61fa84d488422d447cdeb1e4" %}
