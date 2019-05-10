---
id: 2385
title: 'Angular 1 vs Angular 2 &#8211; a high-level comparison'
date: 2015-10-07T17:33:45+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2385
permalink: /jhadesdev/angular-1-vs-angular-2-a-high-level-comparison/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3881588747
fsb_social_facebook:
  - 1
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 6
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - angular
  - angular2
  - angularJS
  - javascript
---
<p style="text-align: justify;">
  Angular 2 is currently still in Alpha/Developer Preview, but the main functionality and the core documentation are both already <a href="https://angular.io/">available</a>. Let&#8217;s gather here what is so far known about the design goals of Angular 2, and how they are planned to be implemented:
</p>

  * Mains goals of Angular 2
  * Simpler to reason about
  * Angular 1 vs Angular 2 change detection
  * More transparent internals with Zones
  * Improved stack traces
  * Much improved performance (and why)
  * Improved modularity
  * Improved Dependency injection
  * Web component friendly (how and why)
  * Support for Shadow DOM
  * Support for **native** mobile rendering in Android and iOs
  * Support for server side rendering
  * Improved testability
  * Migration path to Angular 2
  * Conclusions

<!--more-->

### The main goals of Angular 2 {#themaingoalsofangular2}

The main goal of Angular 2 is to create a web framework that is super easy to learn and just works. Let&#8217;s see how this is achieved:

### Goal: Simpler to reason about {#goalsimplertoreasonabout}

<p style="text-align: justify;">
  In the current version of Angular, we sometimes have to reason about the framework internals for certain use cases, such as having to reason about application event initialization and the digest cycle:
