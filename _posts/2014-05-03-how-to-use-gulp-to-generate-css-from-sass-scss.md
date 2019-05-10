---
id: 1346
title: How to use Gulp to generate CSS from Sass/scss
date: 2014-05-03T14:50:19+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1346
permalink: /ama/how-to-use-gulp-to-generate-css-from-sass-scss/
fsb_show_social:
  - 0
dsq_thread_id:
  - 2657681283
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:55:"https://github.com/podcastpedia/css-generator-sass-gulp";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 2
fsb_social_google:
  - 7
fsb_social_linkedin:
  - 1
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
  - web design
tags:
  - css
  - gulp
  - gulpjs
  - node
  - nodejs
  - sass
  - web design
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Set_up_the_infrastructure">1. Set up the infrastructure</a><ul>
        <li>
          <a href="#11_Install_Nodejs">1.1. Install Node.js</a>
        </li>
        <li>
          <a href="#12_Install_gulp">1.2. Install gulp</a><ul>
            <li>
              <a href="#121_Install_gulp_globally">1.2.1. Install gulp globally</a>
            </li>
            <li>
              <a href="#122_Install_gulp_in_your_project_devDependencies">1.2.2. Install gulp in your project devDependencies:</a>
            </li>
            <li>
              <a href="#123_Create_a_gulpfilejs_at_the_root_of_the_project">1.2.3. Create a gulpfile.js at the root of the project:</a>
            </li>
            <li>
              <a href="#124_Run_gulp">1.2.4. Run gulp</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#13_Install_Gulp_plugins">1.3. Install Gulp plugins</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_gulpfilejs">2. gulpfile.js</a><ul>
        <li>
          <a href="#21_Load_the_modules">2.1. Load the modules</a>
        </li>
        <li>
          <a href="#22_Gulp_tasks">2.2. Gulp tasks</a>
        </li>
        <li>
          <a href="#23_Pipes">2.3. Pipes</a>
        </li>
        <li>
          <a href="#24_8220Watching8221_files_for_modifications">2.4. &#8220;Watching&#8221; files for modifications</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Resources">3. Resources</a><ul>
        <li>
          <a href="#31_Source_code">3.1. Source code</a>
        </li>
        <li>
          <a href="#32_Codingpediaorg_related">3.2. Codingpedia.org related</a>
        </li>
        <li>
          <a href="#33_Web">3.3. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<p class="alert_note" style="text-align: justify;">
  This is the sequel of the post <a title="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" href="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" target="_blank">CSS Preprocessors – Introducing Sass to Podcastpedia.org</a>. If in the first part I presented some Sass-features I use to generated the CSS file for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, in this part I will present how the .css file generation process can be implemented with the help of Gulpjs.
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="1_Set_up_the_infrastructure">1. Set up the infrastructure</span>

### <span id="11_Install_Nodejs">1.1. Install Node.js</span>

Well, the first I needed to do is install <a title="http://nodejs.org/" href="http://nodejs.org/" target="_blank">Nodejs</a>:

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;Node.js is a platform built on Chrome&#8217;s JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.&#8221;</em>[1]<!--more-->
</p>

### <span id="12_Install_gulp">1.2. Install gulp</span>

Gulp is a task runner which uses node.js. It doesn&#8217;t do much &#8211; it provides some streams and a basic task system.

#### <span id="121_Install_gulp_globally"><a class="anchor" href="https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md#1-install-gulp-globally" name="1-install-gulp-globally"></a>1.2.1. Install gulp globally</span>

<pre class="lang:sh decode:true" title="Install gulp globally command">npm install -g gulp</pre>

#### <span id="122_Install_gulp_in_your_project_devDependencies"><a class="anchor" href="https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md#2-install-gulp-in-your-project-devdependencies" name="2-install-gulp-in-your-project-devdependencies"></a>1.2.2. Install gulp in your project devDependencies:</span>

<pre class="lang:sh decode:true" title="Install gulp in the project command">npm install --save-dev gulp</pre>

#### <span id="123_Create_a_gulpfilejs_at_the_root_of_the_project">1.2.3. Create a <code>gulpfile.js</code> at the root of the project:</span>

<pre class="lang:js decode:true" title="Create initial gulpfile.js">var gulp = require('gulp');

gulp.task('default', function() {
  // place code for your default task here
});</pre>

#### <span id="124_Run_gulp"><a class="anchor" href="https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md#4-run-gulp" name="4-run-gulp"></a>1.2.4. Run gulp</span>

The default task from above will run and do nothing for the moment. You can execute it by typing the following command in the root folder of the project

<pre class="lang:sh decode:true" title="Simple command to run gulp ">gulp</pre>

### <span id="13_Install_Gulp_plugins">1.3. Install Gulp plugins</span>

<p style="text-align: justify;">
  One of the Gulp&#8217;s strengths is the myriad of existing &#8220;plugins&#8221;, and because the Gulp community is growing, new ones are added daily. The ones that I use for the generation of the CSS file are
