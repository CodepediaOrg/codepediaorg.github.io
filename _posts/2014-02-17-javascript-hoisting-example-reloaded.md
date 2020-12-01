---
id: 1197
title: JavaScript hoisting example reloaded
description: "Javascript code snippet demonstrating hoisting"
date: 2014-02-17T21:32:01+00:00
author: ama
layout: post
permalink: /ama/javascript-hoisting-example-reloaded/
categories:
  - snippets
tags:
  - javascript
  - html
  - hoisting
---

```html
<head>
    <title>Hoisting example reloaded</title>
</head>
&lt;body&gt;
    &lt;h1&gt;Hoisting in action&lt;/h1&gt;
    &lt;script type="application/javascript"&gt;
        var hoisting = "global variable";
        alert("global 'hoisting' var =&gt; " + hoisting);
        (function(){
            alert("local 'hoisting' var (BEFORE declaration WILL NOT PICK its global 'shadow' -&gt; undefined !) =&gt; " + hoisting);
            var hoisting = "local variable";
            alert("local 'hoisting' var (AFTER declaration WILL HIDE its global 'shadow' -&gt; assigned value !) =&gt; " + hoisting);
            // block
            {
                var hoisting = "block variable";
                alert("block 'hoisting' var (AFTER declaration WILL OVERRIDE its local 'shadow' -&gt; assigned value !) =&gt; " + hoisting);
            }
            // after block
            alert("local 'hoisting' var REPLACED AFTER BLOCK with its block 'shadow' =&gt; " + hoisting);
            // THE moral
            alert("The Hoisting MORAL: ALL local variables (even from blocks) are pre-defined/hoisted by JavaScript Runtime in front of the method body !\nDYI to make it clear !");
        })(); //self-executing function
    &lt;/script&gt;
&lt;/body&gt;
```

Thank you <a title="@devstonez" href="https://twitter.com/devstonez" target="_blank">Dev{eloper} Stonez (@devstonez)</a> for the tip.