</p>

  * In Angular 1 there was no **digest cycle finished** event (see [reasons](https://docs.google.com/document/d/1US9h0ORqBltl71TlEU6s76ix8SUnOLE2jabHVg9xxEA/edit#) for the new Forms module), because such event might trigger further changes that kept the digest cycle going.
  * we had to reason about when to call `$scope.apply` or `$cope.digest`, which was not always straightforward
  * on occasion we had to call `$timeout` to let Angular finish its digest cycle and do some operation only when the DOM is stable

In order to make Angular 2 easier to reason about, one of the goals was to create more transparent internals that work out of the box.

To start, let&#8217;s have a look at how the Angular 1 binding mechanism is implemented, and how it will be made more transparent.

### How Angular 1 implements binding {#howangular1implementsbinding}

<p style="text-align: justify;">
  In Angular 1, the <code>ng-model</code> functionality of being able to edit a form and instantly have those changes reflected on a Javascript POJO is one of the main reasons why the framework became so popular.
</p>

The way this works in Angular 1 is the following, according to this [podcast](http://devchat.tv/js-jabber/032-jsj-angular-js) (see at 3:50):

  * in the Javascript runtime, everything is patcheable by design &#8211; we can change the `Number` class if we need
  * Angular at startup will patch all points of asynchronous interaction:
      * timeouts
      * Ajax requests
      * browser events
      * Websockets, etc.
  * At those interaction points, Angular will run dirty checking on the scope objects to see if changes occurred and if so trigger the corresponding watchers
  * re-run dirty checking to see if more change changes occurred, and re-run watchers, etc.

#### Consequences of how binding works in Angular 1 {#consequencesofhowbindingworksinangular1}

<p style="text-align: justify;">
  The result is that the DOM is kept permanently in sync with a POJO and it just works, but all this can sometimes be hard to reason about:
</p>

<ul style="text-align: justify;">
  <li>
    its not clear which watchers will be fired and in which order, or how many times
  </li>
  <li>
    the order of the model updates is hard to reason about and anticipate
  </li>
  <li>
    the digest cycle can run multiple times which is time consuming
  </li>
</ul>

<p style="text-align: justify;">
  One of the first steps that the Angular team took in the direction of Angular 2, was to extract from the Angular code base the mechanism of patching all asynchronous interaction points, and made it reusable.
</p>

### Introducing Zones {#introducingzones}

The result of such refactoring is [Zone.js](https://github.com/angular/zone.js/), which can be compared to the equivalent of a thread-local context in Java.

It can be used for many use cases, such as for example allowing the framework to generate long stack traces that span multiple Javascript VM turns.

### How Angular 2 is more transparent due to Zones {#howangular2ismoretransparentduetozones}

Angular 2 uses the zones mechanism to make a lot of the reasoning about the digest cycle no longer necessary. This plain non-Angular specific Javascript code will transparently trigger a Angular 2 digest, if triggered by a component that is inside a zone:

<pre><code class="javascript">element.addEventListener('keyup', function () {  
  console.log('Key pressed.');
});
});</code></pre>

No longer `$scope.apply` or `$scope.digest` are needed, everything transparently works. Probably we won&#8217;t have to reason about zones in general in most use cases, and it will still be possible to run code outside the Angular zone by using [VmTurnZone](https://angular.io/docs/js/latest/api/core/VmTurnZone-class.html).

### Goal: Improved performance {#goalimprovedperformance}

<p style="text-align: justify;">
  The above description of the digest cycle makes it clear that all this is can be time consuming, although a lot of performance improvements where made in both Angular 1.3 and Angular 1.4.
</p>

<p style="text-align: justify;">
  But its unclear of those performance improvements can go much further, one of the reasons for that is the possibility of the existence of change detection loops.
</p>

<p style="text-align: justify;">
  To better understand how the performance gains are achieved (up to 5-10 times faster last then Angular 1), its better to refer to this <a href="http://devchat.tv/adventures-in-angular/042-aia-dependency-injection-and-change-detection-with-victor-savkin">podcast</a> and <a href="http://victorsavkin.com/post/110170125256/change-detection-in-angular-2">blog post</a>. I&#8217;ll try to summarize here the two main reasons why Angular 2 is much faster:
</p>

#### Faster checking of a single binding {#fastercheckingofasinglebinding}

<p style="text-align: justify;">
  The mechanism to check a single binding was optimized to allow the Javascript VM to optimize that code into native code via just-in-time compilation. Instead of scanning recursively a tree of objects, a function is created at Angular startup to see if the binding has changed.
</p>

This binding-checking function looks like a function that we would write by hand to test for changes and it can be easily optimized away by the VM.

#### Avoid scanning parts of the component tree {#avoidscanningpartsofthecomponenttree}

<p style="text-align: justify;">
  Angular 2 will allow the developer to provide some guarantees to the change detection mechanism to avoid scanning parts of the component tree. Basically the developer will have two options:
</p>

<li style="text-align: justify;">
  make the model an Observable: Angular will detect this and register itself to observe the model. This way if the model changes/remains the same Angular will know via the observable mechanism and so it does not need to run change detection for that object.
</li>
<li style="text-align: justify;">
  make the model immutable, using for example Facebook&#8217;s <a href="http://facebook.github.io/immutable-js/docs/#/">immutable.js</a>. Again the idea is that Angular will detect this and avoid running change detection on immutable objects.
</li>

### Goal: Improved modularity {#goalimprovedmodularity}

<p style="text-align: justify;">
  In Angular 1, the Angular modules are mostly dependency injection containers that group related functionality.
</p>

<p style="text-align: justify;">
  These modules are for example not asynchronously loaded based on their dependency listings the first time that a dependency is needed, like AMD modules for instance.
</p>

#### Angular 1 and module lazy-loading {#angular1andmodulelazyloading}

<p style="text-align: justify;">
  Angular 1 lazy loading is still possible with a solution like <a href="https://oclazyload.readme.io/">ocLazyLoad</a>, but ideally it should be something native to the framework and more transparent, and according to this <a href="http://devchat.tv/adventures-in-angular/042-aia-dependency-injection-and-change-detection-with-victor-savkin">podcast</a> it seems that in Angular 2 it will be the case (see at 13:06).
</p>

#### Improvements to npm as a frontend package manager {#improvementstonpmasafrontendpackagemanager}

<p style="text-align: justify;">
  Also in general the Angular team is looking into <a href="https://drive.google.com/folderview?id=0B7Ovm8bUYiUDR29iSkEyMk5pVUk&usp=drive_web&urp=https://angular.io/&pli=1&tid=0BxgtL8yFJbacUnUxc3l5aTZrbVk#list">improving npm</a> to make it more frontend friendly, not only for Angular but for any frontend library in general.
</p>

<p style="text-align: justify;">
  Angular 2 does not force this, but if we choose to do so we can take advantage of the new ES6 module loader, which is a standard async module loader that can be already used via the <a href="https://github.com/ModuleLoader/es6-module-loader">es6-module-loader</a> pollyfill.
</p>

### Goal: Improved dependency injection {#goalimproveddependencyinjection}

<p style="text-align: justify;">
  Angular 1 dependency injection is a leap forward in building more modular applications, but it has a couple of corner cases that cannot be fixed anymore without major breaking changes.
</p>

#### Angular 1 has a global pool of objects {#angular1hasaglobalpoolofobjects}

<p style="text-align: justify;">
  One of the DI corner cases in Angular 1 is that there is only one global pool of objects per application. This means that if for example the main route loads <code>backendService</code> and we navigate to route B, we could there lazy-load other services specific of that route.
</p>

<p style="text-align: justify;">
  The problem is, let&#8217;s say we lazy load a second <code>backendService</code> with a completely different implementation: it would overwrite the first one! There is currently no way to have two services with the same name but different implementations, which prevents lazy-loading from being implemented in Angular 1 in a safe way.
</p>

#### Angular 1 silently overwrites modules if they have the same name {#angular1silentlyoverwritesmodulesiftheyhavethesamename}

<p style="text-align: justify;">
  This is a feature to allow for example to replace the service layer services with mocks at test time, but might cause issues if we accidentally load two times the same module.
</p>

#### In Angular 1 there are multiple DI mechanisms {#inangular1therearemultipledimechanisms}

In Angular 1, we can inject dependencies in multiple places in different ways:

  * in the link function by position
  * in the directive definition by name
  * in the controller function by name, etc.

<p style="text-align: justify;">
  In Angular 2 there is the goal to unify these mechanism into a single mechanism, for reducing the learning curve and improve readability.
</p>

#### How will Angular 2 DI improve the situation {#howwillangular2diimprovethesituation}

In Angular 2 there will be only one DI mechanism: constructor injection by type.

<pre><code class="javascript">constructor(keyUtils: KeyboardUtils) {
        this.keyUtils = keyUtils;
    }
});</code></pre>

