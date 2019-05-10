---
id: 2503
title: 'AngularJS Tutorial: A Beginner’s Guide to AngularJS'
date: 2015-10-20T15:12:59+00:00
author: Udemy tutorials
layout: post
guid: http://www.codingpedia.org/?p=2503
permalink: /udemy/angularjs-tutorial-a-beginners-guide-to-angularjs/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4242389456
fsb_social_facebook:
  - 3
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 8
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - angular
  - angularJS
  - beginner
  - tutorial
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Introduction">Introduction</a><ul>
        <li>
          <a href="#History_is_important">History is important</a>
        </li>
        <li>
          <a href="#MV">MV*</a>
        </li>
        <li>
          <a href="#Setup">Setup</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directives">Directives</a><ul>
        <li>
          <a href="#ng-app">ng-app</a>
        </li>
        <li>
          <a href="#ng-controller">ng-controller</a>
        </li>
        <li>
          <a href="#ng-bind">ng-bind</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Bindings">Bindings</a><ul>
        <li>
          <a href="#Expressions">Expressions</a>
        </li>
        <li>
          <a href="#Two-way_bindings">Two-way bindings</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directives_again">Directives again</a><ul>
        <li>
          <a href="#ng-repeat">ng-repeat</a><ul>
            <li>
              <a href="#limitTo_filter">limitTo filter</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#Custom_directives">Custom directives</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Injection">Injection</a><ul>
        <li>
          <a href="#Minimal_theory">Minimal theory</a>
        </li>
        <li>
          <a href="#Dependency_annotations">Dependency annotations</a>
        </li>
        <li>
          <a href="#Custom_injectables">Custom injectables</a><ul>
            <li>
              <a href="#Value_services">Value services</a>
            </li>
            <li>
              <a href="#Factory_services">Factory services</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directivescontinued">Directives continued</a><ul>
        <li>
          <a href="#ng-click">ng-click</a>
        </li>
        <li>
          <a href="#ng-checked">ng-checked</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Server_integration">Server integration</a><ul>
        <li>
          <a href="#http">$http</a>
        </li>
        <li>
          <a href="#httpBackend">$httpBackend</a>
        </li>
        <li>
          <a href="#Promises">Promises</a>
        </li>
        <li>
          <a href="#resource">$resource</a>
        </li>
        <li>
          <a href="#httpBackend_again">$httpBackend again</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Summary">Summary</a>
    </li>
  </ul>
</div>

## <span id="Introduction">Introduction</span>

<p style="text-align: justify;">
  Since I first started using AngularJS several years ago, it has become the most popular single-page application (SPA) framework for the JavaScript language. AngularJS makes it easy to solve simple problems fast, yet has enough features to enable development teams to build complex large-scale applications.
</p>

<p style="text-align: justify;">
  This tutorial aims to introduce a complete beginner to AngularJS by explaining fundamental concepts and building a sample application. The application will be a simplified admin area for a multi-author blog.<!--more-->
</p>

### <span id="History_is_important"><a id="1_1"></a>History is important</span> {#historyisimportant}

<p style="text-align: justify;">
  I believe a limited appreciation of history is important. Often in software, good ideas are a twist or improvement on something that has been done before, made possible by some other technology improvement(s). In the case of AngularJS, the vast performance improvements made to browser JavaScript engines has made ambitious SPA frameworks possible.
</p>

<p style="text-align: justify;">
  AngularJS is an adaptation of the Model-View-Controller (MVC) pattern first created in 1979. MVC helps developers to separate the concerns involved in building user interfaces. Jeff Atwood of Coding Horror <a href="http://blog.codinghorror.com/understanding-model-view-controller/">wrote of an example</a> that we’re all very familiar with, a static webpage:
</p>

<li style="text-align: justify;">
  HTML acting as the model, the content (data) for the page
</li>
<li style="text-align: justify;">
  CSS acting as the view, how that content should be presented
</li>
<li style="text-align: justify;">
  The browser acting as the controller, combining the model and the view to render the page
</li>

### <span id="MV"><a id="1_2"></a>MV*</span> {#mv}

<p style="text-align: justify;">
  Subtle variations to the MVC pattern have come in recent years, none more so than by JavaScript SPA frameworks. For this reason, the MV* phrase has been coined as a way of describing ‘whatever’ variation works for you.
</p>

<p style="text-align: justify;">
  So although it’s definitely worth understanding the origins of MVC, keep in mind that AngularJS is MV* and does not implement MVC strictly.
</p>

### <span id="Setup"><a id="1_3"></a>Setup</span> {#setup}

<p style="text-align: justify;">
  This tutorial will use the stable version of AngularJS at the time of writing (1.4.4). It also intends to only include the bare minimum required to run an AngularJS application.
</p>

<p style="text-align: justify;">
  To get up and running, download an uncompressed copy of AngularJS from <a href="https://angularjs.org/">angularjs.org</a>. Create a file structure as follows with your favorite text editor:
</p>

<p style="text-align: center;">
  <img class="size-full wp-image-147176 aligncenter" src="https://blog.udemy.com/wp-content/uploads/2015/08/initial-file-layout.png" alt="initial-file-layout" width="362" height="136" /><em>Initial file layout</em>
</p>

Edit the contents of `index.html` as follows:

<pre class="lang:xhtml decode:true " title="index.html">&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  Note that in the above snippet we’re serving <code>angular.js</code> from the local file system. This saves us from having to introduce any additional moving parts and is adequate for our purposes. Open <code>index.html</code> in your favorite browser and, although you’ll see nothing but a white screen, check to make sure that you don’t have any errors in your browser’s developer tools/console.
</p>

We will add additional `.js` files as we progress through the tutorial.

## <span id="Directives"><a id="2"></a>Directives</span> {#directives}

<p style="text-align: justify;">
  Considering the fact that I discussed MVC in the introduction, it would be very tempting to structure this tutorial by the AngularJS implementation of models, views and controllers. But there is a construct so fundamental to AngularJS that to not start with it would be an error.
</p>

<p style="text-align: justify;">
  That construct is directives. AngularJS describes itself as “HTML enhanced for web apps.” Directives are what facilitate this enhancement; they extend the capability of HTML elements.
</p>

<p style="text-align: justify;">
  I often find analogies to be a useful tool for explaining abstract concepts, such as directives. Consider a homeowner who is having some building work completed on their house. The team of laborers required to complete any non-trivial building project is varied in role and might include:
</p>

  * a bricklayer
  * an outfitter
  * a plumber

The types of capability directives extend HTML elements with can be categorized similarly:

  * structural
  * decorative
  * plumbing

If we keep the analogy going a moment longer, HTML is the house and directives are the team of laborers.

Previously I introduced a sample application that we will build during this tutorial. Let’s build upon that by displaying an article title contained within a plain JavaScript object:

<pre class="lang:js decode:true ">var article = {
  title: "Learn AngularJS"
};</pre>

Edit `index.html` to contain the `ng-app`, `ng-controller` and `ng-bind` directives as follows:

<pre class="lang:xhtml decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;
    &lt;div ng-bind="title"&gt;&lt;/div&gt;
    &lt;div ng-bind="getTitle()"&gt;&lt;/div&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  You’ll notice that two <code>.js</code> files have appeared. There is a small amount of JavaScript required to make the article title appear in the <code>&lt;div&gt;</code>s and <code>app.js</code> and <code>controllers.js</code> is where it happens. Before we create them, I want to briefly discuss the three directives introduced in the above snippet.
</p>

### <span id="ng-app"><a id="2_1"></a>ng-app</span> {#ng-app}

<p style="text-align: justify;">
  <code>ng-app</code> is a plumbing directive. AngularJS looks for which HTML element has the <code>ng-app</code> directive attached to it and uses it to bootstrap an application. <code>ng-app</code> can be placed on any HTML element, not just <code>&lt;html&gt;</code>, and connects a module (in our case, ‘udemyAdmin’) to it. Modules will be covered shortly.
</p>

### <span id="ng-controller"><a id="2_2"></a>ng-controller</span> {#ng-controller}

`ng-controller` is a plumbing directive. It connects a model and any related behaviors to a branch of the HTML tree made available by the usage of `ng-app`.

### <span id="ng-bind"><a id="2_3"></a>ng-bind</span> {#ng-bind}

<p style="text-align: justify;">
  <code>ng-bind</code> is a plumbing directive. It connects a model property or the result of a model behavior to the text content of an HTML element. In our example, the <code>title</code> property and <code>getTitle</code> behaviour is available because <code>ng-bind</code> has been used on a <code>&lt;div&gt;</code> which is a child of <code>&lt;body&gt;</code> (where <code>ng-controller</code> has been used).
</p>

Edit the file structure to match the following:

<p style="text-align: center;">
  <img class="size-full wp-image-147178 aligncenter" src="https://blog.udemy.com/wp-content/uploads/2015/08/second-file-layout.png" alt="second-file-layout" width="426" height="214" /><em>New file layout</em>
</p>

Edit the contents of `app.js` as follows:

<pre class="lang:js decode:true">angular.module('udemyAdmin', []);</pre>

<p style="text-align: justify;">
  In the above snippet, we first create a module called ‘udemyAdmin’. This module is created with no dependencies (denoted by the empty array). Modules allow us to partition different parts of an application anytime we wish; this is useful in larger applications. For example, a full Udemy blog application might be partitioned as follows:
