---
layout: post
title: Minus duration in Java local datetime from now example
description: "Code snippet showing how to extract duration from current time in Java wiht local datetime"
author: ama
permalink: /snippets/60702c5cc6a18456bf09b81a/minus-duration-in-java-local-datetime-from-now-example
published: true
snippetId: 60702c5cc6a18456bf09b81a
categories: [snippets]
tags: [java, datetime, date, codever]
---

Get current day with `LocalDateTime.now()` from which you can substract years, weeks, days up to nanos (in the shown example days with `minusDays(long days)`):

```java
LocalDateTime dateToLookBack = LocalDateTime.now().minusDays(30);
```

You can achieve the same result by using the `minus(TemporalAmount amountToSubtract)` method as below

```java
final var dateToLookBackTo = LocalDateTime.now().minus(Duration.ofDays(30));
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html" target="_blank" style="font-weight: lighter">
     https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60702c5cc6a18456bf09b81a" %}
