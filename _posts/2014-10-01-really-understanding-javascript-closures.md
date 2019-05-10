---
id: 1899
title: Really Understanding Javascript Closures
date: 2014-10-01T21:34:32+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=1899
permalink: /jhadesdev/really-understanding-javascript-closures/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3073397773
categories:
  - javascript
tags:
  - closure
  - example
  - intermediate
---
<p style="color: #3a4145; text-align: justify;">
  This post will explain in a simple way how Javascript Closures work. We will go over these topics and frequently asked questions:
</p>

<ul style="color: #3a4145;">
  <li>
    What is a Javascript Closure
  </li>
  <li>
    What is the reason behind the name &#8216;Closure&#8217;
  </li>
  <li>
    Actually viewing closures in a debugger
  </li>
  <li>
    how to reason about closures while coding
  </li>
  <li>
    the most common pitfalls of it&#8217;s use
  </li>
</ul>

<!--more-->

<h4 id="asimpleexamplebugincluded" style="color: #3a4145;">
  A Simple Example (bug included)
</h4>

<p style="color: #3a4145; text-align: justify;">
  The simplest way to understand closures is by realizing what problem they are trying to solve. Let&#8217;s take a simple code example with a counter being incremented 3 times inside a loop.
</p>

<p style="color: #3a4145; text-align: justify;">
  But inside the loop, something asynchronous is done with the counter. It could be that a server call was made, in this case let&#8217;s simply call <code>setTimeout</code> that will defer it&#8217;s execution until a timeout occurs:
</p>

<pre class="lang:js decode:true ">// define a function that increments a counter in a loop
function closureExample() {
 
    var i = 0;
 
    for (i = 0; i&lt; 3 ;i++) {    
        setTimeout(function() {
            console.log('counter value is ' + i);
        }, 1000);
    }
 
}
// call the example function
closureExample();</pre>

<p style="color: #3a4145;">
  Some things to bear in mind:
</p>

<ul style="color: #3a4145;">
  <li>
    the variable i exists in the scope of the <code>closureExample</code> function and is not accessible externally
  </li>
  <li>
    while looping through the variable the <code>console.log</code> statement is not immediately executed
  </li>
  <li>
    <code>console/log</code> will be executed asynchronously 3 times, and only after each timeout of 1 second elapses
  </li>
  <li>
    This means that 3 timeouts are set, and then the <code>closureExample</code>returns almost immediately
  </li>
</ul>

<p style="color: #3a4145;">
  Which leads us to the main question about this code:
</p>

<blockquote style="color: #3a4145;">
  <p style="font-style: italic;">
    When the anonymous logging function gets executed, how can it have access to the variable &#8216;i&#8217;?
  </p>
</blockquote>

<p style="color: #3a4145;">
  The question comes bearing in mind that:
</p>

<ul style="color: #3a4145;">
  <li>
    the variable i was <strong>not</strong> passed as an argument
  </li>
  <li>
    when the <code>console.log statement</code> gets executed, the <code>closureExample </code>function has long ended.
  </li>
</ul>

<h4 id="sowhatisaclosurethen" style="color: #3a4145;">
  So What is a Closure then?
</h4>

<p style="color: #3a4145;">
  When the logging function is passed to the <code>setTimeout</code> method, the Javascript engine detects that for the function to be executed in the future, a reference will be needed to variable i.
</p>

<p style="color: #3a4145;">
  To solve this, the engine keeps a link to this variable for later use, and stores that link in a special function scoped execution context.
</p>

<p style="color: #3a4145;">
  Such a function with &#8216;memory&#8217; about the environment where it was created is simply known as: a <strong>Closure</strong>.
</p>

<h4 id="whythenameclosurethen" style="color: #3a4145;">
  Why the name Closure then?
</h4>

<p style="text-align: justify;">
  This is because the function inspects it&#8217;s environment and <em>closes over </em>the variables that it needs to remember for future use. The references to the variables are <strong>closed</strong> in a special data structure that can only be accessed by the Javascript runtime itself.
</p>

<h4 id="isthereanywaytoseetheclosure" style="color: #3a4145;">
  Is there any way to See the Closure?
</h4>

