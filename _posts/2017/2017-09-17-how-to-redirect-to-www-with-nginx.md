---
layout: post
title: How to redirect domain to www url with Nginx
description: "Snippet from nginx config file that redirects all requests (http and https) to the www URL"
author: ama
permalink: /ama/how-to-redirect-domain-to-www-url-with-nginx
published: true
categories: [tutorial]
tags:
    - codever
    - nginx
    - networking
---

This post presents the snippet from the Nginx configuration that redirects all request to **https://www.codever.dev**:

```
# redirect HTTP to www
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name bookmarks.dev www.codever.dev;

    return 301 https://www.bookmarks.dev$request_uri;
}

# redirect HTTPS to wwww
server {
    listen 443;
    server_name bookmarks.dev;

    return 301 https://www.bookmarks.dev$request_uri;
}
```

> Note the missing `www.codever.dev` in the second `server/server_name` entry, to avoid an infinite loop

Lots of other good Nginx resources can be found if you search for the **nginx** tag on **codingmarks**: [https://www.codever.dev?q=[nginx]](https://www.codever.dev?q=[nginx])

{% include source-code-codever.html %}

