---
layout: post
title: Angular routerLink url with parameter in html template
description: "Angular routerLink url with parameter in html template   code snippet"
author: ama
permalink: /snippets/60268af3cd228421f08d748d/angular-routerlink-url-with-parameter-in-html-template
published: true
snippetId: 60268af3cd228421f08d748d
categories: [snippets]
tags: [angular, html-template, angular-routing, codever-snippets]
---

**Project**: [`codever`](https://github.com/codeverland/codever) - **File**:  `bookmark-list-element.component.html`

Use a two-element array when specifying a route parameter:

```typescript
<div *ngFor="let tag of bookmark.tags" class="btn-group tag-list  mr-2 mt-1">
  <a class="dropdown-item"
     [routerLink]="['/tagged', tag]"
     title='Public bookmarks tagged "{{tag}}"'>
    <i class="fas fa-tag"></i> Public
  </a>
</div>
```
This will generate a link similar to the following
 [https://www.codever.land/**tagged/angular**](https://www.codever.land/tagged/angular),
  where the value of the `tag` parameter in `routerLink` is the string `angular`

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/router" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/router
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60268af3cd228421f08d748d" %}
