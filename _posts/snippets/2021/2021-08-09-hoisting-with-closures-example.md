---
layout: post
title: Hoisting with closures example
description: "Hoisting with closures code snippets in javascript"
author: ama
permalink: /snippets/6110fefa3e01006b23fb2f2d/hoisting-with-closures-example
published: true
snippetId: 6110fefa3e01006b23fb2f2d
categories: [snippets]
tags: [javascript, hoisting, closures, codever-snippets]
---

Via [hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting), memory is allocated to the variable `var1`, but it is not initialised by the time the [closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) is executed

```javascript
function one() {
  function two() {
    console.log(`closure var1 - ${var1}`);
  }

  three();
  var var1 = 'var1';
}

one();

//output
hoisting var1 - undefined
```

But if we use `setTimeout()`, by the time callback closure function is executed `var1` will have been initialised and its value is printed:

```javascript
function one() {
 setTimeout(function() {
  console.log(`closure var1 - ${var1}`);
 }, 0);
  var var1 = 'var1';
}

one();

//output
closure var1 - var1
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6110fefa3e01006b23fb2f2d" %}
