---
layout: post
title: Class inheritance in scss example
description: "Class inheritance in scss code snippet"
author: ama
permalink: /snippets/60c5b2ab76edda2bb13819ef/class-inheritance-in-scss-example
published: true
snippetId: 60c5b2ab76edda2bb13819ef
categories: [snippets]
tags: [scss, css, codever-snippets]
---

**Project**: [`codever`](https://github.com/CodeverDotDev/codever)

Use the `@extend` directive. In this case the **border** style is inherited:

```scss
.last-search-border {
  border-width: 0.05rem;
  border-style: solid;
  border-color: #495057;
}

.public-bookmarks-last-search {
  @extend .last-search-border;
  background: #f0f2f5;
  color: black;
}

.my-bookmarks-last-search {
  @extend .last-search-border;
  background: linen;
  color: #495057;
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://sass-lang.com/documentation/at-rules/extend" target="_blank" style="font-weight: lighter">
     https://sass-lang.com/documentation/at-rules/extend
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60c5b2ab76edda2bb13819ef" %}