</p>

<pre class="lang:js decode:true ">angular.module('udemy', ['udemy.courses', 'udemy.authors', 'udemy.students']);
angular.module('udemy.courses', ['udemy.courses.archived', 'udemy.courses.current']);
// and so on</pre>

Edit the contents of `controllers.js` as follows:

<pre class="lang:js decode:true">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  var title = "Learn AngularJS";

  $scope.title = title;
  $scope.getTitle = function() {
    return title;
  };
});</pre>

<p style="text-align: justify;">
  In the above snippet, we first retrieve the ‘udemyAdmin’ module (<code>angular.module</code> is dual operation based on the number of arguments) and then register a controller within it. <code>$scope</code> is a core AngularJS component available for injection to controllers (we cover injection in a later section of this tutorial). <code>$scope</code> is the model exposed by <code>articleCtrl;</code> we add properties and behaviors we want to be available for use in our HTML.
</p>

<p style="text-align: justify;">
  Although <code>app.js</code> and <code>controllers.js</code> are very small at the moment, they will grow as this tutorial progresses and it is useful to refer to snippets of code by file name.
</p>

## <span id="Bindings"><a id="3"></a>Bindings</span> {#bindings}

<p style="text-align: justify;">
  In the previous section of this tutorial, we introduced <code>ng-bind</code> as a way to connect controller models to the text content of HTML elements. There is an alternative, non-directive way of achieving this, namely double curly bracket notation or <code>{{ }}</code>. To use it in <code>index.html</code> we would edit the <code>&lt;div&gt;</code>s as follows:
</p>

<pre class="lang:xhtml decode:true ">&lt;div&gt;{{title}}&lt;/div&gt;
&lt;div&gt;{{getTitle()}}&lt;/div&gt;</pre>

&nbsp;

Typically `{{ }}` is used over `ng-bind` as it is less verbose (4 characters versus 10).

<p style="text-align: justify;">
  <code>ng-bind</code> does have one distinct advantage, though. It prevents any temporary flashing of curly braces, which can happen if there is a gap between the browser rendering HTML and AngularJS executing. The <a href="https://docs.angularjs.org/api/ng/directive/ngCloak">ng-cloak</a> directive is an alternative solution, but we won’t go into details in this tutorial.
</p>

### <span id="Expressions">Expressions</span> {#expressions}

<p style="text-align: justify;">
  Both <code>{{ }}</code> and <code>ng-bind</code> require an AngularJS expression. AngularsJS expressions are similar in concept to JavaScript <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions">expressions</a> but with subtle differences that I’ll highlight with examples.
</p>

We could change the expressions used in `index.html` (currently `title` and `getTitle()`) to any of the following:

  * `title + 1` – expressions can have operators. Here we are operating on a string, so 1 is converted to “1” (as per normal JavaScript) resulting in “Learn AngularJS1”.
  * `title + ': ' + description` – expressions are forgiving. `description` doesn’t exist, but the result is “Learn AngularJS: ” rather than “Learn AngularJS: undefined” as you might expect.
  * `title + (description ? ': ' + description : '')` – conditionals are allowed but only in ternary form, so the result here is just “Learn AngularJS”.

Curly brackets are not allowed in AngularJS expressions, which means that blocks (e.g., `if () { }`) and function declarations (e.g., `function() { }`) are ruled out.

<p style="text-align: justify;">
  The recommendation is to have a model behavior instead of complex expressions. In general, it’s a good idea to keep your expressions looking attractive.
</p>

### <span id="Two-way_bindings"><a id="3_2"></a>Two-way bindings</span> {#two-waybindings}

<p style="text-align: justify;">
  The connections made between controller models and HTML elements are more commonly known as ‘bindings’. All bindings in AngularJS are two-way. This means that any updates to bound model properties are automatically reflected in the HTML. Even at this early stage, our sample application has two-way bindings. It just doesn’t look like it because there is no binding that will update the model. Let’s change that now so that we can properly highlight two-way binding.
</p>

Edit `index.html` as follows:

<pre class="lang:xhtml decode:true">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;
    &lt;div ng-bind="title"&gt;&lt;/div&gt;
    &lt;div&gt;{{title}}&lt;/div&gt;
    &lt;input type="text" ng-model="title" /&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the above snippet, we’ve introduced an HTML text input element and attached an <code>ng-model</code> directive to it. <code>ng-model</code> listens for DOM events (e.g., <code>keyup</code>) raised by the element, reads its value and writes it to the bound model property specified in the expression (i.e., <code>title</code>). The text content of the <code>&lt;div&gt;</code>s updates thanks to the binding created by <code>ng-bind</code> and <code>{{ }}</code>. Try this out and witness the magic of two-way binding.
</p>

## <span id="Directives_again"><a id="4"></a>Directives again</span> {#directivesagain}

<p style="text-align: justify;">
  In the first directives section, I introduced an analogy (a team of laborers completing some building works) and used it to begin categorizing directives as one of three types:
</p>

  * structural
  * decorative
  * plumbing

Unfortunately, I then proceeded to only introduce plumbing directives (`ng-app`, `ng-controller` and `ng-bind`). Let’s fix that now by returning to directives and discussing `ng-repeat`.

### <span id="ng-repeat"><a id="4_1"></a>ng-repeat</span> {#ng-repeat}

<p style="text-align: justify;">
  Most applications built with AngularJS are likely to be data-driven; reading, creating, updating and deleting data. Real-world data is often plural in nature – for example:
</p>

  * walking into your local foreign exchange provider to buy currency for your next holiday. You’ll likely be presented a board of rates for the most popular currency pairings.
  * visiting your local bookstore and browsing your favorite niche. You’ll likely be presented with a range of choices for a book to leaf through.

<p style="text-align: justify;">
  <code>ng-repeat</code> is a structural directive because it modifies the “bricks and mortar” of our HTML. At a simple level, it creates the HTML element it is attached to multiple times based on the contents of a model collection property, e.g., an array.
</p>

Lets see how `ng-repeat` can help us display an array of articles. Edit `index.html` to the following:

<pre class="lang:default decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;div ng-repeat="article in articles" ng-bind="article.title"&gt;&lt;/div&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles"&gt;
        &lt;p&gt;{{article.title}}&lt;/p&gt;
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the snippet above, <code>article in articles</code> is much like a traditional JavaScript <a href="https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for...in">for…in</a> loop, with <code>article</code> being the name of the variable assigned to on each iteration of <code>articles</code>. <code>article</code> is then available for use in other directives and bindings, either on that HTML element or a descendant (the snippet shows both).
</p>

Edit `controllers.js` so that an appropriate model property is available as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  $scope.articles = [
    { title: "Learn AngularJS" },
    { title: "JavaScript closures explained!" }
  ];
});</pre>

All being well, you’ll see the same two article titles displayed twice – inside `<div>` and `<p>` elements, respectively.

<p style="text-align: justify;">
  <code>ng-repeat</code> is a very powerful directive. We’ve used it to simply create an HTML element per item in a static array. It is more likely that you’ll use <code>ng-repeat</code> with dynamic collections – for example, live feeds of buy and sell prices for foreign exchange currency pairings. <code>ng-repeat</code> will track changes in dynamic collections and, in most cases, behave as you would expect, creating, editing and destroying HTML elements appropriately.
</p>

#### <span id="limitTo_filter"><a id="4_1_1"></a>limitTo filter</span> {#limittofilter}

<p style="text-align: justify;">
  We can further demonstrate the power of <code>ng-repeat</code> by combining it with a filter. Filters could demand a section of this tutorial to themselves, but in general they are applied to expressions, using the <code>|</code> operator, and format the result.
</p>

<p style="text-align: justify;">
  AngularJS comes with a handful of built-in filters, including <code>limitTo</code>. <code>limitTo</code> will format the result of the expression we have used with <code>ng-repeat</code> by returning a specified number of articles from an optional starting index.
</p>

Edit `index.html` as follows:

<pre class="lang:default decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;div ng-repeat="article in articles | limitTo:1 " ng-bind="article.title"&gt;&lt;/div&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles | limitTo:1:1"&gt;
        &lt;p&gt;{{article.title}}&lt;/p&gt;
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the above snippet, both <code>ng-repeat</code> expressions include a contrived usage of the <code>limitTo</code> filter (we only have two articles thus far, so limiting to one seems pretty pointless).
</p>

<p style="text-align: justify;">
  The first expression is limited to one article, but we haven’t specified a starting index, so 0 is used. This results in HTML of <code>&lt;div&gt;Learn AngularJS&lt;/div&gt;</code>.
</p>

<p style="text-align: justify;">
  The second expression is also limited to one article, but this time we have specified a starting index. This results in HTML of <code>&lt;p&gt;JavaScript closures explained!&lt;/p&gt;</code>.
</p>

### <span id="Custom_directives"><a id="4_2"></a>Custom directives</span>

AngularJS comes with a handful of structural directives, such as `ng-include` for fetching and displaying external HTML fragments, but `ng-repeat` is the most significant.

