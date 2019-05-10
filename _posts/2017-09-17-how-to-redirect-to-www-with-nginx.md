---
layout: post
title: How to redirect domain to www url with Nginx
description: "Snippet from nginx config file that redirects all requests (http and https) to the www URL"
author: ama
permalink: /ama/how-to-redirect-domain-to-www-url-with-nginx
published: true
categories: [nginx]
tags: [codingmarks, nginx, networking]
---

This post presents the snippet from the Nginx configuration that redirects all request to **https://www.codingmarks.org**:

```
# redirect HTTP to www
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name codingmarks.org www.codingmarks.org;

    return 301 https://www.codingmarks.org$request_uri;
}

# redirect HTTPS to wwww
server {
    listen 443;
    server_name codingmarks.org;

    return 301 https://www.codingmarks.org$request_uri;
}
```

> Note the missing `www.codingmarks.org` in the second `server/server_name` entry, to avoid an infinite loop

Lots of other good Nginx resources can be found if you search for the **nginx** tag on **codingmarks**: [https://www.codingmarks.org?q=[nginx]](https://www.codingmarks.org?q=[nginx])

{% include source-code-codingmarks.html %}

