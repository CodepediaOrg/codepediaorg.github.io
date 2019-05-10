---
id: 2365
title: 'What every Angular project likely needs &#8211; and a Gulp build to provide it'
date: 2015-05-01T14:52:57+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2365
permalink: /jhadesdev/what-every-angular-project-likely-needs-and-a-gulp-build-to-provide-it/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:45:"https://github.com/jhades/angularjs-gulp-todo";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 2
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3727958192
categories:
  - javascript
tags:
  - angular
  - angularJS
  - browserify
  - cache busting
  - CommonJS
  - css
  - gulp
  - gulpjs
  - JsHint
  - Karma
  - minification
  - requireJS
  - sass
  - source maps
  - web development
---
<p class="post-title">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>
    
    <ul class="toc_list">
      <li>
        <a href="#Why_choose_Gulp">Why choose Gulp?</a>
      </li>
      <li>
        <a href="#Goals_of_a_good_Angular_build">Goals of a good Angular build</a>
      </li>
      <li>
        <a href="#A_working_build_and_sample_App">A working build and sample App</a>
      </li>
      <li>
        <a href="#Why_use_a_CSS_pre-processor_and_why_Sass">Why use a CSS pre-processor, and why Sass</a><ul>
          <li>
            <a href="#CSS_enhanced_features">CSS enhanced features</a>
          </li>
          <li>
            <a href="#Sass_or_Less">Sass or Less?</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#The_build-css_task">The build-css task</a>
      </li>
      <li>
        <a href="#Why_use_a_module_system">Why use a module system</a>
      </li>
      <li>
        <a href="#Why_CommonJs_is_appealing">Why CommonJs is appealing</a>
      </li>
      <li>
        <a href="#Angular_now_officially_supports_browserify">Angular now officially supports browserify</a>
      </li>
      <li>
        <a href="#The_build-js_Gulp_task">The build-js Gulp task</a><ul>
          <li>
            <a href="#Why_not_use_the_gulp-browserify_plugin">Why not use the gulp-browserify plugin</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Angular-friendly_Javascript_minification">Angular-friendly Javascript minification</a><ul>
          <li>
            <a href="#Fixing_the_Angular_minification_problem">Fixing the Angular minification problem</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Pre-populating_the_Angular_template_cache">Pre-populating the Angular template cache</a>
      </li>
      <li>
        <a href="#Reducing_the_number_of_HTTP_requests_using_sprites">Reducing the number of HTTP requests using sprites</a>
      </li>
      <li>
        <a href="#Why_use_cache_busting">Why use cache busting?</a>
      </li>
      <li>
        <a href="#Implementing_cache_busting_with_gulp-cachebust">Implementing cache busting with gulp-cachebust</a>
      </li>
      <li>
        <a href="#Running_tests_with_Karma">Running tests with Karma</a>
      </li>
      <li>
        <a href="#Code_quality_with_JsHint">Code quality with JsHint</a>
      </li>
      <li>
        <a href="#A_development_web_server">A development web server</a>
      </li>
      <li>
        <a href="#Conclusions">Conclusions</a>
      </li>
    </ul>
  </div>
</p>

<p class="post-title">
  Starting an AngularJs project also means choosing a whole toolchain that goes along with Angular. In this blog post we will propose a set of tools that a new Angular project will likely need, and present a <a href="https://github.com/jhades/angularjs-gulp-todo/blob/master/gulpfile.js"><strong>simple</strong> Gulp build</a> that provides those features. Let&#8217;s go over the following topics:
</p><section class="post-content"> 

  * Why Gulp? Goals for a baseline Angular build
  * Why use a CSS pre-processor, and why Sass
  * The advantages of a module system, and why browserify
  * Pre-populating the Angular template cache
  * Angular-friendly Javascript minification
  * Reducing the number of HTTP requests using sprites
  * Setup a development web server
  * Running tests with Karma, code quality with JSHint, and more

<!--more-->

### <span id="Why_choose_Gulp">Why choose Gulp?</span> {#whychoosegulp}

Although there are other task runners out there, it seems that Gulp is more and more the solution of choice, because it provides the following features:

  * it&#8217;s super fast as it&#8217;s build for concurrency from the start
  * it provides a simple to learn API (based on a pipes and filters architecture) that produces some very readable and maintainable code.