This is beyond the scope of this tutorial, but it is possible to create your own custom directives of any type (plumbing, structural or decorative). To whet your appetite for what might lie ahead in your own AngularJS journey, check out the directives in the excellent [Angular Material](https://material.angularjs.org) library or my own [angular-charts](http://crudbetter.github.io/angular-charts/) library.

You will probably note that we still haven’t discussed decorative directives. That will come later in one final directives section.

## <span id="Injection"><a id="5"></a>Injection</span> {#injection}

<p style="text-align: justify;">
  In the first directives section, we mentioned injection, stating that we would cover it in a later section. Injection might get a little complicated, but I’ll do my best to keep it concise and understandable.
</p>

### <span id="Minimal_theory"><a id="5_1"></a>Minimal theory</span> {#minimaltheory}

<p style="text-align: justify;">
  Dependency injection is a common software design pattern that is frequently used in any non-trivial application. By writing an AngularJS application, you will use dependency injection, even if you’re not aware of it. As such, it makes sense to learn a little of the theory behind the pattern.
</p>

<p style="text-align: justify;">
  The essence of the pattern is to separate the responsibilities of construction and usage of dependencies. In this tutorial, we have already seen an example of this, as follows:
</p>

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  $scope.title = "Learn AngularJS";
});</pre>

<p style="text-align: justify;">
  In this snippet, <code>articleCtrl</code> is dependent on <code>$scope</code> in order to make model properties and behaviors available for binding. We do not know how <code>$scope</code> is constructed by AngularJS (nor do we really care), as long as we can use it within <code>articleCtrl</code>.
</p>

If we were to construct `$scope` ourselves, in a very simplified form it _might_ look something like the following:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function() {
  var $scope = new angular.Scope();
  $scope.title = "Learn AngularJS";
});</pre>

In this snippet, the responsibility for the construction and use of the ``<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#Introduction">Introduction</a><ul>
        <li>
          <a href="#History_is_important">History is important</a>
        </li>
        <li>
          <a href="#MV">MV*</a>
        </li>
        <li>
          <a href="#Setup">Setup</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directives">Directives</a><ul>
        <li>
          <a href="#ng-app">ng-app</a>
        </li>
        <li>
          <a href="#ng-controller">ng-controller</a>
        </li>
        <li>
          <a href="#ng-bind">ng-bind</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Bindings">Bindings</a><ul>
        <li>
          <a href="#Expressions">Expressions</a>
        </li>
        <li>
          <a href="#Two-way_bindings">Two-way bindings</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directives_again">Directives again</a><ul>
        <li>
          <a href="#ng-repeat">ng-repeat</a><ul>
            <li>
              <a href="#limitTo_filter">limitTo filter</a>
            </li>
          </ul>
        </li>
        
        <li>
          <a href="#Custom_directives">Custom directives</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Injection">Injection</a><ul>
        <li>
          <a href="#Minimal_theory">Minimal theory</a>
        </li>
        <li>
          <a href="#Dependency_annotations">Dependency annotations</a>
        </li>
        <li>
          <a href="#Custom_injectables">Custom injectables</a><ul>
            <li>
              <a href="#Value_services">Value services</a>
            </li>
            <li>
              <a href="#Factory_services">Factory services</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Directivescontinued">Directives continued</a><ul>
        <li>
          <a href="#ng-click">ng-click</a>
        </li>
        <li>
          <a href="#ng-checked">ng-checked</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Server_integration">Server integration</a><ul>
        <li>
          <a href="#http">$http</a>
        </li>
        <li>
          <a href="#httpBackend">$httpBackend</a>
        </li>
        <li>
          <a href="#Promises">Promises</a>
        </li>
        <li>
          <a href="#resource">$resource</a>
        </li>
        <li>
          <a href="#httpBackend_again">$httpBackend again</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#Summary">Summary</a>
    </li>
  </ul>
</div>

## <span id="Introduction">Introduction</span>

<p style="text-align: justify;">
  Since I first started using AngularJS several years ago, it has become the most popular single-page application (SPA) framework for the JavaScript language. AngularJS makes it easy to solve simple problems fast, yet has enough features to enable development teams to build complex large-scale applications.
</p>

<p style="text-align: justify;">
  This tutorial aims to introduce a complete beginner to AngularJS by explaining fundamental concepts and building a sample application. The application will be a simplified admin area for a multi-author blog.<!--more-->
</p>

### <span id="History_is_important"><a id="1_1"></a>History is important</span> {#historyisimportant}

<p style="text-align: justify;">
  I believe a limited appreciation of history is important. Often in software, good ideas are a twist or improvement on something that has been done before, made possible by some other technology improvement(s). In the case of AngularJS, the vast performance improvements made to browser JavaScript engines has made ambitious SPA frameworks possible.
</p>

<p style="text-align: justify;">
  AngularJS is an adaptation of the Model-View-Controller (MVC) pattern first created in 1979. MVC helps developers to separate the concerns involved in building user interfaces. Jeff Atwood of Coding Horror <a href="http://blog.codinghorror.com/understanding-model-view-controller/">wrote of an example</a> that we’re all very familiar with, a static webpage:
</p>

<li style="text-align: justify;">
  HTML acting as the model, the content (data) for the page
</li>
<li style="text-align: justify;">
  CSS acting as the view, how that content should be presented
</li>
<li style="text-align: justify;">
  The browser acting as the controller, combining the model and the view to render the page
</li>

### <span id="MV"><a id="1_2"></a>MV*</span> {#mv}

<p style="text-align: justify;">
  Subtle variations to the MVC pattern have come in recent years, none more so than by JavaScript SPA frameworks. For this reason, the MV* phrase has been coined as a way of describing ‘whatever’ variation works for you.
</p>

<p style="text-align: justify;">
  So although it’s definitely worth understanding the origins of MVC, keep in mind that AngularJS is MV* and does not implement MVC strictly.
</p>

### <span id="Setup"><a id="1_3"></a>Setup</span> {#setup}

<p style="text-align: justify;">
  This tutorial will use the stable version of AngularJS at the time of writing (1.4.4). It also intends to only include the bare minimum required to run an AngularJS application.
</p>

<p style="text-align: justify;">
  To get up and running, download an uncompressed copy of AngularJS from <a href="https://angularjs.org/">angularjs.org</a>. Create a file structure as follows with your favorite text editor:
</p>

<p style="text-align: center;">
  <img class="size-full wp-image-147176 aligncenter" src="https://blog.udemy.com/wp-content/uploads/2015/08/initial-file-layout.png" alt="initial-file-layout" width="362" height="136" /><em>Initial file layout</em>
</p>

Edit the contents of `index.html` as follows:

<pre class="lang:xhtml decode:true " title="index.html">&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  Note that in the above snippet we’re serving <code>angular.js</code> from the local file system. This saves us from having to introduce any additional moving parts and is adequate for our purposes. Open <code>index.html</code> in your favorite browser and, although you’ll see nothing but a white screen, check to make sure that you don’t have any errors in your browser’s developer tools/console.
</p>

We will add additional `.js` files as we progress through the tutorial.

## <span id="Directives"><a id="2"></a>Directives</span> {#directives}

<p style="text-align: justify;">
  Considering the fact that I discussed MVC in the introduction, it would be very tempting to structure this tutorial by the AngularJS implementation of models, views and controllers. But there is a construct so fundamental to AngularJS that to not start with it would be an error.
</p>

<p style="text-align: justify;">
  That construct is directives. AngularJS describes itself as “HTML enhanced for web apps.” Directives are what facilitate this enhancement; they extend the capability of HTML elements.
</p>

<p style="text-align: justify;">
  I often find analogies to be a useful tool for explaining abstract concepts, such as directives. Consider a homeowner who is having some building work completed on their house. The team of laborers required to complete any non-trivial building project is varied in role and might include:
</p>

  * a bricklayer
  * an outfitter
  * a plumber

The types of capability directives extend HTML elements with can be categorized similarly:

  * structural
  * decorative
  * plumbing

If we keep the analogy going a moment longer, HTML is the house and directives are the team of laborers.

Previously I introduced a sample application that we will build during this tutorial. Let’s build upon that by displaying an article title contained within a plain JavaScript object:

<pre class="lang:js decode:true ">var article = {
  title: "Learn AngularJS"
};</pre>

Edit `index.html` to contain the `ng-app`, `ng-controller` and `ng-bind` directives as follows:

<pre class="lang:xhtml decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;
    &lt;div ng-bind="title"&gt;&lt;/div&gt;
    &lt;div ng-bind="getTitle()"&gt;&lt;/div&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  You’ll notice that two <code>.js</code> files have appeared. There is a small amount of JavaScript required to make the article title appear in the <code>&lt;div&gt;</code>s and <code>app.js</code> and <code>controllers.js</code> is where it happens. Before we create them, I want to briefly discuss the three directives introduced in the above snippet.
</p>

### <span id="ng-app"><a id="2_1"></a>ng-app</span> {#ng-app}

<p style="text-align: justify;">
  <code>ng-app</code> is a plumbing directive. AngularJS looks for which HTML element has the <code>ng-app</code> directive attached to it and uses it to bootstrap an application. <code>ng-app</code> can be placed on any HTML element, not just <code>&lt;html&gt;</code>, and connects a module (in our case, ‘udemyAdmin’) to it. Modules will be covered shortly.
</p>

### <span id="ng-controller"><a id="2_2"></a>ng-controller</span> {#ng-controller}

