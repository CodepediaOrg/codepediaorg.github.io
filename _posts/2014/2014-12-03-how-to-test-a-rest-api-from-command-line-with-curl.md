---
title: How to test a REST api from command line with curl
date: 2014-12-03T07:19:41+00:00
author: ama
layout: post
published: true
permalink: /ama/how-to-test-a-rest-api-from-command-line-with-curl/
description: "This post presents examples of making CRUD HTTP calls against a backend REST API. The API chosen
supports www.codever.dev"
categories:
  - article
tags:
  - curl
  - http
  - rest
  - api
  - testing
  - codever
---

If you want to quickly test your REST api from the command line, you can use [curl](https://curl.haxx.se/).
 In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API.
  For the purpose of this blog post I will be using the REST api that supports [www.codever.dev](https://www.codever.dev).
  The API is [documented with OpenAPI](https://github.com/CodeverDotDev/codever/blob/master/backend/docs/openapi/openapi.yaml)
  and available for testing in the browser at [https://www.codever.dev/api/docs/](https://www.codever.dev/api/docs/).

<!--more-->

* TOC
{:toc}

## Introduction

In the first part of the blog post I will do a brief introduction to curl and what it can do (HTTP requests with options).
 In the second part I will show examples with different HTTP operations from [bookmarks.dev-api](https://www.codever.dev/api/docs).


### What is curl?

Curl is a command line tool and library for transferring data with URL syntax, supporting DICT, FILE, FTP, FTPS, Gopher,
 HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, Telnet and TFTP.
  curl supports SSL certificates, HTTP POST, HTTP PUT, FTP uploading, HTTP form based upload, proxies, HTTP/2, cookies,
   user+password authentication (Basic, Digest, NTLM, Negotiate, Kerberos&#8230;), file transfer resume, proxy tunneling and more.

As mentioned, I will be using curl to simulate HEAD, GET, POST, PUT and DELETE request calls against a REST API.

### HEAD requests

If you want to check if a resource is serviceable, what kind of headers it provides and other useful meta-information
 written in response headers, without having to transport the entire content, you can make a `HEAD` request.

Let's say I want to see what I would GET when requesting latest public bookmarks. I would issue the following HEAD request with curl:

**Request**

```shell
curl -I https://www.codever.dev/api/public/bookmarks
```

OR

```shell
curl -i -X HEAD https://www.codever.dev/api/public/bookmarks
```

**Curl options **
  * `-I` or `--head` - fetch the headers only
  * `-i, --include` - include the HTTP response headers in the output
  * `-X, --request` - specify a custom request method to use when communicating with the HTTP server (GET, PUT, DELETE&)

**Response**

```shell
HTTP/1.1 200 OK
Server: nginx/1.12.0
Date: Sun, 23 Feb 2020 21:31:40 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 98452
Connection: keep-alive
X-Powered-By: Express
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization, Location
Access-Control-Expose-Headers: Content-Type, Authorization, Location
ETag: W/"18094-R7MFvLpccDdVu3q8rR1UQBrAaX8"
Strict-Transport-Security: max-age=63072000; includeSubdomains
X-Content-Type-Options: nosniff
```

Note the following headers
  * `Access-Control-Allow-Headers: Content-Type, Authorization, Location`
  * `Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS`
  * `Access-Control-Allow-Origin: *`

in the response.

They've been added to support [Cross-Origing Resource Sharing (CORS)](https://www.w3.org/TR/cors/).


### GET request

Executing curl with no parameters on a URL (resource) will execute a GET.

**Request**

```shell
curl https://www.codever.dev/api/version
```

which is equivalent with

```shell
curl -X GET "https://www.codever.dev/api/version" -H "accept: application/json"
```

**Response**

```json
{"version":"7.0.0","gitSha1":"71eb40fb6d224d5d9a90c89ae943390e15f001c3"}
```


Note the use of `accept: application/json`

**Curl options **

  * `-H, --header` : customer header to pass to the server

If you want to have it displayed prettier I suggest you use a tool like [jq](https://github.com/stedolan/jq):

**Request**

```shell
curl https://www.codever.dev/api/version | jq .
```

**Response**
```shell
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    72  100    72    0     0    611      0 --:--:-- --:--:-- --:--:--   615
{
  "version": "7.0.0",
  "gitSha1": "71eb40fb6d224d5d9a90c89ae943390e15f001c3"
}
```

If you don't want the progress meter (first part) shown, you can silent curl:

**Request**
```shell
curl -s https://www.codever.dev/api/version | jq .
```

**Response**
```shell
{
  "version": "7.0.0",
  "gitSha1": "71eb40fb6d224d5d9a90c89ae943390e15f001c3"
}

```

**Curl options **

  * `-s, --silent` : silent or quiet mode. Don't show progress meter or error messages. Makes Curl mute.
   It will still output the data you ask for, potentially even to the terminal/stdout unless you redirect it.


An alternative to **jq** is to use [Python](https://www.python.org/) if you have it on your machine:

```shell
curl -s https://www.codever.dev/api/version | python -m json.tool
```

### Curl request with multiple headers
All the responses from the api of [bookmarks.dev](https://github.com/CodeverDotDev/codever) are gzipped.
We could ask for the gzipped variant by issuing the following request:

**Request**

```shell
curl -v -H "Accept:application/json" -H "Accept-encoding:gzip" https://www.codever.dev/api/version
```

**Curl options **

  * `-v, --verbose` : the opposite of `--silent`, make the operation more talkative

To achieve that you need to simply **add another** `-H` option with the corresponding value. In this case you
 would get some unreadable characters in the content, if you do not redirect the response to a file:

**Response**
```shell
> GET /api/version HTTP/1.1
> Host: www.codever.dev
> User-Agent: curl/7.54.0
> Accept:application/json
> Accept-encoding:gzip
>
< HTTP/1.1 200 OK
< Server: nginx/1.12.0
< Date: Fri, 06 Mar 2020 14:45:39 GMT
< Content-Type: application/json; charset=utf-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< X-Powered-By: Express
< Access-Control-Allow-Origin: *
< Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS
< Access-Control-Allow-Headers: Content-Type, Authorization, Location
< Access-Control-Expose-Headers: Content-Type, Authorization, Location
< ETag: W/"48-3r0bLwzjWi409jIuXDFQE7IoNtI"
< Strict-Transport-Security: max-age=63072000; includeSubdomains
< X-Content-Type-Options: nosniff
< Content-Encoding: gzip
<
* Connection #0 to host www.codever.dev left intact
�V*K-*���S�R2�3�3P�QJ�, �H4��&��%������X&Z$[X&�Z����&��
```

## CRUD Operations on Bookmarks.dev API

Those were some basic curl HTTP calls with a few options. Now we will combine them and show examples against a production
ready API. For the examples I will use the API running on localhost. It is really easy to setup with Docker-compose if
you follow the instructions from the [Readme](https://github.com/CodeverDotDev/codever#readme) file.

The API is protected with [Keycloak](https://www.keycloak.org) and bearer token. A way to obtain a bearer token in Keycloak
 is to enable Direct Access Grants for the client - this corresponds to the [Resource Owner Password Credentials](https://tools.ietf.org/html/rfc6749#section-1.3.3)
in the OAuth2 Specification. Thus the user's credentials are sent within form parameters. Of course we can do that with curl too:

**Request**
```bash
curl  \
  -d 'client_id=bookmarks' \
  -d 'username=mock' \
  -d "password=mock" \
  -d 'grant_type=password' \
  'http://localhost:8480/auth/realms/bookmarks/protocol/openid-connect/token' \
| jq .
```

> The the `username` and `password` are from the initial [set up](https://github.com/CodeverDotDev/codever#readme).

The response looks something like the following:
```shell
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJMNHV6eUFYbUlTSDJPRi00c2VZZ2Z3UWtJT204QTR3cnBDV0JHSVdOU2c4In0.eyJqdGkiOiJlY2I3YjE0Yi02ZTZhLTQyMzEtYWI5NS04ZDAwZmI5YjNiN2MiLCJleHAiOjE1ODM1MTg0OTUsIm5iZiI6MCwiaWF0IjoxNTgzNTE0ODk1LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0Ojg0ODAvYXV0aC9yZWFsbXMvYm9va21hcmtzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjRjNjE3ZjJiLTJiYWQtNDk4Yi1hOWM2LTRlOWE4YzMwMzc5OCIsInR5cCI6IkJlYXJlciIsImF6cCI6ImJvb2ttYXJrcyIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6ImE5NGVjNGI3LTNhY2YtNGMzOS1iOTBlLTAzOWVlNzZjY2EwYiIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiUk9MRV9VU0VSIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoiZW1haWwgcHJvZmlsZSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYW1lIjoiQWRyaWFuIE1hdGVpIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiYW1hIiwiZ2l2ZW5fbmFtZSI6IkFkcmlhbiIsImZhbWlseV9uYW1lIjoiTWF0ZWkiLCJlbWFpbCI6ImFkcmlhbm1hdGVpQGdtYWlsLmNvbSJ9.cymI5LzrUFje4vZNYMzvS0yhdVx4V8u_XVDUxi4sUk4tKpI2xcFQqWEiN_hxCLHpCDjNCqjw2JQUxQTvbQe_Wf8TGWz1f3nXhKg5CEw29ArCV3lFZL7_QmUOod53KnQ-9umSe2EISv2EbD0__idaivyCIerfV4M0wfgG31iyLPJ6_Pl_nJiw5RgifdqNljpKL9znpt5l3PiU7x2ACGv9V_GPvwAnU-9VxIuEqfErgc6IfhQxg9vuI_kHprXu-ClATA_Zg_xNEw53TD3qHJV_5sCu58MORhPv8fddcAZeLHxsr9sVyhFlmKMz1ZGWH0q5QZwLzKlGaaVr72Y2KSEPyA",
  "expires_in": 3600,
  "refresh_expires_in": 36000,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiNDEwODA3My0yNjZiLTQ4YzAtOGJmYi04ZGJjZmE2NjYxMmEifQ.eyJqdGkiOiI0N2VhYzc3NS1mODdjLTRiYjMtODQxZi0wYTViZGZkMzIxZjYiLCJleHAiOjE1ODM1NTA4OTUsIm5iZiI6MCwiaWF0IjoxNTgzNTE0ODk1LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0Ojg0ODAvYXV0aC9yZWFsbXMvYm9va21hcmtzIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4NDgwL2F1dGgvcmVhbG1zL2Jvb2ttYXJrcyIsInN1YiI6IjRjNjE3ZjJiLTJiYWQtNDk4Yi1hOWM2LTRlOWE4YzMwMzc5OCIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJib29rbWFya3MiLCJhdXRoX3RpbWUiOjAsInNlc3Npb25fc3RhdGUiOiJhOTRlYzRiNy0zYWNmLTRjMzktYjkwZS0wMzllZTc2Y2NhMGIiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiUk9MRV9VU0VSIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoiZW1haWwgcHJvZmlsZSJ9.MNxMEtq5zcVTjxCnws5EotnfuUH7uY_kbKDwRRzGcko",
  "token_type": "bearer",
  "not-before-policy": 0,
  "session_state": "a94ec4b7-3acf-4c39-b90e-039ee76cca0b",
  "scope": "email profile"
}
```

With jq is really easy to extract just the `access_token`:

**Request**
```bash
curl -s \
  -d 'client_id=bookmarks' \
  -d 'username=ama' \
  -d "password=ama" \
  -d 'grant_type=password' \
  'http://localhost:8480/auth/realms/bookmarks/protocol/openid-connect/token' \
| jq -r '.access_token'
```

**Response**
```shell
eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJMNHV6eUFYbUlTSDJPRi00c2VZZ2Z3UWtJT204QTR3cnBDV0JHSVdOU2c4In0.eyJqdGkiOiJiZDQzZWM1ZC1kODkyLTRkYzktOWNjYy03MWViOGE2YWI0MWEiLCJleHAiOjE1ODM1MTg2NTYsIm5iZiI6MCwiaWF0IjoxNTgzNTE1MDU2LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0Ojg0ODAvYXV0aC9yZWFsbXMvYm9va21hcmtzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjRjNjE3ZjJiLTJiYWQtNDk4Yi1hOWM2LTRlOWE4YzMwMzc5OCIsInR5cCI6IkJlYXJlciIsImF6cCI6ImJvb2ttYXJrcyIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6ImY1ZmVkMjIzLTE0ZjQtNDJmZC04YTA5LWE1YWFmNWJmZjMzOCIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiUk9MRV9VU0VSIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoiZW1haWwgcHJvZmlsZSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYW1lIjoiQWRyaWFuIE1hdGVpIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiYW1hIiwiZ2l2ZW5fbmFtZSI6IkFkcmlhbiIsImZhbWlseV9uYW1lIjoiTWF0ZWkiLCJlbWFpbCI6ImFkcmlhbm1hdGVpQGdtYWlsLmNvbSJ9.EJzB7TkOrqY_enYHWgs_6NszI9PtQYfp5yco7OTF4LhcqkKXCoUvE2Jpc6gupX6uMtNPQZtWXSnwVPl8XGR4Z32sSMkxEvDj9B4zPuU2CBe8z9LZFwPjlu5ZMOnl1_hBjNmL8UHWTdCNhYf75PCDneCUM6ugbq5DaMhKkCHo8WD_x8A5I3hSM5pLSow3C82ZdMqkZbyxv28_rul9vsCsppN3CMXQjYDNn1UuVeNl8b5O-KTSumrVjVzw_wjoswva7h0Y3pnQptABDML5Q1mf__FFFHLGN6Y26Ezrjm086oRp-ntxFA9gI41toQ4xgoMyX-6obOhMGwa10RzdNbP4CA
```

**Curl options**

  * `-d, --data` : (HTTP) Sends the specified data in a POST request to the HTTP server, in the same way that a browser
   does when a user has filled in an HTML form and presses the submit button. This will cause curl to pass the data to
   the server using the content-type `application/x-www-form-urlencoded`.


### Create bookmark - POST

**Request**

```shell
curl -i -X POST "http://localhost:3000/api/personal/users/4c617f2b-2bad-498b-a9c6-4e9a8c303798/bookmarks" \
-H "accept: */*" -H "Authorization: Bearer eyJhbGciOiJ...."  \
-H "Content-Type: application/json" -d "{\"name\":\"How to test a REST api from command line with curl – CodepediaOrg\",\"location\":\"https://www.codepedia.org/ama/how-to-test-a-rest-api-from-command-line-with-curl/\",\"language\":\"en\",\"tags\":[\"rest\",\"curl\",\"api\",\"testing\"],\"publishedOn\":\"2020-03-05\",\"sourceCodeURL\":\"https://github.com/CodeverDotDev/codever\",\"description\":\" In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API. For the purpose of this blog post I will be using the REST api that supports [www.codever.dev](https://www.codever.dev)\",\"descriptionHtml\":\"<p>In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API. For the purpose of this blog post I will be using the REST api that supports <a href=\\\"https://www.codever.dev\\\">www.codever.dev</a></p>\",\"userId\":\"4c617f2b-2bad-498b-a9c6-4e9a8c303798\",\"public\":true,\"lastAccessedAt\":\"2020-03-06T20:14:28.101Z\",\"likeCount\":0}"
```

> Note the Bearer token is reduced here (`Bearer eyJhbGciOiJ....`) and in the following examples for brevity

**Response**
```shell
HTTP/1.1 201 Created
X-Powered-By: Express
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization, Location
Access-Control-Expose-Headers: Content-Type, Authorization, Location
Location: http://localhost:3000/api/personal/users/4c617f2b-2bad-498b-a9c6-4e9a8c303798/bookmarks/5e62b18b59770b5487a4c741
Content-Type: application/json; charset=utf-8
Content-Length: 79
ETag: W/"4f-26GcBfsvgN8d+T+zqql3Y5R+Rl8"
Date: Fri, 06 Mar 2020 20:24:44 GMT
Connection: keep-alive

{"response":"Bookmark created for userId 4c617f2b-2bad-498b-a9c6-4e9a8c303798"}
```

> Note the `location` header - it contains the URL of the new created resource

### Read created resource - GET

We will read the previously created bookmark by issuing an GET request on the url from the `location` header

```shell
curl -s -X GET "http://localhost:3000/api/personal/users/4c617f2b-2bad-498b-a9c6-4e9a8c303798/bookmarks/5e62b18b59770b5487a4c741" \
 -H "accept: application/json" \
 -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOi..." | jq .
```

**Response**

```shell
{
  "tags": [
    "rest",
    "curl",
    "api",
    "testing"
  ],
  "_id": "5e62b18b59770b5487a4c741",
  "name": "How to test a REST api from command line with curl – CodepediaOrg",
  "location": "https://www.codepedia.org/ama/how-to-test-a-rest-api-from-command-line-with-curl/",
  "language": "en",
  "description": " In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API. For the purpose of this blog post I will be using the REST api that supports [www.codever.dev](https://www.codever.dev)",
  "descriptionHtml": "<p>In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP Requests against a REST API. For the purpose of this blog post I will be using the REST api that supports <a href=\"https://www.codever.dev\">www.codever.dev</a></p>",
  "publishedOn": "2020-03-05T00:00:00.000Z",
  "sourceCodeURL": "https://github.com/CodeverDotDev/codever",
  "userId": "4c617f2b-2bad-498b-a9c6-4e9a8c303798",
  "public": true,
  "likeCount": 0,
  "youtubeVideoId": null,
  "stackoverflowQuestionId": null,
  "createdAt": "2020-03-06T20:24:43.998Z",
  "updatedAt": "2020-03-06T20:24:43.998Z"
}
```

### Update created resource - PUT

**Request**
```shell
curl -s -X PUT "http://localhost:3000/api/personal/users/4c617f2b-2bad-498b-a9c6-4e9a8c303798/bookmarks/5e62b18b59770b5487a4c741" \
 -H "accept: application/json" -H "Authorization: Bearer eyJhbGciOiJSUzI1NiI..."  \
 -H "Content-Type: application/json" -d "{\"name\":\"How to test a REST api from command line with curl – CodepediaOrg\",\"location\":\"https://www.codepedia.org/ama/how-to-test-a-rest-api-from-command-line-with-curl/\",\"tags\":[\"rest\",\"curl\",\"api\",\"testing\"],\"publishedOn\":\"2020-03-05T00:00:00.000Z\",\"sourceCodeURL\":\"https://github.com/CodeverDotDev/codever\",\"description\":\"In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP requests against a REST API. For the purpose of this blog post I will be using the REST api that supports [www.codever.dev](https://www.codever.dev)\",\"public\":true,\"readLater\":false,\"language\":\"en\",\"youtubeVideoId\":null,\"stackoverflowQuestionId\":null,\"descriptionHtml\":\"<p>In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP requests against a REST API. For the purpose of this blog post I will be using the REST api that supports <a href=\\\"https://www.codever.dev\\\">www.codever.dev</a></p>\",\"updatedAt\":\"2020-03-06T20:42:53.706Z\",\"lastAccessedAt\":\"2020-03-06T20:42:53.706Z\",\"userId\":\"4c617f2b-2bad-498b-a9c6-4e9a8c303798\",\"_id\":\"5e62b18b59770b5487a4c741\"}" | jq .
```

**Response**
```shell
{
  "tags": [
    "rest",
    "curl",
    "api",
    "testing"
  ],
  "_id": "5e62b18b59770b5487a4c741",
  "name": "How to test a REST api from command line with curl – CodepediaOrg",
  "location": "https://www.codepedia.org/ama/how-to-test-a-rest-api-from-command-line-with-curl/",
  "language": "en",
  "description": "In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP requests against a REST API. For the purpose of this blog post I will be using the REST api that supports [www.codever.dev](https://www.codever.dev)",
  "descriptionHtml": "<p>In this post I will present how to execute GET, POST, PUT, HEAD, DELETE HTTP requests against a REST API. For the purpose of this blog post I will be using the REST api that supports <a href=\"https://www.codever.dev\">www.codever.dev</a></p>",
  "publishedOn": "2020-03-05T00:00:00.000Z",
  "sourceCodeURL": "https://github.com/CodeverDotDev/codever",
  "userId": "4c617f2b-2bad-498b-a9c6-4e9a8c303798",
  "public": true,
  "likeCount": 0,
  "youtubeVideoId": null,
  "stackoverflowQuestionId": null,
  "createdAt": "2020-03-06T20:24:43.998Z",
  "updatedAt": "2020-03-06T20:43:53.582Z"
}
```

### Delete created resource - DELETE

**Request**
```shell
curl -i -X DELETE "http://localhost:3000/api/personal/users/4c617f2b-2bad-498b-a9c6-4e9a8c303798/bookmarks/5e62b18b59770b5487a4c741"
 -H "accept: */*" -H "Authorization: Bearer eyJhbGciOiJS...."
```

**Response**
```shell
HTTP/1.1 204 No Content
X-Powered-By: Express
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization, Location
Access-Control-Expose-Headers: Content-Type, Authorization, Location
Date: Fri, 06 Mar 2020 20:53:37 GMT
Connection: keep-alive
}
```

Note the 204 OK Status showing that everything worked as expected.

Trying to execute the deletion again will result in an 404 resource NOT_FOUND status:

**Response**
```shell
HTTP/1.1 404 Not Found
X-Powered-By: Express
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, PUT, PATCH, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization, Location
Access-Control-Expose-Headers: Content-Type, Authorization, Location
Content-Type: application/json; charset=utf-8
Content-Length: 385
ETag: W/"181-i91X0uxhZ3uP0puyszvupTavBQA"
Date: Fri, 06 Mar 2020 20:55:27 GMT
Connection: keep-alive

{"httpStatus":404,"message":"Bookmark NOT_FOUND with id: 5e62b82a9206df5c2f274c3c","stack":"NotFoundError: Bookmark NOT_FOUND with id: 5e62b82a9206df5c2f274c3c\n    at Object.deleteBookmarkById (/Users/ama/projects/dev/personal/bookmarks/bookmarks-api/src/routes/users/bookmarks/personal-bookmarks.service.js:164:11)\n    at process._tickCallback (internal/process/next_tick.js:68:7)"}
```

> The stacktrace is shown because we are in dev modus


## Conclusion

I am just scratching the surface in this blog post. Check out the [curl docs](https://curl.haxx.se/docs/) for further
capabilities.
