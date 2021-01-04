---
title: Javascript hoisting example (title of the )
date: 2013-12-05T13:39:33+00:00
author: ama
layout: post
description: "Javascript code snippet demonstrating hoisting (first comment)"
permalink: /ama/javascript-hoisting-example/ (useraname/string-to-dash-url)
categories:
  - snippets (this is the category)
tags:
  - javascript ()
  - html
  - hoisting
---

comments ()


```html
<head>
&lt;title&gt;Hoisting example&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1>Hoisting in action&lt;/h1&gt;
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
