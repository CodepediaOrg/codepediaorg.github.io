---
layout: post
title: New codingmarks added in the 30th week of 2018
description: "New codingmarks added in the 30th week of 2018. Keywords: angular, bash, cryptography, design-patterns, free-programming-books, git, rxjs and security"
author: ama
permalink: /ama/new-codingmarks-week-30-2018
published: true
categories: [codingmarks]
tags: [codingmarks]
---
New [codingmarks](https://www.codingmarks.org) added in the 30th week of 2018. Hot topics include:

* TOC
{:toc} 

<!--more-->

## angular 

**[RxJs Mapping: switchMap vs mergeMap vs concatMap vs exhaustMap](https://blog.angular-university.io/rxjs-higher-order-mapping/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-07-18
  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/angular-university/rxjs-course/tree/1-operators-finished)

So that is what we will be doing in this post, we are going to learn in a logical order the concat, merge, switch and exhaust strategies and their corresponding mapping operators: concatMap, mergeMap, switchMap and exhaustMap.

We will explain the concepts using a combination of marble diagrams and some practical examples (including running code).

<hr>

**[Hot vs Cold Observables – Ben Lesh](https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-03-29
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular]), &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])

You want a HOT observable when you don’t want to create your producer over and over again.
* COLD is when your observable creates the producer
* HOT is when your observable closes over the producer

<hr>


## bash 

**[git - Remove local branches no longer on remote - Stack Overflow](https://stackoverflow.com/questions/7726949/remove-local-branches-no-longer-on-remote)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash])

To give yourself the opportunity to edit the list before deleting branches, you could do the following in one linee:

```
git branch --merged >/tmp/merged-branches && vi /tmp/merged-branches && xargs git branch -d </tmp/merged-branches
```

<hr>


## cryptography 

**[Public key fingerprint - Wikipedia](https://en.wikipedia.org/wiki/Public_key_fingerprint)**

  * **tags**: &nbsp; [cryptography](https://www.codingmarks.org/search?q=[cryptography]), &nbsp; [security](https://www.codingmarks.org/search?q=[security])

In public-key cryptography, a public key fingerprint is a short sequence of bytes used to identify a longer public key. Fingerprints are created by applying a cryptographic hash function to a public key. Since fingerprints are shorter than the keys they refer to, they can be used to simplify certain key management tasks. In Microsoft software, "thumbprint" is used instead of "fingerprint".

<hr>


## design-patterns 

**[On The Subject Of Subjects (in RxJS) – Ben Lesh ](https://medium.com/@benlesh/on-the-subject-of-subjects-in-rxjs-2b08b7198b93)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-12-10
  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns])

Subjects in RxJS are often misunderstood. Because they allow you to imperatively push values into an observable stream, people tend to abuse Subjects when they’re not quite sure how to make an Observable out of something. 

<hr>


## free-programming-books 

**[Introduction · learn-rxjs](https://www.learnrxjs.io/)**

  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Clear examples, explanations, and resources for RxJS.

<hr>


## git 

**[git - Remove local branches no longer on remote - Stack Overflow](https://stackoverflow.com/questions/7726949/remove-local-branches-no-longer-on-remote)**

  * **tags**: &nbsp; [git](https://www.codingmarks.org/search?q=[git]), &nbsp; [bash](https://www.codingmarks.org/search?q=[bash])

To give yourself the opportunity to edit the list before deleting branches, you could do the following in one linee:

```
git branch --merged >/tmp/merged-branches && vi /tmp/merged-branches && xargs git branch -d </tmp/merged-branches
```

<hr>


## rxjs 

**[Introduction · learn-rxjs](https://www.learnrxjs.io/)**

  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [free-programming-books](https://www.codingmarks.org/search?q=[free-programming-books])

Clear examples, explanations, and resources for RxJS.

<hr>

**[RxJs Mapping: switchMap vs mergeMap vs concatMap vs exhaustMap](https://blog.angular-university.io/rxjs-higher-order-mapping/)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2018-07-18
  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [angular](https://www.codingmarks.org/search?q=[angular])
  * <i class="fa fa-github fa-lg"></i> [github url](https://github.com/angular-university/rxjs-course/tree/1-operators-finished)

So that is what we will be doing in this post, we are going to learn in a logical order the concat, merge, switch and exhaust strategies and their corresponding mapping operators: concatMap, mergeMap, switchMap and exhaustMap.

We will explain the concepts using a combination of marble diagrams and some practical examples (including running code).

<hr>

**[RxJS: Understanding the publish and share Operators](https://blog.angularindepth.com/rxjs-understanding-the-publish-and-share-operators-16ea2f446635)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2017-08-23
  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])

An in-depth look at the multicast, publish and share operators

<hr>

**[On The Subject Of Subjects (in RxJS) – Ben Lesh ](https://medium.com/@benlesh/on-the-subject-of-subjects-in-rxjs-2b08b7198b93)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-12-10
  * **tags**: &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs]), &nbsp; [design-patterns](https://www.codingmarks.org/search?q=[design-patterns])

Subjects in RxJS are often misunderstood. Because they allow you to imperatively push values into an observable stream, people tend to abuse Subjects when they’re not quite sure how to make an Observable out of something. 

<hr>

**[Hot vs Cold Observables – Ben Lesh](https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339)**

  * <i class="fa fa-calendar"></i> **published on**: &nbsp;2016-03-29
  * **tags**: &nbsp; [angular](https://www.codingmarks.org/search?q=[angular]), &nbsp; [rxjs](https://www.codingmarks.org/search?q=[rxjs])

You want a HOT observable when you don’t want to create your producer over and over again.
* COLD is when your observable creates the producer
* HOT is when your observable closes over the producer

<hr>


## security 

**[Public key fingerprint - Wikipedia](https://en.wikipedia.org/wiki/Public_key_fingerprint)**

  * **tags**: &nbsp; [cryptography](https://www.codingmarks.org/search?q=[cryptography]), &nbsp; [security](https://www.codingmarks.org/search?q=[security])

In public-key cryptography, a public key fingerprint is a short sequence of bytes used to identify a longer public key. Fingerprints are created by applying a cryptographic hash function to a public key. Since fingerprints are shorter than the keys they refer to, they can be used to simplify certain key management tasks. In Microsoft software, "thumbprint" is used instead of "fingerprint".

<hr>

