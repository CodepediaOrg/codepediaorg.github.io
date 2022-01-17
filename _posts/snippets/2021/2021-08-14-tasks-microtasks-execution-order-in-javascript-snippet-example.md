---
layout: post
title: Tasks, microtasks execution order in javascript - snippet example
description: "Tasks, microtasks execution order in javascript - code snippet example"
author: ama
permalink: /snippets/61169574f520065311390cd9/tasks-microtasks-execution-order-in-javascript-snippet-example
published: true
snippetId: 61169574f520065311390cd9
categories: [snippets]
tags: [javascript, runtime, node.js, interview-question, event-loop, codever-snippets]
---

What is the order in which the following texts are logged via `console.log` ?

```javascript
console.log('1 - start');
setTimeout(() => console.log('2 - setTimeout1'), 0);
Promise.resolve('Success')
    .then(()=> console.log('3 - promise1'))
    .then(()=> console.log('4 - promise2'));
setTimeout(() => console.log('5 - setTimeout2'), 0);
console.log('6 - end');
```

Output

```javascript
1 - start// statement is executed directly in the script (first "Run script" task)
5 - end // part of the first "Run script" task gets executed too
3 - promise1 // placed in microTasks queue and executed between tasks, after first "Run script" task is ready
4 - promise2 // microtask added  during previous microtask execution  and executed immediately
2 - setTimeout1 // callback execution scheduled in another task in the "macroTasks" queue (also task queue), executed in the next interation of the event-loop
5 - setTimeout2 // callback execution scheduled in another task in the task queue, executed in the next iteration of event-loop
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="61169574f520065311390cd9" %}
