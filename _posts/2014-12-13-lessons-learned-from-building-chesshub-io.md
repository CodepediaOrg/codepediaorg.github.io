---
id: 2148
title: Lessons learned from building ChessHub.io
date: 2014-12-13T15:08:16+00:00
author: Mahmoud Ben Hassine
layout: post
guid: http://www.codingpedia.org/?p=2148
permalink: /mahmoudbenhassine/lessons-learned-from-building-chesshub-io/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Hide";s:10:"github-url";s:36:"https://github.com/benas/chesshub.io";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 0
fsb_social_google:
  - 0
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3319793837
categories:
  - java
  - javascript
tags:
  - expressjs
  - js
  - lessons learned
  - mistake
  - mvc
  - nodejs
  - spa
---
<p style="color: #404040; text-align: justify;">
  I spent most of my (little) career until now building web applications using the MVC pattern with action-based (Spring MVC, Struts2, Servelt/JSP) and component-based (Tapestry, JSF) Java technologies. It has always been a classical model where the client requests a server that responds with a HTML page. Of course, I have built ajax powered web apps but I never got the chance to develop a real time single page application with a modern Javascript framework like Angular, Backbone or Ember.
</p>

<p style="color: #404040; text-align: justify;">
  So I decided to give it a try by building <a style="font-weight: inherit; font-style: inherit; color: #117bb8;" href="https://github.com/benas/chesshub.io" target="_blank">ChessHub.io</a> with a completely new technology stack and way of thinking:
</p>

<ul style="color: #404040;">
  <li style="font-weight: inherit; font-style: inherit;">
    Moving form Java to Javascript
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Moving from SQL to NoSQL
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Moving from multi-threaded Java servers to the single threaded NodeJS
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Moving from classic MVC + Ajax to real time with ExpressJS and Socket IO
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Moving from synchronous processing to an asynchronous model
  </li>
</ul>

<!--more-->

<p style="color: #404040;">
  That was a real challenge since everything was new to me and I had to learn all these technologies at the same time. It was funny because I found the challenge easier than I expected it to be <span class="wp-smiley wp-emoji wp-emoji-smile" style="font-weight: inherit; font-style: inherit;" title=":-)">🙂</span>
</p>

<p style="color: #404040;">
  In this post, I will share my experience on building ChessHub.io and I will try to summarize some of the mistakes I made.
</p>

<p style="color: #404040;">
  First of all, let me begin with the positive parts:
</p>

<ul style="color: #404040;">
  <li style="font-weight: inherit; font-style: inherit; text-align: justify;">
    Using Javascript across all the stack is a real advantage: you have to master only one language
  </li>
  <li style="font-weight: inherit; font-style: inherit; text-align: justify;">
    Using JSON across all the stack makes it easy to pass data from one layer to the other. I remember the pain of mapping data with Dozer from the POJO to the DTO to the View and back again, arrrgh!
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Application packaging is much easier than the jar, war, ear files to deploy in a Java app server
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    You can forget about endless JVM tuning of your Java app server
  </li>
</ul>

<p style="color: #404040; text-align: justify;">
  That said, a stack like Node, Express, Mongo is not suitable for every application and is not intended to replace the good old Java app server backed by a relational database. Here are some use cases where Node JS is a perfect choice:
</p>

<ul style="color: #404040;">
  <li style="font-weight: inherit; font-style: inherit;">
    Real time data intensive apps with many concurrent users : games, chat, social networks
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    System/Application monitoring dashboard
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Data streaming apps
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    Server side proxy
  </li>
</ul>

<p style="color: #404040; text-align: justify;">
  In contrast, Node is not suitable for CPU intensive apps since it is single threaded.<br /> In the same way, Mongo DB is not suitable for apps that require transactions, in such cases, you can keep the RDBMS you love.
</p>

<p style="color: #404040; text-align: justify;">
  One of the mistakes I made was to think synchronous using Node JS. Let me show you an example.<br /> In ChessHub.io, I want to show a random quote from a chess master on the home page, so here is a pseudo code I wrote in the very first stage:
</p>

<pre class="lang:js decode:true ">quote = loadRandomQuoteFromDb();
showQuoteOnHomePage(quote);</pre>

<p style="text-align: justify;">
  <span style="color: #404040;">This is actually a blocking snippet since it will wait the database to respond with the quote to be able to continue to the next instruction. When you spend several years thinking synchronous, it is natural to make such a mistake. After investigation , I realized that this way of thinking when using Node is completely wrong. A asynchronous version of this snippet is:</span>
</p>

<pre class="lang:js decode:true ">loadRandomQuoteFromDb(quote, function() {
 showQuoteOnHomePage(quote);
});</pre>

<p style="color: #404040; text-align: justify;">
  In this configuration, if the database take some time to load data, Node can continue serving other requests and when the data is ready, the callback function will be called asynchronously.
</p>

<p style="color: #404040; text-align: justify;">
  That was my retrospective on building ChessHub.io. If you got the same experience, do not hesitate to share your thoughts in comments.
</p>

<p style="color: #404040;">
  See you soon <span class="wp-smiley wp-emoji wp-emoji-smile" style="font-weight: inherit; font-style: inherit;" title=":-)">🙂</span>
</p>

<p style="color: #404040;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://pbs.twimg.com/profile_images/682858329220190208/YhIK6TIE_400x400.jpg" alt="Mahmoud Ben Hassine" /> 
    
    <p id="about_author_header">
      Mahmoud Ben Hassine
    </p>
    
    <div id="author_details" style="text-align: justify;">
      Passionate Software Engineer who loves to learn and teach. As an open source advocate, I love to create and contribute to open source projects and to share my experience with other developers around the globe. If you are a chess Junkie like me, it will be a pleasure to challenge you on a chess board!
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div id="social_logos">
        <a class="icon-earth" href="http://benas.github.io/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/_benas_" target="_blank"> </a> <a class="icon-github" href="https://github.com/benas" target="_blank"> </a> <a class="icon-linkedin" href="http://fr.linkedin.com/in/mahmoudbenhassine/" target="_blank"> </a>
      </div>
      
      <div class="clear">
      </div>
    </div>
  </div>
</p>

&nbsp;
