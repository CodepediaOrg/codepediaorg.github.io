---
id: 1011
title: Javascript hoisting example
date: 2013-12-05T13:39:33+00:00
author: ama
layout: post
description: "Javascript code snippet demonstrating hoisting"
permalink: /ama/javascript-hoisting-example/
categories:
  - snippets
tags:
  - javascript
  - html
  - hoisting
---
```html
&lt;head&gt;
&lt;title&gt;Hoisting example&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Hoisting in action&lt;/h1&gt;
    &lt;script type="application/javascript"&gt;
        var hoisting = "global variable";
        (function(){
            confirm("\"" + hoisting + "\"" + " click OK" );
            var hoisting = "local variable";
            alert(hoisting);
        })(); //self-executing function

     //Best practice - declare local variables at the beginning of the function
    &lt;/script&gt;
&lt;/body>
```
