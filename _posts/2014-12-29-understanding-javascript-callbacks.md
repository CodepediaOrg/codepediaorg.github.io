---
id: 2157
title: Understanding JavaScript Callbacks
date: 2014-12-29T21:28:27+00:00
author: Christopher Buecheler
layout: post
guid: http://www.codingpedia.org/?p=2157
permalink: /cwbuecheler/understanding-javascript-callbacks/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3366780612
fsb_social_facebook:
  - 3
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - asynchronous
  - callback
  - javascript
  - jQuery
  - js
---
<h2 class="subtitle" style="text-align: center;">
  Unlocking the Asynchronous Powers of JavaScript
</h2>

<p style="text-align: justify;">
  When I got started <a title="Tutorial - Getting Started With Node.js, Express, MongoDB by Christopher Buecheler" href="http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/">working with Node.js and Express</a>, one of the first things I had to really wrap my head around was JavaScript callbacks. This is a powerful functionality built into the language that allows you to defer action until a desired event occurs, while proceeding on with other activities.
</p>

<p style="text-align: justify;">
  The truth is, I&#8217;ve been using callbacks for a long time, I just didn&#8217;t really realize it. I work with jQuery constantly, and it&#8217;s designed with callbacks in mind, passing anonymous functions via its built-in methods to be triggered when certain events occur, for example. Still, I wanted to move beyond &#8220;I know this works&#8221; to &#8220;I know how this works&#8221; both with jQuery and JavaScript, so I dug out my copy of <a title="JavaScript: The Good Parts by Douglas Crockford" href="http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742" target="_blank">JavaScript: The Good Parts</a>, did some reading, hit the web, did some more reading, and then wrote a few simple experiments.
</p>

I thought I&#8217;d write up a little tutorial to help folks who are new to the concept get started. Hope it&#8217;s useful!<!--more-->

### Life Without Callbacks

Let&#8217;s start with a very simple script that uses no callbacks. First we&#8217;ll establish a super-quick HTML structure to output results to:

<pre class="lang:xhtml decode:true ">&lt;html&gt;
    &lt;body&gt;
        &lt;p id="output"&gt;
        &lt;/p&gt;
    &lt;/body&gt;
&lt;/html&gt;</pre>

And then we&#8217;ll add a bit of JavaScript:

<pre class="lang:js decode:true ">document.getElementById('output').innerHTML += ('starting ...');
document.getElementById('output').innerHTML += ('continuing ...');
document.getElementById('output').innerHTML += ('ending!');</pre>

<p style="text-align: justify;">
  <a title="Understanding JavaScript Callbacks - Example 1" href="http://jsfiddle.net/cwbuecheler/RBJ76/1/" target="_blank">Go ahead and run it for yourself</a>. This is tremendously boring code, and it could also be shortened if we were working with jQuery, but it gets the point across. Things in JavaScript typically execute sequentially. Let&#8217;s really hammer this home by <a title="Understanding JavaScript Callbacks - Example 2" href="http://jsfiddle.net/cwbuecheler/rFKmj/" target="_blank">putting the ending in the middle</a>:
</p>

<pre class="lang:js decode:true ">document.getElementById('output').innerHTML += ('starting ...');
document.getElementById('output').innerHTML += ('ending!');
document.getElementById('output').innerHTML += ('continuing ...');</pre>

<p style="text-align: justify;">
  So, great … as you can see when you run this, it prints &#8220;ending!&#8221; before &#8220;continuing …&#8221; pretty much exactly as expected. And no good. We don&#8217;t want to end in the middle. We want to end at the end.
</p>

So let&#8217;s do that.

### setTimeout() – My First Callback

<p style="text-align: justify;">
  JavaScript has a function for delayed execution of commands. You give it a command to run, and the number of milliseconds to wait before running it. It&#8217;s handy for a variety of reasons, but its value here is that it inherently uses callback functionality to work its magic. Here&#8217;s our new code:
</p>

<pre class="lang:js decode:true ">document.getElementById('output').innerHTML += ('starting ...');
var myTimer = window.setTimeout(function() {
    document.getElementById('output').innerHTML += ('ending!');
}, 500);
document.getElementById('output').innerHTML += ('continuing ...');</pre>

<p style="text-align: justify;">
  <a title="Understanding JavaScript Callbacks - Example 3" href="http://jsfiddle.net/cwbuecheler/fgUFF/" target="_blank">Check that out</a>. Now we&#8217;re back to having the right order in our output, even though the code&#8217;s not sequential, because we&#8217;re waiting half a second before executing an anonymous function that writes our &#8220;ending!&#8221; string. Finding the anonymous function thing confusing? You could also write the code like this:
