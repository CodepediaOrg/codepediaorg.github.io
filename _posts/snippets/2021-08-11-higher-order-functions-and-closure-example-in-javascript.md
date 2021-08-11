---
layout: post
title: Higher order functions and closures example in Javascript
description: "Higher order functions and closures example in Javascript - code snippets"
author: ama
permalink: /snippets/6113a246f52006531138ff57/higher-order-functions-and-closure-example-in-javascript
published: true
snippetId: 6113a246f52006531138ff57
categories: [snippets]
tags: [javascript, functions, functional-programming, higher-order-functions, arrow-functions, codever-snippets]
---

First with "normal" functions:

```javascript
//closures and higher order function
function salute(salutation) {
  return function(firstName) {
    return function(lastName) {
      console.log(`hi ${salutation} ${firstName} ${lastName}`)
    }
  }
}

salute('Mr.')('John')('Wick')

//output
hi Mr. John Wick

```

The shorter variant with arrow functions:

```javascript
const saluteArrowFunction = (salutation) => (firstName) => (lastName) => console.log(`hi ${salutation} ${firstName} ${lastName}`);

saluteArrowFunction ('Mr.')('Johnny')('Cage')

//output
hi Mr. Johnny Cage
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6113a246f52006531138ff57" %}