### <span id="Goals_of_a_good_Angular_build">Goals of a good Angular build</span> {#goalsofagoodangularbuild}

Ideally, the build for an Angular project would:

  * be of low complexity, ideally not much more than 100 lines of code
  * be lightning fast, in the order of a few seconds
  * imply no dependencies to non-node tool chains
  * be exactly the same in both development and production
  * solve some Angular-specific issues: Angular-friendly Javascript minification and template cache pre-population
  * provide a development mode for developer productivity
  * allow to run tests and do code quality checks
  * minimize the number of HTTP requests needed in production, via CSS and Javascript bundling, as well as images sprites

### <span id="A_working_build_and_sample_App">A working build and sample App</span> {#aworkingbuildandsampleapp}

In order to make it more concrete, here is the [build](https://github.com/jhades/angularjs-gulp-todo/blob/master/gulpfile.js) and applied it to a sample app: the TODO App from the [TODO MVC](http://todomvc.com/) project. You can try out the sample app [here](http://jhades.github.io/angularjs-gulp-todo/todo-app.html).

<p style="text-align: justify;">
  The build and sample app are also available in this <a href="https://github.com/jhades/angularjs-gulp-todo">Github repository</a>.<br /> In the next following sections we will go over a proposed toolchain for Angular projects, and walk through the Gulp build that provides those features.
</p>

### <span id="Why_use_a_CSS_pre-processor_and_why_Sass">Why use a CSS pre-processor, and why Sass</span> {#whyuseacsspreprocessorandwhysass}

To quote the yearly Thoughworks [Technology radar](http://www.thoughtworks.com/radar/languages-and-frameworks/handwritten-css) from last year:

> We believe that the days of handwritten CSS, for anything apart from trivial work, are over.

<p style="text-align: justify;">
  The limitations of plain CSS are plenty and make it hard to write maintainable stylesheets. Although new CSS3 standards are addressing many of the limitations, widespread adoption takes time.
</p>

#### <span id="CSS_enhanced_features">CSS enhanced features</span> {#cssenhancedfeatures}

In order to produce maintainable CSS, we really need today:

  * A CSS bundling mechanism
  * A partials mechanism, that allows to split CSS in logical chunks, without generating additional http requests
  * the possibility to define CSS variables of limited scope
  * the possibility to embedded related styles in a tree structure
  * the ability to define CSS &#8216;functions&#8217;

<p style="text-align: justify;">
  All these features are provided by the multiple CSS pre-processors available today, the difficulty being how to choose one. The two main ones are <a href="http://sass-lang.com/">Sass</a> and <a href="http://lesscss.org/">Less</a>.
</p>

Many libraries are built on top of these pre-processors that encapsulate commonly used patterns, see for example the Sass [Compass](http://compass-style.org/) library.

#### <span id="Sass_or_Less">Sass or Less?</span> {#sassorless}

The Less pre-processor would seem to be a better choice for a node-based build, as its also a node-based tool.

But today Sass is not tied to the Ruby tool-chain anymore, as there is a C implementation of Sass ([libsass](http://libsass.org/)) that allows for fast Sass compilation in a node environment, via plugins like [gulp-sass](https://github.com/dlmanning/gulp-sass).

Also, the industry seems to be converging around Sass: see for example this post on [css-tricks](https://css-tricks.com/sass-vs-less/). In the end, the three main reasons that I decided to opt for Sass where:

  * good integration with node via libsass, no need to install Ruby
  * knowing that the most used Sass library Compass is being ported to libsass for Compass 2, see this [Github issue](https://github.com/Compass/compass/issues/1916)
  * the fact that Angular Material Design is based on [Sass](https://github.com/angular/material/tree/master/src/core/style), so we are likely to find it again in the future

### <span id="The_build-css_task">The build-css task</span> {#thebuildcsstask}

Sass is integrated in the gulp build via the `build-css` task bellow:

<pre class="lang:js decode:true ">gulp.task('build-css', ['clean'], function() {  
    return gulp.src('./styles/*')
        .pipe(sourcemaps.init())
        .pipe(sass())
        .pipe(cachebust.resources())
        .pipe(sourcemaps.write('./maps'))
        .pipe(gulp.dest('./dist'));
});</pre>

There is support for source maps, so it will be possible to debug the CSS in the browser and have it refer to the original Sass sources. More on the cache busting feature later.

### <span id="Why_use_a_module_system">Why use a module system</span> {#whyuseamodulesystem}

There are several reasons why a module system is likely essential for any non-trivial Javascript project:

  * Adding all the Javascript dependencies in script tags at the bottom of the multiple html pages quickly falls apart as the codebase and the team increases
  * and what about concatenation and minification? repeating the content of the script tags in concatenation tasks at the level of the build quickly becomes unmanageable

Besides solving those common problems out of the box, using a module system encourages:

  * the development of well isolated small modules with clearly defined boundaries and dependencies
  * structuring the code in small chunks that are on its own easy to understand
  * simplify and better enable testing

<p style="text-align: justify;">
  But the industry is very fragmented in what concerns module systems: there is <a href="http://requirejs.org/">requireJs</a>, <a href="http://wiki.commonjs.org/wiki/CommonJS">CommonJs</a>, <a href="http://browserify.org/">browserify</a>, <a href="http://www.2ality.com/2014/09/es6-modules-final.html">ES6 modules</a> to name but a few. So which one to choose?
</p>

### <span id="Why_CommonJs_is_appealing">Why CommonJs is appealing</span> {#whycommonjsisappealing}

CommonJs is the same module system used by node in general. It is very simple to understand and use. If we need a module, say for example [express](http://expressjs.com/), we simply say:

<pre class="lang:js decode:true ">var express = require('express');</pre>

Importing a module is synchronous and very intuitive. The main advantage of this syntax is that there is no need to define a config file where all the dependencies are declared: we only need to point to the entry file, and the `require` calls will themselves implicitly document the dependency tree of Javascript files.

CommonJs used to be a backend-only module system, until [browserify](http://browserify.org/) came along, allowing us to use the same familiar syntax also on the front-end.

### <span id="Angular_now_officially_supports_browserify">Angular now officially supports browserify</span> {#angularnowofficiallysupportsbrowserify}

<p style="text-align: justify;">
  Recently in the Angular release <a href="https://github.com/angular/angular.js/blob/master/CHANGELOG.md#1314-instantaneous-browserification-2015-02-24">1.3.14 instantaneous-browserification</a> the Angular team added improved support for browserify.
</p>

<p style="text-align: justify;">
  Namely it published in npm all Angular files as CommonJs modules, giving them meaningful exports that allow for simplified browserify usage.
</p>

<p style="text-align: justify;">
  Although ES6 modules will be the solution of choice for Angular 2, it seems that browserify is the recommended solution for Angular 1 projects.
</p>

### <span id="The_build-js_Gulp_task">The build-js Gulp task</span> {#thebuildjsgulptask}

Going back to our build, the following is the task that builds the Javascript bundle of our application:

<pre class="lang:js decode:true">gulp.task('build-js', ['clean'], function() {  
    var b = browserify({
        entries: './js/app.js',
        debug: true,
        paths: ['./js/controllers', './js/services', './js/directives'],
        transform: [ngAnnotate]
    });
 
    return b.bundle()
        .pipe(source('bundle.js'))
        .pipe(buffer())
        .pipe(cachebust.resources())
        .pipe(sourcemaps.init({loadMaps: true}))
        .pipe(uglify())
        .on('error', gutil.log)
        .pipe(sourcemaps.write('./'))
        .pipe(gulp.dest('./dist/js/'));
});</pre>

#### <span id="Why_not_use_the_gulp-browserify_plugin">Why not use the gulp-browserify plugin</span> {#whynotusethegulpbrowserifyplugin}

<p style="text-align: justify;">
  Note that browserify is used directly and not via some plugin like <a href="https://www.npmjs.com/package/gulp-browserify">gulp-browserify</a>. This is because the gulp team has blacklisted several plugins such as gulp-browserify, as they make for separate build processes that are hard to integrate well with other gulp plugins.
</p>

<p style="text-align: justify;">
  Instead the gulp team provided a <a href="https://github.com/gulpjs/gulp/blob/master/docs/recipes/browserify-uglify-sourcemap.md">recipe</a> of how to use browserify: we simply call it directly, and pipe its output which includes the concatenated js files wrapped in modules and stream it to other gulp tasks.
</p>

The Gulp recipe was adapted to fix one Angular-specific problem: minification support.

### <span id="Angular-friendly_Javascript_minification">Angular-friendly Javascript minification</span> {#angularfriendlyjavascriptminification}

Angular is well-known for needing an [extra build step](http://stackoverflow.com/questions/18782324/angularjs-minify-best-practice) before minification in order to support its dependency injection mechanism. Take for example this code line:

<pre class="lang:js decode:true ">angular.factory('todoStorage', function ($http, ) { ...</pre>

If we don&#8217;t take any preventive measure, this will get minified to something like:

<pre class="lang:js decode:true">a.factory('todoStorage', function (h, ) { ...</pre>

<p style="text-align: justify;">
  The problem is that the variable name $http is lost, so Angular can no longer inject it by finding the $httpProvider that it identifies by concatenating the variable name $http with Provider. This causes the application to break if minified.
</p>

Before minification, the above line needs to be rewritten to an alternative array syntax that preserves the string names but is a bit more verbose:

<pre class="lang:js decode:true ">angular.factory('todoStorage', ['$http', function ($http, ) { ... } ]</pre>

#### <span id="Fixing_the_Angular_minification_problem">Fixing the Angular minification problem</span> {#fixingtheangularminificationproblem}

One way to fix it is to simply use the array syntax everywhere, as this is still readable and avoids an extra build step.

Alternatively, the browserify transform `browserify-annotate` was included in the `build-js` task above to solve this:

<pre class="lang:js decode:true ">var ngAnnotate = require('browserify-ngannotate');</pre>

Note that this transform applies the array syntax in almost all places, but I still had to apply it manually to route definition `resolve` steps, see the file `app.js`.

### <span id="Pre-populating_the_Angular_template_cache">Pre-populating the Angular template cache</span> {#prepopulatingtheangulartemplatecache}

<p style="text-align: justify;">
  Another Angular-specific problem that a build needs to solve is to pre-populate the Angular template cache.
</p>

<p style="text-align: justify;">
  If we don&#8217;t do this, then every template of every directive will result in a separate HTTP request, resulting in an unacceptably slow application startup time, as the application needs to wait for the templates to be loaded before rendering the page.
</p>

### <span id="Reducing_the_number_of_HTTP_requests_using_sprites">Reducing the number of HTTP requests using sprites</span> {#reducingthenumberofhttprequestsusingsprites}

Many of the optimizations that the build provides revolve around reducing the number of HTTP requests needed to bootstrap the app. And what about images, such as for example icons?

<p style="text-align: justify;">
  One way to reduce the number of HTTP requests related to images is to group them together in an image sprite, and load only that. The following section of the build uses the <code>gulp.spritesmith</code> plugin to generate sprites:
</p>

<pre class="lang:js decode:true ">gulp.task('sprite', function () {
 
    var spriteData = gulp.src('./images/*.png')
        .pipe(spritesmith({
            imgName: 'todo-sprite.png',
            cssName: '_todo-sprite.scss',
            algorithm: 'top-down',
            padding: 5
        }));
 
    spriteData.css.pipe(gulp.dest('./dist'));
    spriteData.img.pipe(gulp.dest('./dist'))
});</pre>

<p style="text-align: justify;">
  All the <code>png</code> images inside the <code>images</code> folder get converted into one single <code>todo-sprite.png</code> file. In order to be able to use the sprites, a Sass mixin is generated in file <code>_todo-sprite.scss</code>. The mixin can be used in the following way:
</p>

<pre class="lang:js decode:true ">@include sprites($spritesheet-sprites);</pre>

<p style="text-align: justify;">
  This will create a set of CSS classes with the same names as the image files, which allow to use the different images. For example the image <code>cal-right-button.jpg</code> could be used in the following way:
</p>

<pre class="lang:xhtml decode:true ">&lt;span class="cal-right-button"&gt;&lt;/span&gt;</pre>

### <span id="Why_use_cache_busting">Why use cache busting?</span> {#whyusecachebusting}

<p style="text-align: justify;">
  A convenient practice that comes in handy in both development and production is to prevent the browser from keeping stale copies of the CSS / Javascript of the application.
</p>

<p style="text-align: justify;">
  This prevents customers from viewing a stale version of the application that no longer matches the Html, and avoids bug reports that are sometimes hard to link back to the presence of stale resources.
</p>

<p style="text-align: justify;">
  The most effective way to achieve cache busting is to append to each CSS/Javascript file a hash of its content. This way when the file changes, the file name also changes and the browser will load the latest version of the resource.
</p>

### <span id="Implementing_cache_busting_with_gulp-cachebust">Implementing cache busting with gulp-cachebust</span> {#implementingcachebustingwithgulpcachebust}

The plugin [gulp-cachebust](https://www.npmjs.com/package/gulp-cachebust) was used to implement the cache busting functionality. We have seen in the `build-css` and `build-js` tasks a call like this:

<pre class="lang:js decode:true ">.pipe(cachebust.resources())</pre>

<p style="text-align: justify;">
  This call keeps track of the CSS and Javascript files that will need to have their names concatenated with the file hash. The actual replacement of the css file names is done in the build task, by calling <code>cachebust.references()</code>:
</p>

<pre class="lang:js decode:true ">gulp.task('build', [ 'clean', 'bower','build-css','build-template-cache', 'jshint', 'build-js'], function() {  
    return gulp.src('index.html')
        .pipe(cachebust.references())
        .pipe(gulp.dest('dist'));
});</pre>

### <span id="Running_tests_with_Karma">Running tests with Karma</span> {#runningtestswithkarma}

<p style="text-align: justify;">
  One of the de-facto Angular test tool is the <a href="http://karma-runner.github.io/0.12/index.html">Karma</a> test runner, and one the most used testing frameworks with it is <a href="http://jasmine.github.io/">Jasmine</a>. The following Gulp task allows to run Jasmine tests from the command line against a headless <a title="http://phantomjs.org/" href="http://phantomjs.org/" target="_blank">PhantomJs</a> browser:
</p>

<pre class="lang:js decode:true">gulp.task('test', ['build-js'], function() {  
    var testFiles = [
        './test/unit/*.js'
    ];
 
    return gulp.src(testFiles)
        .pipe(karma({
            configFile: 'karma.conf.js',
            action: 'run'
        }))
        .on('error', function(err) {
            console.log('karma tests failed: ' + err);
            throw err;
        });
});</pre>

### <span id="Code_quality_with_JsHint">Code quality with JsHint</span> {#codequalitywithjshint}

<p style="text-align: justify;">
  <a href="http://jshint.com/">JsHint</a> is one of the most used Javascript linters. The following gulp task allows to integrate it in our build cycle:
</p>

<pre class="lang:js decode:true ">gulp.task('jshint', function() {  
    gulp.src('/js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});</pre>

### <span id="A_development_web_server">A development web server</span> {#adevelopmentwebserver}

After doing the main build and running all the tests using the `gulp `default task, its useful to be able to startup a local development server.

The `gulp-webserver` plugin was configured to do exactly that in the following way:

<pre class="lang:js decode:true  ">gulp.task('webserver', ['watch','build'], function() {  
    gulp.src('.')
        .pipe(webserver({
            livereload: false,
            directoryListing: true,
            open: "&lt;a href="http://localhost:8000/dist/index.html"&gt;http://localhost:8000/dist/index.html&lt;/a&gt;"
        }));
});</pre>

### <span id="Conclusions">Conclusions</span> {#conclusions}

<p style="text-align: justify;">
  With the Gulp task runner and its rich ecosystem of plugins, its possible to create a baseline Angular build with the most frequently used features that an Angular project will likelly need, and still keep the complexity of the build relativelly low.
</p>

<p style="text-align: justify;">
  Let us know your thoughts on the comments bellow, what does your Angular build look like?
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with permission of Aleksey Novik – source <a title="http://blog.jhades.org/what-every-angular-project-likely-needs-and-a-gulp-build-to-provide-it/" href="http://blog.jhades.org/what-every-angular-project-likely-needs-and-a-gulp-build-to-provide-it/" target="_blank"><em>What every Angular project likely needs &#8211; and a Gulp build to provide it</em></a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p></section>
