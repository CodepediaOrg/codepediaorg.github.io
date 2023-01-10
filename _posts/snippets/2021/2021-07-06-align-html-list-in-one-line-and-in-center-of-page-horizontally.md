---
layout: post
title: Align html list in one line and in center of page horizontally
description: "Code snippet showing how to align html list in one line and in center of page horizontally with css and flexbox."
author: ama
permalink: /snippets/60e3dbf1a9a4e07a75cbcbed/align-html-list-in-one-line-and-in-center-of-page-horizontally
published: true
snippetId: 60e3dbf1a9a4e07a75cbcbed
categories: [snippets]
tags: [scss, css, html, flexbox, codever-snippets]
---

Use `diplay:flex` on both wrapper `div` and `ul` element and set `margin-right` and `margin-left` to `auto` at the list level:

```scss
.identity-providers-list {
  display: flex;
  ul {
    margin-left: auto;
    margin-right: auto;
    display: flex;
    list-style: none;
    li {
      margin-right: 2rem;
    }
  }
}
```

Usage example

```scss
  <div class="identity-providers-list">
    <ul>
      <li>
        <i class="fab fa-github"></i> Github
      </li>
      <li>
        <i class="fab fa-google"></i> Google
      </li>
      <li>
        <i class="fab fa-gitlab"></i> Gitlab
      </li>
      <li>
        <i class="fab fa-stack-overflow"></i> StackOverflow
      </li>
    </ul>
  </div>
```

> You can see how it looks in the [register](https://www.codever.dev/register) page on Codever

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="60e3dbf1a9a4e07a75cbcbed" %}
