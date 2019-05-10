---
layout: post
title: Lots of exciting dev bookmarks added in march 2019
description: "Lots of exciting dev #bookmarks added in march 2019. Keywords: alias, api, api-management, architecture, arquillian, async, asynchronous, awesome, awesome-list, aws, bash, bootstrap, c, chai, cheatsheet, circuit-breaker, css, devops, diagnostics, diagram, dns, docker, docker-compose, docs, documentation, domain-driven-design, dotfiles, email, error-handling, facebook, favicon, ffmpeg, free-programming-books, functional-programming, gif, git, gradle, graph, helm, homebrew, html, html5, immutable.js, install, integration-testing, istio, java, javascript, jenv, junit, jvm, kotlin, kubernetes, kubernetes-helm, latex, linux, macos, markdown, maven-plugin, microservices, middleware, mocha, mongodb, monitoring, nginx, nodejs, oidc, online-tools, open-source, openid-connect, osx, package-manager, pandoc, performance, podcast, promise, proxy, r, reactive, reactive-programming, reactjs, redis, resources, rest, rxjs, security, shell, spring, spring-boot, spring-cloud, spring-cloud-gateway, spring-security, spring-webflux, sql, testing, tools, user-experience, video-processing, vim, visualization, web, websocket, windows and zuul"
author: ama
permalink: /ama/new-dev-bookmarks-march-2019
published: true
categories: [codingmarks]
tags: [codingmarks]
---
New [dev bookmarks](https://www.bookmarks.dev) added in march 2019. Hot topics include:

* TOC
{:toc} 

<!--more-->

## alias 

**[Nice dotfiles example](https://github.com/jessfraz/dotfiles)**

  * **tags**: &nbsp; [dotfiles](https://www.codingmarks.org/search?q=[dotfiles]), &nbsp; [alias](https://www.codingmarks.org/search?q=[alias]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jessfraz/dotfiles)

My dotfiles. Buyer beware ;)

<hr>


## api 

**[API Infrastructure at Knewton: What’s in an Edge Service?](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-05-09
  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx]), &nbsp; [zuul](https://www.codingmarks.org/search?q=[zuul])

In this post, we will pull back the covers of our API to explain how we handle user requests. You will first learn how to build an edge service with Netflix Zuul, the framework we chose for its simplicity and flexibility. Then, we’ll dive into the Knewton edge service to show you how it improves API simplicity, flexibility, and performance.

What’s in an Edge Service
-------
An edge service is a component which is exposed to the public internet. It acts as a gateway to all other services, which we will refer to as platform services. For example, consider an Nginx reverse proxy in front of some web resource servers. Here, Nginx acts as an edge service by routing public HTTP requests to the appropriate platform service.

<hr>

**[Dredd — HTTP API Testing Framework — Dredd latest documentation](https://dredd.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/dredd)

Dredd is a language-agnostic command-line tool for validating API description document against backend implementation of the API.

Dredd reads your API description and step by step validates whether your API implementation replies with responses as they are described in the documentation.

<hr>

**[API Blueprint home page](https://apiblueprint.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/api-blueprint/)

API Blueprint is simple and accessible to everybody involved in the API lifecycle. Its syntax is concise yet expressive. With API Blueprint you can quickly design and prototype APIs to be created or document and test already deployed mission-critical APIs.

<hr>


## api-management 

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>


## architecture 

**[Oliver Gierke - Whoops! Where did my architecture go](http://olivergierke.de/2013/01/whoops-where-did-my-architecture-go/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-01-01
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/odrotbohm/whoops-architecture)

#### Summary

The basic approach I recommend is to move the vertical slices into the focus of the package naming and try to model them in a way that the public API of a slice is as tiny as possible in the first place. This is of course no silver bullet approach as packages can grow significantly and it might make sense to extract certain types into a sub-package or the like which then usually leads you to the need to use an architecture management tool. The core idea here is to try to use the means of visibility control that are available in Java to write code that is not a giant potential dependency mess in the first place. Packages can actually help you to achieve exactly that.

<hr>

**[Layers, hexagons, features and components ](http://www.codingthearchitecture.com/2016/04/25/layers_hexagons_features_and_components.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-04-25
  * **tags**: &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Layers, hexagons, features and components - Coding the Architecture

<hr>

**[BoundedContext](https://martinfowler.com/bliki/BoundedContext.html)**

  * **tags**: &nbsp; [domain-driven-design](https://www.codingmarks.org/search?q=[domain-driven-design]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Bounded Context is a central pattern in Domain-Driven Design. It is the focus of DDD's strategic design section which is all about dealing with large models and teams. DDD deals with large models by dividing them into different Bounded Contexts and being explicit about their interrelationships.

<hr>

**[The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2012-08-13
  * **tags**: &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Over the last several years we’ve seen a whole range of ideas regarding the architecture of systems. These include:

* [Hexagonal Architecture ](http://alistair.cockburn.us/Hexagonal+architecture)(a.k.a. Ports and Adapters) by Alistair Cockburn and adopted by Steve Freeman, and Nat Pryce in their wonderful book Growing Object Oriented Software
* [Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/) by Jeffrey Palermo
* [Screaming Architecture](http://blog.cleancoders.com/2011-09-30-Screaming-Architecture) from a blog of mine last year
* [DCI](http://www.amazon.com/Lean-Architecture-Agile-Software-Development/dp/0470684208/) from James Coplien, and Trygve Reenskaug.
* [BCE](http://www.amazon.com/Object-Oriented-Software-Engineering-Approach/dp/0201544350) by Ivar Jacobson from his book Object Oriented Software Engineering: A Use-Case Driven Approach

Though these architectures all vary somewhat in their details, they are very similar. They all have the same objective, which is the separation of concerns. They all achieve this separation by dividing the software into layers. Each has at least one layer for business rules, and another for interfaces.

The diagram at the top of this article is an attempt at integrating all these architectures into a single actionable idea.

<hr>

**[Strategic Domain Driven Design with Context Mapping](https://www.infoq.com/articles/ddd-contextmapping)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2009-11-25
  * **tags**: &nbsp; [domain-driven-design](https://www.codingmarks.org/search?q=[domain-driven-design]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Many approaches to object oriented modeling tend not to scale well when the applications grow in size and complexity. **Context Mapping** technique can be used to manage the complexity in large software development projects. In this article, author Alberto Brandolini discusses the many sides of **bounded contexts** and how to use them to build a context map to support key decisions in a software project.

"_Code is the primary form of expression of the model. Although other artifacts might be necessary along the way to capture requirements or portions of the design, the only one that'll be constantly in sync with the application behavior is code itself._"

"_Trying to set up a partnership without a collaborative environment, is clearly a dead end strategy._"

<hr>

**[Big Ball of Mud](http://www.laputan.org/mud/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;1999-06-26
  * **tags**: &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

While much attention has been focused on  high-level software architectural patterns, what is, in effect, the de-facto standard software architecture is seldom discussed. This paper examines the most frequently deployed architecture: 
the **BIG BALL OF MUD**

<hr>


## arquillian 

**[Arquillian  Home Page](http://arquillian.org/)**

  * **tags**: &nbsp; [arquillian](https://www.codingmarks.org/search?q=[arquillian]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arquillian)

An innovative testing platform for the Java Virtual Machine (JVM). Open source. Highly extensible.

<hr>


## async 

**[Java 8: Definitive guide to CompletableFuture](https://www.nurkiewicz.com/2013/05/java-8-definitive-guide-to.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-05-09
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous]), &nbsp; [async](https://www.codingmarks.org/search?q=[async])

Examples of using and combining `CompletableFuture` API

<hr>


## asynchronous 

**[Java 8: Definitive guide to CompletableFuture](https://www.nurkiewicz.com/2013/05/java-8-definitive-guide-to.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-05-09
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous]), &nbsp; [async](https://www.codingmarks.org/search?q=[async])

Examples of using and combining `CompletableFuture` API

<hr>


## awesome 

**[useful-java-links](https://github.com/Vedenin/useful-java-links#readme)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [resources](https://www.codingmarks.org/search?q=[resources]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Vedenin/useful-java-links)

A list of useful Java frameworks, libraries, software and hello worlds examples - Vedenin/useful-java-links

<hr>

**[GitHub - stoeffel/awesome-fp-js](https://github.com/stoeffel/awesome-fp-js#readme)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/stoeffel/awesome-fp-js)

:sunglasses: A curated list of awesome functional programming stuff in js - stoeffel/awesome-fp-js

<hr>

**[GitHub - arslanbilal/git-cheat-sheet: git and git flow cheat sheet](https://github.com/arslanbilal/git-cheat-sheet#readme)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arslanbilal/git-cheat-sheet#readme)

##### Git Cheat Sheet English
* [Set Up](https://github.com/arslanbilal/git-cheat-sheet#setup)
* [Configuration Files](https://github.com/arslanbilal/git-cheat-sheet#configuration-files)
* [Create](https://github.com/arslanbilal/git-cheat-sheet#create)
* [Local Changes](https://github.com/arslanbilal/git-cheat-sheet#local-changes)
* [Search](https://github.com/arslanbilal/git-cheat-sheet#search)
* [Commit History](https://github.com/arslanbilal/git-cheat-sheet#commit-history)
* [Branches & Tags](https://github.com/arslanbilal/git-cheat-sheet#branches--tags)
* [Update & Publish](https://github.com/arslanbilal/git-cheat-sheet#update--publish)
* [Merge & Rebase](https://github.com/arslanbilal/git-cheat-sheet#merge--rebase)
* [Undo](https://github.com/arslanbilal/git-cheat-sheet#undo)
* [Git Flow](https://github.com/arslanbilal/git-cheat-sheet#git-flow)

<hr>


## awesome-list 

**[useful-java-links](https://github.com/Vedenin/useful-java-links#readme)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [resources](https://www.codingmarks.org/search?q=[resources]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Vedenin/useful-java-links)

A list of useful Java frameworks, libraries, software and hello worlds examples - Vedenin/useful-java-links

<hr>

**[GitHub - stoeffel/awesome-fp-js](https://github.com/stoeffel/awesome-fp-js#readme)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/stoeffel/awesome-fp-js)

:sunglasses: A curated list of awesome functional programming stuff in js - stoeffel/awesome-fp-js

<hr>


## aws 

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>

**[AWS re:Invent 2015: DevOps at Amazon: A Look at Our Tools and Processes (DVO202) - YouTube](https://www.youtube.com/watch?v=esEFaY0FDKc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-10-15
  * **tags**: &nbsp; [aws](https://www.codingmarks.org/search?q=[aws]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])

Learn about Amazon's transition to a service-oriented architecture over a decade ago. We share lessons learned, processes adopted, and the tools built to increase both agility and reliability. 

<hr>


## bash 

**[Nice dotfiles example](https://github.com/jessfraz/dotfiles)**

  * **tags**: &nbsp; [dotfiles](https://www.codingmarks.org/search?q=[dotfiles]), &nbsp; [alias](https://www.codingmarks.org/search?q=[alias]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jessfraz/dotfiles)

My dotfiles. Buyer beware ;)

<hr>

**[command line - How to list all symbolic links in a directory - Ask Ubuntu](https://askubuntu.com/questions/522051/how-to-list-all-symbolic-links-in-a-directory)**

  * **tags**: &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [shell](https://www.codingmarks.org/search?q=[shell]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

```
find . -type l -ls
```

To only process the current directory:
```
find . -maxdepth 1 -type l -ls
```

<hr>

**[Install Bash git completion](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [osx](https://www.codingmarks.org/search?q=[osx]), &nbsp; [windows](https://www.codingmarks.org/search?q=[windows]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

How to install git completion on different platforms...

<hr>


## bootstrap 

**[Bootstrap 4 Cheat Sheet - The ultimate list of Bootstrap classes](https://hackerthemes.com/bootstrap-cheatsheet/)**

  * **tags**: &nbsp; [bootstrap](https://www.codingmarks.org/search?q=[bootstrap]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])

Quickly find your Bootstrap classes on this interactive Bootstrap cheat sheet. It includes code samples and live preview of elements.

<hr>


## c 

**[GitHub - krallin/tini](https://github.com/krallin/tini)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux]), &nbsp; [c](https://www.codingmarks.org/search?q=[c])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/krallin/tini)

A tiny but valid `init` for containers. 

<hr>


## chai 

**[Modern Node.js: async/await based testing with Mocha & Chai](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-04-09
  * **tags**: &nbsp; [mocha](https://www.codingmarks.org/search?q=[mocha]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)

Mocha is a JavaScript test framework running on Node.js and in the browser. It can run both asynchronous and synchronous code serially. Test cases are created using describe() and it() methods, the former is used to provide a structure by allowing to put various tests cases in logical groups while the latter is where the tests are written.

In order to perform actual tests, there is a need for an assertion library: a runtime mechanism which can be used to verify assumptions made by the program and print a diagnostic message if this assumption is false. Node.js comes with a built-in assert library. Chai is another popular assertion library that provides both the BDD and TDD styles of programming for testing the code. BDD stands for Behavior-driven development while TDD stands for Test-driven development. In a nutshell, Chai provides a should keyword for BDD style which allows to chain assertions and an expect() method for TDD style. Choosing between one and another is a matter of personal preference.

<hr>

**[GitHub - domenic/chai-as-promised](https://github.com/domenic/chai-as-promised)**

  * **tags**: &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [promise](https://www.codingmarks.org/search?q=[promise]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/domenic/chai-as-promised)

Extends Chai with assertions about promises. Contribute to domenic/chai-as-promised development by creating an account on GitHub.

<hr>


## cheatsheet 

**[GitHub - arslanbilal/git-cheat-sheet: git and git flow cheat sheet](https://github.com/arslanbilal/git-cheat-sheet#readme)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arslanbilal/git-cheat-sheet#readme)

##### Git Cheat Sheet English
* [Set Up](https://github.com/arslanbilal/git-cheat-sheet#setup)
* [Configuration Files](https://github.com/arslanbilal/git-cheat-sheet#configuration-files)
* [Create](https://github.com/arslanbilal/git-cheat-sheet#create)
* [Local Changes](https://github.com/arslanbilal/git-cheat-sheet#local-changes)
* [Search](https://github.com/arslanbilal/git-cheat-sheet#search)
* [Commit History](https://github.com/arslanbilal/git-cheat-sheet#commit-history)
* [Branches & Tags](https://github.com/arslanbilal/git-cheat-sheet#branches--tags)
* [Update & Publish](https://github.com/arslanbilal/git-cheat-sheet#update--publish)
* [Merge & Rebase](https://github.com/arslanbilal/git-cheat-sheet#merge--rebase)
* [Undo](https://github.com/arslanbilal/git-cheat-sheet#undo)
* [Git Flow](https://github.com/arslanbilal/git-cheat-sheet#git-flow)

<hr>

**[GitHub - audreyr/favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [web](https://www.codingmarks.org/search?q=[web]), &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/audreyr/favicon-cheat-sheet)

Obsessive cheat sheet to favicon sizes/types. Please contribute! (Note: this may be in flux as I learn new things about favicon best practices.) - audreyr/favicon-cheat-sheet

<hr>

**[Bootstrap 4 Cheat Sheet - The ultimate list of Bootstrap classes](https://hackerthemes.com/bootstrap-cheatsheet/)**

  * **tags**: &nbsp; [bootstrap](https://www.codingmarks.org/search?q=[bootstrap]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])

Quickly find your Bootstrap classes on this interactive Bootstrap cheat sheet. It includes code samples and live preview of elements.

<hr>


## circuit-breaker 

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>


## css 

**[Angular Progressbar](https://murhafsousli.github.io/ngx-progressbar/#/)**

  * **tags**: &nbsp; [css](https://www.codingmarks.org/search?q=[css]), &nbsp; [user-experience](https://www.codingmarks.org/search?q=[user-experience])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/murhafsousli/ngx-progressbar)

A nanoscopic progress/loading bar. Featuring realistic trickle animations to convince your users that something is happening!

<hr>


## devops 

**[Kubernetes + Compose = Kompose](http://kompose.io/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/kubernetes/kompose)

kompose is a tool to help users familiar with docker-compose move to Kubernetes. It takes a Docker Compose file and translates it into Kubernetes resources.


<hr>

**[GitHub - docker/docker-bench-security](https://github.com/docker/docker-bench-security)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [security](https://www.codingmarks.org/search?q=[security]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/docker/docker-bench-security)

The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production. - docker/docker-bench-security

<hr>

**[AWS re:Invent 2015: DevOps at Amazon: A Look at Our Tools and Processes (DVO202) - YouTube](https://www.youtube.com/watch?v=esEFaY0FDKc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-10-15
  * **tags**: &nbsp; [aws](https://www.codingmarks.org/search?q=[aws]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])

Learn about Amazon's transition to a service-oriented architecture over a decade ago. We share lessons learned, processes adopted, and the tools built to increase both agility and reliability. 

<hr>


## diagnostics 

**[Arthas Java Diagnostics Tool - home page ](https://alibaba.github.io/arthas/en/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [diagnostics](https://www.codingmarks.org/search?q=[diagnostics]), &nbsp; [performance](https://www.codingmarks.org/search?q=[performance])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/alibaba/arthas)

#### Key features

* Check whether a class is loaded, or where the class is being loaded. (Useful for troubleshooting jar file conflicts)
* Decompile a class to ensure the code is running as expected.
* View classloader statistics, e.g. the number of classloaders, the number of classes loaded per classloader, the classloader hierarchy, possible classloader leaks, etc.
* View the method invocation details, e.g. method parameter, return object, thrown exception, and etc.
* Check the stack trace of specified method invocation. This is useful when a developers wants to know the caller of the said method.
* Trace the method invocation to find slow sub-invocations.
* Monitor method invocation statistics, e.g. qps, rt, success rate and etc.
* Monitor system metrics, thread states and cpu usage, gc statistics, and etc.
* Supports command line interactive mode, with auto-complete feature enabled.
* Supports telnet and websocket, which enables both local and remote diagnostics with command line and browsers.
* Supports JDK 6+.
* Supports Linux/Mac/Windows.

<hr>


## diagram 

**[mermaid docs](https://mermaidjs.github.io/)**

  * **tags**: &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [diagram](https://www.codingmarks.org/search?q=[diagram])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/knsv/mermaid)

Generation of diagrams and flowcharts from text in a similar manner as markdown.

Ever wanted to simplify documentation and avoid heavy tools like Visio when explaining your code?

This is why mermaid was born, a simple markdown-like script language for generating charts from text via javascript.

<hr>


## dns 

**[MX Lookup Tool - Check your DNS MX Records online - MxToolbox](https://mxtoolbox.com/)**

  * **tags**: &nbsp; [dns](https://www.codingmarks.org/search?q=[dns]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

MxToolbox supports global Internet operations by providing free, fast and accurate network diagnostic and lookup tools. Millions of technology professionals use our tools to help diagnose and resolve a wide range of infrastructure issues.

<hr>


## docker 

**[Kubernetes + Compose = Kompose](http://kompose.io/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/kubernetes/kompose)

kompose is a tool to help users familiar with docker-compose move to Kubernetes. It takes a Docker Compose file and translates it into Kubernetes resources.


<hr>

**[Manage data in Docker](https://docs.docker.com/storage/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

Overview of persisting data in containers

Types of mounts and where they live on the Docker host

* **Volumes** are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
* **Bind mounts** may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.
* **tmpfs** mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

<hr>

**[GitHub - dockersamples/example-voting-app](https://github.com/dockersamples/example-voting-app)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/dockersamples/example-voting-app)

Example Docker Compose app. A simple distributed application running across multiple Docker containers.

<hr>

**[GitHub - docker/docker-bench-security](https://github.com/docker/docker-bench-security)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [security](https://www.codingmarks.org/search?q=[security]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/docker/docker-bench-security)

The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production. - docker/docker-bench-security

<hr>

**[GitHub - krallin/tini](https://github.com/krallin/tini)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux]), &nbsp; [c](https://www.codingmarks.org/search?q=[c])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/krallin/tini)

A tiny but valid `init` for containers. 

<hr>

**[jib home page](https://github.com/GoogleContainerTools/jib)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [gradle](https://www.codingmarks.org/search?q=[gradle])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/GoogleContainerTools/jib)

:sailboat: Build container images for your Java applications. - GoogleContainerTools/jib

## Quickstart

* **Maven** - See the jib-maven-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart).

* **Gradle** - See the jib-gradle-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart).

* **Jib Core** - See the Jib Core [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-core#adding-jib-core-to-your-build).

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>

**[Testcontainers home page](https://www.testcontainers.org/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/testcontainers/testcontainers-java/)

Testcontainers is a Java 8 library that supports JUnit tests, providing lightweight, throwaway instances of common databases, Selenium web browsers, or anything else that can run in a Docker container.

<hr>


## docker-compose 

**[Kubernetes + Compose = Kompose](http://kompose.io/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/kubernetes/kompose)

kompose is a tool to help users familiar with docker-compose move to Kubernetes. It takes a Docker Compose file and translates it into Kubernetes resources.


<hr>

**[GitHub - dockersamples/example-voting-app](https://github.com/dockersamples/example-voting-app)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/dockersamples/example-voting-app)

Example Docker Compose app. A simple distributed application running across multiple Docker containers.

<hr>


## docs 

**[Manage data in Docker](https://docs.docker.com/storage/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docs](https://www.codingmarks.org/search?q=[docs])

Overview of persisting data in containers

Types of mounts and where they live on the Docker host

* **Volumes** are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
* **Bind mounts** may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.
* **tmpfs** mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

<hr>


## documentation 

**[mermaid docs](https://mermaidjs.github.io/)**

  * **tags**: &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [diagram](https://www.codingmarks.org/search?q=[diagram])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/knsv/mermaid)

Generation of diagrams and flowcharts from text in a similar manner as markdown.

Ever wanted to simplify documentation and avoid heavy tools like Visio when explaining your code?

This is why mermaid was born, a simple markdown-like script language for generating charts from text via javascript.

<hr>

**[Graphviz - Graph Visualization Software](https://graphviz.org/)**

  * **tags**: &nbsp; [graph](https://www.codingmarks.org/search?q=[graph]), &nbsp; [visualization](https://www.codingmarks.org/search?q=[visualization]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://gitlab.com/graphviz/graphviz/)

Graphviz is open source graph visualization software. It has several main layout programs. See the gallery for sample layouts. It also has web and interactive graphical interfaces, and auxiliary tools, libraries, and language bindings. We're not able to put a lot of work into GUI editors but there are quite a few external projects and even commercial tools that incorporate Graphviz. You can find some of these in the Resources section.

<hr>

**[Dredd — HTTP API Testing Framework — Dredd latest documentation](https://dredd.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/dredd)

Dredd is a language-agnostic command-line tool for validating API description document against backend implementation of the API.

Dredd reads your API description and step by step validates whether your API implementation replies with responses as they are described in the documentation.

<hr>

**[API Blueprint home page](https://apiblueprint.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/api-blueprint/)

API Blueprint is simple and accessible to everybody involved in the API lifecycle. Its syntax is concise yet expressive. With API Blueprint you can quickly design and prototype APIs to be created or document and test already deployed mission-critical APIs.

<hr>


## domain-driven-design 

**[BoundedContext](https://martinfowler.com/bliki/BoundedContext.html)**

  * **tags**: &nbsp; [domain-driven-design](https://www.codingmarks.org/search?q=[domain-driven-design]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Bounded Context is a central pattern in Domain-Driven Design. It is the focus of DDD's strategic design section which is all about dealing with large models and teams. DDD deals with large models by dividing them into different Bounded Contexts and being explicit about their interrelationships.

<hr>

**[Strategic Domain Driven Design with Context Mapping](https://www.infoq.com/articles/ddd-contextmapping)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2009-11-25
  * **tags**: &nbsp; [domain-driven-design](https://www.codingmarks.org/search?q=[domain-driven-design]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])

Many approaches to object oriented modeling tend not to scale well when the applications grow in size and complexity. **Context Mapping** technique can be used to manage the complexity in large software development projects. In this article, author Alberto Brandolini discusses the many sides of **bounded contexts** and how to use them to build a context map to support key decisions in a software project.

"_Code is the primary form of expression of the model. Although other artifacts might be necessary along the way to capture requirements or portions of the design, the only one that'll be constantly in sync with the application behavior is code itself._"

"_Trying to set up a partnership without a collaborative environment, is clearly a dead end strategy._"

<hr>


## dotfiles 

**[Vim dotfiles and configurations example](https://github.com/jessfraz/.vim)**

  * **tags**: &nbsp; [vim](https://www.codingmarks.org/search?q=[vim]), &nbsp; [dotfiles](https://www.codingmarks.org/search?q=[dotfiles])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jessfraz/.vim)

My .vim dotfiles and configurations. Contribute to jessfraz/.vim

<hr>

**[Nice dotfiles example](https://github.com/jessfraz/dotfiles)**

  * **tags**: &nbsp; [dotfiles](https://www.codingmarks.org/search?q=[dotfiles]), &nbsp; [alias](https://www.codingmarks.org/search?q=[alias]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jessfraz/dotfiles)

My dotfiles. Buyer beware ;)

<hr>


## email 

**[Nodemailer home page](http://nodemailer.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [email](https://www.codingmarks.org/search?q=[email])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/nodemailer/nodemailer)

Nodemailer is a module for Node.js to send emails

<hr>


## error-handling 

**[Sentry home page ](https://sentry.io/)**

  * **tags**: &nbsp; [error-handling](https://www.codingmarks.org/search?q=[error-handling]), &nbsp; [monitoring](https://www.codingmarks.org/search?q=[monitoring])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/getsentry/sentry)

Open-source error tracking that helps developers monitor and fix crashes in real time. Iterate continuously. Boost workflow efficiency. Improve user experience. 

Error Tracking Software — JavaScript, Python, PHP, Ruby, more...

<hr>


## facebook 

**[La Gazzetta dello Sport I News su Calcio, Basket, NBA, F1 e MotoGp](https://www.gazzetta.it/)**

  * **tags**: &nbsp; [facebook](https://www.codingmarks.org/search?q=[facebook])

Leggi La Gazzetta dello Sport: news, foto, video e risultati su calcio di serie A, calciomercato, basket, F1, motoGp, ciclismo e tennis.\n
<b>BOLD</b>

<hr>


## favicon 

**[X-Icon Editor](http://www.xiconeditor.com/)**

  * **tags**: &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

X-Icon Editor, create high resolution icons from your own browser

<hr>

**[GitHub - audreyr/favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [web](https://www.codingmarks.org/search?q=[web]), &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/audreyr/favicon-cheat-sheet)

Obsessive cheat sheet to favicon sizes/types. Please contribute! (Note: this may be in flux as I learn new things about favicon best practices.) - audreyr/favicon-cheat-sheet

<hr>


## ffmpeg 

**[High Quality Gifs with FFMPEG ](https://medium.com/@colten_jackson/doing-the-gif-thing-on-debian-82b9760a8483)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-10-28
  * **tags**: &nbsp; [gif](https://www.codingmarks.org/search?q=[gif]), &nbsp; [ffmpeg](https://www.codingmarks.org/search?q=[ffmpeg]), &nbsp; [video-processing](https://www.codingmarks.org/search?q=[video-processing])

After getting FFMPEG installed, let’s try it out on a MOV downloaded from my google photos account:
```
ffmpeg -i MVI_6654.MOV firsttry.gif
```
We’re calling the ffmpeg program and telling it that MVI_6654.MOV is our input file with the -i flag. the filename at the end defines the conversion and creates the new file

<hr>

**[
FFmpeg](https://www.ffmpeg.org/)**

  * **tags**: &nbsp; [ffmpeg](https://www.codingmarks.org/search?q=[ffmpeg]), &nbsp; [video-processing](https://www.codingmarks.org/search?q=[video-processing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://www.ffmpeg.org/download.html#get-sources)

A complete, cross-platform solution to record, convert and stream audio and video.


<hr>


## free-programming-books 

**[Dive Into HTML5](https://diveintohtml5.info/)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [html5](https://www.codingmarks.org/search?q=[html5]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Dive Into HTML5 elaborates on a hand-picked selection of features from the HTML5 specification and other fine standards.

<hr>

**[rx-book RxJS - Javascript library for functional reactive programming.](http://xgrommx.github.io/rx-book)**

  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators.

<hr>


## functional-programming 

**[GitHub - stoeffel/awesome-fp-js](https://github.com/stoeffel/awesome-fp-js#readme)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/stoeffel/awesome-fp-js)

:sunglasses: A curated list of awesome functional programming stuff in js - stoeffel/awesome-fp-js

<hr>

**[Simplify your JavaScript – Use .map(), .reduce(), and .filter()](https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-01-29
  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

Simplify the way you write your JavaScript by using .map(), .reduce() and .filter() instead of for() and forEach() loops. You’ll end up with clearer, less clunky code!

<hr>


## gif 

**[High Quality Gifs with FFMPEG ](https://medium.com/@colten_jackson/doing-the-gif-thing-on-debian-82b9760a8483)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-10-28
  * **tags**: &nbsp; [gif](https://www.codingmarks.org/search?q=[gif]), &nbsp; [ffmpeg](https://www.codingmarks.org/search?q=[ffmpeg]), &nbsp; [video-processing](https://www.codingmarks.org/search?q=[video-processing])

After getting FFMPEG installed, let’s try it out on a MOV downloaded from my google photos account:
```
ffmpeg -i MVI_6654.MOV firsttry.gif
```
We’re calling the ffmpeg program and telling it that MVI_6654.MOV is our input file with the -i flag. the filename at the end defines the conversion and creates the new file

<hr>


## git 

**[GitHub - arslanbilal/git-cheat-sheet: git and git flow cheat sheet](https://github.com/arslanbilal/git-cheat-sheet#readme)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arslanbilal/git-cheat-sheet#readme)

##### Git Cheat Sheet English
* [Set Up](https://github.com/arslanbilal/git-cheat-sheet#setup)
* [Configuration Files](https://github.com/arslanbilal/git-cheat-sheet#configuration-files)
* [Create](https://github.com/arslanbilal/git-cheat-sheet#create)
* [Local Changes](https://github.com/arslanbilal/git-cheat-sheet#local-changes)
* [Search](https://github.com/arslanbilal/git-cheat-sheet#search)
* [Commit History](https://github.com/arslanbilal/git-cheat-sheet#commit-history)
* [Branches & Tags](https://github.com/arslanbilal/git-cheat-sheet#branches--tags)
* [Update & Publish](https://github.com/arslanbilal/git-cheat-sheet#update--publish)
* [Merge & Rebase](https://github.com/arslanbilal/git-cheat-sheet#merge--rebase)
* [Undo](https://github.com/arslanbilal/git-cheat-sheet#undo)
* [Git Flow](https://github.com/arslanbilal/git-cheat-sheet#git-flow)

<hr>

**[Install Bash git completion](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [osx](https://www.codingmarks.org/search?q=[osx]), &nbsp; [windows](https://www.codingmarks.org/search?q=[windows]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

How to install git completion on different platforms...

<hr>


## gradle 

**[jib home page](https://github.com/GoogleContainerTools/jib)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [gradle](https://www.codingmarks.org/search?q=[gradle])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/GoogleContainerTools/jib)

:sailboat: Build container images for your Java applications. - GoogleContainerTools/jib

## Quickstart

* **Maven** - See the jib-maven-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart).

* **Gradle** - See the jib-gradle-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart).

* **Jib Core** - See the Jib Core [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-core#adding-jib-core-to-your-build).

<hr>


## graph 

**[Graphviz - Graph Visualization Software](https://graphviz.org/)**

  * **tags**: &nbsp; [graph](https://www.codingmarks.org/search?q=[graph]), &nbsp; [visualization](https://www.codingmarks.org/search?q=[visualization]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://gitlab.com/graphviz/graphviz/)

Graphviz is open source graph visualization software. It has several main layout programs. See the gallery for sample layouts. It also has web and interactive graphical interfaces, and auxiliary tools, libraries, and language bindings. We're not able to put a lot of work into GUI editors but there are quite a few external projects and even commercial tools that incorporate Graphviz. You can find some of these in the Resources section.

<hr>


## helm 

**[Helm Docs home page](https://helm.sh/)**

  * **tags**: &nbsp; [helm](https://www.codingmarks.org/search?q=[helm]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/helm/helm)

Helm - The Kubernetes Package Manager.

Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

Charts are easy to create, version, share, and publish — so start using Helm and stop the copy-and-paste.

<hr>


## homebrew 

**[GitHub - Homebrew/homebrew-cask home page](https://github.com/Homebrew/homebrew-cask)**

  * **tags**: &nbsp; [homebrew](https://www.codingmarks.org/search?q=[homebrew]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Homebrew/homebrew-cask)

🍻 A CLI workflow for the administration of macOS applications distributed as binaries - Homebrew/homebrew-cask

<hr>


## html 

**[GitHub - audreyr/favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [web](https://www.codingmarks.org/search?q=[web]), &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/audreyr/favicon-cheat-sheet)

Obsessive cheat sheet to favicon sizes/types. Please contribute! (Note: this may be in flux as I learn new things about favicon best practices.) - audreyr/favicon-cheat-sheet

<hr>

**[Dive Into HTML5](https://diveintohtml5.info/)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [html5](https://www.codingmarks.org/search?q=[html5]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Dive Into HTML5 elaborates on a hand-picked selection of features from the HTML5 specification and other fine standards.

<hr>


## html5 

**[Dive Into HTML5](https://diveintohtml5.info/)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [html5](https://www.codingmarks.org/search?q=[html5]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Dive Into HTML5 elaborates on a hand-picked selection of features from the HTML5 specification and other fine standards.

<hr>


## immutable.js 

**[Immutable.js home page](https://immutable-js.github.io/immutable-js/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [immutable.js](https://www.codingmarks.org/search?q=[immutable.js])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/immutable-js/immutable-js)

[Immutable][] data cannot be changed once created, leading to much simpler
application development, no defensive copying, and enabling advanced memoization
and change detection techniques with simple logic. [Persistent][] data presents
a mutative API which does not update the data in-place, but instead always
yields new updated data.

Immutable.js provides many Persistent Immutable data structures including:
`List`, `Stack`, `Map`, `OrderedMap`, `Set`, `OrderedSet` and `Record`.

These data structures are highly efficient on modern JavaScript VMs by using
structural sharing via [hash maps tries][] and [vector tries][] as popularized
by Clojure and Scala, minimizing the need to copy or cache data.

Immutable.js also provides a lazy `Seq`, allowing efficient
chaining of collection methods like `map` and `filter` without creating
intermediate representations. Create some `Seq` with `Range` and `Repeat`.

Want to hear more? Watch the presentation about Immutable.js:

<a href="https://youtu.be/I7IdS-PbEgI" target="_blank" alt="Immutable Data and React"><img src="https://img.youtube.com/vi/I7IdS-PbEgI/0.jpg" /></a>

[Persistent]: http://en.wikipedia.org/wiki/Persistent_data_structure
[Immutable]: http://en.wikipedia.org/wiki/Immutable_object
[hash maps tries]: http://en.wikipedia.org/wiki/Hash_array_mapped_trie
[vector tries]: http://hypirion.com/musings/understanding-persistent-vector-pt-1

<hr>


## install 

**[macos - Mac OS X and multiple Java versions - Stack Overflow](https://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos]), &nbsp; [install](https://www.codingmarks.org/search?q=[install])
<hr>


## integration-testing 

**[Arquillian  Home Page](http://arquillian.org/)**

  * **tags**: &nbsp; [arquillian](https://www.codingmarks.org/search?q=[arquillian]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arquillian)

An innovative testing platform for the Java Virtual Machine (JVM). Open source. Highly extensible.

<hr>


## istio 

**[Making microservices micro with Istio Service Mesh & Kubernetes by Ray Tsang @ Spring I/O 2018 - YouTube](https://www.youtube.com/watch?v=s31kdh7Q7Hc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

<hr>

**[A Tale of Two Frameworks: Spring Cloud and Istio - YouTube](https://www.youtube.com/watch?v=AMJQO9zs2eo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [spring-cloud](https://www.codingmarks.org/search?q=[spring-cloud]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

Istio or Spring Cloud (or both?)

If its part of the platform, use it. Except for...
  * Fallbacks
  * Tracing Propagation
  * Security

Istio or a service mesh architecture works better for polyglot environments 

<hr>

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>


## java 

**[Java 8: Definitive guide to CompletableFuture](https://www.nurkiewicz.com/2013/05/java-8-definitive-guide-to.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-05-09
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [asynchronous](https://www.codingmarks.org/search?q=[asynchronous]), &nbsp; [async](https://www.codingmarks.org/search?q=[async])

Examples of using and combining `CompletableFuture` API

<hr>

**[Arthas Java Diagnostics Tool - home page ](https://alibaba.github.io/arthas/en/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [diagnostics](https://www.codingmarks.org/search?q=[diagnostics]), &nbsp; [performance](https://www.codingmarks.org/search?q=[performance])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/alibaba/arthas)

#### Key features

* Check whether a class is loaded, or where the class is being loaded. (Useful for troubleshooting jar file conflicts)
* Decompile a class to ensure the code is running as expected.
* View classloader statistics, e.g. the number of classloaders, the number of classes loaded per classloader, the classloader hierarchy, possible classloader leaks, etc.
* View the method invocation details, e.g. method parameter, return object, thrown exception, and etc.
* Check the stack trace of specified method invocation. This is useful when a developers wants to know the caller of the said method.
* Trace the method invocation to find slow sub-invocations.
* Monitor method invocation statistics, e.g. qps, rt, success rate and etc.
* Monitor system metrics, thread states and cpu usage, gc statistics, and etc.
* Supports command line interactive mode, with auto-complete feature enabled.
* Supports telnet and websocket, which enables both local and remote diagnostics with command line and browsers.
* Supports JDK 6+.
* Supports Linux/Mac/Windows.

<hr>

**[macos - Mac OS X and multiple Java versions - Stack Overflow](https://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos]), &nbsp; [install](https://www.codingmarks.org/search?q=[install])
<hr>

**[jEnv - Manage your Java environment](http://www.jenv.be/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jenv/jenv)

Discover jenv, the command line Java manager

<hr>

**[Oliver Gierke - Whoops! Where did my architecture go](http://olivergierke.de/2013/01/whoops-where-did-my-architecture-go/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2013-01-01
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [architecture](https://www.codingmarks.org/search?q=[architecture])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/odrotbohm/whoops-architecture)

#### Summary

The basic approach I recommend is to move the vertical slices into the focus of the package naming and try to model them in a way that the public API of a slice is as tiny as possible in the first place. This is of course no silver bullet approach as packages can grow significantly and it might make sense to extract certain types into a sub-package or the like which then usually leads you to the need to use an architecture management tool. The core idea here is to try to use the means of visibility control that are available in Java to write code that is not a giant potential dependency mess in the first place. Packages can actually help you to achieve exactly that.

<hr>

**[useful-java-links](https://github.com/Vedenin/useful-java-links#readme)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [resources](https://www.codingmarks.org/search?q=[resources]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Vedenin/useful-java-links)

A list of useful Java frameworks, libraries, software and hello worlds examples - Vedenin/useful-java-links

<hr>

**[jib home page](https://github.com/GoogleContainerTools/jib)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [gradle](https://www.codingmarks.org/search?q=[gradle])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/GoogleContainerTools/jib)

:sailboat: Build container images for your Java applications. - GoogleContainerTools/jib

## Quickstart

* **Maven** - See the jib-maven-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart).

* **Gradle** - See the jib-gradle-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart).

* **Jib Core** - See the Jib Core [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-core#adding-jib-core-to-your-build).

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>

**[Testcontainers home page](https://www.testcontainers.org/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/testcontainers/testcontainers-java/)

Testcontainers is a Java 8 library that supports JUnit tests, providing lightweight, throwaway instances of common databases, Selenium web browsers, or anything else that can run in a Docker container.

<hr>

**[JUnit 5 Articles and source code from Baeldung](https://github.com/eugenp/tutorials/tree/master/testing-modules/junit-5)**

  * **tags**: &nbsp; [junit](https://www.codingmarks.org/search?q=[junit]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/testing-modules/junit-5)

- [The Basics of JUnit 5 – A Preview](http://www.baeldung.com/junit-5-preview)
- [A Guide to JUnit 5](http://www.baeldung.com/junit-5)
- [A Guide to @RepeatedTest in Junit 5](http://www.baeldung.com/junit-5-repeated-test)
- [Guide to Dynamic Tests in Junit 5](http://www.baeldung.com/junit5-dynamic-tests)
- [A Guide to JUnit 5 Extensions](http://www.baeldung.com/junit-5-extensions)
- [Mockito and JUnit 5 – Using ExtendWith](http://www.baeldung.com/mockito-junit-5-extension)
- [JUnit 5 – @RunWith](http://www.baeldung.com/junit-5-runwith)
- [JUnit 5 @Test Annotation](http://www.baeldung.com/junit-5-test-annotation)
- [Assert an Exception is Thrown in JUnit 4 and 5](http://www.baeldung.com/junit-assert-exception)
- [@Before vs @BeforeClass vs @BeforeEach vs @BeforeAll](http://www.baeldung.com/junit-before-beforeclass-beforeeach-beforeall)
- [Migrating from JUnit 4 to JUnit 5](http://www.baeldung.com/junit-5-migration)
- [The Order of Tests in JUnit](http://www.baeldung.com/junit-5-test-order)
- [Running JUnit Tests Programmatically, from a Java Application](https://www.baeldung.com/junit-tests-run-programmatically-from-java)
- [Testing an Abstract Class With JUnit](https://www.baeldung.com/junit-test-abstract-class)
- [Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)
- [JUnit 5 Conditional Test Execution with Annotations](https://www.baeldung.com/junit-5-conditional-test-execution)

<hr>

**[Reactor vs. RxJava](http://helmbold.de/artikel/reactor-vs-rxjava/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-06-05
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive])

Was spricht für und gegen Reactor und RxJava?

<hr>


## javascript 

**[Immutable.js home page](https://immutable-js.github.io/immutable-js/)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [immutable.js](https://www.codingmarks.org/search?q=[immutable.js])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/immutable-js/immutable-js)

[Immutable][] data cannot be changed once created, leading to much simpler
application development, no defensive copying, and enabling advanced memoization
and change detection techniques with simple logic. [Persistent][] data presents
a mutative API which does not update the data in-place, but instead always
yields new updated data.

Immutable.js provides many Persistent Immutable data structures including:
`List`, `Stack`, `Map`, `OrderedMap`, `Set`, `OrderedSet` and `Record`.

These data structures are highly efficient on modern JavaScript VMs by using
structural sharing via [hash maps tries][] and [vector tries][] as popularized
by Clojure and Scala, minimizing the need to copy or cache data.

Immutable.js also provides a lazy `Seq`, allowing efficient
chaining of collection methods like `map` and `filter` without creating
intermediate representations. Create some `Seq` with `Range` and `Repeat`.

Want to hear more? Watch the presentation about Immutable.js:

<a href="https://youtu.be/I7IdS-PbEgI" target="_blank" alt="Immutable Data and React"><img src="https://img.youtube.com/vi/I7IdS-PbEgI/0.jpg" /></a>

[Persistent]: http://en.wikipedia.org/wiki/Persistent_data_structure
[Immutable]: http://en.wikipedia.org/wiki/Immutable_object
[hash maps tries]: http://en.wikipedia.org/wiki/Hash_array_mapped_trie
[vector tries]: http://hypirion.com/musings/understanding-persistent-vector-pt-1

<hr>

**[Jake Archibald: In The Loop - JSConf.Asia 2018 - YouTube](https://www.youtube.com/watch?v=cCOL7MC4Pl0)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-01-27
  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript])

Have you ever had a bug where things were happening in the wrong order, or particular style changes were being ignored? Ever fixed that bug by wrapping a section of code in a setTimeout? Ever found that fix to be unreliable, and played around with the timeout number until it kinda almost always worked? 
This talk looks at the browser's event loop, the thing that orchestrates the main thread of the browser, which includes JavaScript, events, and rendering. We'll look at the difference between tasks, microtasks, requestAnimationFrame, requestIdleCallback, and where events land. 
Hopefully you'll never have to use setTimeout hacks again!"

Jake is developer advocate for Google Chrome. He's one of the editors of the service worker spec, so he's into offline-first, push messaging and web performance.

JSConf.Asia - Capitol Theatre, Singapore - 27 January 2018

<hr>

**[GitHub - stoeffel/awesome-fp-js](https://github.com/stoeffel/awesome-fp-js#readme)**

  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/stoeffel/awesome-fp-js)

:sunglasses: A curated list of awesome functional programming stuff in js - stoeffel/awesome-fp-js

<hr>

**[The Cost Of JavaScript In 2018 – Addy Osmani – Medium](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-08-01
  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [performance](https://www.codingmarks.org/search?q=[performance])

[Youtube video](https://www.youtube.com/watch?v=63I-mEuSvGA)

Building interactive sites can involve sending JavaScript to your users. Often, too much of it. Have you been on a mobile page that looked like it had loaded only to tap on a link or tried to scroll and nothing happens?

**Byte-for-byte, JavaScript is still the most expensive resource we send to mobile phones, because it can delay interactivity in large ways.**

<hr>

**[Simplify your JavaScript – Use .map(), .reduce(), and .filter()](https://medium.com/poka-techblog/simplify-your-javascript-use-map-reduce-and-filter-bd02c593cc2d)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-01-29
  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [functional-programming](https://www.codingmarks.org/search?q=[functional-programming])

Simplify the way you write your JavaScript by using .map(), .reduce() and .filter() instead of for() and forEach() loops. You’ll end up with clearer, less clunky code!

<hr>

**[mermaid docs](https://mermaidjs.github.io/)**

  * **tags**: &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [diagram](https://www.codingmarks.org/search?q=[diagram])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/knsv/mermaid)

Generation of diagrams and flowcharts from text in a similar manner as markdown.

Ever wanted to simplify documentation and avoid heavy tools like Visio when explaining your code?

This is why mermaid was born, a simple markdown-like script language for generating charts from text via javascript.

<hr>


## jenv 

**[macos - Mac OS X and multiple Java versions - Stack Overflow](https://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos]), &nbsp; [install](https://www.codingmarks.org/search?q=[install])
<hr>

**[jEnv - Manage your Java environment](http://www.jenv.be/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jenv/jenv)

Discover jenv, the command line Java manager

<hr>


## junit 

**[JUnit 5 Articles and source code from Baeldung](https://github.com/eugenp/tutorials/tree/master/testing-modules/junit-5)**

  * **tags**: &nbsp; [junit](https://www.codingmarks.org/search?q=[junit]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/testing-modules/junit-5)

- [The Basics of JUnit 5 – A Preview](http://www.baeldung.com/junit-5-preview)
- [A Guide to JUnit 5](http://www.baeldung.com/junit-5)
- [A Guide to @RepeatedTest in Junit 5](http://www.baeldung.com/junit-5-repeated-test)
- [Guide to Dynamic Tests in Junit 5](http://www.baeldung.com/junit5-dynamic-tests)
- [A Guide to JUnit 5 Extensions](http://www.baeldung.com/junit-5-extensions)
- [Mockito and JUnit 5 – Using ExtendWith](http://www.baeldung.com/mockito-junit-5-extension)
- [JUnit 5 – @RunWith](http://www.baeldung.com/junit-5-runwith)
- [JUnit 5 @Test Annotation](http://www.baeldung.com/junit-5-test-annotation)
- [Assert an Exception is Thrown in JUnit 4 and 5](http://www.baeldung.com/junit-assert-exception)
- [@Before vs @BeforeClass vs @BeforeEach vs @BeforeAll](http://www.baeldung.com/junit-before-beforeclass-beforeeach-beforeall)
- [Migrating from JUnit 4 to JUnit 5](http://www.baeldung.com/junit-5-migration)
- [The Order of Tests in JUnit](http://www.baeldung.com/junit-5-test-order)
- [Running JUnit Tests Programmatically, from a Java Application](https://www.baeldung.com/junit-tests-run-programmatically-from-java)
- [Testing an Abstract Class With JUnit](https://www.baeldung.com/junit-test-abstract-class)
- [Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)
- [JUnit 5 Conditional Test Execution with Annotations](https://www.baeldung.com/junit-5-conditional-test-execution)

<hr>


## jvm 

**[Project Reactor Home Page](https://projectreactor.io/)**

  * **tags**: &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [jvm](https://www.codingmarks.org/search?q=[jvm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/reactor)

Reactor is a fourth-generation Reactive library for building non-blocking applications on
the JVM based on the [Reactive Streams Specification](https://github.com/reactive-streams/reactive-streams-jvm)

<hr>


## kotlin 

**[Reactive Spring - Josh Long, Mark Heckler - YouTube](https://www.youtube.com/watch?v=l7VBdWhtl7A)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-14
  * **tags**: &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [kotlin](https://www.codingmarks.org/search?q=[kotlin]), &nbsp; [redis](https://www.codingmarks.org/search?q=[redis]), &nbsp; [spring-cloud-gateway](https://www.codingmarks.org/search?q=[spring-cloud-gateway])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshlong/flux-flix-service)

Spring Framework 5.0 is here! One of the most exciting introductions in this release is support for reactive programming, building on the Pivotal Reactor project to support message-driven, elastic, resilient and responsive services. Spring Framework 5 integrates an MVC-like component model adapted to support reactive processing and a new type of web endpoint, functional reactive endpoints. In this talk, we'll look at the net-new Netty-based web runtime, how existing Servlet code can run on the new world, and how to integrate it with existing Spring-stack technologies.

<hr>


## kubernetes 

**[Kubernetes + Compose = Kompose](http://kompose.io/)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [docker-compose](https://www.codingmarks.org/search?q=[docker-compose]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/kubernetes/kompose)

kompose is a tool to help users familiar with docker-compose move to Kubernetes. It takes a Docker Compose file and translates it into Kubernetes resources.


<hr>

**[Helm Docs home page](https://helm.sh/)**

  * **tags**: &nbsp; [helm](https://www.codingmarks.org/search?q=[helm]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/helm/helm)

Helm - The Kubernetes Package Manager.

Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

Charts are easy to create, version, share, and publish — so start using Helm and stop the copy-and-paste.

<hr>

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>


## kubernetes-helm 

**[Helm Docs home page](https://helm.sh/)**

  * **tags**: &nbsp; [helm](https://www.codingmarks.org/search?q=[helm]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/helm/helm)

Helm - The Kubernetes Package Manager.

Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

Charts are easy to create, version, share, and publish — so start using Helm and stop the copy-and-paste.

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>


## latex 

**[Pandoc home page](https://pandoc.org/)**

  * **tags**: &nbsp; [pandoc](https://www.codingmarks.org/search?q=[pandoc]), &nbsp; [markdown](https://www.codingmarks.org/search?q=[markdown]), &nbsp; [latex](https://www.codingmarks.org/search?q=[latex])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jgm/pandoc)

Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library. 

<hr>


## linux 

**[GitHub - krallin/tini](https://github.com/krallin/tini)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux]), &nbsp; [c](https://www.codingmarks.org/search?q=[c])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/krallin/tini)

A tiny but valid `init` for containers. 

<hr>

**[command line - How to list all symbolic links in a directory - Ask Ubuntu](https://askubuntu.com/questions/522051/how-to-list-all-symbolic-links-in-a-directory)**

  * **tags**: &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [shell](https://www.codingmarks.org/search?q=[shell]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

```
find . -type l -ls
```

To only process the current directory:
```
find . -maxdepth 1 -type l -ls
```

<hr>

**[Install Bash git completion](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [osx](https://www.codingmarks.org/search?q=[osx]), &nbsp; [windows](https://www.codingmarks.org/search?q=[windows]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

How to install git completion on different platforms...

<hr>


## macos 

**[GitHub - Homebrew/homebrew-cask home page](https://github.com/Homebrew/homebrew-cask)**

  * **tags**: &nbsp; [homebrew](https://www.codingmarks.org/search?q=[homebrew]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Homebrew/homebrew-cask)

🍻 A CLI workflow for the administration of macOS applications distributed as binaries - Homebrew/homebrew-cask

<hr>

**[macos - Mac OS X and multiple Java versions - Stack Overflow](https://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [jenv](https://www.codingmarks.org/search?q=[jenv]), &nbsp; [macos](https://www.codingmarks.org/search?q=[macos]), &nbsp; [install](https://www.codingmarks.org/search?q=[install])
<hr>


## markdown 

**[Pandoc home page](https://pandoc.org/)**

  * **tags**: &nbsp; [pandoc](https://www.codingmarks.org/search?q=[pandoc]), &nbsp; [markdown](https://www.codingmarks.org/search?q=[markdown]), &nbsp; [latex](https://www.codingmarks.org/search?q=[latex])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jgm/pandoc)

Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library. 

<hr>


## maven-plugin 

**[jib home page](https://github.com/GoogleContainerTools/jib)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [gradle](https://www.codingmarks.org/search?q=[gradle])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/GoogleContainerTools/jib)

:sailboat: Build container images for your Java applications. - GoogleContainerTools/jib

## Quickstart

* **Maven** - See the jib-maven-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart).

* **Gradle** - See the jib-gradle-plugin [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart).

* **Jib Core** - See the Jib Core [Quickstart](https://github.com/GoogleContainerTools/jib/tree/master/jib-core#adding-jib-core-to-your-build).

<hr>

**[kubernetes-for-java-developers repo from Aron Gupta](https://github.com/aws-samples/kubernetes-for-java-developers)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes]), &nbsp; [kubernetes-helm](https://www.codingmarks.org/search?q=[kubernetes-helm]), &nbsp; [maven-plugin](https://www.codingmarks.org/search?q=[maven-plugin]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [aws](https://www.codingmarks.org/search?q=[aws])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/aws-samples/kubernetes-for-java-developers)

A Day in Java Developer’s Life, with a taste of Kubernetes - aws-samples/kubernetes-for-java-developers. sample. example.

<hr>


## microservices 

**[Making microservices micro with Istio Service Mesh & Kubernetes by Ray Tsang @ Spring I/O 2018 - YouTube](https://www.youtube.com/watch?v=s31kdh7Q7Hc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

<hr>

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>

**[API Infrastructure at Knewton: What’s in an Edge Service?](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-05-09
  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx]), &nbsp; [zuul](https://www.codingmarks.org/search?q=[zuul])

In this post, we will pull back the covers of our API to explain how we handle user requests. You will first learn how to build an edge service with Netflix Zuul, the framework we chose for its simplicity and flexibility. Then, we’ll dive into the Knewton edge service to show you how it improves API simplicity, flexibility, and performance.

What’s in an Edge Service
-------
An edge service is a component which is exposed to the public internet. It acts as a gateway to all other services, which we will refer to as platform services. For example, consider an Nginx reverse proxy in front of some web resource servers. Here, Nginx acts as an edge service by routing public HTTP requests to the appropriate platform service.

<hr>


## middleware 

**[Connect home page](https://github.com/senchalabs/connect)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [middleware](https://www.codingmarks.org/search?q=[middleware])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/senchalabs/connect)

Connect is a middleware layer for Node.js. 

<hr>


## mocha 

**[Modern Node.js: async/await based testing with Mocha & Chai](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-04-09
  * **tags**: &nbsp; [mocha](https://www.codingmarks.org/search?q=[mocha]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)

Mocha is a JavaScript test framework running on Node.js and in the browser. It can run both asynchronous and synchronous code serially. Test cases are created using describe() and it() methods, the former is used to provide a structure by allowing to put various tests cases in logical groups while the latter is where the tests are written.

In order to perform actual tests, there is a need for an assertion library: a runtime mechanism which can be used to verify assumptions made by the program and print a diagnostic message if this assumption is false. Node.js comes with a built-in assert library. Chai is another popular assertion library that provides both the BDD and TDD styles of programming for testing the code. BDD stands for Behavior-driven development while TDD stands for Test-driven development. In a nutshell, Chai provides a should keyword for BDD style which allows to chain assertions and an expect() method for TDD style. Choosing between one and another is a matter of personal preference.

<hr>


## mongodb 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>

**[How do you rename a MongoDB database? - Stack Overflow](https://stackoverflow.com/questions/9201832/how-do-you-rename-a-mongodb-database)**

  * **tags**: &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb])

```
//copy the database
> db.copyDatabase("db_to_rename","db_renamed")

//drop the old database
> use db_to_rename
> db.dropDatabase();
```

<hr>


## monitoring 

**[Sentry home page ](https://sentry.io/)**

  * **tags**: &nbsp; [error-handling](https://www.codingmarks.org/search?q=[error-handling]), &nbsp; [monitoring](https://www.codingmarks.org/search?q=[monitoring])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/getsentry/sentry)

Open-source error tracking that helps developers monitor and fix crashes in real time. Iterate continuously. Boost workflow efficiency. Improve user experience. 

Error Tracking Software — JavaScript, Python, PHP, Ruby, more...

<hr>


## nginx 

**[Understanding Nginx Server and Location Block Selection Algorithms](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2014-11-17
  * **tags**: &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx])

Nginx is one of the most popular web servers in the world. In this guide, we will discuss how Nginx selects the server and location block that will handle a given client's request. We will go over the algorithm in place, as well as the directives and

<hr>

**[API Infrastructure at Knewton: What’s in an Edge Service?](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-05-09
  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx]), &nbsp; [zuul](https://www.codingmarks.org/search?q=[zuul])

In this post, we will pull back the covers of our API to explain how we handle user requests. You will first learn how to build an edge service with Netflix Zuul, the framework we chose for its simplicity and flexibility. Then, we’ll dive into the Knewton edge service to show you how it improves API simplicity, flexibility, and performance.

What’s in an Edge Service
-------
An edge service is a component which is exposed to the public internet. It acts as a gateway to all other services, which we will refer to as platform services. For example, consider an Nginx reverse proxy in front of some web resource servers. Here, Nginx acts as an edge service by routing public HTTP requests to the appropriate platform service.

<hr>


## nodejs 

**[Nodemailer home page](http://nodemailer.com/)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [email](https://www.codingmarks.org/search?q=[email])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/nodemailer/nodemailer)

Nodemailer is a module for Node.js to send emails

<hr>

**[Connect home page](https://github.com/senchalabs/connect)**

  * **tags**: &nbsp; [nodejs](https://www.codingmarks.org/search?q=[nodejs]), &nbsp; [middleware](https://www.codingmarks.org/search?q=[middleware])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/senchalabs/connect)

Connect is a middleware layer for Node.js. 

<hr>


## oidc 

**[Identity, Claims, & Tokens – An OpenID Connect Primer, Part 1 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-1)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-07-25
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [oidc](https://www.codingmarks.org/search?q=[oidc]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-oidc-flows-example)

In this post, we learned some basics about OpenID Connect, its history, and a bit about the various flow types, scopes, and tokens involved.

* [OIDC in Action – An OpenID Connect Primer, Part 2 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-2)
* [What's in a Token? – An OpenID Connect Primer, Part 3 of 3](https://developer.okta.com/blog/2017/08/01/oidc-primer-part-3)

<hr>


## online-tools 

**[Online SQLite browser](https://extendsclass.com/sqlite-browser.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2019-03-06
  * **tags**: &nbsp; [online-tools](https://www.codingmarks.org/search?q=[online-tools]), &nbsp; [sql](https://www.codingmarks.org/search?q=[sql])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/hautdefrance/Web-GUI-for-SQLite)

SQLite Browser is a online SQL interpreter for SQLite database. Open and execute queries from a SQLite file. Neither download nor installation.

<hr>


## open-source 

**[Contributor Covenant: A Code of Conduct for Open Source Projects](http://www.contributor-covenant.org/)**

  * **tags**: &nbsp; [open-source](https://www.codingmarks.org/search?q=[open-source])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/ContributorCovenant/contributor_covenant)

Open Source has always been a foundation of the Internet, and with the advent of social open source networks this is more true than ever. But free, libre, and open source projects suffer from a startling lack of diversity, with dramatically low representation by women, people of color, and other marginalized populations.

Often it is the unintentional assumptions and actions of project maintainers and participants that make open source projects unwelcoming (or even hostile) to marginalized people: making assumptions about gender or race, reinforcing stereotypes, using sexualized or otherwise inappropriate language, or demonstrating a lack of regard for the safety and well-being of vulnerable people.

One way to begin addressing this problem is to be overt in our openness, welcoming all people to contribute, and pledging in return to value them as whole human beings and to foster an atmosphere of kindness, cooperation, and understanding.

Adopting the Contributor Covenant can be one way to express and codify these values and signal your intention to make your open source community welcoming, diverse, and inclusive.

<hr>


## openid-connect 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>

**[Identity, Claims, & Tokens – An OpenID Connect Primer, Part 1 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-1)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-07-25
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [oidc](https://www.codingmarks.org/search?q=[oidc]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-oidc-flows-example)

In this post, we learned some basics about OpenID Connect, its history, and a bit about the various flow types, scopes, and tokens involved.

* [OIDC in Action – An OpenID Connect Primer, Part 2 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-2)
* [What's in a Token? – An OpenID Connect Primer, Part 3 of 3](https://developer.okta.com/blog/2017/08/01/oidc-primer-part-3)

<hr>

**[OpenID Connect Scopes](https://auth0.com/docs/scopes/current/oidc-scopes)**

  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect])

Understand scopes and claims used with the OpenID Connect (OIDC) protocol.

<hr>


## osx 

**[Install Bash git completion](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [osx](https://www.codingmarks.org/search?q=[osx]), &nbsp; [windows](https://www.codingmarks.org/search?q=[windows]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

How to install git completion on different platforms...

<hr>


## package-manager 

**[Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)**

  * **tags**: &nbsp; [r](https://www.codingmarks.org/search?q=[r]), &nbsp; [package-manager](https://www.codingmarks.org/search?q=[package-manager])

Writing R Extensions - the canonical reference for writing R packages.

<hr>


## pandoc 

**[Pandoc home page](https://pandoc.org/)**

  * **tags**: &nbsp; [pandoc](https://www.codingmarks.org/search?q=[pandoc]), &nbsp; [markdown](https://www.codingmarks.org/search?q=[markdown]), &nbsp; [latex](https://www.codingmarks.org/search?q=[latex])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jgm/pandoc)

Pandoc is a Haskell library for converting from one markup format to another, and a command-line tool that uses this library. 

<hr>


## performance 

**[Arthas Java Diagnostics Tool - home page ](https://alibaba.github.io/arthas/en/)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [diagnostics](https://www.codingmarks.org/search?q=[diagnostics]), &nbsp; [performance](https://www.codingmarks.org/search?q=[performance])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/alibaba/arthas)

#### Key features

* Check whether a class is loaded, or where the class is being loaded. (Useful for troubleshooting jar file conflicts)
* Decompile a class to ensure the code is running as expected.
* View classloader statistics, e.g. the number of classloaders, the number of classes loaded per classloader, the classloader hierarchy, possible classloader leaks, etc.
* View the method invocation details, e.g. method parameter, return object, thrown exception, and etc.
* Check the stack trace of specified method invocation. This is useful when a developers wants to know the caller of the said method.
* Trace the method invocation to find slow sub-invocations.
* Monitor method invocation statistics, e.g. qps, rt, success rate and etc.
* Monitor system metrics, thread states and cpu usage, gc statistics, and etc.
* Supports command line interactive mode, with auto-complete feature enabled.
* Supports telnet and websocket, which enables both local and remote diagnostics with command line and browsers.
* Supports JDK 6+.
* Supports Linux/Mac/Windows.

<hr>

**[The Cost Of JavaScript In 2018 – Addy Osmani – Medium](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-08-01
  * **tags**: &nbsp; [javascript](https://www.codingmarks.org/search?q=[javascript]), &nbsp; [performance](https://www.codingmarks.org/search?q=[performance])

[Youtube video](https://www.youtube.com/watch?v=63I-mEuSvGA)

Building interactive sites can involve sending JavaScript to your users. Often, too much of it. Have you been on a mobile page that looked like it had loaded only to tap on a link or tried to scroll and nothing happens?

**Byte-for-byte, JavaScript is still the most expensive resource we send to mobile phones, because it can delay interactivity in large ways.**

<hr>


## podcast 

**[Podcasts – React](https://reactjs.org/community/podcasts.html)**

  * **tags**: &nbsp; [reactjs](https://www.codingmarks.org/search?q=[reactjs]), &nbsp; [podcast](https://www.codingmarks.org/search?q=[podcast])

Podcasts dedicated to React and individual podcast episodes with React discussions.

#### Podcasts

- [The React Podcast](https://reactpodcast.simplecast.fm/) - The podcast about everything React.js, hosted by [React Training](https://reacttraining.com)

- [JavaScript Air](https://javascriptair.com/) - All about JavaScript (currently not producing new episodes)

- [React 30](https://react30.com/) - A 30-minute podcast all about React (moved to [The React Podcast](https://reactpodcast.simplecast.fm/)).

- [React Native Radio](https://devchat.tv/react-native-radio)

#### Episodes

- [CodeWinds Episode 4](https://codewinds.com/podcast/004.html) - Pete Hunt talks with Jeff Barczewski about React.

- [JavaScript Jabber 73](https://devchat.tv/js-jabber/073-jsj-react-with-pete-hunt-and-jordan-walke) - Pete Hunt and Jordan Walke talk about React.


<hr>


## promise 

**[GitHub - domenic/chai-as-promised](https://github.com/domenic/chai-as-promised)**

  * **tags**: &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [promise](https://www.codingmarks.org/search?q=[promise]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/domenic/chai-as-promised)

Extends Chai with assertions about promises. Contribute to domenic/chai-as-promised development by creating an account on GitHub.

<hr>


## proxy 

**[Istio home page](https://istio.io/)**

  * **tags**: &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [api-management](https://www.codingmarks.org/search?q=[api-management]), &nbsp; [circuit-breaker](https://www.codingmarks.org/search?q=[circuit-breaker]), &nbsp; [proxy](https://www.codingmarks.org/search?q=[proxy]), &nbsp; [kubernetes](https://www.codingmarks.org/search?q=[kubernetes])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/istio/istio)

Istio is an open platform for providing a uniform way to integrate microservices, manage traffic flow across microservices, enforce policies and aggregate telemetry data. Istio's control plane provides an abstraction layer over the underlying cluster management platform, such as Kubernetes, Mesos, etc.

<hr>


## r 

**[Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)**

  * **tags**: &nbsp; [r](https://www.codingmarks.org/search?q=[r]), &nbsp; [package-manager](https://www.codingmarks.org/search?q=[package-manager])

Writing R Extensions - the canonical reference for writing R packages.

<hr>


## reactive 

**[Reactor vs. RxJava](http://helmbold.de/artikel/reactor-vs-rxjava/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-06-05
  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive])

Was spricht für und gegen Reactor und RxJava?

<hr>

**[Reactive Spring - Josh Long, Mark Heckler - YouTube](https://www.youtube.com/watch?v=l7VBdWhtl7A)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-14
  * **tags**: &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [kotlin](https://www.codingmarks.org/search?q=[kotlin]), &nbsp; [redis](https://www.codingmarks.org/search?q=[redis]), &nbsp; [spring-cloud-gateway](https://www.codingmarks.org/search?q=[spring-cloud-gateway])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshlong/flux-flix-service)

Spring Framework 5.0 is here! One of the most exciting introductions in this release is support for reactive programming, building on the Pivotal Reactor project to support message-driven, elastic, resilient and responsive services. Spring Framework 5 integrates an MVC-like component model adapted to support reactive processing and a new type of web endpoint, functional reactive endpoints. In this talk, we'll look at the net-new Netty-based web runtime, how existing Servlet code can run on the new world, and how to integrate it with existing Spring-stack technologies.

<hr>

**[Project Reactor Home Page](https://projectreactor.io/)**

  * **tags**: &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [jvm](https://www.codingmarks.org/search?q=[jvm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/reactor)

Reactor is a fourth-generation Reactive library for building non-blocking applications on
the JVM based on the [Reactive Streams Specification](https://github.com/reactive-streams/reactive-streams-jvm)

<hr>


## reactive-programming 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>

**[Spring 5 WebClient](https://www.baeldung.com/spring-5-webclient)**

  * **tags**: &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/spring-5-reactive)

Discover Spring 5's WebClient - a new reactive RestTemplate alternative.

<hr>


## reactjs 

**[Podcasts – React](https://reactjs.org/community/podcasts.html)**

  * **tags**: &nbsp; [reactjs](https://www.codingmarks.org/search?q=[reactjs]), &nbsp; [podcast](https://www.codingmarks.org/search?q=[podcast])

Podcasts dedicated to React and individual podcast episodes with React discussions.

#### Podcasts

- [The React Podcast](https://reactpodcast.simplecast.fm/) - The podcast about everything React.js, hosted by [React Training](https://reacttraining.com)

- [JavaScript Air](https://javascriptair.com/) - All about JavaScript (currently not producing new episodes)

- [React 30](https://react30.com/) - A 30-minute podcast all about React (moved to [The React Podcast](https://reactpodcast.simplecast.fm/)).

- [React Native Radio](https://devchat.tv/react-native-radio)

#### Episodes

- [CodeWinds Episode 4](https://codewinds.com/podcast/004.html) - Pete Hunt talks with Jeff Barczewski about React.

- [JavaScript Jabber 73](https://devchat.tv/js-jabber/073-jsj-react-with-pete-hunt-and-jordan-walke) - Pete Hunt and Jordan Walke talk about React.


<hr>


## redis 

**[Reactive Spring - Josh Long, Mark Heckler - YouTube](https://www.youtube.com/watch?v=l7VBdWhtl7A)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-14
  * **tags**: &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [kotlin](https://www.codingmarks.org/search?q=[kotlin]), &nbsp; [redis](https://www.codingmarks.org/search?q=[redis]), &nbsp; [spring-cloud-gateway](https://www.codingmarks.org/search?q=[spring-cloud-gateway])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshlong/flux-flix-service)

Spring Framework 5.0 is here! One of the most exciting introductions in this release is support for reactive programming, building on the Pivotal Reactor project to support message-driven, elastic, resilient and responsive services. Spring Framework 5 integrates an MVC-like component model adapted to support reactive processing and a new type of web endpoint, functional reactive endpoints. In this talk, we'll look at the net-new Netty-based web runtime, how existing Servlet code can run on the new world, and how to integrate it with existing Spring-stack technologies.

<hr>


## resources 

**[useful-java-links](https://github.com/Vedenin/useful-java-links#readme)**

  * **tags**: &nbsp; [java](https://www.codingmarks.org/search?q=[java]), &nbsp; [resources](https://www.codingmarks.org/search?q=[resources]), &nbsp; [awesome](https://www.codingmarks.org/search?q=[awesome]), &nbsp; [awesome-list](https://www.codingmarks.org/search?q=[awesome-list])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/Vedenin/useful-java-links)

A list of useful Java frameworks, libraries, software and hello worlds examples - Vedenin/useful-java-links

<hr>


## rest 

**[API Blueprint home page](https://apiblueprint.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation]), &nbsp; [rest](https://www.codingmarks.org/search?q=[rest])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/api-blueprint/)

API Blueprint is simple and accessible to everybody involved in the API lifecycle. Its syntax is concise yet expressive. With API Blueprint you can quickly design and prototype APIs to be created or document and test already deployed mission-critical APIs.

<hr>


## rxjs 

**[rx-book RxJS - Javascript library for functional reactive programming.](http://xgrommx.github.io/rx-book)**

  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators.

<hr>


## security 

**[GitHub - docker/docker-bench-security](https://github.com/docker/docker-bench-security)**

  * **tags**: &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [security](https://www.codingmarks.org/search?q=[security]), &nbsp; [devops](https://www.codingmarks.org/search?q=[devops])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/docker/docker-bench-security)

The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production. - docker/docker-bench-security

<hr>


## shell 

**[command line - How to list all symbolic links in a directory - Ask Ubuntu](https://askubuntu.com/questions/522051/how-to-list-all-symbolic-links-in-a-directory)**

  * **tags**: &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [shell](https://www.codingmarks.org/search?q=[shell]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

```
find . -type l -ls
```

To only process the current directory:
```
find . -maxdepth 1 -type l -ls
```

<hr>


## spring 

**[Making microservices micro with Istio Service Mesh & Kubernetes by Ray Tsang @ Spring I/O 2018 - YouTube](https://www.youtube.com/watch?v=s31kdh7Q7Hc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

<hr>

**[Spring 5 WebClient](https://www.baeldung.com/spring-5-webclient)**

  * **tags**: &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/spring-5-reactive)

Discover Spring 5's WebClient - a new reactive RestTemplate alternative.

<hr>

**[Guide to Spring 5 WebFlux](https://www.baeldung.com/spring-webflux)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2019-02-21
  * **tags**: &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/spring-5-reactive-security)

A brief guide to using WebFlux with annotations, in Spring 5.

<hr>

**[Project Reactor Home Page](https://projectreactor.io/)**

  * **tags**: &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [jvm](https://www.codingmarks.org/search?q=[jvm])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/reactor)

Reactor is a fourth-generation Reactive library for building non-blocking applications on
the JVM based on the [Reactive Streams Specification](https://github.com/reactive-streams/reactive-streams-jvm)

<hr>


## spring-boot 

**[Making microservices micro with Istio Service Mesh & Kubernetes by Ray Tsang @ Spring I/O 2018 - YouTube](https://www.youtube.com/watch?v=s31kdh7Q7Hc)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio]), &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

<hr>

**[Identity, Claims, & Tokens – An OpenID Connect Primer, Part 1 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-1)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-07-25
  * **tags**: &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [oidc](https://www.codingmarks.org/search?q=[oidc]), &nbsp; [spring-boot](https://www.codingmarks.org/search?q=[spring-boot])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-oidc-flows-example)

In this post, we learned some basics about OpenID Connect, its history, and a bit about the various flow types, scopes, and tokens involved.

* [OIDC in Action – An OpenID Connect Primer, Part 2 of 3](https://developer.okta.com/blog/2017/07/25/oidc-primer-part-2)
* [What's in a Token? – An OpenID Connect Primer, Part 3 of 3](https://developer.okta.com/blog/2017/08/01/oidc-primer-part-3)

<hr>


## spring-cloud 

**[A Tale of Two Frameworks: Spring Cloud and Istio - YouTube](https://www.youtube.com/watch?v=AMJQO9zs2eo)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-10-03
  * **tags**: &nbsp; [spring-cloud](https://www.codingmarks.org/search?q=[spring-cloud]), &nbsp; [istio](https://www.codingmarks.org/search?q=[istio])

Spring has been the leader as a microservices framework for Java with Spring Boot, Spring and Spring Cloud. Istio has emerged as a polyglot alternative to Spring Cloud as an open platform to connect, manage and secure microservices. Let's explore how Istio compares to Spring Cloud and what each platform provides in addition to the other. We will also explore the use cases that are best suited to use Spring Cloud or Istio, or perhaps a combination of both.

Istio or Spring Cloud (or both?)

If its part of the platform, use it. Except for...
  * Fallbacks
  * Tracing Propagation
  * Security

Istio or a service mesh architecture works better for polyglot environments 

<hr>


## spring-cloud-gateway 

**[Reactive Spring - Josh Long, Mark Heckler - YouTube](https://www.youtube.com/watch?v=l7VBdWhtl7A)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-14
  * **tags**: &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [kotlin](https://www.codingmarks.org/search?q=[kotlin]), &nbsp; [redis](https://www.codingmarks.org/search?q=[redis]), &nbsp; [spring-cloud-gateway](https://www.codingmarks.org/search?q=[spring-cloud-gateway])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshlong/flux-flix-service)

Spring Framework 5.0 is here! One of the most exciting introductions in this release is support for reactive programming, building on the Pivotal Reactor project to support message-driven, elastic, resilient and responsive services. Spring Framework 5 integrates an MVC-like component model adapted to support reactive processing and a new type of web endpoint, functional reactive endpoints. In this talk, we'll look at the net-new Netty-based web runtime, how existing Servlet code can run on the new world, and how to integrate it with existing Spring-stack technologies.

<hr>


## spring-security 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>


## spring-webflux 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>

**[Spring 5 WebClient](https://www.baeldung.com/spring-5-webclient)**

  * **tags**: &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/spring-5-reactive)

Discover Spring 5's WebClient - a new reactive RestTemplate alternative.

<hr>

**[Guide to Spring 5 WebFlux](https://www.baeldung.com/spring-webflux)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2019-02-21
  * **tags**: &nbsp; [spring](https://www.codingmarks.org/search?q=[spring]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/eugenp/tutorials/tree/master/spring-5-reactive-security)

A brief guide to using WebFlux with annotations, in Spring 5.

<hr>

**[Reactive Spring - Josh Long, Mark Heckler - YouTube](https://www.youtube.com/watch?v=l7VBdWhtl7A)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-12-14
  * **tags**: &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [reactive](https://www.codingmarks.org/search?q=[reactive]), &nbsp; [kotlin](https://www.codingmarks.org/search?q=[kotlin]), &nbsp; [redis](https://www.codingmarks.org/search?q=[redis]), &nbsp; [spring-cloud-gateway](https://www.codingmarks.org/search?q=[spring-cloud-gateway])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/joshlong/flux-flix-service)

Spring Framework 5.0 is here! One of the most exciting introductions in this release is support for reactive programming, building on the Pivotal Reactor project to support message-driven, elastic, resilient and responsive services. Spring Framework 5 integrates an MVC-like component model adapted to support reactive processing and a new type of web endpoint, functional reactive endpoints. In this talk, we'll look at the net-new Netty-based web runtime, how existing Servlet code can run on the new world, and how to integrate it with existing Spring-stack technologies.

<hr>


## sql 

**[Online SQLite browser](https://extendsclass.com/sqlite-browser.html)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2019-03-06
  * **tags**: &nbsp; [online-tools](https://www.codingmarks.org/search?q=[online-tools]), &nbsp; [sql](https://www.codingmarks.org/search?q=[sql])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/hautdefrance/Web-GUI-for-SQLite)

SQLite Browser is a online SQL interpreter for SQLite database. Open and execute queries from a SQLite file. Neither download nor installation.

<hr>


## testing 

**[Testcontainers home page](https://www.testcontainers.org/)**

  * **tags**: &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [docker](https://www.codingmarks.org/search?q=[docker]), &nbsp; [java](https://www.codingmarks.org/search?q=[java])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/testcontainers/testcontainers-java/)

Testcontainers is a Java 8 library that supports JUnit tests, providing lightweight, throwaway instances of common databases, Selenium web browsers, or anything else that can run in a Docker container.

<hr>

**[Dredd — HTTP API Testing Framework — Dredd latest documentation](https://dredd.org/)**

  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/apiaryio/dredd)

Dredd is a language-agnostic command-line tool for validating API description document against backend implementation of the API.

Dredd reads your API description and step by step validates whether your API implementation replies with responses as they are described in the documentation.

<hr>

**[Modern Node.js: async/await based testing with Mocha & Chai](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-04-09
  * **tags**: &nbsp; [mocha](https://www.codingmarks.org/search?q=[mocha]), &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://zaiste.net/modern_node_js_async_await_based_testing_with_mocha_chai/)

Mocha is a JavaScript test framework running on Node.js and in the browser. It can run both asynchronous and synchronous code serially. Test cases are created using describe() and it() methods, the former is used to provide a structure by allowing to put various tests cases in logical groups while the latter is where the tests are written.

In order to perform actual tests, there is a need for an assertion library: a runtime mechanism which can be used to verify assumptions made by the program and print a diagnostic message if this assumption is false. Node.js comes with a built-in assert library. Chai is another popular assertion library that provides both the BDD and TDD styles of programming for testing the code. BDD stands for Behavior-driven development while TDD stands for Test-driven development. In a nutshell, Chai provides a should keyword for BDD style which allows to chain assertions and an expect() method for TDD style. Choosing between one and another is a matter of personal preference.

<hr>

**[GitHub - domenic/chai-as-promised](https://github.com/domenic/chai-as-promised)**

  * **tags**: &nbsp; [chai](https://www.codingmarks.org/search?q=[chai]), &nbsp; [promise](https://www.codingmarks.org/search?q=[promise]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/domenic/chai-as-promised)

Extends Chai with assertions about promises. Contribute to domenic/chai-as-promised development by creating an account on GitHub.

<hr>

**[Arquillian  Home Page](http://arquillian.org/)**

  * **tags**: &nbsp; [arquillian](https://www.codingmarks.org/search?q=[arquillian]), &nbsp; [testing](https://www.codingmarks.org/search?q=[testing]), &nbsp; [integration-testing](https://www.codingmarks.org/search?q=[integration-testing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/arquillian)

An innovative testing platform for the Java Virtual Machine (JVM). Open source. Highly extensible.

<hr>


## tools 

**[MX Lookup Tool - Check your DNS MX Records online - MxToolbox](https://mxtoolbox.com/)**

  * **tags**: &nbsp; [dns](https://www.codingmarks.org/search?q=[dns]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

MxToolbox supports global Internet operations by providing free, fast and accurate network diagnostic and lookup tools. Millions of technology professionals use our tools to help diagnose and resolve a wide range of infrastructure issues.

<hr>

**[X-Icon Editor](http://www.xiconeditor.com/)**

  * **tags**: &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [tools](https://www.codingmarks.org/search?q=[tools])

X-Icon Editor, create high resolution icons from your own browser

<hr>


## user-experience 

**[Angular Progressbar](https://murhafsousli.github.io/ngx-progressbar/#/)**

  * **tags**: &nbsp; [css](https://www.codingmarks.org/search?q=[css]), &nbsp; [user-experience](https://www.codingmarks.org/search?q=[user-experience])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/murhafsousli/ngx-progressbar)

A nanoscopic progress/loading bar. Featuring realistic trickle animations to convince your users that something is happening!

<hr>


## video-processing 

**[High Quality Gifs with FFMPEG ](https://medium.com/@colten_jackson/doing-the-gif-thing-on-debian-82b9760a8483)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2015-10-28
  * **tags**: &nbsp; [gif](https://www.codingmarks.org/search?q=[gif]), &nbsp; [ffmpeg](https://www.codingmarks.org/search?q=[ffmpeg]), &nbsp; [video-processing](https://www.codingmarks.org/search?q=[video-processing])

After getting FFMPEG installed, let’s try it out on a MOV downloaded from my google photos account:
```
ffmpeg -i MVI_6654.MOV firsttry.gif
```
We’re calling the ffmpeg program and telling it that MVI_6654.MOV is our input file with the -i flag. the filename at the end defines the conversion and creates the new file

<hr>

**[
FFmpeg](https://www.ffmpeg.org/)**

  * **tags**: &nbsp; [ffmpeg](https://www.codingmarks.org/search?q=[ffmpeg]), &nbsp; [video-processing](https://www.codingmarks.org/search?q=[video-processing])
  * <i class="fa fa-github fa-lg"></i> [github url](https://www.ffmpeg.org/download.html#get-sources)

A complete, cross-platform solution to record, convert and stream audio and video.


<hr>


## vim 

**[Vim dotfiles and configurations example](https://github.com/jessfraz/.vim)**

  * **tags**: &nbsp; [vim](https://www.codingmarks.org/search?q=[vim]), &nbsp; [dotfiles](https://www.codingmarks.org/search?q=[dotfiles])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/jessfraz/.vim)

My .vim dotfiles and configurations. Contribute to jessfraz/.vim

<hr>


## visualization 

**[Graphviz - Graph Visualization Software](https://graphviz.org/)**

  * **tags**: &nbsp; [graph](https://www.codingmarks.org/search?q=[graph]), &nbsp; [visualization](https://www.codingmarks.org/search?q=[visualization]), &nbsp; [documentation](https://www.codingmarks.org/search?q=[documentation])
  * <i class="fa fa-github fa-lg"></i> [github url](https://gitlab.com/graphviz/graphviz/)

Graphviz is open source graph visualization software. It has several main layout programs. See the gallery for sample layouts. It also has web and interactive graphical interfaces, and auxiliary tools, libraries, and language bindings. We're not able to put a lot of work into GUI editors but there are quite a few external projects and even commercial tools that incorporate Graphviz. You can find some of these in the Resources section.

<hr>


## web 

**[GitHub - audreyr/favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)**

  * **tags**: &nbsp; [html](https://www.codingmarks.org/search?q=[html]), &nbsp; [web](https://www.codingmarks.org/search?q=[web]), &nbsp; [favicon](https://www.codingmarks.org/search?q=[favicon]), &nbsp; [cheatsheet](https://www.codingmarks.org/search?q=[cheatsheet])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/audreyr/favicon-cheat-sheet)

Obsessive cheat sheet to favicon sizes/types. Please contribute! (Note: this may be in flux as I learn new things about favicon best practices.) - audreyr/favicon-cheat-sheet

<hr>


## websocket 

**[Build Reactive APIs with Spring WebFlux](https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux#secure-your-spring-webflux-reactive-api-with-oidc)**

  * **tags**: &nbsp; [reactive-programming](https://www.codingmarks.org/search?q=[reactive-programming]), &nbsp; [spring-webflux](https://www.codingmarks.org/search?q=[spring-webflux]), &nbsp; [mongodb](https://www.codingmarks.org/search?q=[mongodb]), &nbsp; [spring-security](https://www.codingmarks.org/search?q=[spring-security]), &nbsp; [openid-connect](https://www.codingmarks.org/search?q=[openid-connect]), &nbsp; [websocket](https://www.codingmarks.org/search?q=[websocket])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/oktadeveloper/okta-spring-webflux-react-example)

You've heard that reactive programming can help you scale? But how do you implement it? Using Spring WebFlux, of course! This article shows you how.

<hr>


## windows 

**[Install Bash git completion](https://github.com/bobthecow/git-flow-completion/wiki/Install-Bash-git-completion)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash]), &nbsp; [osx](https://www.codingmarks.org/search?q=[osx]), &nbsp; [windows](https://www.codingmarks.org/search?q=[windows]), &nbsp; [linux](https://www.codingmarks.org/search?q=[linux])

How to install git completion on different platforms...

<hr>


## zuul 

**[API Infrastructure at Knewton: What’s in an Edge Service?](https://medium.com/knerd/api-infrastructure-at-knewton-whats-in-an-edge-service-51a3777aeb41)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-05-09
  * **tags**: &nbsp; [api](https://www.codingmarks.org/search?q=[api]), &nbsp; [microservices](https://www.codingmarks.org/search?q=[microservices]), &nbsp; [nginx](https://www.codingmarks.org/search?q=[nginx]), &nbsp; [zuul](https://www.codingmarks.org/search?q=[zuul])

In this post, we will pull back the covers of our API to explain how we handle user requests. You will first learn how to build an edge service with Netflix Zuul, the framework we chose for its simplicity and flexibility. Then, we’ll dive into the Knewton edge service to show you how it improves API simplicity, flexibility, and performance.

What’s in an Edge Service
-------
An edge service is a component which is exposed to the public internet. It acts as a gateway to all other services, which we will refer to as platform services. For example, consider an Nginx reverse proxy in front of some web resource servers. Here, Nginx acts as an edge service by routing public HTTP requests to the appropriate platform service.

<hr>