`ng-controller` is a plumbing directive. It connects a model and any related behaviors to a branch of the HTML tree made available by the usage of `ng-app`.

### <span id="ng-bind"><a id="2_3"></a>ng-bind</span> {#ng-bind}

<p style="text-align: justify;">
  <code>ng-bind</code> is a plumbing directive. It connects a model property or the result of a model behavior to the text content of an HTML element. In our example, the <code>title</code> property and <code>getTitle</code> behaviour is available because <code>ng-bind</code> has been used on a <code>&lt;div&gt;</code> which is a child of <code>&lt;body&gt;</code> (where <code>ng-controller</code> has been used).
</p>

Edit the file structure to match the following:

<p style="text-align: center;">
  <img class="size-full wp-image-147178 aligncenter" src="https://blog.udemy.com/wp-content/uploads/2015/08/second-file-layout.png" alt="second-file-layout" width="426" height="214" /><em>New file layout</em>
</p>

Edit the contents of `app.js` as follows:

<pre class="lang:js decode:true">angular.module('udemyAdmin', []);</pre>

<p style="text-align: justify;">
  In the above snippet, we first create a module called ‘udemyAdmin’. This module is created with no dependencies (denoted by the empty array). Modules allow us to partition different parts of an application anytime we wish; this is useful in larger applications. For example, a full Udemy blog application might be partitioned as follows:
</p>

<pre class="lang:js decode:true ">angular.module('udemy', ['udemy.courses', 'udemy.authors', 'udemy.students']);
angular.module('udemy.courses', ['udemy.courses.archived', 'udemy.courses.current']);
// and so on</pre>

Edit the contents of `controllers.js` as follows:

<pre class="lang:js decode:true">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  var title = "Learn AngularJS";

  $scope.title = title;
  $scope.getTitle = function() {
    return title;
  };
});</pre>

<p style="text-align: justify;">
  In the above snippet, we first retrieve the ‘udemyAdmin’ module (<code>angular.module</code> is dual operation based on the number of arguments) and then register a controller within it. <code>$scope</code> is a core AngularJS component available for injection to controllers (we cover injection in a later section of this tutorial). <code>$scope</code> is the model exposed by <code>articleCtrl;</code> we add properties and behaviors we want to be available for use in our HTML.
</p>

<p style="text-align: justify;">
  Although <code>app.js</code> and <code>controllers.js</code> are very small at the moment, they will grow as this tutorial progresses and it is useful to refer to snippets of code by file name.
</p>

## <span id="Bindings"><a id="3"></a>Bindings</span> {#bindings}

<p style="text-align: justify;">
  In the previous section of this tutorial, we introduced <code>ng-bind</code> as a way to connect controller models to the text content of HTML elements. There is an alternative, non-directive way of achieving this, namely double curly bracket notation or <code>{{ }}</code>. To use it in <code>index.html</code> we would edit the <code>&lt;div&gt;</code>s as follows:
</p>

<pre class="lang:xhtml decode:true ">&lt;div&gt;{{title}}&lt;/div&gt;
&lt;div&gt;{{getTitle()}}&lt;/div&gt;</pre>

&nbsp;

Typically `{{ }}` is used over `ng-bind` as it is less verbose (4 characters versus 10).

<p style="text-align: justify;">
  <code>ng-bind</code> does have one distinct advantage, though. It prevents any temporary flashing of curly braces, which can happen if there is a gap between the browser rendering HTML and AngularJS executing. The <a href="https://docs.angularjs.org/api/ng/directive/ngCloak">ng-cloak</a> directive is an alternative solution, but we won’t go into details in this tutorial.
</p>

### <span id="Expressions">Expressions</span> {#expressions}

<p style="text-align: justify;">
  Both <code>{{ }}</code> and <code>ng-bind</code> require an AngularJS expression. AngularsJS expressions are similar in concept to JavaScript <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions">expressions</a> but with subtle differences that I’ll highlight with examples.
</p>

We could change the expressions used in `index.html` (currently `title` and `getTitle()`) to any of the following:

  * `title + 1` – expressions can have operators. Here we are operating on a string, so 1 is converted to “1” (as per normal JavaScript) resulting in “Learn AngularJS1”.
  * `title + ': ' + description` – expressions are forgiving. `description` doesn’t exist, but the result is “Learn AngularJS: ” rather than “Learn AngularJS: undefined” as you might expect.
  * `title + (description ? ': ' + description : '')` – conditionals are allowed but only in ternary form, so the result here is just “Learn AngularJS”.

Curly brackets are not allowed in AngularJS expressions, which means that blocks (e.g., `if () { }`) and function declarations (e.g., `function() { }`) are ruled out.

<p style="text-align: justify;">
  The recommendation is to have a model behavior instead of complex expressions. In general, it’s a good idea to keep your expressions looking attractive.
</p>

### <span id="Two-way_bindings"><a id="3_2"></a>Two-way bindings</span> {#two-waybindings}

<p style="text-align: justify;">
  The connections made between controller models and HTML elements are more commonly known as ‘bindings’. All bindings in AngularJS are two-way. This means that any updates to bound model properties are automatically reflected in the HTML. Even at this early stage, our sample application has two-way bindings. It just doesn’t look like it because there is no binding that will update the model. Let’s change that now so that we can properly highlight two-way binding.
</p>

Edit `index.html` as follows:

<pre class="lang:xhtml decode:true">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;
    &lt;div ng-bind="title"&gt;&lt;/div&gt;
    &lt;div&gt;{{title}}&lt;/div&gt;
    &lt;input type="text" ng-model="title" /&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the above snippet, we’ve introduced an HTML text input element and attached an <code>ng-model</code> directive to it. <code>ng-model</code> listens for DOM events (e.g., <code>keyup</code>) raised by the element, reads its value and writes it to the bound model property specified in the expression (i.e., <code>title</code>). The text content of the <code>&lt;div&gt;</code>s updates thanks to the binding created by <code>ng-bind</code> and <code>{{ }}</code>. Try this out and witness the magic of two-way binding.
</p>

## <span id="Directives_again"><a id="4"></a>Directives again</span> {#directivesagain}

<p style="text-align: justify;">
  In the first directives section, I introduced an analogy (a team of laborers completing some building works) and used it to begin categorizing directives as one of three types:
</p>

  * structural
  * decorative
  * plumbing

Unfortunately, I then proceeded to only introduce plumbing directives (`ng-app`, `ng-controller` and `ng-bind`). Let’s fix that now by returning to directives and discussing `ng-repeat`.

### <span id="ng-repeat"><a id="4_1"></a>ng-repeat</span> {#ng-repeat}

<p style="text-align: justify;">
  Most applications built with AngularJS are likely to be data-driven; reading, creating, updating and deleting data. Real-world data is often plural in nature – for example:
</p>

  * walking into your local foreign exchange provider to buy currency for your next holiday. You’ll likely be presented a board of rates for the most popular currency pairings.
  * visiting your local bookstore and browsing your favorite niche. You’ll likely be presented with a range of choices for a book to leaf through.

<p style="text-align: justify;">
  <code>ng-repeat</code> is a structural directive because it modifies the “bricks and mortar” of our HTML. At a simple level, it creates the HTML element it is attached to multiple times based on the contents of a model collection property, e.g., an array.
</p>

Lets see how `ng-repeat` can help us display an array of articles. Edit `index.html` to the following:

<pre class="lang:default decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;div ng-repeat="article in articles" ng-bind="article.title"&gt;&lt;/div&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles"&gt;
        &lt;p&gt;{{article.title}}&lt;/p&gt;
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the snippet above, <code>article in articles</code> is much like a traditional JavaScript <a href="https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for...in">for…in</a> loop, with <code>article</code> being the name of the variable assigned to on each iteration of <code>articles</code>. <code>article</code> is then available for use in other directives and bindings, either on that HTML element or a descendant (the snippet shows both).
</p>

Edit `controllers.js` so that an appropriate model property is available as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  $scope.articles = [
    { title: "Learn AngularJS" },
    { title: "JavaScript closures explained!" }
  ];
});</pre>

All being well, you’ll see the same two article titles displayed twice – inside `<div>` and `<p>` elements, respectively.

<p style="text-align: justify;">
  <code>ng-repeat</code> is a very powerful directive. We’ve used it to simply create an HTML element per item in a static array. It is more likely that you’ll use <code>ng-repeat</code> with dynamic collections – for example, live feeds of buy and sell prices for foreign exchange currency pairings. <code>ng-repeat</code> will track changes in dynamic collections and, in most cases, behave as you would expect, creating, editing and destroying HTML elements appropriately.
</p>

#### <span id="limitTo_filter"><a id="4_1_1"></a>limitTo filter</span> {#limittofilter}

<p style="text-align: justify;">
  We can further demonstrate the power of <code>ng-repeat</code> by combining it with a filter. Filters could demand a section of this tutorial to themselves, but in general they are applied to expressions, using the <code>|</code> operator, and format the result.
</p>

<p style="text-align: justify;">
  AngularJS comes with a handful of built-in filters, including <code>limitTo</code>. <code>limitTo</code> will format the result of the expression we have used with <code>ng-repeat</code> by returning a specified number of articles from an optional starting index.
</p>

Edit `index.html` as follows:

