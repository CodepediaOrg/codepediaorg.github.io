---
id: 2570
title: Introduction to the Jspm package manager and the SystemJs module loader
date: 2016-01-11T16:35:30+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2570
permalink: /jhadesdev/introduction-to-the-jspm-package-manager-and-the-systemjs-module-loader/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4315684752
fsb_social_facebook:
  - 0
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 3
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - bower
  - es6
  - javascript
  - jspm
  - package manager
  - systemjs
---
<p style="text-align: justify;">
  In this post we will go over the current state of ES6 modularity, by learning how to use the Jspm package manager and its associated SystemJs module loader (the sample code is available <a href="https://github.com/jhades/blog.jhades.org/tree/master/systemjs-introduction">here</a>). We will go through the following topics:
</p>

<ul style="text-align: justify;">
  <li>
    Using ES6 modules today
  </li>
  <li>
    Why is modularity important in Javascript
  </li>
  <li>
    The Jspm package manager and how to use it today
  </li>
  <li>
    creating a ready to use bundle with Jspm
  </li>
  <li>
    The SystemJs module loader
  </li>
  <li>
    Jspm vs Bower
  </li>
  <li>
    Jspm vs npm
  </li>
  <li>
    Conclusions
  </li>
</ul>

<p style="text-align: justify;">
  <!--more-->
</p>

<h3 id="usinges6modulestoday" style="text-align: justify;">
  Using ES6 modules today
</h3>

<p style="text-align: justify;">
  One of the main goals of ES6 modularity is to make it really simple to install and use any Javascript library from anywhere on the Internet (github, npm, etc.). Only two things are needed:
</p>

<ul style="text-align: justify;">
  <li>
    a single command to install the library
  </li>
  <li>
    one single line of code to import the library and use it
  </li>
</ul>

<p style="text-align: justify;">
  Imagine a world where you could install a library with a command like this:
</p>

<pre class="lang:default decode:true">jspm install jquery</pre>

<div class="fix-syntax-highlight" style="text-align: justify;">
</div>

<p style="text-align: justify;">
  And then import the library with a single line of code, and just start using it:
</p>

<pre class="lang:default decode:true">var $ = require('jquery'); 

$('body').append("I've imported jQuery!");</pre>

<p style="text-align: justify;">
  It turns out that this is actually possible <strong>today</strong> with Jspm and its associated SystemJs module loader, without any further tooling needed.
</p>

<p style="text-align: justify;">
  Note that the import above can also be done in the ES6 syntax if using a transpiler like Babel:
</p>

<pre class="lang:default decode:true">import $ from 'jquery';</pre>

<p style="text-align: justify;">
  We will see how all this works, and how its really easy to use.
</p>

<h3 id="whyismodularityimportantinjavascript" style="text-align: justify;">
  Why is modularity important in Javascript
</h3>

<p style="text-align: justify;">
  Modularity is important in Javascript by the same reasons its important in any other language:
</p>

<ul style="text-align: justify;">
  <li>
    it encourages the development of small isolated modules with clear interfaces, as opposed to large chunks of monolithic code
  </li>
  <li>
    it helps with testability, as modules can be replaced at runtime with mocks that implement the same interface
  </li>
  <li>
    it improves code maintainability, as smaller and well isolated modules are easier to understand
  </li>
</ul>

<h3 id="fundamentalproblemsthatjspmsolves" style="text-align: justify;">
  Fundamental problems that Jspm solves
</h3>

<p style="text-align: justify;">
  When projects get to a certain size, they will have to deal sooner or later with the &#8216;dependency hell&#8217; problem. This happens when two different versions of the same library are needed at the same time.
</p>

<p style="text-align: justify;">
  Another problem related to dependency management is making sure we always have a valid combination of library versions, meaning that if we require a library that itself has a dependency, that transitive dependency should be downloaded and included transparently for us.
</p>

<p style="text-align: justify;">
  All these problems are handled transparently by Jspm: it allows multiple versions of the same package and it even supports circular dependencies.
</p>

<h3 id="thejspmpackagemanagerinaction" style="text-align: justify;">
  The Jspm package manager in action
</h3>

<p style="text-align: justify;">
  As we mentioned, in order to install a library we only need one command. Libraries can be installed from multiple sources, for example from github or npm. Let&#8217;s install a couple of libraries:
</p>

<pre class="lang:default decode:true">jspm install npm:timezone-js
jspm install github:pablojim/highcharts-ng</pre>

<p style="text-align: justify;">
  The installation command takes care of downloading the whole dependency tree, and copy it into the file system under the form of a versioned flat tree that looks like this:
</p>

<pre class="lang:default decode:true">babel-core@5.8.25
babel-runtime@5.8.25
core-js@1.1.4</pre>

<h3 id="creatingareadytousebundlewithjspm" style="text-align: justify;">
  Creating a ready to use bundle with Jspm
</h3>

<p style="text-align: justify;">
  Jspm not only installs dependencies, but it also knows about module loading and bundling. This might seem odd at first, as those tasks are usually handled by different tools. But the result is very powerful: we can create a ready to use javascript bundle with one command:
</p>

<pre class="lang:default decode:true">jspm bundle-sfx --minify src/main bundle.min.js</pre>

<p style="text-align: justify;">
  This command creates a self-executing and minified bundle, where the execution entry point is the file <code>src/main.js</code>. Jspm will follow the imports on that file and recursively bundle all the needed Javascript files.
</p>

<p style="text-align: justify;">
  The result is a ready to use bundle that can be used on a web page simply like this:
</p>

<pre class="lang:default decode:true">&lt;script src="bundle.min.js"&gt;&lt;/script&gt;</pre>

<p style="text-align: justify;">
  Notice that we managed to use all our dependencies without even thinking about configuring a module loader.
