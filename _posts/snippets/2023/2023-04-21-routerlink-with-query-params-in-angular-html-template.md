---
layout: post
title: routerLink with query params in Angular html template
description: "routerLink with query params in Angular html template code snippet"
author: ama
permalink: /snippets/644268c1aaf4c5228c969eb1/routerlink-with-query-params-in-angular-html-template
published: true
snippetId: 644268c1aaf4c5228c969eb1
categories: [snippets]
tags: [angular, html, routing, query-string, codever-snippets]
---

Use `routerLink` and `queryParams` as inputs to the link as in the following example:

```html
<a class="dropdown-item" [routerLink]="['/my-bookmarks/new']" [queryParams]="{initiator: 'browser'}">
  <i class="fa fa-bookmark"></i>
  Bookmark
</a>
```

This will generate an url like the following https://www.codever.dev/my-bookmarks/new?initiator=browser

**Project**: `codever` - **File**:  `navigation.component.html`

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="644268c1aaf4c5228c969eb1" %}
