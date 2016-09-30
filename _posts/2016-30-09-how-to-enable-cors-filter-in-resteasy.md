---
layout: post
title: How to enable the CORS Filter in RESTEasy
description: "In this post I will shortly present how to enable the provided RESTEasy CORS Filter in a REST API backend application"
author: ama
permalink: /ama/how-to-enable-cors-filter-in-resteasy
published: false
categories: [java ee]
tags: [jboss, wildfly, rest, resteasy, java ee, cors]
---

In this post I will shortly present how to enable the provided RESTEasy CORS Filter in a REST API backend application. Well it is officially documented, but for me was not that
straightforward and I want to share how I accomplished this. If you want to learn more about CORS I suggest you read the document from Mozilla Developer Network - [HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

<!--more-->
Starting with version 3.0.7.Final the RESTEasy framework has a `ContainerRequestFilter that can be used to handle CORS preflight and actual requests - `org.jboss.resteasy.plugins.interceptors.CorsFilter`.
The documentation is pretty simple. It says that "ou must allocate this and register it as a singleton provider from your Application class. See the javadoc or its various settings." [^1]
Here is the code snippet provided:
```java
CorsFilter filter = new CorsFilter();
filter.getAllowedOrigins().add("http://localhost");
```

Well, to make it work for me I had to register it to the `classes` component of the JAX-RS Application bootstrap class:


```java
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;
import java.util.HashSet;
import java.util.Set;

@ApplicationPath("")
public class TestApplication extends Application {

    private Set<Object> singletons = new HashSet<Object>();
    private HashSet<Class<?>> classes = new HashSet<Class<?>>();

    public TestApplication()
    {
        CorsFilter corsFilter = new CorsFilter();
        corsFilter.getAllowedOrigins().add("*");
        corsFilter.setAllowedMethods("OPTIONS, GET, POST, DELETE, PUT, PATCH");
        singletons.add(corsFilter);

        classes.add(ApiResource.class);
        classes.add(UsersResource.class);
    }

    @Override
    public Set<Object> getSingletons() {
        return singletons;
    }

    @Override
    public HashSet<Class<?>> getClasses(){
      return classes;
    }
}
```

In the example above I allow the HTTP Methods `OPTIONS, GET, POST, DELETE, PUT, PATCH` and  calls from all origins, specified by __*__ - `corsFilter.getAllowedOrigins().add("*");`
If you want to implement a CORS filter yourself and not make you dependent of the RESTEasy framework you can just inspire yourself from the [RESTEasy implementation](https://github.com/resteasy/Resteasy/blob/4e5acbb3f61263a300f0316d952233a404f9b702/resteasy-jaxrs/src/main/java/org/jboss/resteasy/plugins/interceptors/CorsFilter.java), which, as said, handles CORS[^12] requests both preflight and simple CORS requests.

> About this and a migration to JAX-RS 2.0 I have written in my article

> Mention the other CORS article from codingpedia 

## Other good REST(easy) related resources

* [RESTEasy Documentation examples, HTML, PDF, Javadocs](http://resteasy.jboss.org/docs.html) - for all versions...
* [Java EE 7 and JAX-RS 2.0](http://www.oracle.com/technetwork/articles/java/jaxrs20-1929352.html) by Adam Bien
* [What's new in JAX-RS 2.0](https://www.infoq.com/news/2013/06/Whats-New-in-JAX-RS-2.0) at InfoQ
* [Java EE 7 Deployment Descriptors](https://antoniogoncalves.org/2013/06/04/java-ee-7-deployment-descriptors/)

## References
