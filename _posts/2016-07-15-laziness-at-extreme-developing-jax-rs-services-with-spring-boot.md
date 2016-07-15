---
layout: post
title: Laziness at extreme: developing JAX-RS services with Spring Boot
description: "In this post we are going to take a look at how we can use Spring Boot in conjunction with Apache CXF project for a rapid REST(ful) web services development."
author: aredko
permalink: /aredko/laziness-at-extreme-developing-jax-rs-services-with-spring-boot
modified: 2016-07-15
tags: [spring boot, jax-rs, rest, apache cxf]
categories: [java]
---
I think it would be fair to state that we, as software developers, are always looking for the ways to write less code
which does more stuff, automagically or not. With this regards, [Spring Boot](http://projects.spring.io/spring-boot/) project, proud member of the Spring portfolio, disrupted the traditional approaches, dramatically speeding up and simplifying Spring-based applications development.

There is a lot to be said about Spring Boot, intrinsic details of how it works and its seamless integration with most if not all Spring projects. But its capabilities go far beyond that, supporting first-class integration with popular Java frameworks.

In this post we are going to take a look at how we can use Spring Boot in conjunction with [Apache CXF](https://cxf.apache.org/)
project for a rapid [REST(ful) web services](https://en.wikipedia.org/wiki/Representational_state_transfer) development. As we are going to see very soon, Spring Boot takes care of quite a lot of boilerplate, letting us to concentrate on the parts of the application which do have real value. Hopefully, at the end of this post the benefits of adopting Spring Boot for your projects become apparent.

With that, let us get started by developing a simple people management REST(ful) web service, wrapped up into familiar **PeopleRestService** [JAX-RS](https://jax-rs-spec.java.net/) resource:
```java
@Path("/people")
@Component
public class PeopleRestService {
    @GET
    @Produces({MediaType.APPLICATION_JSON})
    public Collection<Person> getPeople() {
        return Collections.singletonList(new Person("a@b.com", "John", "Smith"));
    }
}
```
Not much to add here, pretty simple implementation which returns the hard-coded collection of people.
There are a couple of ways we can package and deploy this JAX-RS service,
but arguably the simplest one is by hosting it inside embedded servlet container
like [Tomcat](http://tomcat.apache.org/), [Jetty](http://www.eclipse.org/jetty/) or [Undertow](http://undertow.io/). With that comes the routine: container initialization, configuring Spring context locations, registering listeners, ... Let us see how Spring Boot can help here by dissecting the Spring context configuration below.
