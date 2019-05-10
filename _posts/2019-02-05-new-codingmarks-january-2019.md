---
layout: post
title: Lots of exciting codingmarks added in january 2019
description: "Lots of exciting #codingmarks added in january 2019. Keywords: angular, api, architecture, async-await, asynchronous, awesome, awesome-list, aws, bdd, blog, caching, cdi, chai, clean-code, cloud, concurrency, csv, date, date-manipulation, datetime, dependency-injection, design-patterns, devops, docker, docker-compose, docs, expressjs, flexbox, free-programming-books, generics, html, integration-testing, jackson, java, javaee, javascript, jersey, json, linux, markdown, memory-management, microservices, mocking, mockito, mongodb, mongoose, multithreading, nginx, nodejs, npm, open-source, orm, programming, reactjs, rest, rest-client, resteasy, rxjs, serialization, serverless, software-architecture, software-development, spring, supertest, test-coverage, testing, time, tools and weld"
author: ama
permalink: /ama/new-codingmarks-in-january-2019
published: true
categories: [codingmarks]
tags: [codingmarks]
---
New [#codingmarks](https://www.codingmarks.org) added in january 2018. Hot topics include:

* TOC
{:toc} 

<!--more-->

## angular 

**[ Angular Observable Data Services](https://coryrylan.com/blog/angular-observable-data-services)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-17
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])
  * <i class="fa fa-github fa-lg"></i> [github url](https://stackblitz.com/edit/angular-fh1kyp)

A look into Observables and how they can improve your Angular data services.

<hr>

**[Advanced caching with RxJS by thoughtram](https://blog.thoughtram.io/angular/2018/03/05/advanced-caching-with-rxjs.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-05
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular]), &nbsp; [caching](https://www.codingmarks.org/search?q=[caching]), &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://stackblitz.com/edit/advanced-caching-with-rxjs-step-4)

When building web applications, performance should always be a top priority. One very efficient way to optimize the performance of our applications and improve the experience of our site is to use caching mechanisms. In this post we'll develop an advanced caching mechanism with RxJS and the tools provided by Angular to cache application data.

<hr>

**[A deep dive on Angular decorators](https://toddmotto.com/angular-decorators)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-01-26
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])

Decorators are a core concept when developing with Angular (versions 2 and above). There’s also an official TC39 proposal, currently at Stage-2, so expect decorators to become a core language feature soon in JavaScript as well.

Back to Angular, the internal codebase uses decorators extensively and in this post we’re going to look at the different types of decorators, the code they compile to and how they work.

When I was first introduced to TypeScript and decorators, I wondered why we needed them at all, but once you dig a little deeper you can understand the benefits to creating decorators (not only for use in Angular).

AngularJS didn’t use decorators, opting for a different registration method - such as defining a component for example with the .component() method. So why has Angular chose to use them? Let’s explore.

<hr>

**[[How-to] Implement custom form validators with Angular](https://blog.angulartraining.com/how-to-implement-custom-form-validators-with-angular-861651b0dc2c)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-02-18
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])
  * <i class="fa fa-github fa-lg"></i> [github url](https://stackblitz.com/edit/angular-custom-validator-form)

Form validation is always a hot topic whenever I teach Angular. People usually ask me whether they should use Template driven forms or Reactive forms, and I used to tell them that reactive forms are a better option when you need custom validators.

That was partially wrong though: It is actually fairly easy to write custom validators that work seamlessly with both template-driven and reactive forms, which is what I’m going to show you in this post.

I’m going to create a custom credit card number validator. The validator is going to make sure that a credit card number has 16 digits and that it comes from an accepted credit card company.

<hr>

**[Exploring Angular DOM manipulation techniques using ViewContainerRef](https://blog.angularindepth.com/exploring-angular-dom-abstractions-80b3ebcfc02)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-03-04
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])

Whenever I read about working with DOM in Angular I always see one or few of these classes mentioned: `ElementRef`, `TemplateRef`, `ViewContainerRef` and others. Unfortunately, although some of them are covered in Angular docs or related articles, I’ve yet to found the description of the overall mental model and examples of how these work together. This article aims to describe such model.

<hr>

**[The essential difference between Constructor and ngOnInit in Angular](https://blog.angularindepth.com/the-essential-difference-between-constructor-and-ngoninit-in-angular-c9930c209a42)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-09-07
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])

One of the most popular Angular questions on stackoverflow is Difference between Constructor and ngOnInit with over 100k views. I gave my answer to this question there but also decided to expand on it in this article. While most answers in the thread and articles on the web focus on the difference between the usage of the two here I’d like to give a more comprehensive comparison that taps into components initialization process.

<hr>


## api 

**[MockAPI docs](https://www.mockapi.io/docs)**

  * **tags**: &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking]), &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

MockAPI is a simple tool that lets you easily mock up APIs, generate custom data, and preform operations on it using RESTful interface. MockAPI is meant to be used as a prototyping/testing/learning tool.

<hr>


## architecture 