</p>

<h3 id="whataboutsystemjs" style="text-align: justify;">
  What about SystemJs?
</h3>

<p style="text-align: justify;">
  Did you notice that so far the SystemJs module loader is nowhere to be seen? This is because Jspm knows about SystemJs, and will transparently install it, include it in the self-executing bundle and use it to load the entry point of the application.
</p>

<p style="text-align: justify;">
  This is very powerful: we can use ES6 modules in production today in a transparent way. It all just works, even in a ES5-only project (using the require syntax).
</p>

<h3 id="whatisthesystemjsmoduleloader" style="text-align: justify;">
  What is the SystemJs module loader?
</h3>

<p style="text-align: justify;">
  SystemJs is an universal module loader capable of loading modules of different formats: AMD, CommonJs, globals. It works both in node and in the browser.
</p>

<p style="text-align: justify;">
  SystemJs is composed of an ES6 module loader polyfill and a compatibility layer thats allows it to use different module formats. SystemJs can be used independently of Jspm, but the two tools are really best used together.
</p>

<p style="text-align: justify;">
  Using SystemJs independently of Jspm does not provide so many advantages over other solutions like <strong>Bower + Browserify</strong> or <strong>Bower + Webpack</strong>. You would still have to download the dependencies using another package manager and configure a build tool to create a bundle. Also you would need to install the module loader and learn how to configure it and use it.
</p>

<p style="text-align: justify;">
  With Jspm, we basically can more or less forget that there is a module loader being used, and simply use the ES6 import syntax (or the ES5 equivalent).
</p>

<h3 id="jspmvsbower" style="text-align: justify;">
  Jspm vs Bower
</h3>

<p style="text-align: justify;">
  Jspm and Bower where built with very different philosophies. Bower is based on the assumption that there can only be one version of a library at the same time in the browser.
</p>

<p style="text-align: justify;">
  Bower will download transitive dependencies into a flat tree, but if two versions of the same library are required it will ask the user which one it should choose. Bower allows to save that decision into <code>bower.json</code> so that other users can have reproducible builds.
</p>

<p style="text-align: justify;">
  Jspm is designed for a world were developers build many small different frontend modules that get reused a lot, similarly to what happens in the npm world. To handle this, multiple versions of the same module can be loaded independently without conflict, but the user ultimately keeps control on those decisions if he wants to. SystemJs can even handle circular library references!
</p>

<h3 id="jspmvsnpm" style="text-align: justify;">
  Jspm vs npm
</h3>

<p style="text-align: justify;">
  Having said all that, the tendency is that npm becomes more and more the package manager for all Javascript in general, including for the frontend.
</p>

<p style="text-align: justify;">
  More and more frontend libraries are getting published to npm with meaningful <strong>CommonJs</strong> exports, but those libraries cannot be used in node as they often require a DOM. The goal is to have a CommonJs module easily installable so that frontend libraries can be for example consumed by <strong>browserify</strong>, or even SystemJs and used in the browser instead of node. One frontend library that gets published to npm is for example <strong>AngularJs</strong>.
</p>

<p style="text-align: justify;">
  The npm team is planning to make npm more frontend friendly. The Angular team made or will make npm a concrete <a href="https://drive.google.com/folderview?id=0B7Ovm8bUYiUDR29iSkEyMk5pVUk&usp=drive_web&urp=https://angular.io/&pli=1&tid=0BxgtL8yFJbacUnUxc3l5aTZrbVk">proposal</a> for how that could be achieved.
</p>

<p style="text-align: justify;">
  Npm just released version 3 a couple of weeks ago and only in this latest version they implemented a maximally flat dependency tree. This is certainly a good first step to help cover frontend modularity, but Jspm has a huge head start on this and has a lot of momentum behind it.
</p>

<h3 id="conclusions" style="text-align: justify;">
  Conclusions
</h3>

<p style="text-align: justify;">
  Jspm allows for ES6 modules to already be used today, even with an ES5 syntax if needed. Although the SystemJs module loader can be used separately, you really get the most benefit if you use it transparently via Jspm.
</p>

<p style="text-align: justify;">
  Conceptually and feature-wise, Jspm and SystemJs are way ahead of other similar tools. Not only Jspm/SystemJs have more features but they are actually much simpler to use, especially in the self-executing bundle scenario presented above.
</p>

<h3 id="relatedlinks" style="text-align: justify;">
  Related Links
</h3>

<p style="text-align: justify;">
  The following post provides an in-depth comparison of Bower, Jspm and npm &#8211; <a href="http://www.symbiotics.co.za/blog/knowledge-share-2/post/is-bower-dead-what-is-jspm-npm-for-client-side-92">Is Bower dead? What is JSPM? Npm for client-side?</a>
</p>

<p style="text-align: justify;">
  This talk from the creator of Jspm, SystemJs and the ES6 module loader polyfill Guy Bedford provides an in-depth dive into Jspm and SystemJs &#8211;<a href="https://www.youtube.com/watch?t=3&v=szJjsduHBQQ">Package Management for ES6 Modules</a>
</p>

<p style="text-align: justify;">
  Also, the code in this post can be found <a href="https://github.com/jhades/blog.jhades.org/tree/master/systemjs-introduction">here</a>, with running instructions for seeing Jspm in action for the first time.
</p>

<p style="text-align: justify;" class="note_normal">
  Published at Codingpedia.org with permission of Aleksey Novik – source <em><a title="The main goals of Angular 2 and how they will be achieved" href="http://blog.jhades.org/introduction-to-es6-modularity-the-jspm-package-manager-and-the-systemjs-loader/" target="_blank">Introduction to the Jspm package manager and the SystemJs module loader</a></em> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/
  
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
  </div></a>
</p>
