---
id: 1197
title: JavaScript hoisting example reloaded
date: 2014-02-17T21:32:01+00:00
author: Adrian Matei
layout: post
guid: http://www.codepedia.org/?p=1197
permalink: /ama/javascript-hoisting-example-reloaded/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 2
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2280310621
categories:
  - javascript
tags:
  - example
  - function
  - hoisting
  - javascript
---
<pre class="lang:xhtml decode:true crayon-selected" title="HTML page containing JavaScript hoisting example">&lt;head&gt;
&lt;title&gt;Hoisting example&lt;/title&gt;
&lt;/head&gt;
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
&lt;/body&gt;</pre>

Thank you <a title="@devstonez" href="https://twitter.com/devstonez" target="_blank">Dev{eloper} Stonez (@devstonez)</a> for the tip.
