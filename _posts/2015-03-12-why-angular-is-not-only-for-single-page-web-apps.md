---
id: 2331
title: Why Angular is not only for single page web apps
date: 2015-03-12T10:46:43+00:00
author: Aleksey Novik
layout: post
guid: http://www.codingpedia.org/?p=2331
permalink: /jhadesdev/why-angular-is-not-only-for-single-page-web-apps/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:74:"https://github.com/jhades/blog.jhades.org/tree/master/angular-not-only-spa";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 3588499111
fsb_social_facebook:
  - 1
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - javascript
tags:
  - angular
  - angularJS
  - form
  - forma validation
  - javascript
  - single page application
  - spa
  - web design
---
<p style="text-align: justify;">
  AngularJs is often seen as a viable solution for building full scale single page web applications. Although that is a good use case for Angular, in this post we will explore how the framework can also be used to incrementally add functionality to any web page in general, in some cases <em>with little to no Javascript needed</em> (<a href="https://jsfiddle.net/jhadesdev/yfLqfzLw/2/">example</a>).<!--more-->
</p>

### The initial intent of Angular {#theinitialintentofangular}

<p style="text-align: justify;">
  Before it became an open source framework, Angular was meant as a development tool targeted mostly at web designers. The framework would allow designers to enhance HTML pages and give them the ability to validate and persist forms in a transparent way (see reference <a href="http://devchat.tv/adventures-in-angular/001-aia-the-birth-of-angular-1">here</a>).
</p>

<p style="text-align: justify;">
  The initial idea was to allow designers to use some extra HTML tags or attributes for adding dynamic capabilities to a web page, without having to write any Javascript at all.
</p>

### Using Angular incrementally in existing apps {#usingangularincrementallyinexistingapps}

Currently Angular is likely to be used as the main MVC framework of a medium to large scale single page web application. But Angular can also be used to incrementally add functionality to existing web applications, in a way very similar to how it was initially intended to be used.

Let&#8217;s give one concrete example: form validation.

### Example: using Angular for form validation {#exampleusingangularforformvalidation}

At 44k minified and compressed, Angular packs a lot of interesting functionality for doing dynamic form validation.

Take for example the following form for user creation, it does the following (interactive example bellow):

  * highlights fields in error dynamically _as we type_
  * provide inline messages while the user is typing in a field
  * disable the submit button until all the needed data is available and the terms and conditions checkbox is checked

