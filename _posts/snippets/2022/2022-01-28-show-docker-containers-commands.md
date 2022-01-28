---
layout: post
title: Show docker containers commands
description: "Show docker containers commands code snippet"
author: ama
permalink: /snippets/60bc572a5eeee76d9163b692/show-docker-containers-commands
published: true
snippetId: 60bc572a5eeee76d9163b692
categories: [snippets]
tags: [docker, codever-snippets]
---

To list the docker **running** containers use the following command:


```shell
docker ps
```

To show **all** running containers use the `-a` (`--all`) option:

```shell
docker ps -a
```

If you want to filter, by name for example you can use the `--filter` option with the `name=containing_text` value

```shell
$ docker ps --filter "name=codever"
8a01ae8ef526   jboss/keycloak:6.0.1   "/opt/jboss/tools/do…"   3 days ago      Exited (255) 10 hours ago   0.0.0.0:8480->8080/tcp     codever-keycloak
f7c51c81dc4f   mongo:3.4              "docker-entrypoint.s…"   3 days ago      Exited (255) 10 hours ago   0.0.0.0:27017->27017/tcp   codever-mongo
5b2f88b9fe57   postgres               "docker-entrypoint.s…"   3 days ago      Exited (255) 10 hours ago   5432/tcp                   codever-postgres
```

You could also obtain similar results if piping the `docker ps` command to `grep` - `docker ps -a | grep codever`

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.docker.com/engine/reference/commandline/ps/" target="_blank" style="font-weight: lighter">
     https://docs.docker.com/engine/reference/commandline/ps/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60bc572a5eeee76d9163b692" %}