**[Feature Toggles (aka Feature Flags)](https://martinfowler.com/articles/feature-toggles.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-10-09
  * **tags**: &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Feature Toggles (often also refered to as Feature Flags) are a powerful technique, allowing teams to modify system behavior without changing code. They fall into various usage categories, and it's important to take that categorization into account when implementing and managing toggles. Toggles introduce complexity. We can keep that complexity in check by using smart toggle implementation practices and appropriate tools to manage our toggle configuration, but we should also aim to constrain the number of toggles in our system.

<hr>


## async-await 

**[Cleaner code in NodeJs with async-await - Mongoose calls example – CodingpediaOrg](http://www.codingpedia.org/ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-05
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [async-await](https://www.codingmarks.org/search?q=[async-await]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks-api)

Example showing migration of Mongoose calls from previously using callbacks to using the new async-await feature in NodeJs

<hr>


## asynchronous 

**[java.util.concurrent - Java Concurrency Utilities](http://tutorials.jenkov.com/java-util-concurrent/index.html)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [concurrency](https://www.codingmarks.org/search?q=[concurrency]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

In this tutorial I will take you through the new java.util.concurrent classes, one by one, so you can learn how to use them. I will use the versions in Java 6. I am not sure if there are any differences to the Java 5 versions.

Includes theory about 
* Java Executor Service
* Java Callable
* Java Future
* ThreadPoolExecutor
and much more

<hr>

**[Asynchronous REST Services with JAX-RS and CompletableFuture · allegro.tech](https://allegro.tech/2014/10/async-rest.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-10-29
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

One of new features introduced by JAX-RS 2.0 is asynchronous processing in Server and Client APIs. We use these APIs together with CompletableFutur...

<hr>


## awesome 

**[GitHub - anaibol/awesome-serverless](https://github.com/anaibol/awesome-serverless)**

  * **tags**: &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless]), &nbsp; [cloud](https://www.codingmarks.org/search?q=[cloud]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/anaibol/awesome-serverless)

:cloud: A curated list of awesome services, solutions and resources for serverless / nobackend applications. - anaibol/awesome-serverless

<hr>


## awesome-list 

**[GitHub - anaibol/awesome-serverless](https://github.com/anaibol/awesome-serverless)**

  * **tags**: &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless]), &nbsp; [cloud](https://www.codingmarks.org/search?q=[cloud]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/anaibol/awesome-serverless)

:cloud: A curated list of awesome services, solutions and resources for serverless / nobackend applications. - anaibol/awesome-serverless

<hr>


## aws 

**[Claudia.js Home Page](https://claudiajs.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws]), &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/claudiajs/claudia)

Deploy Node.js projects to AWS Lambda and API Gateway easily.

<hr>


## bdd 

**[Should.js API Documentation](http://shouldjs.github.io/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [bdd](https://www.codingmarks.org/search?q=[bdd])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/shouldjs/should.js)

_should_ is an expressive, readable, framework-agnostic assertion library. The main goals of this library are to be expressive and to be helpful. It keeps your test code clean, and your error messages helpful.

<hr>


## blog 

**[Ted Vinke's Blog – Java, Groovy and Grails](https://tedvinke.wordpress.com/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [blog](https://www.codingmarks.org/search?q=[blog])

Java, Groovy and Grails

<hr>


## caching 

**[Advanced caching with RxJS by thoughtram](https://blog.thoughtram.io/angular/2018/03/05/advanced-caching-with-rxjs.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-05
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular]), &nbsp; [caching](https://www.codingmarks.org/search?q=[caching]), &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://stackblitz.com/edit/advanced-caching-with-rxjs-step-4)

When building web applications, performance should always be a top priority. One very efficient way to optimize the performance of our applications and improve the experience of our site is to use caching mechanisms. In this post we'll develop an advanced caching mechanism with RxJS and the tools provided by Angular to cache application data.

<hr>


## cdi 

**[Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-i/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2011-09-25
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [cdi](https://www.codingmarks.org/search?q=[cdi]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/agoncal/agoncal-sample-cdi)

There are three parts:
* [Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - Focused on default injection and qualifiers
* [Injection with CDI (Part II)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - This is the second post based on pure CDI Injection (see Part I) after having talked about how to bootstrap CDI in several environments and how to add CDI to an existing Java EE 6 application. In this post I quickly want to show the different injection points in CDI : field, constructor and setter.
* [Injection with CDI (Part III)](https://antoniogoncalves.org/2011/09/25/injection-with-cdi-part-iii/) - In this post I’ll explain producers or “how you can inject anything anywhere in a type safe manner“.

<hr>


## chai 

**[Express Integration Testing with SuperTest · InVision Engineering Blog](https://engineering.invisionapp.com/post/express-integration-testing-supertest/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-06-27
  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing]), &nbsp; [supertest](https://www.codingmarks.org/search?q=[supertest]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshmatz/supertest-example)

Put down that REST Client (or, gasp, the browser!) you’re using to test that API you’re developing and back away slowly! There’s a better way! With SuperTest there’s no need to verify your API by hand. Plus, using it gives you virtually free integration tests. Now you can test and code new features at the same time.

<hr>


## clean-code 

**[Why I Changed My Mind About Field Injection?](https://www.petrikainulainen.net/software-development/design/why-i-changed-my-mind-about-field-injection/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-06-26
  * **tags**: &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns]), &nbsp; [clean-code](https://www.codingmarks.org/search?q=[clean-code])

I used to be a huge fan of field injection.
But one day I started to question myself. Could it be possible that I have been mistaken?
Let’s find out what happened.

<hr>


## cloud 

**[GitHub - anaibol/awesome-serverless](https://github.com/anaibol/awesome-serverless)**

  * **tags**: &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless]), &nbsp; [cloud](https://www.codingmarks.org/search?q=[cloud]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/anaibol/awesome-serverless)

:cloud: A curated list of awesome services, solutions and resources for serverless / nobackend applications. - anaibol/awesome-serverless

<hr>


## concurrency 

**[java.util.concurrent - Java Concurrency Utilities](http://tutorials.jenkov.com/java-util-concurrent/index.html)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [concurrency](https://www.codingmarks.org/search?q=[concurrency]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

In this tutorial I will take you through the new java.util.concurrent classes, one by one, so you can learn how to use them. I will use the versions in Java 6. I am not sure if there are any differences to the Java 5 versions.

Includes theory about 
* Java Executor Service
* Java Callable
* Java Future
* ThreadPoolExecutor
and much more

<hr>


## csv 

**[GitHub - Keyang/node-csvtojson](https://github.com/Keyang/node-csvtojson)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [csv](https://www.codingmarks.org/search?q=[csv]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Keyang/node-csvtojson)

Blazing fast and Comprehensive CSV Parser for Node.JS / Browser / Command Line.  - Keyang/node-csvtojson

<hr>


## date 

**[Moment.js Home Page](http://momentjs.com/)**

  * **tags**: &nbsp; [date](https://www.codingmarks.org/search?q=[date]), &nbsp; [datetime](https://www.codingmarks.org/search?q=[datetime]), &nbsp; [date-manipulation](https://www.codingmarks.org/search?q=[date-manipulation]), &nbsp; [time](https://www.codingmarks.org/search?q=[time]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/moment/moment/)

A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.

**[Documentation](http://momentjs.com/docs/)**

<hr>


## date-manipulation 

**[Moment.js Home Page](http://momentjs.com/)**

  * **tags**: &nbsp; [date](https://www.codingmarks.org/search?q=[date]), &nbsp; [datetime](https://www.codingmarks.org/search?q=[datetime]), &nbsp; [date-manipulation](https://www.codingmarks.org/search?q=[date-manipulation]), &nbsp; [time](https://www.codingmarks.org/search?q=[time]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/moment/moment/)

A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.

**[Documentation](http://momentjs.com/docs/)**

<hr>


## datetime 

**[Moment.js Home Page](http://momentjs.com/)**

  * **tags**: &nbsp; [date](https://www.codingmarks.org/search?q=[date]), &nbsp; [datetime](https://www.codingmarks.org/search?q=[datetime]), &nbsp; [date-manipulation](https://www.codingmarks.org/search?q=[date-manipulation]), &nbsp; [time](https://www.codingmarks.org/search?q=[time]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/moment/moment/)

A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.

**[Documentation](http://momentjs.com/docs/)**

<hr>


## dependency-injection 

**[ CDI Reference Implementation](http://docs.jboss.org/weld/reference/latest/en-US/html_single/)**

  * **tags**: &nbsp; [weld](https://www.codingmarks.org/search?q=[weld]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

CDI: Contexts and Dependency Injection for the Java EE platform

<hr>

**[Oliver Gierke - Why field injection is evil](http://olivergierke.de/2013/11/why-field-injection-is-evil/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-11-22
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring])

I’m quite frequently getting pulled into discussions on Twitter about the different flavors of Dependency Injection. Also, I’ve repeatedly expressed my distaste for field injection but as Twitter is not the right communication channel to give an in-depth rational about my opinion. So here we go.

The comments section is also interesting. 

<hr>

**[Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-i/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2011-09-25
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [cdi](https://www.codingmarks.org/search?q=[cdi]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/agoncal/agoncal-sample-cdi)

There are three parts:
* [Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - Focused on default injection and qualifiers
* [Injection with CDI (Part II)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - This is the second post based on pure CDI Injection (see Part I) after having talked about how to bootstrap CDI in several environments and how to add CDI to an existing Java EE 6 application. In this post I quickly want to show the different injection points in CDI : field, constructor and setter.
* [Injection with CDI (Part III)](https://antoniogoncalves.org/2011/09/25/injection-with-cdi-part-iii/) - In this post I’ll explain producers or “how you can inject anything anywhere in a type safe manner“.

<hr>

**[The One Correct Way to do Dependency Injection](http://blog.schauderhaft.de/2012/01/01/the-one-correct-way-to-do-dependency-injection/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2012-01-01
  * **tags**: &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection])

A couple of weeks ago a coworker told me that they have a little utility in their projects in order to set private fields for tests. He kind of claimed they needed that since they are using Spring, which in production sets the dependency. I was a little confused. But after looking into it I realized that there is an (anti)pattern going on that needs some fighting against. Lets have a look at the pattern.

Spring (and other DI frameworks) can inject dependencies into private fields. It looks like people like to use this because it combines some compelling properties:

* Very little boilerplate code. 
* All you need is a simple annotation on the fi

<hr>

**[
  Constructor Injection vs. Setter Injection](http://misko.hevery.com/2009/02/19/constructor-injection-vs-setter-injection/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2009-02-19
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection])

There seems to be two camps in dependency-injection: (1) The constructor-injection camp and (2) the setter-injection camp. Historically the setter-injection camp come from spring, whereas constructor-injection camp are from pico-container and GUICE. But lets leave the history behind and explore the differences in the strategies.

<hr>


## design-patterns 

**[The One Correct Way to do Dependency Injection](http://blog.schauderhaft.de/2012/01/01/the-one-correct-way-to-do-dependency-injection/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2012-01-01
  * **tags**: &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection])

A couple of weeks ago a coworker told me that they have a little utility in their projects in order to set private fields for tests. He kind of claimed they needed that since they are using Spring, which in production sets the dependency. I was a little confused. But after looking into it I realized that there is an (anti)pattern going on that needs some fighting against. Lets have a look at the pattern.

Spring (and other DI frameworks) can inject dependencies into private fields. It looks like people like to use this because it combines some compelling properties:

* Very little boilerplate code. 
* All you need is a simple annotation on the fi

<hr>

**[Why I Changed My Mind About Field Injection?](https://www.petrikainulainen.net/software-development/design/why-i-changed-my-mind-about-field-injection/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-06-26
  * **tags**: &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns]), &nbsp; [clean-code](https://www.codingmarks.org/search?q=[clean-code])

I used to be a huge fan of field injection.
But one day I started to question myself. Could it be possible that I have been mistaken?
Let’s find out what happened.

<hr>


## devops 

**[dotenv homepage](https://github.com/motdotla/dotenv)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/motdotla/dotenv)

Loads environment variables from .env for nodejs projects.

<hr>


## docker 

**[Building Microservices with Node, Docker and Nginx pt 1 - What is a Microservice?](https://www.youtube.com/watch?v=EsCfPxjmnjo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-29
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fChristenson/microservices-example)

`Building` Microservices with Node, Docker and Nginx  
* Part 1 What is a Microservice?" I walk you through what a Microservice is and what this 3 part tutorial will show you.
* Part 2 [Making a microservice](https://www.youtube.com/watch?v=aWxR05rUoto) - shows you how to make a Microservices and some of the thoughts I have about them.
* Part 3 [Connecting the microservices](https://www.youtube.com/watch?v=QjhJs31h_4k) - shows you how to use Docker compose and Nginx to connect your fleet of Microservices




<hr>


## docker-compose 

**[Building Microservices with Node, Docker and Nginx pt 1 - What is a Microservice?](https://www.youtube.com/watch?v=EsCfPxjmnjo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-29
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fChristenson/microservices-example)

`Building` Microservices with Node, Docker and Nginx  
* Part 1 What is a Microservice?" I walk you through what a Microservice is and what this 3 part tutorial will show you.
* Part 2 [Making a microservice](https://www.youtube.com/watch?v=aWxR05rUoto) - shows you how to make a Microservices and some of the thoughts I have about them.
* Part 3 [Connecting the microservices](https://www.youtube.com/watch?v=QjhJs31h_4k) - shows you how to use Docker compose and Nginx to connect your fleet of Microservices




<hr>


## docs 

**[ CDI Reference Implementation](http://docs.jboss.org/weld/reference/latest/en-US/html_single/)**

  * **tags**: &nbsp; [weld](https://www.codingmarks.org/search?q=[weld]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

CDI: Contexts and Dependency Injection for the Java EE platform

<hr>

**[npm documentation  ](https://docs.npmjs.com/)**

  * **tags**: &nbsp; [npm](https://www.codingmarks.org/search?q=[npm]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

* About npm
* Getting started
* Packages and modules
* Integrations
* Orgs
* npm Enterprise
* CLI documentation

<hr>


## expressjs 

**[Express Integration Testing with SuperTest · InVision Engineering Blog](https://engineering.invisionapp.com/post/express-integration-testing-supertest/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-06-27
  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing]), &nbsp; [supertest](https://www.codingmarks.org/search?q=[supertest]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshmatz/supertest-example)

Put down that REST Client (or, gasp, the browser!) you’re using to test that API you’re developing and back away slowly! There’s a better way! With SuperTest there’s no need to verify your API by hand. Plus, using it gives you virtually free integration tests. Now you can test and code new features at the same time.

<hr>

**[express-validator](https://express-validator.github.io)**

  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/express-validator/express-validator)

An express.js middleware for validator.js. 

<hr>


## flexbox 

**[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)**

  * **tags**: &nbsp; [flexbox](https://www.codingmarks.org/search?q=[flexbox])

Our comprehensive guide to CSS flexbox layout. This complete guide explains everything about flexbox, focusing on all the different possible properties for the parent element (the flex container) and the child elements (the flex items). It also includes history, demos, patterns, and a browser support chart.

<hr>


## free-programming-books 

**[Coding bookmarks, aka #codingmarks](https://www.codingmarks.org)**

  * **tags**: &nbsp; [software-development](https://www.codingmarks.org/search?q=[software-development]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books]), &nbsp; [programming](https://www.codingmarks.org/search?q=[programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks)

Efficiently manage and retrieve your coding bookmarks. Contribute by sharing and voting the worthy ones, so others can benefit

<hr>


## generics 

**[The Basics of Java Generics](https://www.baeldung.com/java-generics)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [generics](https://www.codingmarks.org/search?q=[generics])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/core-java-lang-syntax)

A quick intro tot he basics of Java Generics.

<hr>


## html 

**[Using CORS - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/cors/)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html])

An introduction to Cross Origin Resource Sharing (CORS), which gives scripts the ability to make cross-origin XHRs.

<hr>


## integration-testing 

**[Express Integration Testing with SuperTest · InVision Engineering Blog](https://engineering.invisionapp.com/post/express-integration-testing-supertest/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-06-27
  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing]), &nbsp; [supertest](https://www.codingmarks.org/search?q=[supertest]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshmatz/supertest-example)

Put down that REST Client (or, gasp, the browser!) you’re using to test that API you’re developing and back away slowly! There’s a better way! With SuperTest there’s no need to verify your API by hand. Plus, using it gives you virtually free integration tests. Now you can test and code new features at the same time.

<hr>


## jackson 

**[How To Serialize Enums as JSON Objects with Jackson](https://www.baeldung.com/jackson-serialize-enums)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-23
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [serialization](https://www.codingmarks.org/search?q=[serialization]), &nbsp; [jackson](https://www.codingmarks.org/search?q=[jackson]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/jackson#readme)

How to serialize an Enum as a JSON Object using Jackson 2.

<hr>


## java 

**[ORM Is an Offensive Anti-Pattern ](https://www.yegor256.com/2014/12/01/orm-offensive-anti-pattern.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-12-01
  * **tags**: &nbsp; [orm](https://www.codingmarks.org/search?q=[orm]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])

TL;DR ORM is a terrible anti-pattern that violates all principles of object-oriented programming, tearing objects apart and turning them into dumb and passive data bags. There is no excuse for ORM existence in any application, be it a small web app or an enterprise-size system with thousands of tables and CRUD manipulations on them. What is the alternative? **SQL-speaking objects**.

<hr>

**[Ted Vinke's Blog – Java, Groovy and Grails](https://tedvinke.wordpress.com/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [blog](https://www.codingmarks.org/search?q=[blog])

Java, Groovy and Grails

<hr>

**[java.util.concurrent - Java Concurrency Utilities](http://tutorials.jenkov.com/java-util-concurrent/index.html)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [concurrency](https://www.codingmarks.org/search?q=[concurrency]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

In this tutorial I will take you through the new java.util.concurrent classes, one by one, so you can learn how to use them. I will use the versions in Java 6. I am not sure if there are any differences to the Java 5 versions.

Includes theory about 
* Java Executor Service
* Java Callable
* Java Future
* ThreadPoolExecutor
and much more

<hr>

**[Java Lambda Expressions](http://tutorials.jenkov.com/java/lambda-expressions.html)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java])

Java lambda expressions are new in Java 8. Java lambda expressions are Java's first step into functional programming. A Java lambda expression is thus a function which can be created without belonging to any class. A Java lambda expression can be passed around as if it was an object and executed on demand.

Java lambda expressions are commonly used to implement simple event listeners / callbacks, or in functional [programming with the Java Streams API](http://tutorials.jenkov.com/java-collections/streams.html).

If you prefer video, I have a video version of this tutorial in this [Java Lambda Expression YouTube Playlist](Java Lambda Expression YouTube Playlist). Here is the first video in this playlist:

<hr>

**[The Basics of Java Generics](https://www.baeldung.com/java-generics)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [generics](https://www.codingmarks.org/search?q=[generics])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/core-java-lang-syntax)

A quick intro tot he basics of Java Generics.

<hr>

**[Finally Getting the Most out of the Java Thread Pool](https://stackify.com/java-thread-pools/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-09-06
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [multithreading](https://www.codingmarks.org/search?q=[multithreading])

Finally understanding how thread pools really work in Java can be the difference between your application being a slog, or a clean and efficient system.

<hr>

**[How To Serialize Enums as JSON Objects with Jackson](https://www.baeldung.com/jackson-serialize-enums)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-23
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [serialization](https://www.codingmarks.org/search?q=[serialization]), &nbsp; [jackson](https://www.codingmarks.org/search?q=[jackson]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/jackson#readme)

How to serialize an Enum as a JSON Object using Jackson 2.

<hr>

**[Asynchronous REST Services with JAX-RS and CompletableFuture · allegro.tech](https://allegro.tech/2014/10/async-rest.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-10-29
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

One of new features introduced by JAX-RS 2.0 is asynchronous processing in Server and Client APIs. We use these APIs together with CompletableFutur...

<hr>


## javaee 

**[Thorntail Home Page](http://thorntail.io/)**

  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/thorntail/thorntail)

Thorntail provides a mechanism for building applications as uber jars, with just enough of the WildFly application server wrapped around it to support each application's use-case.

<hr>

**[ CDI Reference Implementation](http://docs.jboss.org/weld/reference/latest/en-US/html_single/)**

  * **tags**: &nbsp; [weld](https://www.codingmarks.org/search?q=[weld]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

CDI: Contexts and Dependency Injection for the Java EE platform

<hr>

**[Mockito: Why You Should Not Use InjectMocks Annotation to Autowire Fields – Ted Vinke's Blog](https://tedvinke.wordpress.com/2014/02/13/mockito-why-you-should-not-use-injectmocks-annotation-to-autowire-fields/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-02-13
  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [mockito](https://www.codingmarks.org/search?q=[mockito]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking])

People like the way how Mockito is able to mock Spring's auto-wired fields with the @InjectMocks annotation. When I read this post of Lubos Krnac last week, I thought I should explain why I think the use of InjectMocks is a bad signal and how you should avoid it. Hint: it's about visibility.

<hr>

**[Oliver Gierke - Why field injection is evil](http://olivergierke.de/2013/11/why-field-injection-is-evil/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-11-22
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring])

I’m quite frequently getting pulled into discussions on Twitter about the different flavors of Dependency Injection. Also, I’ve repeatedly expressed my distaste for field injection but as Twitter is not the right communication channel to give an in-depth rational about my opinion. So here we go.

The comments section is also interesting. 

<hr>

**[Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-i/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2011-09-25
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [cdi](https://www.codingmarks.org/search?q=[cdi]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/agoncal/agoncal-sample-cdi)

There are three parts:
* [Injection with CDI (Part I)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - Focused on default injection and qualifiers
* [Injection with CDI (Part II)](https://antoniogoncalves.org/2011/05/03/injection-with-cdi-part-ii/) - This is the second post based on pure CDI Injection (see Part I) after having talked about how to bootstrap CDI in several environments and how to add CDI to an existing Java EE 6 application. In this post I quickly want to show the different injection points in CDI : field, constructor and setter.
* [Injection with CDI (Part III)](https://antoniogoncalves.org/2011/09/25/injection-with-cdi-part-iii/) - In this post I’ll explain producers or “how you can inject anything anywhere in a type safe manner“.

<hr>

**[Asynchronous REST Services with JAX-RS and CompletableFuture · allegro.tech](https://allegro.tech/2014/10/async-rest.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-10-29
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

One of new features introduced by JAX-RS 2.0 is asynchronous processing in Server and Client APIs. We use these APIs together with CompletableFutur...

<hr>


## javascript 

**[Moment.js Home Page](http://momentjs.com/)**

  * **tags**: &nbsp; [date](https://www.codingmarks.org/search?q=[date]), &nbsp; [datetime](https://www.codingmarks.org/search?q=[datetime]), &nbsp; [date-manipulation](https://www.codingmarks.org/search?q=[date-manipulation]), &nbsp; [time](https://www.codingmarks.org/search?q=[time]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/moment/moment/)

A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.

**[Documentation](http://momentjs.com/docs/)**

<hr>

**[express-validator](https://express-validator.github.io)**

  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/express-validator/express-validator)

An express.js middleware for validator.js. 

<hr>

**[validator.js](https://github.com/chriso/validator.js)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/chriso/validator.js)

String validation. 

<hr>

**[Jest · 🃏 Delightful JavaScript Testing](https://jestjs.io/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [reactjs](https://www.codingmarks.org/search?q=[reactjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/facebook/jest)

🃏 Delightful JavaScript Testing

<hr>

**[CodeMirror](http://codemirror.net/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/codemirror/codemirror)

CodeMirror is a versatile text editor implemented in JavaScript for the browser. It is specialized for editing code, and comes with a number of language modes and addons that implement more advanced editing functionality.

<hr>

**[Marked.js Homepage](https://marked.js.org/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [markdown](https://www.codingmarks.org/search?q=[markdown])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/markedjs/marked)

A markdown parser and compiler. Built for speed. 

<hr>


## jersey 

**[How To Use Jersey Client Efficiently](https://blogs.oracle.com/japod/how-to-use-jersey-client-efficiently)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-03-30
  * **tags**: &nbsp; [rest-client](https://www.codingmarks.org/search?q=[rest-client]), &nbsp; [jersey](https://www.codingmarks.org/search?q=[jersey])

In this blog post, i would like to introduce some tips to avoid unnecessary memory allocation and also to save some CPU cycles to achieve better throughput.

* Tip 1: Be careful when updating WebTarget config!
* Tip 2: Avoid WebTarget.register()
* Tip 3: Avoid WebTarget.property()
* Tip 4: Use response.close() to release network connections!


<hr>


## json 

**[GitHub - Keyang/node-csvtojson](https://github.com/Keyang/node-csvtojson)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [csv](https://www.codingmarks.org/search?q=[csv]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Keyang/node-csvtojson)

Blazing fast and Comprehensive CSV Parser for Node.JS / Browser / Command Line.  - Keyang/node-csvtojson

<hr>

**[How To Serialize Enums as JSON Objects with Jackson](https://www.baeldung.com/jackson-serialize-enums)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-23
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [serialization](https://www.codingmarks.org/search?q=[serialization]), &nbsp; [jackson](https://www.codingmarks.org/search?q=[jackson]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/jackson#readme)

How to serialize an Enum as a JSON Object using Jackson 2.

<hr>


## linux 

**[5 Commands for Checking Memory Usage in Linux](https://www.linux.com/learn/5-commands-checking-memory-usage-linux)**

  * **tags**: &nbsp; [linux](https://www.codingmarks.org/search?q=[linux]), &nbsp; [memory-management](https://www.codingmarks.org/search?q=[memory-management])

* `free -m`
* `egrep --color 'Mem|Cache|Swap' /proc/meminfo` - This will produce an easy to read listing of all entries that contain Mem, Cache, and Swap ... with a splash of color

<hr>


## markdown 

**[Marked.js Homepage](https://marked.js.org/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [markdown](https://www.codingmarks.org/search?q=[markdown])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/markedjs/marked)

A markdown parser and compiler. Built for speed. 

<hr>


## memory-management 

**[5 Commands for Checking Memory Usage in Linux](https://www.linux.com/learn/5-commands-checking-memory-usage-linux)**

  * **tags**: &nbsp; [linux](https://www.codingmarks.org/search?q=[linux]), &nbsp; [memory-management](https://www.codingmarks.org/search?q=[memory-management])

* `free -m`
* `egrep --color 'Mem|Cache|Swap' /proc/meminfo` - This will produce an easy to read listing of all entries that contain Mem, Cache, and Swap ... with a splash of color

<hr>


## microservices 

**[Thorntail Home Page](http://thorntail.io/)**

  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/thorntail/thorntail)

Thorntail provides a mechanism for building applications as uber jars, with just enough of the WildFly application server wrapped around it to support each application's use-case.

<hr>

**[What We Got Wrong: Lessons from the Birth of Microservices - YouTube](https://www.youtube.com/watch?v=-pDyNsB9Zr0)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-08
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [software-architecture](https://www.codingmarks.org/search?q=[software-architecture])

Ben Sigelman talks about what Google got wrong about microservices, the lessons learned along the way and how to apply those lessons today.

<hr>

**[Building Microservices with Node, Docker and Nginx pt 1 - What is a Microservice?](https://www.youtube.com/watch?v=EsCfPxjmnjo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-29
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fChristenson/microservices-example)

`Building` Microservices with Node, Docker and Nginx  
* Part 1 What is a Microservice?" I walk you through what a Microservice is and what this 3 part tutorial will show you.
* Part 2 [Making a microservice](https://www.youtube.com/watch?v=aWxR05rUoto) - shows you how to make a Microservices and some of the thoughts I have about them.
* Part 3 [Connecting the microservices](https://www.youtube.com/watch?v=QjhJs31h_4k) - shows you how to use Docker compose and Nginx to connect your fleet of Microservices




<hr>


## mocking 

**[Mockito: Why You Should Not Use InjectMocks Annotation to Autowire Fields – Ted Vinke's Blog](https://tedvinke.wordpress.com/2014/02/13/mockito-why-you-should-not-use-injectmocks-annotation-to-autowire-fields/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-02-13
  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [mockito](https://www.codingmarks.org/search?q=[mockito]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking])

People like the way how Mockito is able to mock Spring's auto-wired fields with the @InjectMocks annotation. When I read this post of Lubos Krnac last week, I thought I should explain why I think the use of InjectMocks is a bad signal and how you should avoid it. Hint: it's about visibility.

<hr>

**[MockAPI docs](https://www.mockapi.io/docs)**

  * **tags**: &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking]), &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

MockAPI is a simple tool that lets you easily mock up APIs, generate custom data, and preform operations on it using RESTful interface. MockAPI is meant to be used as a prototyping/testing/learning tool.

<hr>


## mockito 

**[Mockito: Why You Should Not Use InjectMocks Annotation to Autowire Fields – Ted Vinke's Blog](https://tedvinke.wordpress.com/2014/02/13/mockito-why-you-should-not-use-injectmocks-annotation-to-autowire-fields/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-02-13
  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [mockito](https://www.codingmarks.org/search?q=[mockito]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking])

People like the way how Mockito is able to mock Spring's auto-wired fields with the @InjectMocks annotation. When I read this post of Lubos Krnac last week, I thought I should explain why I think the use of InjectMocks is a bad signal and how you should avoid it. Hint: it's about visibility.

<hr>


## mongodb 

**[Generating Globally Unique Identifiers for Use with MongoDB](https://www.mongodb.com/blog/post/generating-globally-unique-identifiers-for-use-with-mongodb)**

  * **tags**: &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])

By default, MongoDB generates a unique identifier that is assigned to the `_id` field in a new document before writing that document to the database. In many cases the default unique identifiers assigned by MongoDB will meet application requirements. However, in some cases an application may need to create custom unique identifiers

<hr>

**[Mongoose - SubDocuments](https://mongoosejs.com/docs/subdocs.html)**

  * **tags**: &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])

Subdocuments are documents embedded in other documents. In Mongoose, this means you can nest schemas in other schemas. Mongoose has two distinct notions of subdocuments: arrays of subdocuments and single nested subdocuments.

```
var childSchema = new Schema({ name: 'string' });

var parentSchema = new Schema({
  // Array of subdocuments
  children: [childSchema],
  // Single nested subdocuments. Caveat: single nested subdocs only work
  // in mongoose >= 4.2.0
  child: childSchema
});
```


<hr>

**[Cleaner code in NodeJs with async-await - Mongoose calls example – CodingpediaOrg](http://www.codingpedia.org/ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-05
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [async-await](https://www.codingmarks.org/search?q=[async-await]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks-api)

Example showing migration of Mongoose calls from previously using callbacks to using the new async-await feature in NodeJs

<hr>


## mongoose 

**[Mongoose - SubDocuments](https://mongoosejs.com/docs/subdocs.html)**

  * **tags**: &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])

Subdocuments are documents embedded in other documents. In Mongoose, this means you can nest schemas in other schemas. Mongoose has two distinct notions of subdocuments: arrays of subdocuments and single nested subdocuments.

```
var childSchema = new Schema({ name: 'string' });

var parentSchema = new Schema({
  // Array of subdocuments
  children: [childSchema],
  // Single nested subdocuments. Caveat: single nested subdocs only work
  // in mongoose >= 4.2.0
  child: childSchema
});
```


<hr>

**[Cleaner code in NodeJs with async-await - Mongoose calls example – CodingpediaOrg](http://www.codingpedia.org/ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-05
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [async-await](https://www.codingmarks.org/search?q=[async-await]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks-api)

Example showing migration of Mongoose calls from previously using callbacks to using the new async-await feature in NodeJs

<hr>


## multithreading 

**[Finally Getting the Most out of the Java Thread Pool](https://stackify.com/java-thread-pools/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-09-06
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [multithreading](https://www.codingmarks.org/search?q=[multithreading])

Finally understanding how thread pools really work in Java can be the difference between your application being a slog, or a clean and efficient system.

<hr>


## nginx 

**[Building Microservices with Node, Docker and Nginx pt 1 - What is a Microservice?](https://www.youtube.com/watch?v=EsCfPxjmnjo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-29
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fChristenson/microservices-example)

`Building` Microservices with Node, Docker and Nginx  
* Part 1 What is a Microservice?" I walk you through what a Microservice is and what this 3 part tutorial will show you.
* Part 2 [Making a microservice](https://www.youtube.com/watch?v=aWxR05rUoto) - shows you how to make a Microservices and some of the thoughts I have about them.
* Part 3 [Connecting the microservices](https://www.youtube.com/watch?v=QjhJs31h_4k) - shows you how to use Docker compose and Nginx to connect your fleet of Microservices




<hr>


## nodejs 

**[GitHub - Keyang/node-csvtojson](https://github.com/Keyang/node-csvtojson)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [csv](https://www.codingmarks.org/search?q=[csv]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Keyang/node-csvtojson)

Blazing fast and Comprehensive CSV Parser for Node.JS / Browser / Command Line.  - Keyang/node-csvtojson

<hr>

**[Claudia.js Home Page](https://claudiajs.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws]), &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/claudiajs/claudia)

Deploy Node.js projects to AWS Lambda and API Gateway easily.

<hr>

**[Fastify Homepage](https://www.fastify.io/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fastify/fastify)

Fast and low overhead web framework, for Node.js

<hr>

**[dotenv homepage](https://github.com/motdotla/dotenv)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/motdotla/dotenv)

Loads environment variables from .env for nodejs projects.

<hr>

**[Cleaner code in NodeJs with async-await - Mongoose calls example – CodingpediaOrg](http://www.codingpedia.org/ama/cleaner-code-in-nodejs-with-async-await-mongoose-calls-example)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-11-05
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [async-await](https://www.codingmarks.org/search?q=[async-await]), &nbsp; [mongoose](https://www.codingmarks.org/search?q=[mongoose]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks-api)

Example showing migration of Mongoose calls from previously using callbacks to using the new async-await feature in NodeJs

<hr>

**[Building Microservices with Node, Docker and Nginx pt 1 - What is a Microservice?](https://www.youtube.com/watch?v=EsCfPxjmnjo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-29
  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fChristenson/microservices-example)

`Building` Microservices with Node, Docker and Nginx  
* Part 1 What is a Microservice?" I walk you through what a Microservice is and what this 3 part tutorial will show you.
* Part 2 [Making a microservice](https://www.youtube.com/watch?v=aWxR05rUoto) - shows you how to make a Microservices and some of the thoughts I have about them.
* Part 3 [Connecting the microservices](https://www.youtube.com/watch?v=QjhJs31h_4k) - shows you how to use Docker compose and Nginx to connect your fleet of Microservices




<hr>


## npm 

**[npm documentation  ](https://docs.npmjs.com/)**

  * **tags**: &nbsp; [npm](https://www.codingmarks.org/search?q=[npm]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

* About npm
* Getting started
* Packages and modules
* Integrations
* Orgs
* npm Enterprise
* CLI documentation

<hr>


## open-source 

**[The Open Source Definition](https://opensource.org/docs/osd)**

  * **tags**: &nbsp; [open-source](https://www.codingmarks.org/search?q=[open-source])

Open source doesn't just mean access to the source code. The distribution terms of open-source software must comply with the criteria listed in the link.

<hr>


## orm 

**[ORM Is an Offensive Anti-Pattern ](https://www.yegor256.com/2014/12/01/orm-offensive-anti-pattern.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-12-01
  * **tags**: &nbsp; [orm](https://www.codingmarks.org/search?q=[orm]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])

TL;DR ORM is a terrible anti-pattern that violates all principles of object-oriented programming, tearing objects apart and turning them into dumb and passive data bags. There is no excuse for ORM existence in any application, be it a small web app or an enterprise-size system with thousands of tables and CRUD manipulations on them. What is the alternative? **SQL-speaking objects**.

<hr>


## programming 

**[Coding bookmarks, aka #codingmarks](https://www.codingmarks.org)**

  * **tags**: &nbsp; [software-development](https://www.codingmarks.org/search?q=[software-development]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books]), &nbsp; [programming](https://www.codingmarks.org/search?q=[programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks)

Efficiently manage and retrieve your coding bookmarks. Contribute by sharing and voting the worthy ones, so others can benefit

<hr>


## reactjs 

**[Jest · 🃏 Delightful JavaScript Testing](https://jestjs.io/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [reactjs](https://www.codingmarks.org/search?q=[reactjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/facebook/jest)

🃏 Delightful JavaScript Testing

<hr>


## rest 

**[Fastify Homepage](https://www.fastify.io/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/fastify/fastify)

Fast and low overhead web framework, for Node.js

<hr>

**[
Architectural Styles and the Design of Network-based Software Architectures
](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)**

  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])

This dissertation defines a framework for understanding software architecture via architectural styles and demonstrates how styles can be used to guide the architectural design of network-based application software. A survey of architectural styles for network-based applications is used to classify styles according to the architectural properties they induce on an architecture for distributed hypermedia. I then introduce the Representational State Transfer (REST) architectural style and describe how REST has been used to guide the design and development of the architecture for the modern Web.

REST emphasizes scalability of component interactions, generality of interfaces, independent deployment of components, and intermediary components to reduce interaction latency, enforce security, and encapsulate legacy systems. I describe the software engineering principles guiding REST and the interaction constraints chosen to retain those principles, contrasting them to the constraints of other architectural styles. Finally, I describe the lessons learned from applying REST to the design of the Hypertext Transfer Protocol and Uniform Resource Identifier standards, and from their subsequent deployment in Web client and server software.

<hr>

**[RESTEasy Client API](https://www.baeldung.com/resteasy-client-tutorial)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-01
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [rest-client](https://www.codingmarks.org/search?q=[rest-client]), &nbsp; [resteasy](https://www.codingmarks.org/search?q=[resteasy])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/resteasy)

A quick guide to building out a client for the REST API using RESTEasy.

<hr>

**[Asynchronous REST Services with JAX-RS and CompletableFuture · allegro.tech](https://allegro.tech/2014/10/async-rest.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-10-29
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous])

One of new features introduced by JAX-RS 2.0 is asynchronous processing in Server and Client APIs. We use these APIs together with CompletableFutur...

<hr>


## rest-client 

**[How To Use Jersey Client Efficiently](https://blogs.oracle.com/japod/how-to-use-jersey-client-efficiently)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-03-30
  * **tags**: &nbsp; [rest-client](https://www.codingmarks.org/search?q=[rest-client]), &nbsp; [jersey](https://www.codingmarks.org/search?q=[jersey])

In this blog post, i would like to introduce some tips to avoid unnecessary memory allocation and also to save some CPU cycles to achieve better throughput.

* Tip 1: Be careful when updating WebTarget config!
* Tip 2: Avoid WebTarget.register()
* Tip 3: Avoid WebTarget.property()
* Tip 4: Use response.close() to release network connections!


<hr>

**[RESTEasy Client API](https://www.baeldung.com/resteasy-client-tutorial)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-01
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [rest-client](https://www.codingmarks.org/search?q=[rest-client]), &nbsp; [resteasy](https://www.codingmarks.org/search?q=[resteasy])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/resteasy)

A quick guide to building out a client for the REST API using RESTEasy.

<hr>


## resteasy 

**[RESTEasy Client API](https://www.baeldung.com/resteasy-client-tutorial)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-01
  * **tags**: &nbsp; [rest](https://www.codingmarks.org/search?q=[rest]), &nbsp; [rest-client](https://www.codingmarks.org/search?q=[rest-client]), &nbsp; [resteasy](https://www.codingmarks.org/search?q=[resteasy])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/resteasy)

A quick guide to building out a client for the REST API using RESTEasy.

<hr>


## rxjs 

**[Advanced caching with RxJS by thoughtram](https://blog.thoughtram.io/angular/2018/03/05/advanced-caching-with-rxjs.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-03-05
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular]), &nbsp; [caching](https://www.codingmarks.org/search?q=[caching]), &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://stackblitz.com/edit/advanced-caching-with-rxjs-step-4)

When building web applications, performance should always be a top priority. One very efficient way to optimize the performance of our applications and improve the experience of our site is to use caching mechanisms. In this post we'll develop an advanced caching mechanism with RxJS and the tools provided by Angular to cache application data.

<hr>


## serialization 

**[How To Serialize Enums as JSON Objects with Jackson](https://www.baeldung.com/jackson-serialize-enums)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-23
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [serialization](https://www.codingmarks.org/search?q=[serialization]), &nbsp; [jackson](https://www.codingmarks.org/search?q=[jackson]), &nbsp; [json](https://www.codingmarks.org/search?q=[json])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/jackson#readme)

How to serialize an Enum as a JSON Object using Jackson 2.

<hr>


## serverless 

**[Claudia.js Home Page](https://claudiajs.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws]), &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/claudiajs/claudia)

Deploy Node.js projects to AWS Lambda and API Gateway easily.

<hr>

**[GitHub - anaibol/awesome-serverless](https://github.com/anaibol/awesome-serverless)**

  * **tags**: &nbsp; [serverless](https://www.codingmarks.org/search?q=[serverless]), &nbsp; [cloud](https://www.codingmarks.org/search?q=[cloud]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/anaibol/awesome-serverless)

:cloud: A curated list of awesome services, solutions and resources for serverless / nobackend applications. - anaibol/awesome-serverless

<hr>


## software-architecture 

**[What We Got Wrong: Lessons from the Birth of Microservices - YouTube](https://www.youtube.com/watch?v=-pDyNsB9Zr0)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-11-08
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [software-architecture](https://www.codingmarks.org/search?q=[software-architecture])

Ben Sigelman talks about what Google got wrong about microservices, the lessons learned along the way and how to apply those lessons today.

<hr>


## software-development 

**[Coding bookmarks, aka #codingmarks](https://www.codingmarks.org)**

  * **tags**: &nbsp; [software-development](https://www.codingmarks.org/search?q=[software-development]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books]), &nbsp; [programming](https://www.codingmarks.org/search?q=[programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Codingpedia/codingmarks)

Efficiently manage and retrieve your coding bookmarks. Contribute by sharing and voting the worthy ones, so others can benefit

<hr>


## spring 

**[Oliver Gierke - Why field injection is evil](http://olivergierke.de/2013/11/why-field-injection-is-evil/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-11-22
  * **tags**: &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring])

I’m quite frequently getting pulled into discussions on Twitter about the different flavors of Dependency Injection. Also, I’ve repeatedly expressed my distaste for field injection but as Twitter is not the right communication channel to give an in-depth rational about my opinion. So here we go.

The comments section is also interesting. 

<hr>


## supertest 

**[Express Integration Testing with SuperTest · InVision Engineering Blog](https://engineering.invisionapp.com/post/express-integration-testing-supertest/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-06-27
  * **tags**: &nbsp; [expressjs](https://www.codingmarks.org/search?q=[expressjs]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing]), &nbsp; [supertest](https://www.codingmarks.org/search?q=[supertest]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshmatz/supertest-example)

Put down that REST Client (or, gasp, the browser!) you’re using to test that API you’re developing and back away slowly! There’s a better way! With SuperTest there’s no need to verify your API by hand. Plus, using it gives you virtually free integration tests. Now you can test and code new features at the same time.

<hr>


## test-coverage 

**[Istanbul, a JavaScript test coverage tool.](https://istanbul.js.org/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [test-coverage](https://www.codingmarks.org/search?q=[test-coverage])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istanbuljs)

Istanbul instruments your ES5 and ES2015+ JavaScript code with line counters, so that you can track how well your unit-tests exercise your codebase.

<hr>


## testing 

**[Mockito: Why You Should Not Use InjectMocks Annotation to Autowire Fields – Ted Vinke's Blog](https://tedvinke.wordpress.com/2014/02/13/mockito-why-you-should-not-use-injectmocks-annotation-to-autowire-fields/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-02-13
  * **tags**: &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [mockito](https://www.codingmarks.org/search?q=[mockito]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking])

People like the way how Mockito is able to mock Spring's auto-wired fields with the @InjectMocks annotation. When I read this post of Lubos Krnac last week, I thought I should explain why I think the use of InjectMocks is a bad signal and how you should avoid it. Hint: it's about visibility.

<hr>

**[Should.js API Documentation](http://shouldjs.github.io/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [bdd](https://www.codingmarks.org/search?q=[bdd])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/shouldjs/should.js)

_should_ is an expressive, readable, framework-agnostic assertion library. The main goals of this library are to be expressive and to be helpful. It keeps your test code clean, and your error messages helpful.

<hr>

**[Istanbul, a JavaScript test coverage tool.](https://istanbul.js.org/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [test-coverage](https://www.codingmarks.org/search?q=[test-coverage])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istanbuljs)

Istanbul instruments your ES5 and ES2015+ JavaScript code with line counters, so that you can track how well your unit-tests exercise your codebase.

<hr>

**[Jest · 🃏 Delightful JavaScript Testing](https://jestjs.io/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [reactjs](https://www.codingmarks.org/search?q=[reactjs])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/facebook/jest)

🃏 Delightful JavaScript Testing

<hr>


## time 

**[Moment.js Home Page](http://momentjs.com/)**

  * **tags**: &nbsp; [date](https://www.codingmarks.org/search?q=[date]), &nbsp; [datetime](https://www.codingmarks.org/search?q=[datetime]), &nbsp; [date-manipulation](https://www.codingmarks.org/search?q=[date-manipulation]), &nbsp; [time](https://www.codingmarks.org/search?q=[time]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/moment/moment/)

A lightweight JavaScript date library for parsing, validating, manipulating, and formatting dates.

**[Documentation](http://momentjs.com/docs/)**

<hr>


## tools 

**[MockAPI docs](https://www.mockapi.io/docs)**

  * **tags**: &nbsp; [mocking](https://www.codingmarks.org/search?q=[mocking]), &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

MockAPI is a simple tool that lets you easily mock up APIs, generate custom data, and preform operations on it using RESTful interface. MockAPI is meant to be used as a prototyping/testing/learning tool.

<hr>

**[dotenv homepage](https://github.com/motdotla/dotenv)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/motdotla/dotenv)

Loads environment variables from .env for nodejs projects.

<hr>

**[CodeMirror](http://codemirror.net/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/codemirror/codemirror)

CodeMirror is a versatile text editor implemented in JavaScript for the browser. It is specialized for editing code, and comes with a number of language modes and addons that implement more advanced editing functionality.

<hr>


## weld 

**[ CDI Reference Implementation](http://docs.jboss.org/weld/reference/latest/en-US/html_single/)**

  * **tags**: &nbsp; [weld](https://www.codingmarks.org/search?q=[weld]), &nbsp; [dependency-injection](https://www.codingmarks.org/search?q=[dependency-injection]), &nbsp; [javaee](https://www.codingmarks.org/search?q=[javaee]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

CDI: Contexts and Dependency Injection for the Java EE platform

<hr>

