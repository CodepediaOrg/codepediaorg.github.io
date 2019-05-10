---
layout: post
title: New codingmarks published in week 50 of 2018
description: "New codingmarks published in week 50 of 2018. Keywords: aop, azure-active-directory, book, curl, expressjs, free-programming-books, functional-programming, github-pages, http, immutable, jakartaee, java, javaee, javascript, jvm, jwt, mongodb, mongoose, nodejs, npm, oauth2, openid-connect, programming, reactive, refactoring, rest, rfc, rh-sso, scala and troubleshooting"
author: ama
permalink: /ama/new-codingmarks-week-50-2018
published: true
categories: [codingmarks]
tags: [codingmarks]
---
New [codingmarks](https://www.codingmarks.org) added in week 50 of 2018. Hot topics include:

* TOC
{:toc} 

<!--more-->

## aop 

**[AOP for JS](https://github.com/cujojs/meld)**

  * **tags**: &nbsp; [aop](https://www.codingmarks.org/search?q=[aop]), &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/cujojs/meld)

AOP for JS with before, around, on, afterReturning, afterThrowing, after advice, and pointcuts - cujojs/meld

<hr>


## azure-active-directory 

**[Differences Between Azure Active Directory and Red Hat SSO v7.1](https://medium.com/@robert.broeckelmann/differences-between-azure-active-directory-and-red-hat-sso-v7-1-239dd77a5e9a)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-30
  * **tags**: &nbsp; [rh-sso](https://www.codingmarks.org/search?q=[rh-sso]), &nbsp; [azure-active-directory](https://www.codingmarks.org/search?q=[azure-active-directory])

I recently finished implementing OAuth2 and OIDC support for Azure Active Directory in my OAuth2 + OIDC Debugger. Previously, we implemented support for Red Hat SSO v7.1 and 3Scale. This post compares the two product’s implementations of these protocols (OAuth2 and OIDC).

<hr>


## book 

**[Programming, Motherfucker - Do you speak it?](http://programming-motherfucker.com/)**

  * **tags**: &nbsp; [programming](https://www.codingmarks.org/search?q=[programming]), &nbsp; [book](https://www.codingmarks.org/search?q=[book]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

We are a community of motherfucking programmers who have been humiliated by software development methodologies for years.

We are tired of XP, Scrum, Kanban, Waterfall, Software Craftsmanship (aka XP-Lite) and anything else getting in the way of...Programming, Motherfucker.

<hr>


## curl 

**[How do I make curl ignore the proxy? - Stack Overflow](https://stackoverflow.com/questions/800805/how-do-i-make-curl-ignore-the-proxy)**

  * **tags**: &nbsp; [curl](https://www.codingmarks.org/search?q=[curl])

If your curl is at least version 7.19.4, you could just use the --noproxy flag.
```
$ curl --noproxy "*" http://www.stackoverflow.com
```

<hr>


## expressjs 

**[Express routing](https://expressjs.com/en/guide/routing.html)**

  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs])

**Routing** refers to how an application’s endpoints (URIs) respond to client requests. 
- app routes, `express.Router` and more 

<hr>

**[Node Http Status Codes](https://github.com/prettymuchbryce/node-http-status)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/prettymuchbryce/node-http-status)

Constants enumerating the HTTP status codes. All status codes defined in RFC1945 (HTTP/1.0, RFC2616 (HTTP/1.1), and RFC2518 (WebDAV) are supported. - prettymuchbryce/node-http-status

<hr>


## free-programming-books 

**[Programming, Motherfucker - Do you speak it?](http://programming-motherfucker.com/)**

  * **tags**: &nbsp; [programming](https://www.codingmarks.org/search?q=[programming]), &nbsp; [book](https://www.codingmarks.org/search?q=[book]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

We are a community of motherfucking programmers who have been humiliated by software development methodologies for years.

We are tired of XP, Scrum, Kanban, Waterfall, Software Craftsmanship (aka XP-Lite) and anything else getting in the way of...Programming, Motherfucker.

<hr>


## functional-programming 

**[Java Functional Programming](http://tutorials.jenkov.com/java-functional-programming/index.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-08-05
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

The term _Java functional programming_ refers to functional programming in Java. Functional programming in Java has not been easy historically, and there were even several aspects of functional programming that were not even really possible in Java. In Java 8 Oracle made an effort to make functional programming easier, and this effort did succeed to some extent. In this Java functional programming tutorial I will go through the basics of functional programming, and what parts of it that are possible in Java. Addressed are pure functions, higher order functions, functional interfaces etc.

<hr>

**[Finally Functional Programming in Java – Hacker Noon](https://hackernoon.com/finally-functional-programming-in-java-ad4d388fb92e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-02-26
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [scala](https://www.codingmarks.org/search?q=[scala]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

In many post we have explored Functional Programming concept on different languages being F# and Scala the focus of the conversation. However, because I have been doing some Java on my workplace, exploring these same concept seems interesting and eyes opening because it has been a long time since last time I seriously used Java.

<hr>

**[What's Functional Programming All About?](http://www.lihaoyi.com/post/WhatsFunctionalProgrammingAllAbout.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-01-25
  * **tags**: &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

This post is my own understanding of what is the "core" of "functional programming", how it differs from "imperative" programming, and what the main benefits of the approach are. As a worked example, we will use a kitchen recipe as a proxy for the more-abstract kind of logic you find in program source code, to try and make concrete what is normally a very abstract topic. That recipe is one of my favorite recipes available online, Michael Chu's Classic Tiramisu.

**The core of Functional Programming is thinking about data-flow rather than control-flow.** Although, by virtue of editing plain text, you are forced to order your code in a linear sequence of statements, those statements are a thin skin over what you really care about: the shape and structure of the data-flow graph within your program.

<hr>


## github-pages 

**[Dependency versions](https://pages.github.com/versions/)**

  * **tags**: &nbsp; [github-pages](https://www.codingmarks.org/search?q=[github-pages])

GitHub Pages uses the following dependencies and versions

<hr>


## http 

**[Hypertext Transfer Protocol -- HTTP/1.1](https://tools.ietf.org/html/rfc2616)**

  * **tags**: &nbsp; [http](https://www.codingmarks.org/search?q=[http]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])
<hr>


## immutable 

**[Refactoring to Immutability - Kevlin Henney - YouTube](https://www.youtube.com/watch?v=APUCMSPiNh4)**

  * **tags**: &nbsp; [refactoring](https://www.codingmarks.org/search?q=[refactoring]), &nbsp; [immutable](https://www.codingmarks.org/search?q=[immutable])

It has been said that immutability changes everything. But what does that mean in practice? What does it mean for existing code that looks more like the muta...

<hr>


## jakartaee 

**[Jakarta EE Software  Homeage](https://jakarta.ee/)**

  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [jakartaee](https://www.codingmarks.org/search?q=[jakartaee])

Jakarta Enterprise Edition (EE) is the future of cloud native Java. Jakarta EE open source software drives cloud native innovation, modernizes enterprise applications and protects investments in Java EE.

<hr>


## java 

**[Java Functional Programming](http://tutorials.jenkov.com/java-functional-programming/index.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-08-05
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

The term _Java functional programming_ refers to functional programming in Java. Functional programming in Java has not been easy historically, and there were even several aspects of functional programming that were not even really possible in Java. In Java 8 Oracle made an effort to make functional programming easier, and this effort did succeed to some extent. In this Java functional programming tutorial I will go through the basics of functional programming, and what parts of it that are possible in Java. Addressed are pure functions, higher order functions, functional interfaces etc.

<hr>

**[Finally Functional Programming in Java – Hacker Noon](https://hackernoon.com/finally-functional-programming-in-java-ad4d388fb92e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-02-26
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [scala](https://www.codingmarks.org/search?q=[scala]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

In many post we have explored Functional Programming concept on different languages being F# and Scala the focus of the conversation. However, because I have been doing some Java on my workplace, exploring these same concept seems interesting and eyes opening because it has been a long time since last time I seriously used Java.

<hr>

**[Understanding, Accepting and Leveraging Optional in Java](https://stackify.com/optional-java/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;1999-09-14
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java])

Optional is a simple yet very significant addition in Java 8. See examples of how it can be useful both on its own, but also in functional code where it really shines.

<hr>


## javaee 

**[Jakarta EE Software  Homeage](https://jakarta.ee/)**

  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [jakartaee](https://www.codingmarks.org/search?q=[jakartaee])

Jakarta Enterprise Edition (EE) is the future of cloud native Java. Jakarta EE open source software drives cloud native innovation, modernizes enterprise applications and protects investments in Java EE.

<hr>


## javascript 

**[AOP for JS](https://github.com/cujojs/meld)**

  * **tags**: &nbsp; [aop](https://www.codingmarks.org/search?q=[aop]), &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/cujojs/meld)

AOP for JS with before, around, on, afterReturning, afterThrowing, after advice, and pointcuts - cujojs/meld

<hr>

**[Node Http Status Codes](https://github.com/prettymuchbryce/node-http-status)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/prettymuchbryce/node-http-status)

Constants enumerating the HTTP status codes. All status codes defined in RFC1945 (HTTP/1.0, RFC2616 (HTTP/1.1), and RFC2518 (WebDAV) are supported. - prettymuchbryce/node-http-status

<hr>

**[How to use module.exports in Node.js](https://stackabuse.com/how-to-use-module-exports-in-node-js/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-08-14
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])

Explains with examples how to use nodejs modules and in the end a comparison with JavaScript ES6 modules system

<hr>


## jvm 

**[Eclipse Vert.x homepage](https://vertx.io/)**

  * **tags**: &nbsp; [jvm](https://www.codingmarks.org/search?q=[jvm]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eclipse-vertx/vert.x/)

Eclipse Vert.x is a tool-kit for building reactive applications on the JVM.

<hr>


## jwt 

**[Understanding ID Token](https://medium.com/@darutk/understanding-id-token-5f83f50fa02e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-06
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [jwt](https://www.codingmarks.org/search?q=[jwt]), &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

From an engineer's point of view, an abstract explanation like “ID Token is a token issued as a result of user authentication” is not so valuable because engineers cannot imagine how to implement ID Token at all by the explanation. Instead, what engineers want to know first, at least what I should have known before diving into reading OpenID Connect Core 1.0, is what an ID token looks like.

<hr>


## mongodb 

**[mongoose-unique-validator](https://github.com/blakehaswell/mongoose-unique-validator)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/blakehaswell/mongoose-unique-validator)

mongoose-unique-validator is a plugin which adds pre-save validation for unique fields within a Mongoose schema.

This makes error handling much easier, since you will get a Mongoose validation error when you attempt to violate a unique constraint, rather than an E11000 error from MongoDB.

<hr>


## mongoose 

**[mongoose-unique-validator](https://github.com/blakehaswell/mongoose-unique-validator)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/blakehaswell/mongoose-unique-validator)

mongoose-unique-validator is a plugin which adds pre-save validation for unique fields within a Mongoose schema.

This makes error handling much easier, since you will get a Mongoose validation error when you attempt to violate a unique constraint, rather than an E11000 error from MongoDB.

<hr>


## nodejs 

**[AOP for JS](https://github.com/cujojs/meld)**

  * **tags**: &nbsp; [aop](https://www.codingmarks.org/search?q=[aop]), &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/cujojs/meld)

AOP for JS with before, around, on, afterReturning, afterThrowing, after advice, and pointcuts - cujojs/meld

<hr>

**[Restify Homepage](http://restify.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/restify/node-restify)

A Node.js web service framework optimized for building semantically correct RESTful web services ready for production use at scale. restify optimizes for introspection and perfromance, and is used in some of the largest Node.js deployments on Earth.

<hr>

**[Node Http Status Codes](https://github.com/prettymuchbryce/node-http-status)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/prettymuchbryce/node-http-status)

Constants enumerating the HTTP status codes. All status codes defined in RFC1945 (HTTP/1.0, RFC2616 (HTTP/1.1), and RFC2518 (WebDAV) are supported. - prettymuchbryce/node-http-status

<hr>

**[How to use module.exports in Node.js](https://stackabuse.com/how-to-use-module-exports-in-node-js/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-08-14
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])

Explains with examples how to use nodejs modules and in the end a comparison with JavaScript ES6 modules system

<hr>

**[mongoose-unique-validator](https://github.com/blakehaswell/mongoose-unique-validator)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/blakehaswell/mongoose-unique-validator)

mongoose-unique-validator is a plugin which adds pre-save validation for unique fields within a Mongoose schema.

This makes error handling much easier, since you will get a Mongoose validation error when you attempt to violate a unique constraint, rather than an E11000 error from MongoDB.

<hr>


## npm 

**[npm-check ](https://www.npmjs.com/package/npm-check)**

  * **tags**: &nbsp; [npm](https://www.codingmarks.org/search?q=[npm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/dylang/npm-check)

Check for outdated, incorrect, and unused dependencies. Update npm dependencies

<hr>

**[Resolving EACCES permissions errors when installing packages globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)**

  * **tags**: &nbsp; [npm](https://www.codingmarks.org/search?q=[npm]), &nbsp; [troubleshooting](https://www.codingmarks.org/search?q=[troubleshooting])

If you see an EACCES error when you try to **install a package globally**, you can either:
* Reinstall npm with a node version manager (recommended),
or
* Manually change npm’s default directory


<hr>


## oauth2 

**[Understanding ID Token](https://medium.com/@darutk/understanding-id-token-5f83f50fa02e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-06
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [jwt](https://www.codingmarks.org/search?q=[jwt]), &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

From an engineer's point of view, an abstract explanation like “ID Token is a token issued as a result of user authentication” is not so valuable because engineers cannot imagine how to implement ID Token at all by the explanation. Instead, what engineers want to know first, at least what I should have known before diving into reading OpenID Connect Core 1.0, is what an ID token looks like.

<hr>

**[Welcome to OpenID Connect – OpenID Homepage](https://openid.net/connect/)**

  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.


<hr>

**[User-Managed Access (UMA) Profile of OAuth
    2.0](https://docs.kantarainitiative.org/uma/rec-uma-core.html)**

  * **tags**: &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

User-Managed Access (UMA) is a profile of OAuth 2.0. UMA defines how resource owners can control protected-resource access by clients operated by arbitrary requesting parties, where the resources reside on any number of resource servers, and where a centralized authorization server governs access based on resource owner policies.

<hr>

**[The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://tools.ietf.org/html/rfc6750)**

  * **tags**: &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])

This specification describes how to use bearer tokens in HTTP requests to access OAuth 2.0 protected resources.  Any party in possession of a bearer token (a "bearer") can use it to get access to the associated resources (without demonstrating possession of a cryptographic key).  To prevent misuse, bearer tokens need to be protected from disclosure in storage and in transport.


<hr>

**[The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)**

  * **tags**: &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])

The OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service, either of a behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.  This specification replaces and obsoletes the OAuth 1.0 protocol described in RFC 5849.

<hr>


## openid-connect 

**[Understanding ID Token](https://medium.com/@darutk/understanding-id-token-5f83f50fa02e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-06
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [jwt](https://www.codingmarks.org/search?q=[jwt]), &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

From an engineer's point of view, an abstract explanation like “ID Token is a token issued as a result of user authentication” is not so valuable because engineers cannot imagine how to implement ID Token at all by the explanation. Instead, what engineers want to know first, at least what I should have known before diving into reading OpenID Connect Core 1.0, is what an ID token looks like.

<hr>

**[Welcome to OpenID Connect – OpenID Homepage](https://openid.net/connect/)**

  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2])

OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.


<hr>


## programming 

**[Programming, Motherfucker - Do you speak it?](http://programming-motherfucker.com/)**

  * **tags**: &nbsp; [programming](https://www.codingmarks.org/search?q=[programming]), &nbsp; [book](https://www.codingmarks.org/search?q=[book]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

We are a community of motherfucking programmers who have been humiliated by software development methodologies for years.

We are tired of XP, Scrum, Kanban, Waterfall, Software Craftsmanship (aka XP-Lite) and anything else getting in the way of...Programming, Motherfucker.

<hr>


## reactive 

**[Eclipse Vert.x homepage](https://vertx.io/)**

  * **tags**: &nbsp; [jvm](https://www.codingmarks.org/search?q=[jvm]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eclipse-vertx/vert.x/)

Eclipse Vert.x is a tool-kit for building reactive applications on the JVM.

<hr>


## refactoring 

**[Refactoring to Immutability - Kevlin Henney - YouTube](https://www.youtube.com/watch?v=APUCMSPiNh4)**

  * **tags**: &nbsp; [refactoring](https://www.codingmarks.org/search?q=[refactoring]), &nbsp; [immutable](https://www.codingmarks.org/search?q=[immutable])

It has been said that immutability changes everything. But what does that mean in practice? What does it mean for existing code that looks more like the muta...

<hr>


## rest 

**[Restify Homepage](http://restify.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/restify/node-restify)

A Node.js web service framework optimized for building semantically correct RESTful web services ready for production use at scale. restify optimizes for introspection and perfromance, and is used in some of the largest Node.js deployments on Earth.

<hr>


## rfc 

**[The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://tools.ietf.org/html/rfc6750)**

  * **tags**: &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])

This specification describes how to use bearer tokens in HTTP requests to access OAuth 2.0 protected resources.  Any party in possession of a bearer token (a "bearer") can use it to get access to the associated resources (without demonstrating possession of a cryptographic key).  To prevent misuse, bearer tokens need to be protected from disclosure in storage and in transport.


<hr>

**[Hypertext Transfer Protocol -- HTTP/1.1](https://tools.ietf.org/html/rfc2616)**

  * **tags**: &nbsp; [http](https://www.codingmarks.org/search?q=[http]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])
<hr>

**[The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)**

  * **tags**: &nbsp; [oauth2](https://www.codingmarks.org/search?q=[oauth2]), &nbsp; [rfc](https://www.codingmarks.org/search?q=[rfc])

The OAuth 2.0 authorization framework enables a third-party application to obtain limited access to an HTTP service, either of a behalf of a resource owner by orchestrating an approval interaction between the resource owner and the HTTP service, or by allowing the third-party application to obtain access on its own behalf.  This specification replaces and obsoletes the OAuth 1.0 protocol described in RFC 5849.

<hr>


## rh-sso 

**[Differences Between Azure Active Directory and Red Hat SSO v7.1](https://medium.com/@robert.broeckelmann/differences-between-azure-active-directory-and-red-hat-sso-v7-1-239dd77a5e9a)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-30
  * **tags**: &nbsp; [rh-sso](https://www.codingmarks.org/search?q=[rh-sso]), &nbsp; [azure-active-directory](https://www.codingmarks.org/search?q=[azure-active-directory])

I recently finished implementing OAuth2 and OIDC support for Azure Active Directory in my OAuth2 + OIDC Debugger. Previously, we implemented support for Red Hat SSO v7.1 and 3Scale. This post compares the two product’s implementations of these protocols (OAuth2 and OIDC).

<hr>


## scala 

**[Finally Functional Programming in Java – Hacker Noon](https://hackernoon.com/finally-functional-programming-in-java-ad4d388fb92e)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-02-26
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [scala](https://www.codingmarks.org/search?q=[scala]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

In many post we have explored Functional Programming concept on different languages being F# and Scala the focus of the conversation. However, because I have been doing some Java on my workplace, exploring these same concept seems interesting and eyes opening because it has been a long time since last time I seriously used Java.

<hr>


## troubleshooting 

**[Resolving EACCES permissions errors when installing packages globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)**

  * **tags**: &nbsp; [npm](https://www.codingmarks.org/search?q=[npm]), &nbsp; [troubleshooting](https://www.codingmarks.org/search?q=[troubleshooting])

If you see an EACCES error when you try to **install a package globally**, you can either:
* Reinstall npm with a node version manager (recommended),
or
* Manually change npm’s default directory


<hr>
