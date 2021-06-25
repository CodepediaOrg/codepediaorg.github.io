---
layout: post
title: Kubectl commands to list services, deployments and pods from namespace
description: "Code snippet showing kubectl commands to list services, deployments and pods from namespace"
author: ama
permalink: /snippets/60d577854095c2046612eaf5/kubectl-commands-to-list-services-deployments-and-pods-from-namespace
published: true
snippetId: 60d577854095c2046612eaf5
categories: [snippets]
tags: [kubernetes, kubectl, codever]
---

To list services, pods and deployments resources from kubernetes namespace use the `kubectl get`
 command with `--namespace=my-namespace` (short `-n`)

```shell
# Set the "current" namespace for context
kubectl config set-context --current --namespace=my-namespace

# Get commands with basic output
kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the current namespace, with more details
kubectl get deployment my-dep                 # List a particular deployment
kubectl get pods                              # List all pods in the namespace
kubectl get pods  -n my-namespace             # List all pods in the "my-namespace" namespace (in namespace not set)
kubectl get pod my-pod -o yaml                # Get a pod's YAML
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://kubernetes.io/docs/reference/kubectl/cheatsheet/" target="_blank" style="font-weight: lighter">
     https://kubernetes.io/docs/reference/kubectl/cheatsheet/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60d577854095c2046612eaf5" %}
