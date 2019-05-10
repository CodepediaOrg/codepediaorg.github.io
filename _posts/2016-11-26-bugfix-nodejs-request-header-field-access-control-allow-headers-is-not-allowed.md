---
layout: post
title: Request header field is not allow by Access-Control-Allow-Headers in preflight response
description: "This erros happens in an Angular 2 app, protected by Keycloak that uses a NodeJS/ExpressJS backend."
author: ama
permalink: /ama/first-rest-operation-with-express
published: false
categories: [nodejs]
tags: [codingpedia bookmarks, keycloak, nodejs, expressjs, bug]
---

After enabling Keycloak on the front-end for the Codingpedia-Bookmarks project, I get the following error in the console and the bookmarks
are not being loaded anymore.

TODO - add link to GITHUB (project and sources)

```
XMLHttpRequest cannot load http://localhost:3000/bookmarks.
Request header field authorization is not allowed by Access-Control-Allow-Headers in preflight response.
```

This happens in the preflight request[^1], which  is a CORS request that checks to see if the CORS protocol is understood. It uses `OPTIONS` as method and includes these headers:
`Access-Control-Request-Method`
Indicates which method a future CORS request to the same resource might use.

`Access-Control-Request-Headers`
Indicates which headers a future CORS request to the same resource might use.

[^1]: <https://fetch.spec.whatwg.org/#http-cors-protocol>

The fix is to add the __authorization__ header mentioned in the error in the ExpressJs middleware:

```javascript
//add CORS support
app.use(function(req, res, next) {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'POST, GET, PUT, PATCH, DELETE, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  next();
});
```

This is so because from now on the future requests will contain the bearer token in the __Authorization__ header. Once enabled you can clearly
see that in the __Request Headers__ of following __GET__ request:

```
GET /bookmarks HTTP/1.1
Host: localhost:3000
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36
Origin: http://localhost:8080
authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzT1hpUGc4ZWJKQ2VNYjJtemdUcFNVLXpFT1NCMDBaREh0S3ZBWGdydkFRIn0.eyJqdGkiOiIyMzE2MTE1ZS01MTJhLTRjYWQtYTZjNS00ZGZkODA1YTlkZWMiLCJleHAiOjE0ODAxNDU1NjMsIm5iZiI6MCwiaWF0IjoxNDgwMTQ1MjYzLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgzODAvYXV0aC9yZWFsbXMvY29kaW5ncGVkaWEiLCJhdWQiOiJib29rbWFya3MiLCJzdWIiOiIwNzhmOWYwOS1lMTE0LTQ4ZmQtYWI4NS00NzAwNTlkMGMyNzgiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJib29rbWFya3MiLCJub25jZSI6ImRjMThhZTdhLTdjMTctNGEwMi1iYTU2LWEwMzJkODdhNzA2ZCIsImF1dGhfdGltZSI6MTQ4MDE0MzI5OSwic2Vzc2lvbl9zdGF0ZSI6IjgwMTkzM2VkLWU4OGEtNGFjNi05NzQyLWIzM2M4Y2RhZmZlNiIsImFjciI6IjAiLCJjbGllbnRfc2Vzc2lvbiI6ImRkZDFlZmJhLWJkYzYtNDcyNS1iZmFmLWQ4MzcwMjM4NWMzZSIsImFsbG93ZWQtb3JpZ2lucyI6WyJodHRwOi8vbG9jYWxob3N0OjgzODAiLCJodHRwOi8vbG9jYWxob3N0OjgwODAiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsInZpZXctcHJvZmlsZSJdfX0sIm5hbWUiOiJBZHJpYW4gTWF0ZWkgR21haWwiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhZHJpYW5tYXRlaUBnbWFpbC5jb20iLCJnaXZlbl9uYW1lIjoiQWRyaWFuIiwiZmFtaWx5X25hbWUiOiJNYXRlaSBHbWFpbCIsImVtYWlsIjoiYWRyaWFubWF0ZWlAZ21haWwuY29tIn0.vG0B03nhQL_5SZe11HbGL227HdmCWrdimvHf74hZZm5sVbhq6flAs888nADT6nOo4dCVI4YljneM2tapBw9ZasrLzB4hKi07auFcDRcr0hCnBBpTnoO5_pRLEilAe5VI-Q8sSqeSnyXF_u8R0cuTPvJTHmtR_YcgmF8jk487LpeZ4dqAg0_Q5RReyZl76qoD8t2p6UPKn41ltvBY0lb5m8ezT3DjPBvC8Hau56W_BS0TPSur-xx65eyDSBZu8RybSm1_9sjYKwvMFU27G61LrhXoLi400ZTIadi4kndD3nBEjGcmAOy_enNAQbwaWfQcFZzbVZBpVHy2sBZkKFnjkA
Accept: */*
Referer: http://localhost:8080/
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: en-US,en;q=0.8,de;q=0.6,ro;q=0.4,fr;q=0.2
```


## References