For a working version of this form implementing the above functionality, check the following [github](https://github.com/jhades/blog.jhades.org/tree/master/angular-not-only-spa) repository.

Note that the example above only needs _a single line of Javascript code_, that is necessary to import the `ng-messages` module:

<pre class="lang:js decode:true ">angular.module('DemoApp', ['ngMessages']);</pre>

Apart from that, the whole functionality is based on built-in Angular directives and `ng-messages`.

### The Angular form directive {#theangularformdirective}

When Angular compiles the DOM inside `ng-app`, it applies to form tags the built-in form directive. In the case of the above form, this means that the initial HTML of the form will look something like this:

<pre class="lang:js decode:true">&lt;form class="ng-pristine ng-invalid  
ng-invalid-required" name="frm"&gt;  
&lt;/form&gt;</pre>

Notice the strange looking `ng-pristine` and the `ng-invalid` classes that Angular automatically adds depending on the form state.

#### How do these built-in state classes work? {#howdothesebuiltinstateclasseswork}

These classes are applied to both the form and the individual input fields, and can be used to help style the form. The following states are provided by Angular:

  * ng-pristine: this means the form or input is in it&#8217;s initial state: no input fields where touched or edited
  * ng-touched: the control or form was clicked by the user, but not necessarily edited
  * ng-dirty: the input control was edited by the user
  * ng-invalid: the form control was edited but it contains an invalid value
  * ng-valid: the input control was edited by the user and contains a valid value

For example the `ng-invalid` state can be used to paint a red border around fields in error.

### Angular built-in validation directives {#angularbuiltinvalidationdirectives}

The validations of each field can be declared directly in HTML. For example the password field has the following validations:

  * field is mandatory
  * minimum length of 6 characters
  * maximum length of 10 characters

These validations are declared using the `required`, `ng-minlength` and
  
`ng-maxlength` directives:

<pre class="lang:js decode:true">&lt;input name="password" ng-model="user.password" type="password"  placeholder="Password" required ng-minlength="6" ng-maxlength="10"&gt;</pre>

See [here](https://docs.angularjs.org/api/ng/directive/input) the full list of built-in directives that can be applied to a form input. It&#8217;s also possible to create our own validation directives.

### Enhanced form validation with ng-messages {#enhancedformvalidationwithngmessages}

While the user is entering data, it&#8217;s helpful to show a message informing him of the next step in the validation process, for example:

  * first show that a field is required
  * when the user starts typing, show a message indicating the minimum length

This is how the form should look with several of it&#8217;s components dirty and invalid:

<div>
  <img src="http://d2huq83j2o5dyd.cloudfront.net/angular-not-only-spa/invalid.png" alt="" />
</div>

This functionality can be implemented using the [ng-messages](https://docs.angularjs.org/api/ngMessages/directive/ngMessages) directive:

<pre class="lang:xhtml decode:true ">&lt;div class="pure-control-group"&gt;
    &lt;label&gt;Password&lt;/label&gt;
    &lt;input name="password" ng-model="user.password"
        type="password" placeholder="Password"
        required ng-minlength="6" ng-maxlength="10"&gt;
    &lt;div class="field-message"  
    ng-messages="frm.password.$error" 
    ng-if='frm.password.$dirty' ng-cloak&gt;
        &lt;div ng-message="required"&gt;Password is required&lt;/div&gt;
        &lt;div ng-message="minlength"&gt;
            Password must have minimum 6 characters
        &lt;/div&gt;
        &lt;div ng-message="maxlength"&gt;
            Password must have maximum 10 characters
        &lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;</pre>

### Controlling the state of the submit button {#controllingthestateofthesubmitbutton}

The submit button can be made enabled/disabled according to the form validity and the condition checkbox using the `ng-disabled` directive:

<pre class="lang:js decode:true">&lt;button type="submit" class="pure-button pure-button-primary" ng-disabled="frm.$invalid || !conditions"&gt;Submit&lt;/button&gt;</pre>

### Conclusions {#conclusions}

<p style="text-align: justify;">
  With the use of the Angular built-in directives and some additional modules, it&#8217;s possible to add a lot of functionality to an existing web application.
</p>

<p style="text-align: justify;">
  In the concrete case of form validation, this can be done with almost no Javascript at all (see the above <a href="https://github.com/jhades/blog.jhades.org/tree/master/angular-not-only-spa">example</a>), and at a footprint cost (44k minified and compressed) similar to other alternatives.
</p>

<p style="text-align: justify;">
  But form validation is just an example of how Angular can also be used outside of the context of a single page web application in an incremental way.
</p>

<p style="text-align: justify;">
  Any type of existing app can benefit from it for doing small enhancements, such as to validate a form or implement a particular widget.
</p>

### References {#references}

A thorough walk-through of Angular form validation can be found on this [screencast](https://www.youtube.com/watch?v=t6XUPVmlYbY) (pre-ngMessages).

<p class="note_normal">
  Published at Codingpedia.org with permission of Aleksey Novik – source<a title="http://blog.jhades.org/why-angular-is-not-only-for-single-page-web-apps/" href="http://blog.jhades.org/why-angular-is-not-only-for-single-page-web-apps/"> Why Angular is not only for single page web apps</a> from <a title="http://blog.jhades.org/" href="http://blog.jhades.org/" target="_blank">http://blog.jhades.org/</a>
</p>

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