<p style="text-align: justify;">
  The fact that there is only one mechanism makes it easier to learn. Also the dependency injector is hierarchical, meaning that at different points of the component tree it&#8217;s possible to have different implementations of the same type.
</p>

<p style="text-align: justify;">
  If a component does not have a dependency defined, it will delegate the lookup to it&#8217;s parent injector and so forth. This sets the ground for providing native lazy-loading support in Angular 2.
</p>

### Goal: Web Component friendly {#goalwebcomponentfriendly}

<p style="text-align: justify;">
  The future of the web is Web Components, and Angular 2 wants from the start to play well with future web component libraries. For this, one of the goals of the Angular 2 template syntax is to keep attributes clean and don&#8217;t put any Angular expressions on them &#8211; everything is binded via properties only.
</p>

To understand why this is important, take a look at this example:

<pre><code class="xml">&lt;ng1-component&gt;
     &lt;web-component-widget setting="{{angularExpresssion}}"&gt;&lt;/web-component-widget&gt;
&lt;/ng1-component&gt;</code></pre>

Here we have an Angular 1 component that interacts with a future web component library.

#### What is the problem here? {#whatistheproblemhere}

Well, the web component behaves just like a browser component, such has for example the `img` tag.

<p style="text-align: justify;">
  Therefore at page startup time and before Angular gets a chance to kick in, the Angular expression will be passed to the component which would act upon it directly, like the image element immediately loads the image using the url provided.
</p>

<p style="text-align: justify;">
  This is actually the reason why all the attributes like <code>ng-src</code> are needed, to work around this issue.
</p>

#### How Angular 2 will interact better with Web components {#howangular2willinteractbetterwithwebcomponents}

In Angular 2, the template syntax will avoid to bind to plain attributes, unless to read constants:

<pre><code class="xml">&lt;ng2-component&gt;  
     &lt;web-component-widget [setting]="angularExpresssion"&gt;&lt;/web-component-widget&gt;
&lt;/ng2-component&gt;</code></pre>

<p style="text-align: justify;">
  The <code>[setting]</code> is a property binding that will write a value of an expression into a component property. In no place will there be Angular expressions in plain attributes, to prevent interoperability problems with web components.
</p>

#### Support for Shadow DOM {#supportforshadowdom}

<p style="text-align: justify;">
  One of the key features of web components is the <a href="http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/">Shadow DOM</a>. This is a native browser mechanism that will allow to build native looking components, say a new implementation of <code>select</code>.
</p>