<pre class="lang:default decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;div ng-repeat="article in articles | limitTo:1 " ng-bind="article.title"&gt;&lt;/div&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles | limitTo:1:1"&gt;
        &lt;p&gt;{{article.title}}&lt;/p&gt;
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the above snippet, both <code>ng-repeat</code> expressions include a contrived usage of the <code>limitTo</code> filter (we only have two articles thus far, so limiting to one seems pretty pointless).
</p>

<p style="text-align: justify;">
  The first expression is limited to one article, but we haven’t specified a starting index, so 0 is used. This results in HTML of <code>&lt;div&gt;Learn AngularJS&lt;/div&gt;</code>.
</p>

<p style="text-align: justify;">
  The second expression is also limited to one article, but this time we have specified a starting index. This results in HTML of <code>&lt;p&gt;JavaScript closures explained!&lt;/p&gt;</code>.
</p>

### <span id="Custom_directives"><a id="4_2"></a>Custom directives</span>

AngularJS comes with a handful of structural directives, such as `ng-include` for fetching and displaying external HTML fragments, but `ng-repeat` is the most significant.

This is beyond the scope of this tutorial, but it is possible to create your own custom directives of any type (plumbing, structural or decorative). To whet your appetite for what might lie ahead in your own AngularJS journey, check out the directives in the excellent [Angular Material](https://material.angularjs.org) library or my own [angular-charts](http://crudbetter.github.io/angular-charts/) library.

You will probably note that we still haven’t discussed decorative directives. That will come later in one final directives section.

## <span id="Injection"><a id="5"></a>Injection</span> {#injection}

<p style="text-align: justify;">
  In the first directives section, we mentioned injection, stating that we would cover it in a later section. Injection might get a little complicated, but I’ll do my best to keep it concise and understandable.
</p>

### <span id="Minimal_theory"><a id="5_1"></a>Minimal theory</span> {#minimaltheory}

<p style="text-align: justify;">
  Dependency injection is a common software design pattern that is frequently used in any non-trivial application. By writing an AngularJS application, you will use dependency injection, even if you’re not aware of it. As such, it makes sense to learn a little of the theory behind the pattern.
</p>

<p style="text-align: justify;">
  The essence of the pattern is to separate the responsibilities of construction and usage of dependencies. In this tutorial, we have already seen an example of this, as follows:
</p>

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope) {
  $scope.title = "Learn AngularJS";
});</pre>

<p style="text-align: justify;">
  In this snippet, <code>articleCtrl</code> is dependent on <code>$scope</code> in order to make model properties and behaviors available for binding. We do not know how <code>$scope</code> is constructed by AngularJS (nor do we really care), as long as we can use it within <code>articleCtrl</code>.
</p>

If we were to construct `$scope` ourselves, in a very simplified form it _might_ look something like the following:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function() {
  var $scope = new angular.Scope();
  $scope.title = "Learn AngularJS";
});</pre>

In this snippet, the responsibility for the construction and use of the`` dependency is solely with `articleCtrl`. A principle of good software design is for a component, such as `articleCtrl`, to do one thing and to do it well.

### <span id="Dependency_annotations"><a id="5_2"></a>Dependency annotations</span> {#dependencyannotations}

<p style="text-align: justify;">
  AngularJS needs assistance to know what to inject into the callbacks we provide to certain functions, such as <code>controller()</code>. In the examples we’ve seen so far, AngularJS uses the argument name, so if we were to make a typo such as <code>£scope </code>AngularJS wouldn’t know what to inject.
</p>

<p style="text-align: justify;">
  Although typos are possible, they’re fairly quick to spot (via errors in your browser’s developer tools/console) and fix. A more significant problem is associated with the minification of scripts comprising our AngularJS applications. We’ve all seen the output of most minifers: unintelligble single-character variable names everywhere! Minifers will rename <code>$scope,</code> for example, and AngularJS won’t get any injection assistance.
</p>

To be able to use minifers, we can use dependency annotations as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', ['$scope', function($scope) {
  $scope.title = "Learn AngularJS";
}]);</pre>

<p style="text-align: justify;">
  In the above snippet, we’re now passing an array as the second parameter to <code>controller()</code>. The last item in the array needs to be our original callback, while the first items are the string names of the dependencies to be injected. A rudimentary minifier might output the above snippet as follows:
</p>

<pre class="lang:js decode:true ">a.m('udemyAdmin').c('articleCtrl', ['$scope', function(s) {
  s.t = "Learn AngularJS";
}]);</pre>

<p style="text-align: justify;">
  Strings can’t be renamed, so AngularJS can use dependency annotations to get the injection assistance it needs. There’s a small caveat to this approach: the array item order and callback argument order must be in sync. Additionally, we’re still prone to errors from typos, but overall the benefit of minification far outweighs these caveats in any larger application.
</p>

However, we won’t use dependency annotations in this tutorial, preferring instead to keep snippets lean and focused.

### <span id="Custom_injectables"><a id="5_3"></a>Custom injectables</span> {#custominjectables}

AngularJS offers four techniques for registering our own injectable components, much like `$scope`, as follows:

  * constant
  * value
  * factory
  * service

<p style="text-align: justify;">
  Injectable components are known as services in AngularJS speak, so it’s a shame that they decided to call one of the techniques the same.
</p>

<p style="text-align: justify;">
  We will discuss the more common techniques that I have used myself and have seen used by others, namely value and factory. Constant and service are less common and more complicated in use; as such, I consider them beyond the scope of this tutorial.
</p>

#### <span id="Value_services"><a id="5_3_1"></a>Value services</span> {#valueservices}

<p style="text-align: justify;">
  A value service allows us to register injectable values (be it strings, numbers, objects, arrays or functions). They allow us to define a value that can’t be changed, much like a constant in other programming languauges. The fact that there is also a constant service is another unfortunate naming choice by AngularJS. The difference between the two is associated with the configuration life cycle of an AngularJS application, rather than anything to do with the value registered.
</p>

<p style="text-align: justify;">
  A value service allows us to register a value once and use it many times (via injection). Consider the <code>limitTo</code> filter from the “Directives again” section of this tutorial. We could introduce a <code>pageSize</code> value service, inject it into <code>articleCtrl</code> and use it as the default number of articles to display. <code>pageSize</code> could also be reused in other areas of the application, such as administering a subset of a large number of categories.
</p>

Edit the file structure to match the following:

<p style="text-align: center;">
  <a href="https://blog.udemy.com/wp-content/uploads/2015/08/third-file-layout.png"><img class="size-full wp-image-147179 aligncenter" src="https://blog.udemy.com/wp-content/uploads/2015/08/third-file-layout.png" alt="third-file-layout" width="442" height="254" /></a><em>New file layout</em>
</p>

Edit the contents of `services.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').value('pageSize', 2);</pre>

In the above snippet, we create the most basic of value services, registering the value 2 for the _`pageSize`_ injectable component.

Edit the contents of `controller.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, pageSize) {
  $scope.articles = [
    { title: "Arduino Tutorial" },
    { title: "After Effects Tutorial" },
    { title: "Django Tutorial" }
  ];

  $scope.numArticles = pageSize;
});</pre>

In the above snippet, `articleCtrl` now has a dependency on `pageSize`, so the value 2 is injected in. We then store it as `$scope.numArticles` for use in our HTML.

Finally, edit `index.html` as follows:

<pre class="lang:xhtml decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    Number of articles to display: &lt;input type="text" ng-model="numArticles" /&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles | limitTo:numArticles"&gt;
        {{article.title}}
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
    &lt;script src="services.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the snippet above, <code>limitTo</code> is part of an expression and can therefore include<code>$scope</code> properties. We’ve replaced the hard-coded number of articles to limit to with <code>numArticles</code>. Additionally, we’ve attached <code>ng-model</code> to a new HTML text input. This is to allow a user to override the default provided by <code>pageSize</code>, should they wish to do so.
</p>

#### <span id="Factory_services"><a id="5_3_2"></a>Factory services</span> {#factoryservices}

<p style="text-align: justify;">
  A factory service also allows us to register injectable values. The key difference is that the injectable value is the return value of a function that can itself be injected with dependencies. This is definitely best explained with an example.
</p>

Edit `services.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin')

  .value('pageSize', 2)

  .value('calculateCategoryPercentage', function(articles) {
    var availableCategories = ['tutorial', 'graphics', 'hardware'];
    var uniqueCategories = [];

    articles.forEach(function(article) {
      article.categories.forEach(function(category) {
        if (uniqueCategories.indexOf(category) == -1) {
          uniqueCategories.push(category);
        }
      });
    });

    return Math.floor(100 * (uniqueCategories.length / availableCategories.length));
  });</pre>

<p style="text-align: justify;">
  In the above snippet, we have registered <code>calculateCategoryPercentage</code> as a value service that will calculate a percentage of used categories from an array from articles. The details of how (i.e., the nested looping) isn’t important, but note how we have hard-coded the available categories in a local variable.
</p>

Now edit `services.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin')

  .value('pageSize', 1)

  .value('availableCategories', ['tutorial', 'graphics', 'hardware'])

  .factory('calculateCategoryPercentage', function(availableCategories) {

    return function(articles) {
      var uniqueCategories = [];

      articles.forEach(function(article) {
        article.categories.forEach(function(category) {
          if (uniqueCategories.indexOf(category) == -1) {
            uniqueCategories.push(category);
          }
        });
      });

      return Math.floor(100 * (uniqueCategories.length / availableCategories.length));
    };
  });</pre>

