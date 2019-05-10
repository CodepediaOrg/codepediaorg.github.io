---
layout: post
title: Header not showing in Angular2 http response
description: "How I managed to show the Location header in the Angular2 http response"
author: ama
permalink: /ama/header-not-showing-in-angular2-http-response
published: false
categories: [nodejs]
tags: [codingpedia bookmarks, angular, angular2, webpack, bugfix]
---


Solution:

```
//add CORS support
app.use(function(req, res, next) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'POST, GET, PUT, PATCH, DELETE, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization, Location');
  res.setHeader('Access-Control-Expose-Headers', 'Content-Type, Authorization, Location');
  next();
});
```

https://www.html5rocks.com/en/tutorials/cors/

Access-Control-Expose-Headers (optional) - The XMLHttpRequest 2 object has a getResponseHeader() method that returns the value of a particular response header. During a CORS request, the getResponseHeader() method can only access simple response headers. Simple response headers are defined as follows:

Cache-Control
Content-Language
Content-Type
Expires
Last-Modified
Pragma
If you want clients to be able to access other headers, you have to use the Access-Control-Expose-Headers header. The value of this header is a comma-delimited list of response headers you want to expose to the client.

## References