<p style="text-align: justify;">
  A web component can still be implemented in plain HTML/CSS but isolated from the main page, in some ways in a similar way to as if it where inside an iframe with a separate document root.
</p>

Because the Shadow DOM is currently only implemented in Chrome, Angular 2 will support it via 3 different mechanisms:

  * default mode: by default the Shadow DOM is not on and the components internals show up in the same document tree as the main page
  * emulated Shadow DOM: the Shadow DOM CSS isolation mechanism can be pollyfiled using [Polymer](https://www.polymer-project.org/0.5/) by having the CSS inside the component being prefixed on the fly to make the CSS more specific
  * true Shadow DOM: as mentioned this only works in Chrome

### Goal: support for native mobile &#8211; iOS and Android {#goalsupportfornativemobileiosandandroid}

<p style="text-align: justify;">
  Angular 2 will have two layers, the application layer and the rendering layer. For example a component can be annotated with different <code>@View</code>annotations that can be enabled at runtime depending on the environment.
</p>

In a similar way to [React Native](https://facebook.github.io/react-native/), Angular 2 will allow to support the notion of:

> Learn once, write anywhere

This means the knowledge of Angular can be reused to build native applications as well as web applications, although there will always be some differences.

### Goal: support for server side rendering {#goalsupportforserversiderendering}

<p style="text-align: justify;">
  Support for server-side rendering is important for SEO and user perception: in a large Angular 1 app we can clearly see the bootstrap process of the page while the startup is ongoing, even if using template cache pre-population.
</p>

This is one of the parts in Angular 2 that is less clear at this moment, but the idea might be implemented like this:

  * the startup occurs and all components get binded
  * but the rendering does not occur
  * a page gets rendered in the server and sent over the wire
  * Angular will parse it and inject parts of the page into the DOM, avoiding the flickering effect

### Goal: Improved testability {#goalimprovedtestability}

In Angular 2 it&#8217;s relatively hard to write true unit tests, because for example `ng-model` really needs a DOM to be tested which leads to use of solutions like using [PhantomJs](http://phantomjs.org/).

The problem with this approach is that those tests are no longer unit tests, they are integration tests which have the following issues:

  * they are slow to execute
  * they are fragile and harder to maintain

<p style="text-align: justify;">
  This leads to an inversion of the <a href="http://martinfowler.com/bliki/TestPyramid.html">test pyramid</a>, which is a situation where most of our tests are either UI tests or integration tests, and there are hardly any true unit tests. This means the build constantly breaks by reasons other than true bugs and the testing effort pulls less of its weight than what we would want.
</p>

<p style="text-align: justify;">
  The introduction of a separate rendering layer will allow to make unit tests super fast and with minimal dependencies, making them easier to write and maintain and allowing to run them much more often.
</p>

### Goal: migration path to Angular 2 {#goalmigrationpathtoangular2}

<p style="text-align: justify;">
  One of the goals of Angular 2 is to provide a clear migration path from Angular 1. This will only become clear when Angular 2 is near it&#8217;s initial release, but for now the router will be one of the main enablers of that migration.
</p>

<p style="text-align: justify;">
  The new Angular 2 router is being backported to Angular 1, and will allow the same application to have both Angular 1 and Angular 2 routes.
</p>

### Conclusions {#conclusions}

<p style="text-align: justify;">
  I&#8217;m really excited about Angular 2, after having tried it out with a few components, I can see how its easier to learn and more transparent to the developer. Again much of the things described in this post such as Zones will just work.
</p>

<p style="text-align: justify;">
  The integration with 3rd party libraries is much improved and if npm can be improved to be more helpful for frontend code that would be huge.
</p>

### Want to try it out? {#wanttotryitout}

It&#8217;s definitely not too soon to try it out, if you want to give it a go this is a [seed project](https://github.com/mgechev/angular2-seed), and the Visual Studio Code [editor](https://www.youtube.com/watch?v=HmWm21cCAXM) or [Webstorm](https://www.jetbrains.com/webstorm/) already provide great Typescript 1.5 support.

<p class="note_normal">
  Published at Codingpedia.org with permission of Aleksey Novik – source <em><a title="The main goals of Angular 2 and how they will be achieved" href="http://blog.jhades.org/introduction-to-angular2-the-main-goals/" target="_blank">The main goals of Angular 2 and how they will be achieved</a></em> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>