<p style="text-align: justify;">
  In the snippet above, we have registered the previously hard-coded available categories as a value service, enabling them to be used elsewhere in the application via injection. We have then registered <code>calculateCategoryPercentage </code>as a factory service, a function that gets called once with its dependency (<code>availableCategories</code>) injected and then returns the same function as the value service version.
</p>

To see how to use `calculateCategoryPercentage,` edit `controllers.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, pageSize, calculateCategoryPercentage) {
  $scope.articles = [
    { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
    { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
    { title: "Django Tutorial", categories: ['tutorial'] }
  ];

  $scope.numArticles = pageSize;

  $scope.categoryPercentage = calculateCategoryPercentage($scope.articles);
});</pre>

<p style="text-align: justify;">
  In the snippet above, <code>calculateCategoryPercentage</code> is injected into the controller and then invoked with our hard-coded list of articles. It’s important to note that <code>availableCategories</code> could also be injected into the controller, and in a later section we will do so.
</p>

<p style="text-align: justify;">
  As with other examples we’ve seen, <code>$scope.categoryPercentage</code> can be used in <code>index.html</code> with a simple binding, such as:
</p>

<pre class="lang:default decode:true">&lt;span&gt;Percentage of categories used: {{categoryPercentage}}&lt;/span&gt;</pre>

<p style="text-align: justify;">
  Value and factory services are a more advanced technique, but their use allows us to achieve a level of decoupling in our AngularJS applications that will greatly improve the speed of implementing any future requirements.
</p>

## <span id="Directivescontinued"><a id="6"></a>Directives continued</span> {#directivesthethird}

<p style="text-align: justify;">
  In this last directives section, we again return to our analogy (a team of laborers completing some building works), as we have yet to cover any decorative directives.
</p>

<p style="text-align: justify;">
  Rather than strictly being about styling and CSS, by decorative I mean in the sense of HTML elements being decorated with additional behaviour. This is like having a new kitchen fitted, which isn’t stuctural “bricks and mortar”, nor is it plumbing as the pipes have already been laid and are waiting to be used. But a new kitchen definitely gives a house additional behavior, especially if it has one of those fancy taps producing instant boiling water.
</p>

<p style="text-align: justify;">
  Lets now look at a couple of example decorative directives, namely <code>ng-click</code> and<code>ng-checked</code>.
</p>

### <span id="ng-click"><a id="6_1"></a>ng-click</span> {#ng-click}

<p style="text-align: justify;">
  <code>ng-click</code> decorates HTML elements with the ability to invoke a model behaviour when they are clicked, which I suspect is fairly self-explanatory.
</p>

<p style="text-align: justify;">
  In the previous section, we introduced a factory service<code>calculateCategoryPercentage</code> and used it with a hard-coded list of articles. Let’s make our application a little more interesting by allowing new articles to be created.
</p>

Edit `index.html` as follows:

<pre class="lang:xhtml decode:true ">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;input type="text" ng-model="newTitle" placeholder="Enter article name..." /&gt;
    &lt;button name="Add" ng-click="addArticle()"&gt;Add&lt;/button&gt;

    &lt;hr /&gt;

    Number of articles to display: &lt;input type="text" ng-model="numArticles" /&gt;

    &lt;ul&gt;
      &lt;li ng-repeat="article in articles | limitTo:numArticles"&gt;
        {{article.title}}
      &lt;/li&gt;
    &lt;/ul&gt;

    &lt;span&gt;Percentage of categories used: {{categoryPercentage}}&lt;/span&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
    &lt;script src="services.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the snippet above, we’ve added two new HTML elements. The text input element will update a model property <code>newTitle</code> via the <code>ng-model</code> directive (which has been discussed in a previous section on “Bindings”). The button element will invoke a model behaviour <code>addArticle</code> when clicked via <code>ng-click</code>.
</p>

Edit `controllers.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, pageSize, calculateCategoryPercentage) {
  $scope.articles = [
    { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
    { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
    { title: "Django Tutorial", categories: ['tutorial'] }
  ];

  $scope.newTitle = '';

  $scope.addArticle = function() {
    $scope.articles.push({ title: $scope.newTitle, categories: [] });
  };

  $scope.numArticles = pageSize;

  $scope.categoryPercentage = calculateCategoryPercentage($scope.articles);
});</pre>

In the above snippet, `addArticle` uses `$scope.newTitle` when invoked to push a new article onto `$scope.articles`. Note that the new article has an empty categories array.

### <span id="ng-checked"><a id="6_2"></a>ng-checked</span> {#ng-checked}

<p style="text-align: justify;">
  Our blog admin application needs to allow articles, new and old alike, to be re-categorized. Wouldn’t it be great if when this happens <code>$scope.categoryPercentage</code> is re-calculated? This can be accomplished with another usage of <code>ng-click</code> and a new directive <code>ng-checked</code>. Let’s explore how now.
</p>

Edit `controllers.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, calculateCategoryPercentage, pageSize, availableCategories) {

  $scope.categories = availableCategories;

  $scope.articles = [
    { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
    { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
    { title: "Django Tutorial", categories: ['tutorial'] }
  ];

  $scope.$watch('articles', function(articles) {
    $scope.categoryPercentage = calculateCategoryPercentage(articles);
  }, true);

  $scope.containsCategory = function(article, category) {
    return article.categories.indexOf(category) &gt;= 0;
  };

  $scope.toggleCategory = function(article, category) {
    var index = article.categories.indexOf(category);

    if (index == -1) {
      article.categories.push(category);
    } else {
      article.categories.splice(index, 1);
    }
  };

  $scope.newTitle = '';

  $scope.addArticle = function() {
    $scope.articles.push({ title: $scope.newTitle, categories: [] });
  };

  $scope.numArticles = pageSize;
});</pre>

We’ve introduced quite a lot in the above snippet. Let’s discuss each in turn.

<p style="text-align: justify;">
  We previously mentioned that we would find another use for<code>availableCategories</code>. It is now also injected into <code>articleCtrl</code> and set as<code>$scope.categories</code>.
</p>

<p style="text-align: justify;">
  When we create bindings in our HTML (e.g., via <code>ng-bind</code>, <code>{{ }}</code>, <code>ng-model</code> and many others), we are implicitly creating watches. At a simplified level, whenever AngularJS detects something that might affect an application (be it a browser event, HTTP response, and many others), it checks all of its watches against their previous values and updates bindings if there is a difference. We use <code>$scope.$watch</code> to create a manual watch of the <code>articles</code> array and re-calculate <code>$scope.categoryPercentage</code> if there is a difference. The third parameter to <code>$scope.$watch</code> indicates that we want a “deep” watch. For more information, please have a look at this excellent <a href="http://stackoverflow.com/a/29189252/305844">StackOverflow answer</a>.
</p>

<p style="text-align: justify;">
  Finally, we introduce two new model behaviors for interacting with an article’s categories. <code>$scope.containsCategory</code> simply saves us from having an ugly expression in our HTML. <code>$scope.toggleCategory</code> either adds or removes a category based on whether an article is already categorized as such.
</p>

To see all of this in action, edit `index.html` as follows:

<pre class="lang:xhtml decode:true">&lt;!DOCTYPE html&gt;
&lt;html ng-app="udemyAdmin"&gt;
  &lt;head&gt;
    &lt;title&gt;Udemy tutorials - admin area&lt;/title&gt;
  &lt;/head&gt;
  &lt;body ng-controller="articleCtrl"&gt;

    &lt;input type="text" ng-model="newTitle" placeholder="Enter article name..." /&gt;
    &lt;button name="Add" ng-click="addArticle()"&gt;Add&lt;/button&gt;

    &lt;hr /&gt;

    Number of articles to display: &lt;input type="text" ng-model="numArticles" /&gt;

    &lt;div ng-repeat="article in articles | limitTo:numArticles"&gt;
      &lt;p&gt;{{article.title}}&lt;/p&gt;
      &lt;label ng-repeat="category in categories"&gt;
        &lt;input type="checkbox" 
          ng-checked="containsCategory(article, category)" 
          ng-click="toggleCategory(article, category)" /&gt;
        {{category}}
      &lt;/label&gt;
    &lt;/div&gt;

    &lt;span&gt;Percentage of categories used: {{categoryPercentage}}&lt;/span&gt;

    &lt;script src="angular.js"&gt;&lt;/script&gt;
    &lt;script src="app.js"&gt;&lt;/script&gt;
    &lt;script src="controllers.js"&gt;&lt;/script&gt;
    &lt;script src="services.js"&gt;&lt;/script&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>

<p style="text-align: justify;">
  In the above snippet, we come across the usage of <code>ng-click</code> and <code>ng-checked </code>mentioned a little while ago. We’ve introduced a second <code>ng-repeat</code> loop to create a checkbox for each category. This <code>ng-repeat</code> loop is nested and, as such, creates the set of checkboxes for each article. <code>ng-checked</code> is used to ensure that the underlying state of the HTML checkbox element is kept in sync with the model (this is very similar to <code>ng-model</code>). <code>ng-click</code> is used to invoke <code>toggleCategory</code> with the appropriate article and category.
</p>

If you try this out, you should see the percentage of categories used being updated as you categorize articles.

## <span id="Server_integration"><a id="7"></a>Server integration</span> {#serverintegration}

<p style="text-align: justify;">
  The aim of this section is to discuss two services AngularJS provides for integrating with a server, namely <code>$http</code> and <code>$resource</code>. We’ll look at <code>$http</code> first and <code>$resource</code> second. <code>$resource</code> is essentially a RESTful extension of <code>$http</code>.
</p>

<p style="text-align: justify;">
  Previously we introduced a feature to our blog admin application that calculated the percentage of used categories. We originally used this feature to look at how we can create our own injectable components. We’ll now use it again to discuss <code>$http</code>.
</p>

### <span id="http"><a id="7_1"></a>$http</span> {#http}

<p style="text-align: justify;">
  The requirements for our blog admin application have changed, and we now need to fetch the categories from a server.
</p>

<p style="text-align: justify;">
  The simplest way to achieve this is to use <code>$http</code> directly in <code>articleCtrl</code>.
</p>

<p style="text-align: justify;">
  The contents of <code>articleCtrl</code> became quite lengthy in the previous section, so the following two snippets are intended to be illustrative.
</p>

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, $http, calculateCategoryPercentage) {

    $scope.articles = [
      { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
      { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
      { title: "Django Tutorial", categories: ['tutorial'] }
    ];

    $scope.categoryPercentage = 0;

    $http.get('/categories').then(function(response) {
      $scope.categoryPercentage = calculateCategoryPercentage($scope.articles, response.data);
    });
  });</pre>

<p style="text-align: justify;">
  In the above snippet, we issue an HTTP request to the URL ‘/categories’ and, if it is successful, we pass the response data to <code>calculateCategoryPercentage</code>. There are two key things to note here.
</p>

<p style="text-align: justify;">
  First, there will be a delay while the server responds. During this delay, the <code>$scope </code>property is not set and therefore a percentage is not displayed in the browser. We can lessen the impact of this by setting an initial value.
</p>

<p style="text-align: justify;">
  Second, we have returned <code>calculateCategoryPercentage</code> to its simple value service version. But rather than hard-coding the available categories, <code>calculateCategoryPercentage</code> is passed as follows:
</p>

<pre class="lang:js decode:true">angular.module('udemyAdmin')

  .value('calculateCategoryPercentage', function(articles, availableCategories) {
    var uniqueCategories = [];

    articles.forEach(function(article) {
      article.categories.forEach(function(category) {
        if (uniqueCategories.indexOf(category) == -1) {
          uniqueCategories.push(category);
        }
      });
    });

    return Math.floor(100 * (uniqueCategories.length / availableCategories.length));
  });</pre>

### <span id="httpBackend"><a id="7_2"></a>$httpBackend</span> {#httpbackend}

<p style="text-align: justify;">
  For the purposes of a tutorial, it is desirable to avoid introducing any tangential complexity. In our case, that would be a server to respond to the URL ‘/categories’.
</p>

<p style="text-align: justify;">
  AngularJS has a very useful additional module for backend-less development, namely <code>$httpBackend</code>. This is perfect for tutorials, but I have also used it in a team of four AngularJS developers building an application against a partially built server. We didn’t want the road map of server features to impact us, so for a period of time, we developed backend-less.
</p>

<p style="text-align: justify;">
  Download <a href="https://code.angularjs.org/1.4.4/angular-mocks.js">angular-mocks.js</a> and add another script reference in <code>index.html</code> in the following order:
</p>

<pre class="lang:default decode:true">&lt;script src="angular.js"&gt;&lt;/script&gt;
&lt;script src="angular-mocks.js"&gt;&lt;/script&gt;
&lt;script src="app.js"&gt;&lt;/script&gt;
&lt;script src="controllers.js"&gt;&lt;/script&gt;
&lt;script src="services.js"&gt;&lt;/script&gt;</pre>

<p style="text-align: justify;">
  There are two variants of <code>$httpBackend</code>, one for unit testing and one for backend-less development. We need to tell our application to use the backend-less variant; this is achieved by adding a dependency to <code>ngMockE2E</code> to our <code>udemyAdmin</code> module. We then to configure how <code>$httpBackend</code> should respond to URL and HTTP verb combinations. Currently we only need one combination, HTTP GET and ‘/categories’.
</p>

Edit `app.js` as follows:

<pre class="lang:default decode:true  ">angular.module('udemyAdmin', ['ngMockE2E']).run(function($httpBackend) {
  $httpBackend.whenGET('/categories').respond(['tutorial', 'hardware', 'graphics']);
});</pre>

In the above snippet, the `run` method of our ‘udemyAdmin’ module is used for one-off configuration code that needs to execute as our application starts up.

### <span id="Promises"><a id="7_3"></a>Promises</span> {#promises}

<p style="text-align: justify;">
  We first looked at fetching categories from a server by using <code>$http</code> directly in<code>articleCtrl</code>. For this to work, we would have to undo some of the decoupling we previously achieved by having <code>calculateCategoryPercentage</code> as a factory service with a dependency on <code>availableCategories</code>.
</p>

<p style="text-align: justify;">
  Let’s now return to that and see if we can extract <code>$http</code> from <code>articleCtrl</code> and use it in <code>availableCategories</code>. If we can achieve that, <code>articleCtrl</code> and <code>calculateCategoryPercentage</code> can become oblivious to how the available categories are obtained. This is a more advanced technique that will help keep code flexible as our applications grow.
</p>

<p style="text-align: justify;">
  We glossed over it in our simple illustration of <code>$http</code>, but the <code>get</code> method made the HTTP request (which is inherently asynchronous) and immediately returned a Promise object. A Promise object represents the future value of an asynchronous operation. If the operation succeeds, then the Promise is ‘resolved’; if it fails, then the Promise is ‘rejected’. A Promise object exposes several methods, one of which <code>then</code> allows us to register a resolution and/or rejection handler. In our simple usage, we just registered a resolution handler as follows:
</p>

<pre class="lang:default decode:true">$http.get('/categories').then(function(response) {
  $scope.categoryPercentage = calculateCategoryPercentage($scope.articles, response.data);
});</pre>

<p style="text-align: justify;">
  Promises can, without doubt, get reasonably complicated. If you want to learn more, I suggest studying the readme for the <a href="https://github.com/kriskowal/q">Q library</a>. AngularJS implements a lightweight version of the Q library.
</p>

Now that we’ve briefly introduced Promises, let’s look at how we can use them to get our decoupling back. In the following snippets, I’ve added numbered comments, which we’ll discuss in detail shortly.

Edit `controllers.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin').controller('articleCtrl', function($scope, calculateCategoryPercentage, pageSize, availableCategories) {

  $scope.categories = [];

  availableCategories.then(function(categories) {
    Array.prototype.splice.apply(
      $scope.categories,
      [$scope.categories.length, 0].concat(categories)
    );
  });  

  $scope.articles = [
    { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
    { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
    { title: "Django Tutorial", categories: ['tutorial'] }
  ];

  $scope.$watch('articles', function(articles) {
    // 3
    calculateCategoryPercentage(articles).then(function(percentage) {
      // 6
      $scope.categoryPercentage = percentage;
    });
  }, true);

  $scope.containsCategory = function(article, category) {
    return article.categories.indexOf(category) &gt;= 0;
  };

  $scope.toggleCategory = function(article, category) {
    var index = article.categories.indexOf(category);

    if (index == -1) {
      article.categories.push(category);
    } else {
      article.categories.splice(index, 1);
    }
  };

  $scope.newTitle = '';

  $scope.addArticle = function() {
    $scope.articles.push({ title: $scope.newTitle, categories: [] });
  };

  $scope.numArticles = pageSize;
});</pre>

Edit `services.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin')

  .value('pageSize', 1)

  .factory('availableCategories', function($http) {
    // 1
    return $http.get('/categories').then(function(response) {
      return response.data;
    });
  })

  .factory('calculateCategoryPercentage', function(availableCategories) {
    // 2
    return function calculate(articles) {
      var uniqueCategories = [];

      articles.forEach(function(article) {
        article.categories.forEach(function(category) {
          if (uniqueCategories.indexOf(category) == -1) {
            uniqueCategories.push(category);
          }
        });
      });

      // 4
      return availableCategories.then(function(categories) {
        // 5
        return Math.floor(100 * (uniqueCategories.length / categories.length));
      });
    };
  });</pre>

In the above snippets, the numbered comments relate to the following points:

  1. `articleCtrl` depends on `calculateCategoryPercentage` which depends on`availableCategories`. The Promise object returned from `$http.get('/categpries').then` is registered as `availableCategories`. Note that `availableCategories` has been made a factory service so that it, too, can have a dependency injected (namely, `$http`).
  2. `calculateCategoryPercentage` is next in the dependency chain, so the function `calculate` is registered.
  3. `articleCtrl` runs as the last step in the dependency chain. It calls `calculateCategoryPercentage` each time its articles change (via `$scope.$watch`). A Promise object is returned, and `articleCtrl` assigns a resolution handler.
  4. A resolution handler is assigned to the `availableCategories` Promise object. Assigning resolution handlers via `then` returns another Promise object, which allows for chained resolution.
  5. `availableCategories` is resolved (i.e., a response is received from the server), and the category percentage is calculated and returned.
  6. The chained resolution set in step 4 allows `articleCtrl` to set the category percentage as a `$scope` property.

<p style="text-align: justify;">
  You may wonder about the benefit of this approach over the simpler use of <code>$http </code>direct in <code>articleCtrl</code> we had previously. In both approaches, we have had to change how <code>calculateCategoryPercentage</code> is used in <code>articleCtrl</code>. In this approach, the change has been to work with Promises. Promises are a very general API. For example, in the future our application could first look in the browser’s local storage for categories before resorting to an HTTP server call. The Promise API that <code>articleCtrl</code> works with wouldn’t change one bit, but behind the scenes, obtaining the categories would be more involved. With Promises, <code>articleCtrl</code> has no insight into how the categories are obtained for the calculation, just that somehow they are.
</p>

### <span id="resource"><a id="7_4"></a>$resource</span> {#resource}

<p style="text-align: justify;">
  Until now, the initial articles in our blog admin application have been hard-coded in <code>articleCtrl</code>. This clearly isn’t the most flexible application around; as such, the requirements have changed yet again.
</p>

<p style="text-align: justify;">
  We’re now asked to provide a means of retrieving and creating articles stored on a server. The server has provided us a RESTful API for interacting with articles. Sending an HTTP GET request to the URL ‘/articles’ will return an array of articles, while sending an HTTP POST request will create and return a new article. <code>$resource</code> is another additional module and is perfect for working with this type of API.
</p>

Download [angular-resource.js](https://code.angularjs.org/1.4.4/angular-resource.js) and add another script reference in `index.html` in the following order:

<pre class="lang:default decode:true">&lt;script src="angular.js"&gt;&lt;/script&gt;
&lt;script src="angular-resource.js"&gt;&lt;/script&gt;
&lt;script src="angular-mocks.js"&gt;&lt;/script&gt;
&lt;script src="app.js"&gt;&lt;/script&gt;
&lt;script src="controllers.js"&gt;&lt;/script&gt;
&lt;script src="services.js"&gt;&lt;/script&gt;</pre>

<p style="text-align: justify;">
  As with <code>$http</code> previously, the simplest way to get up and running with <code>$resource</code> is to use it directly in <code>articleCtrl</code>. We could encapsulate <code>$resource</code> in another factory service so that <code>articleCtrl</code> isn’t aware of how articles are retrieved and created. For our purposes, the first approach allows us to focus on the detail of using <code>$resource</code>, but in a larger real-world application, I would certainly consider the latter approach.
</p>

Edit `app.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin', ['ngResource', 'ngMockE2E']).run(function($httpBackend) {
  $httpBackend.whenGET('/categories').respond(['tutorial', 'hardware', 'graphics']);
});</pre>