</p>

<pre class="lang:js decode:true ">document.getElementById('output').innerHTML += ('starting ...');

// Wait half a second before firing the writeEnding() function
var myTimer = window.setTimeout(writeEnding, 500);
document.getElementById('output').innerHTML += ('continuing ...');

// Define the Write Ending Function
function writeEnding() {
    // Write "ending!"
    document.getElementById('output').innerHTML += ('ending!');
}</pre>

Same exact thing, but now the function&#8217;s not anonymous because we declared it at the bottom. We&#8217;re still calling it with `setTimeout`, though, after 500 milliseconds. This is nice for re-use, obviously, and is also often handy for keeping code readable (which is important, especially if you work with multiple developers).

### Creating Our Own Callbacks

We&#8217;ve established that `setTimeout` uses a callback to enact a delayed execution of a function, but how do we write one of our own? Easy! Like this:

<pre class="lang:js decode:true ">// Call our main function. Pass it a URI and a callback function
getData('http://fakedomain1234.com/userlist', writeData);

// Write some stuff to the p tag
document.getElementById('output').innerHTML += 'show this before data ...';

// Define our main function
function getData(dataURI, callback) {

    // Normally you would actually connect to a server here.
    // We're just going to simulate a 3-second delay.
    var timer = setTimeout(function () {

    	// Here's some data which we're pretending came from dataURI
        var dataArray = [123, 456, 789, 012, 345, 678];

    	// run our callback function
        callback(dataArray);

    }, 3000);
}

function writeData(myData) {
    document.getElementById('output').innerHTML += myData;
}</pre>

<p style="text-align: justify;">
  If you <a title="Understanding JavaScript Callbacks - Example 4" href="http://jsfiddle.net/cwbuecheler/Y9Ca8/" target="_blank">run this in jsFiddle</a>, you&#8217;ll see it behaves just as we want it to: even though the <code>getData</code> function is the first thing called, and takes three seconds to run, the script continues right along. When the three seconds are up and <code>getData</code> responds with data, then <code>writeData</code> fires and writes the data.
</p>

<p style="text-align: justify;">
  Obviously we&#8217;re still using a <code>setTimeout</code> here, but that&#8217;s not required for callbacks, it&#8217;s just hard to simulate long delays in a tutorial setting without it. Here is a <strong>non-working</strong> example that uses a fake data call, so you can get an idea for what it would look like:
</p>

<pre class="lang:js decode:true">getData('http://fakedomain1234.com/userlist', writeData);

document.getElementById('output').innerHTML += "show this before data ...";

function getData(dataURI, callback) {
    var myData = getSomeData(); // fake function
    callback(myData);
}

function writeData(myData) {
    document.getElementById('output').innerHTML += myData;
}</pre>

<p style="text-align: justify;">
  The key thing to understand here is that you don&#8217;t have to be using AJAX. Merely by adding callback functionality to your JavaScript, it automatically becomes asynchronous. This has far-reaching value, particularly when you&#8217;re working with Node.js.
</p>

<h3 style="text-align: justify;">
  A Brief Caveat
</h3>

<p style="text-align: justify;">
  It&#8217;s important to note that callbacks <strong>do not have to be asynchronous</strong>. You can use them just as easily in sequential code. In fact, it&#8217;s best not to rely on callbacks for a delay, unless you&#8217;re manually controlling that delay. For example, let&#8217;s use our code above but <strong>without</strong> forcing a delay:
</p>

<pre class="lang:js decode:true ">getData('http://fakedomain1234.com/userlist', writeData);

document.getElementById('output').innerHTML += "show this before data ...";

function getData(dataURI, callback) {
    var dataArray = [123, 456, 789, 012, 345, 678];
    callback(dataArray);
}

function writeData(myData) {
    document.getElementById('output').innerHTML += myData;
}</pre>

<p style="text-align: justify;">
  <a title="Understanding JavaScript Callbacks - Example 5" href="http://jsfiddle.net/cwbuecheler/upQuM/" target="_blank">Go ahead and run that</a>. Whoops … it&#8217;s not behaving right. That&#8217;s because since we&#8217;re not forcing a delay with <code>setTimeout</code>, or doing something that would naturally cause a delay (like connecting to a DB and snagging a whole big chunk of data), the function is executing basically instantaneously, and calling the callback.
</p>

