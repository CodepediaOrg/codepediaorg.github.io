---
layout: post
title: Parse LocalDateTime from String in ISO Format with 0 milliseconds
description: "Parse LocalDateTime from String in ISO Format with 0 milliseconds code snippet"
author: ama
permalink: /snippets/614312d7fb0221040e46c190/parse-localdatetime-from-string-in-iso-format-with-0-milliseconds
published: true
snippetId: 614312d7fb0221040e46c190
categories: [snippets]
tags: [java, datetime, codever-snippets]
---

Use `parse` method and apply `withNano` with value `0` seconds on the result

```java
final var fromDateTime = LocalDateTime.parse("2017-02-26T01:02:03").withNano(0);
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="614312d7fb0221040e46c190" %}
