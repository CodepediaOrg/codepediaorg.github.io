---
id: 1011
title: Javascript hoisting example
date: 2013-12-05T13:39:33+00:00
author: Adrian Matei
layout: post
guid: http://www.codepedia.org/?p=1011
permalink: /ama/javascript-hoisting-example/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 1
dsq_thread_id:
  - 2032664772
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - example
  - hoisting
---
<pre class="lang:default mark:7,10 decode:true crayon-selected" title="Hoisting in action html page">&lt;head&gt;
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
&lt;/body&gt;</pre>