</p>

  * <a title="https://github.com/gulpjs/gulp-util" href="https://github.com/gulpjs/gulp-util" target="_blank">gulp-util</a> &#8211; utility functions for gulp plugins
  * <a title="https://www.npmjs.org/package/gulp-sass" href="https://www.npmjs.org/package/gulp-sass" target="_blank">gulp-sass</a> &#8211; gulp plugin for sass
  * <a title="https://www.npmjs.org/package/gulp-minify-css" href="https://www.npmjs.org/package/gulp-minify-css" target="_blank">gulp-minify-css</a> &#8211; minify css with clean-css
  * <a title="https://www.npmjs.org/package/gulp-rename" href="https://www.npmjs.org/package/gulp-rename" target="_blank">gulp-rename</a> &#8211; rename files
  * <a title="https://www.npmjs.org/package/gulp-autoprefixer" href="https://www.npmjs.org/package/gulp-autoprefixer" target="_blank">gulp-autoprefixer</a> &#8211; autoprefixer for gulp
  * <a title="https://github.com/gulpjs/gulp/blob/master/docs/recipes/delete-files-folder.md" href="https://github.com/gulpjs/gulp/blob/master/docs/recipes/delete-files-folder.md" target="_blank">del </a>&#8211; removes files and folders
  * <a title="https://www.npmjs.com/package/gulp-uglify" href="https://www.npmjs.com/package/gulp-uglify" target="_blank">gulp-uglify</a> &#8211; minify files with UglifyJS

To install these plugins issue the following command in the console in the root folder of the project:

<pre class="lang:sh decode:true " title="install gulp plugins command">npm install --save-dev gulp-util  gulp-sass  gulp-minify-css gulp-rename  gulp-autoprefixer gulp-util gulp-minify-css gulp-rename del gulp-uglify gulp-concat jshint gulp-jshint gulp-wrapper gulp-sourcemaps</pre>

<p style="text-align: justify;">
  The <code>--save-dev</code> option saves these packages to the <code>devDependencies</code> list in the <code>package.json:</code>
</p>

<pre class="lang:js decode:true" title="Gulp development dependencies">"devDependencies": {
	"gulp": "~3.6.1",
	"gulp-minify-css": "~0.3.1",
	"gulp-util": "~2.2.14",
	"gulp-rename": "~1.2.0",
	"gulp-sass": "~0.7.1",
	"gulp-autoprefixer": "0.0.7"
}</pre>

<p class="alert_note" style="text-align: justify;">
  There are all development dependencies as I only use this project to generate a CSS file. For libraries used in production you would add those under the &#8220;dependencies&#8221; element in your <em>package.json</em> file. The <em><a title="https://npmjs.org/doc/json.html" href="https://npmjs.org/doc/json.html" target="_blank">package.json</a></em> file holds variaous metadata relevant to the project and is contained in all <a title="http://en.wikipedia.org/wiki/Npm_%28software%29" href="http://en.wikipedia.org/wiki/Npm_%28software%29" target="_blank">npm</a> packages. You can create the <em>package.json</em> file according to the <a title="https://www.npmjs.org/doc/json.html" href="https://www.npmjs.org/doc/json.html" target="_blank">docs</a> or by issueing <code>npm init</code> in the root of of your project. You can find the complete package.json file for this project on <a href="https://github.com/Codingpedia/podcastpedia">GitHub</a>.
</p>

<p style="text-align: justify;">
  Now that we have everything set up, let&#8217;s do some actual work, and by that I mean some <strong>coding</strong> not configuration, as with Gulp the build file is code not config.
</p>

<h2 style="text-align: justify;">
  <span id="2_gulpfilejs">2. gulpfile.js</span>
</h2>

### <span id="21_Load_the_modules">2.1. Load the modules</span>

<p style="text-align: justify;">
  Node has a simple module loading system. In Node, files and modules are in one-to-one correspondence. When I installed the plugins in the previous step, they have been downloaded locally in the root folder under <em>node_modules</em>. You can load them now in the application via the <code>require()</code> function. Variables are defined to be used later in the gulp tasks:
</p>

<pre class="lang:js decode:true" title="define plugin variables">var gulp = require("gulp"),//http://gulpjs.com/
	util = require("gulp-util"),//https://github.com/gulpjs/gulp-util
	sass = require("gulp-sass"),//https://www.npmjs.org/package/gulp-sass
	autoprefixer = require('gulp-autoprefixer'),//https://www.npmjs.org/package/gulp-autoprefixer
	minifycss = require('gulp-minify-css'),//https://www.npmjs.org/package/gulp-minify-css
	rename = require('gulp-rename'),//https://www.npmjs.org/package/gulp-rename
	log = util.log;</pre>

### <span id="22_Gulp_tasks">2.2. Gulp tasks</span>

Gulp works with tasks. You define them by

  * registering a name (e.g. `"sass"`, `"watch"`)
  * an array of tasks to be executed and completed before your task will run
  * and a function that performs the task&#8217;s operations:

<pre class="lang:js decode:true" title="How to define a Gulp task">gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});</pre>

### <span id="23_Pipes">2.3. Pipes</span>