<p style="text-align: justify;">
  The change in the above snippet is a simple one: we’ve just added an additional dependency for the ‘udemyAdmin’ module, namely ‘ngResource’.
</p>

Edit `controllers.js` as follows:

<pre class="lang:js decode:true">angular.module('udemyAdmin').controller('articleCtrl', function($scope, calculateCategoryPercentage, pageSize, availableCategories, $resource) {

  var Article = $resource('/articles');

  $scope.categories = [];

  availableCategories.then(function(categories) {
    Array.prototype.splice.apply(
      $scope.categories,
      [$scope.categories.length, 0].concat(categories)
    );
  });  

  $scope.articles = Article.query();

  $scope.$watch('articles', function(articles) {
    calculateCategoryPercentage(articles).then(function(percentage) {
      $scope.categoryPercentage = percentage;
    });
  }, true);

  $scope.containsCategory = function(article, category) {
    return article.categories.indexOf(category) &gt;= 0;
  };

  $scope.toggleCategory = function(article, category) {
    var index = article.categories.indexOf(category);

    if (index == -1) {
      article.categories.push(category);
    } else {
      article.categories.splice(index, 1);
    }
  };

  $scope.newTitle = '';

  $scope.addArticle = function() {
    var newArticle = new Article({ title: $scope.newTitle });
    newArticle.$save().then(function(article) {
      $scope.articles.push(article);
    });
  };

  $scope.numArticles = pageSize;
});</pre>