<p style="text-align: justify;">
  This is why it&#8217;s better to rely on callbacks for events than for actual timing purposes. If you really want to introduce specific, milisecond-based actions to your code, use setTimout and its brother <code>setInterval</code> (which will repeatedly fire a callback function rather than just doing so once). Use callbacks when you want to fire something off, move on to other things, and then react when what you originally started is finished.
</p>

### OK, callbacks are nifty. Why do I care?

<p style="text-align: justify;">
  If you&#8217;re working with Node, you care because the entire platform is built around the callback concept. Everything Node does is asynchronous. You fire a command, and move on to other commands, and when your first command is finished executing it calls a callback. If you don&#8217;t code your Node apps this way, you&#8217;re basically nullifying one of the two biggest advantages Node has (those two being: its async nature, and the fact that it&#8217;s all JavaScript and thus nearly every single web developer can code for it).
</p>

<p style="text-align: justify;">
  If you lace your Node app with blocking code – code that doesn&#8217;t use callbacks and thus forces everything else to sit and wait as it executes – your app is going to be slow and clunky. This is exactly what you <em>don&#8217;t</em> want, so don&#8217;t do it! Make sure you&#8217;re using callbacks for pretty much everything.
</p>

<p style="text-align: justify;">
  In addition to helping with node, understanding callbacks (and &#8216;event based programming&#8217;) will make interacting with frameworks like jQuery much more clear. Let&#8217;s take a look at a very simple, very common jQuery action:
</p>

<pre class="lang:js decode:true ">$('body').on('click', 'p#output', function() {
    alert('You clicked the output paragraph.');
});</pre>

<p style="text-align: justify;">
  <a title="Understanding JavaScript Callbacks - Example 6" href="http://jsfiddle.net/cwbuecheler/YTWdn/" target="_blank">Here&#8217;s that super-exciting script in jsFiddle just for fun</a>. You see the callback happening here, right? It&#8217;s the anonymous function that triggers the alert. This is essentially how all jQuery events work: listen for an event, and when it happens, run the callback function, and in the meantime don&#8217;t hold up any other JS on the page. If you want a named function, you could just as easily do:
</p>

<pre class="lang:js decode:true ">$('body').on('click', 'p#output', showAlert);

function showAlert() {
    alert('You clicked the output paragraph');
}</pre>

Same deal either way, whether the callback function is declared or anonymized.

<p style="text-align: justify;">
  That&#8217;s really all there is to callbacks. They&#8217;re really simple once you get used to them. JavaScript: The Good Parts only gives them about a page and a half total because there&#8217;s not that much to talk about. The most important thing is just to remember to use them. Your code will be speedier, you&#8217;ll be better-prepared to take advantage of event-based frameworks, and you&#8217;ll have an easier time understanding and working with things like the MEAN stack (<a href="http://www.mongodb.org/" target="_blank">Mongo</a>, <a href="http://expressjs.com/" target="_blank">Express</a>, <a href="http://angularjs.org/" target="_blank">Angular</a>, <a href="http://nodejs.org/" target="_blank">Node</a>).
</p>

Hope this helps! Any questions? Drop me a line or comment in the discussion below.

<p class="note_normal" style="text-align: justify;">
  Published on Codingpedia.org with the permission of Christopher Buecheler – source <a title="http://cwbuecheler.com/web/tutorials/2013/javascript-callbacks/" href="http://cwbuecheler.com/web/tutorials/2013/javascript-callbacks/" target="_blank">UNDERSTANDING JAVASCRIPT CALLBACKS</a> from <a title="http://cwbuecheler.com/" href="http://cwbuecheler.com/" target="_blank">http://cwbuecheler.com/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/cwbuecheler.png" alt="Johannes Brodwall" /> 
  
  <p id="about_author_header">
    Christopher Buecheler</strong>
  </p>
  
  <div id="social_logos_up">
    <a class="icon-earth" href="http://cwbuecheler.com/" target="_blank"> </a> <a class="icon-twitter" href="http://twitter.com/cwbuecheler" target="_blank"> </a>
  </div>
  
  <div id="author_details" style="text-align: justify;">
    Christopher Buecheler has been designing and developing web pages professionally since 1997. He's worked as the principal front-end developer for four startups: GameSpy Industries, OkCupid, CrispyGamer, and GoldenSpear. A self-taught programmer, he understands the value of a good tutorial, and has tried to give a few back to the community. In addition to web work, he's independently published four novels and is currently working with his agent to find a publisher for his fifth. He also writes cocktail articles for Primer Magazine and runs a cocktail blog at DrinkShouts.com. He lives in Providence, Rhode Island, with his amazing French wife and their two cats.
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div class="clear">
    </div>
  </div>
</div>

&nbsp;

&nbsp;