Gulp&#8217;s power lies in its code over configuration approach and the use of <a title="http://nodejs.org/api/stream.html" href="http://nodejs.org/api/stream.html" target="_blank">streams</a>. Streams use `.pipe()` to pair inputs with outputs.

`".pipe()` is just a function that takes a readable source stream `src` and hooks the output to a destination writable stream `dst`:

<pre class="lang:default decode:true">src.pipe(dst)</pre>

`.pipe(dst)` returns `dst` so that you can chain together multiple `.pipe()` calls together:

<pre class="lang:default decode:true">a.pipe(b).pipe(c).pipe(d)</pre>

which is the same as:

<pre class="lang:default decode:true">a.pipe(b);
b.pipe(c);
c.pipe(d);</pre>

This is very much like what you might do on the command-line to pipe programs together:

<pre>a | b | c | d</pre>

except in node instead of the shell! &#8221; [7]

<p style="text-align: justify;">
  Using the principle mentioned above I defined a &#8220;sass&#8221; task, which will eventually generate the CSS file. To achieve that I &#8220;piped&#8221; the following operations
</p>

  * load the .scss files
  * autoprefix them,
  * write them expanded to file
  * minify them
  * and write them (_podcastpedia.css_, _podcastpedia.min.css_) minified to disk in the folder _target/css_:

<pre class="lang:js decode:true" title="Define gulp sass piped task">gulp.task("sass", function(){
	log("Generate CSS files " + (new Date()).toString());
    gulp.src(sassFiles)
		.pipe(sass({ style: 'expanded' }))
					.pipe(autoprefixer("last 3 version","safari 5", "ie 8", "ie 9"))
		.pipe(gulp.dest("target/css"))
		.pipe(rename({suffix: '.min'}))
		.pipe(minifycss())
		.pipe(gulp.dest('target/css'));
});</pre>

To run the task I have to execute `gulp sass` on the command line in the root folder.

<p class="alert_note" style="text-align: justify;">
  With the help of gulp-autoprefixer plugin, you can avoid having to write tedious mixins for vendor prefixes. As it is set up here &#8211; <code>autoprefixer("last 3 version","safari 5", "ie 8", "ie 9")</code>, this plugin will do the vendor prefixes covering the last 3 versions of major browsers plus dedicated configuration for safari5, and internet explorer 8 and 9.
</p>

<h3 style="text-align: justify;">
  <span id="24_8220Watching8221_files_for_modifications">2.4. &#8220;Watching&#8221; files for modifications</span>
</h3>

<p style="text-align: justify;">
  Gulp has a built-in &#8220;watch&#8221; functionality &#8211; you can watch files and every time when a file changes you can do something. In the following example:
</p>

<pre class="lang:js decode:true" title="Define gulp watch task">gulp.task("watch", function(){
	log("Watching scss files for modifications");
	gulp.watch(sassFiles, ["sass"]);
});</pre>

<p style="text-align: justify;">
  every time I modify a <em>.scss</em> file the &#8220;sass&#8221; task will be executed.
</p>

Well, that&#8217;s it &#8211; few lines of code for pretty good functionality. I am looking forward to dwelve more and find out what Gulp can do&#8230; Your comments, suggestions and code contributions are very welcomed.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="3_Resources">3. Resources</span>

### <span id="31_Source_code">3.1. Source code</span>

  * Github repository &#8211; <a title="https://github.com/podcastpedia/css-generator-sass-gulp" href="https://github.com/podcastpedia/css-generator-sass-gulp" target="_blank">https://github.com/podcastpedia/css-generator-sass-gulp</a>

### <span id="32_Codingpediaorg_related">3.2. Codingpedia.org related</span>

  * <a title="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" href="http://www.codingpedia.org/ama/css-preprocessors-introducing-sass-to-podcastpedia-org/" target="_blank">CSS Preprocessors – Introducing Sass to Podcastpedia.org</a>

### <span id="33_Web">3.3. Web</span>

  1. <a title="http://nodejs.org/" href="http://nodejs.org/" target="_blank">nodejs</a>
  2. <a title="http://gulpjs.com/" href="http://gulpjs.com/" target="_blank">gulpjs</a>
  3. <a title="http://gulpjs.com/plugins/" href="http://gulpjs.com/plugins/" target="_blank">gulp plugins</a>
  4. <a title="https://www.npmjs.org/doc/json.html" href="https://www.npmjs.org/doc/json.html" target="_blank">package.json</a>
  5. <a title="http://markgoodyear.com/2014/01/getting-started-with-gulp/" href="http://markgoodyear.com/2014/01/getting-started-with-gulp/" target="_blank">Getting started with Gulp </a>
  6. <a title="http://danreev.es/posts/getting-started-with-gulp/" href="http://danreev.es/posts/getting-started-with-gulp/" target="_blank">Getting Started With Gulp.js</a>
  7. <a title="https://github.com/substack/stream-handbook" href="https://github.com/substack/stream-handbook" target="_blank">Stream-handbook</a>
  8. <a title="http://slides.com/contra/gulp" href="http://slides.com/contra/gulp" target="_blank">Gulp &#8211; the streaming build system [presentation]</a>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="Codingpedia, sharing coding knowledge" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