The simplest way is to use the Chrome Developer Tools debugger, and set a breakpoint in line 7 of the code snippet above.

When the first timeout gets hit, the closure will show up in the Scope Variables panel of the debugger:

<img class="aligncenter" src="http://i.imgur.com/WMRwmsi.jpg" alt="Viewing the Javascript closure" />

As we can see, the closure is just a simple data structure with links to the variables that the function needs to &#8216;remember&#8217;, in this case the _i_ variable.

<h4 id="butthenwhereisthepitfall" style="color: #3a4145;">
  But then, where is the Pitfall?
</h4>

<p style="color: #3a4145;">
  We could expect that the execution log would show:
</p>

<pre class="lang:default decode:true ">counter value is 0
counter value is 1
counter value is 2</pre>

But the real execution log is actually:

<pre class="lang:default decode:true">counter value is 3
counter value is 3
counter value is 3</pre>

This is not a bug, it&#8217;s the way closures work. The logging function _is_ a closure (or _has_ a closure, as the term is used in both ways) containing a reference to the _i_ variable.

<p style="color: #3a4145;">
  This is a reference, and <strong>not</strong> a copy, so what happens is:
</p>

<ul style="color: #3a4145;">
  <li>
    the loop finishes and the <em>i</em> variable value is 3
  </li>
  <li>
    only later will the first timeout expire, and the logging function will log the value 3
  </li>
  <li>
    the second timeout expires, and the logging function still logs 3, etc.
  </li>
</ul>

<h4 id="howtohaveadifferentcountervalueperasyncoperation" style="color: #3a4145;">
  How to have a different counter value per async operation?
</h4>

<p style="color: #3a4145;">
  This can be done for example by creating a separate function to trigger the async operation. The following snippet would give the expected result:
</p>

<pre class="lang:js decode:true ">function asyncOperation(counter) {  
    setTimeout(function() {
        console.log('counter value is ' + counter);
    }, 1000);
}
 
function otherClosureExample() {  
    var i = 0;
 
    for (i = 0; i &lt; 3 ;i++) {    
        asyncOperation(i);
    }
}
 
otherClosureExample();</pre>

This works because when calling `asyncOperation` a copy is made of the _counter_ value, and the logging will &#8216;close over&#8217; that copied value. This means each invocation of the logging function will see a different variable with values 0, 1, 2.

<h4 id="conclusion" style="color: #3a4145;">
  Conclusion
</h4>

Javascript closures are a powerful feature that is mostly transparent in the day to day use of the language.

<p style="color: #3a4145;">
  They can be a convenient way to reduce the number of parameters passed to a function.
</p>

<p style="color: #3a4145; text-align: justify;">
  But mostly the fact that the closed variables are inaccessible to outside of the function makes closures a good way to achieve &#8216;private&#8217; variables and encapsulation in Javascript.
</p>

<p style="color: #3a4145; text-align: justify;">
  Mostly the feature &#8216;just works&#8217; and Javascript functions transparently remember any variables needed for future execution in a convenient way.
</p>

<p style="color: #3a4145; text-align: justify;">
  But beware of the pitfall: closures keep references and not copies (even of primitive values), so make sure that that is really the intended logic.
</p>

<p class="note_normal" style="color: #3a4145;">
  Published at Codingpedia.org with permission of Aleksey Novik &#8211; source <a title="http://blog.jhades.org/really-understanding-javascript-closures/" href="http://blog.jhades.org/really-understanding-javascript-closures/" target="_blank"><em>Really Understanding Javascript Closures</em></a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

<p style="color: #3a4145;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh6.googleusercontent.com/-nJLCOBcwQyQ/U3PTSOfhw_I/AAAAAAAAABI/w21JxlhW4lo/s498-no/my-blog-53.jpg" alt="Podcastpedia image" /> 
    
    <p id="about_author_header">
      <strong>Aleksey Novik</strong>
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Software developer, likes to learn new technologies, hang out on stackoverflow and blog on tips and tricks on Java/Javascript polyglot enterprise development.
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://blog.jhades.org/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/113901291479894108481/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/JhadesDev" target="_blank"> </a> <a class="icon-github" href="https://github.com/jhades" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>
