---
layout: post
title: Conditional css class in angular html element
description: "Conditional css class in angular html element Codever code snippet"
author: ama
permalink: /snippets/638daa94d00726522c6cdd7c/conditional-css-class-in-angular-html-element
published: true
snippetId: 638daa94d00726522c6cdd7c
categories: [snippets]
tags: [angular, html, css, codever-snippets]
---

Use the `ngClass` input attribute with the **object** notation,
where the **keys** are CSS classes that get added when the expression given in the value evaluates to **truthy** value,
otherwise they are removed. In the example below the class `navbar-dev-green` is added to the `nav` element only for the **dev** environment (non-prod):

```html
<nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top shadow"
     [ngClass]="{'navbar-dev-green' : environment.production === false}" >
...
</nav>
```

**Project**: `codever` - **File**:  `navigation.component.html`


To add multiple classes you can list them with space between them.
To add different conditions for each class use a boolean expression evaluator for each class as shown in the example of the documentation:


```html
<some-element [ngClass]="{'class1 class2 class3' : true}">...</some-element>
<some-element [ngClass]="{'first': true, 'second': true, 'third': false}">...</some-element>

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/api/common/NgClass" target="_blank" style="font-weight: lighter">
     https://angular.io/api/common/NgClass
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="638daa94d00726522c6cdd7c" %}