<p style="text-align: justify;">
  In the above snippet, <code>$resource</code> is declared as a dependency of <code>articleCtrl</code> and subsequently injected in. We create a resource object <code>Article</code> by invoking <code>$resource</code> with our URL.
</p>

<p style="text-align: justify;">
  We then invoke <code>Article.query</code> to retrieve an array of articles from the server. This results in an HTTP GET request to ‘/articles’. As this is an asynchronous operation, it does not block the application waiting for the server to respond, but what value would be useful to return in the meantime? <code>$resource</code> immediately returns an empty array, which will be filled when the server responds. This is a neat trick, as combined with setting the array as a property on <code>$scope</code>, any binding we make in our HTML will automatically update.
</p>

<p style="text-align: justify;">
  We haven’t looked at the contents of <code>index.html</code> for a while, but the usage of <code>ng-repeat</code> doesn’t need to change, even though we’ve switched to <code>$resource</code> and <code>$scope.articles</code> is, at first, an empty array.
</p>

<p style="text-align: justify;">
  Returning to the snippet of <code>articleCtrl</code> above, when <code>$scope. addArticle</code> is invoked we now create a new instance of <code>Article</code> with <code>$scope.newTitle</code> as its title. We then invoke <code>$save</code> resulting in an HTTP POST request to ‘/articles’. A Promise object is returned, which is resolved by the server responding with an HTTP 200 status code and body. A instance of <code>Article</code> is created from the server response for us, and we simply push it onto <code>$scope.articles</code>. The usage of <code>$scope.addArticle</code> and <code>$scope.newTitle</code> in <code>index.html</code> does not need to change.
</p>

### <span id="httpBackend_again"><a id="7_5"></a>$httpBackend again</span> {#httpbackendagain}

As `$resource` is an extension of `$http`, we can also use `$httpBackend` to configure how requests to the URL ‘/articles’ should be handled.

Edit `app.js` as follows:

<pre class="lang:js decode:true ">angular.module('udemyAdmin', ['ngResource', 'ngMockE2E']).run(function($httpBackend) {
  $httpBackend.whenGET('/categories').respond(['tutorial', 'hardware', 'graphics']);

  var articles = [
    { title: "Arduino Tutorial", categories: ['tutorial', 'hardware'] },
    { title: "After Effects Tutorial", categories: ['tutorial', 'graphics'] },
    { title: "Django Tutorial", categories: ['tutorial'] }
  ];

  $httpBackend.whenGET('/articles').respond(articles);
  $httpBackend.whenPOST('/articles').respond(function(method, url, data) {
    var article = angular.fromJson(data);
    article.categories = article.categories || [];
    articles.push(article);
    return [200, article, {}];
  });
});</pre>

<p style="text-align: justify;">
  In the above snippet, we’ve simply moved the hard-coded array of articles from <code>articleCtrl</code> and returned it whenever an HTTP GET request is made to ‘/articles’. An HTTP POST request to ‘/articles’ pushes another article onto the array, meaning that any subsequent GET requests would include it.
</p>

<h2 id="summary" style="text-align: justify;">
  <span id="Summary"><a id="8"></a>Summary</span>
</h2>

<p style="text-align: justify;">
  If you’ve made this far, after all that copy and pasting of snippets, then I salute you for sticking with me.
</p>

<p style="text-align: justify;">
  You should have a working sample application that demonstrates AngularJS as a great framework that’s both powerful and fun to use.
</p>

<p style="text-align: justify;">
  This tutorial certainly doesn’t cover everything. My hope is that it has covered enough of the basics that, along with a couple of intermediate concepts, it will give you the knowledge to continue the journey you have started with AngularJS.
</p>

<p style="text-align: justify;">
  Thanks for reading!
</p>

<p class="note_normal" style="text-align: justify;">
  Published on Codingpedia.org with the permission of<span class="Apple-converted-space"> </span>Udemy<span class="Apple-converted-space"> </span>– source <a title="http://aredko.blogspot.ch/2015/02/a-fresh-look-on-accessing-database-on.html" href="https://blog.udemy.com/angularjs-tutorial/" target="_blank">AngularJS Tutorial: A Beginner’s Guide to AngularJS</a> from<span class="Apple-converted-space"> </span><a title="http://aredko.blogspot.com" href="https://blog.udemy.com/" target="_blank">https://blog.udemy.com/</a>
</p>

<p style="text-align: justify;">
  <div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
    <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/udemy-logo.png" alt="Udemy blog" /> 
    
    <p id="about_author_header">
      <strong>Udemy</strong>
    </p>
    
    <div id="social_logos_up">
      <a class="icon-earth" href="https://blog.udemy.com/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/udemy" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/udemy" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/+UdemySF" target="_blank"> </a>
    </div>
    
    <div id="author_details" style="text-align: justify;">
      Udemy.com is a platform or marketplace for online learning. Unlike academic MOOC programs driven by traditional collegiate coursework, Udemy provides a platform for experts of any kind to create courses which can be offered to the public, either at no charge or for a tuition fee. Udemy provides tools which enable users to create a course, promote it and earn money from student tuition charges.
    </div>
    
    <div id="follow_social" style="clear: both;">
      <div class="clear">
      </div>
    </div>
  </div>
</p>
