---
layout: post
title: How to connect to SQL Plus in oracle xe running in docker container
description: "How to connect to SQL Plus in oracle xe running in docker container code snippet"
author: ama
permalink: /snippets/605b4228cfab4d624761569f/how-to-connect-to-sql-plus-in-oracle-xe-running-in-docker-container
published: true
snippetId: 605b4228cfab4d624761569f
categories: [snippets]
tags: [oracle, docker, sql, sqlplus, database, codever-snippets]
---

Issue the following command in terminal:

```shell
docker exec -it YOUR_IMAGE bash
```

Once in bash issue the following command, and you are there:

```shell
sqlplus YOUR_USERNAME/USERNAME_PASSWORD@localhost:1521/XE
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="605b4228cfab4d624761569f" %}
