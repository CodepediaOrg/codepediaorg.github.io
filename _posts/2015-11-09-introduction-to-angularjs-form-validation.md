---
id: 2535
title: Introduction to AngularJs Form Validation
date: 2015-11-09T21:03:12+00:00
author: Aleksey Novik
layout: post
guid: http://www.codepedia.org/?p=2535
permalink: /jhadesdev/introduction-to-angularjs-form-validation/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4304221659
fsb_social_facebook:
  - 0
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
  - web development
tags:
  - angular
  - angularJS
  - form validation
  - web development
---
<p style="text-align: justify;">
  AngularJs is often seen as a viable solution for building full scale single page web applications. In this post we will go over how Angular is particularly well suited for building form-intensive large scale apps due it&#8217;s numerous form validation features (<a href="https://jsfiddle.net/jhadesdev/yfLqfzLw/2/">example</a>).<!--more-->
</p>

<h3 id="angularwasinitiallyatoolformakingformssimpler" style="text-align: justify;">
  Angular was initially a tool for making forms simpler
</h3>

<p style="text-align: justify;">
  Before it became an open source framework, Angular was meant as a development tool targeted mostly at web designers. The framework would allow designers to enhance plain HTML pages and give them the ability to validate and persist forms in a transparent way (see reference<a href="http://devchat.tv/adventures-in-angular/001-aia-the-birth-of-angular-1">here</a>).
</p>

<p style="text-align: justify;">
  The initial idea was to allow designers to use some extra HTML tags or attributes for adding dynamic capabilities to a web page, without having to write almost any Javascript at all.
</p>

<h3 id="usingangularforformvalidation" style="text-align: justify;">
  Using Angular for form validation
</h3>

<p style="text-align: justify;">
  At 44k minified and compressed, Angular packs a lot of interesting functionality for doing dynamic form validation.
</p>

<p style="text-align: justify;">
  Take for example the following form for user creation, it does the following (interactive example bellow):
</p>

<ul style="text-align: justify;">
  <li>
    highlights fields in error dynamically <em>as we type</em>
  </li>
  <li>
    provide inline messages while the user is typing in a field
  </li>
  <li>
    disable the submit button until all the needed data is available and the terms and conditions checkbox is checked
  </li>
</ul>

<p style="text-align: justify;">
  For a working version of this form implementing the above functionality, check the following <a href="https://github.com/jhades/blog.jhades.org/tree/master/angular-not-only-spa">github</a> repository.
</p>

<p style="text-align: justify;">
  Note that the example above only needs <em>a single line of Javascript code</em>, that is necessary to import the <code>ng-messages</code> module:
</p>

<pre class="lang:default decode:true">angular.module('DemoApp', ['ngMessages']);</pre>

<p style="text-align: justify;">
  Apart from that, the whole functionality is based on built-in Angular directives and <code>ng-messages</code>.
</p>

<h3 id="theangularformdirective" style="text-align: justify;">
  The Angular form directive
</h3>

<p style="text-align: justify;">
  When Angular compiles the DOM inside <code>ng-app</code>, it applies to form tags a built-in form directive. In the case of the above form, this means that the initial HTML of the form will look something like this:
</p>

<pre class="lang:default decode:true">&lt;form class="ng-pristine ng-invalid 
ng-invalid-required" name="frm"&gt;</pre>

<p style="text-align: justify;">
  Notice the strange looking <code>ng-pristine</code> and the <code>ng-invalid</code> classes that Angular automatically adds depending on the form state.
</p>

<h4 id="howdothesebuiltinstateclasseswork" style="text-align: justify;">
  How do these built-in state classes work?
</h4>

<p style="text-align: justify;">
  These classes are applied to both the form and the individual input fields, and can be used to help style the form. The following states are provided by Angular:
</p>

<ul style="text-align: justify;">
  <li>
    ng-pristine: this means the form or input is in it&#8217;s initial state: no input fields where touched or edited
  </li>
  <li>
    ng-touched: the control or form was clicked by the user, but not necessarily edited
  </li>
  <li>
    ng-dirty: the input control was edited by the user
  </li>
  <li>
    ng-invalid: the form control was edited but it contains an invalid value
  </li>
  <li>
    ng-valid: the input control was edited by the user and contains a valid value
  </li>
</ul>

<p style="text-align: justify;">
  For example the <code>ng-invalid</code> state can be used to paint a red border around fields in error.
</p>

<h3 id="angularbuiltinvalidationdirectives" style="text-align: justify;">
  Angular built-in validation directives
</h3>

<p style="text-align: justify;">
  The validations of each field can be declared directly in HTML. For example the password field has the following validations:
</p>

<ul style="text-align: justify;">
  <li>
    field is mandatory
  </li>
  <li>
    minimum length of 6 characters
  </li>
  <li>
    maximum length of 10 characters
  </li>
</ul>

<p style="text-align: justify;">
  These validations are declared using the <code>required</code>, <code>ng-minlength</code> and<br /> <code>ng-maxlength</code> directives:
</p>

<pre class="lang:default decode:true">&lt;input name="password" ng-model="user.password" type="password"  placeholder="Password" required ng-minlength="6" ng-maxlength="10"&gt;</pre>

<div class="fix-syntax-highlight" style="text-align: justify;">
</div>

<p style="text-align: justify;">
  See <a href="https://docs.angularjs.org/api/ng/directive/input">here</a> the full list of built-in directives that can be applied to a form input. It&#8217;s also possible to create our own validation directives.
</p>

<h3 id="enhancedformvalidationwithngmessages" style="text-align: justify;">
  Enhanced form validation with ng-messages
</h3>

<p style="text-align: justify;">
  While the user is entering data, it&#8217;s helpful to show a message informing him of the next step in the validation process, for example:
</p>

<ul style="text-align: justify;">
  <li>
    first show that a field is required
  </li>
  <li>
    when the user starts typing, show a message indicating the minimum length
  </li>
</ul>

<p style="text-align: justify;">
  This is how the form should look with several of it&#8217;s components dirty and invalid:
</p>

<div style="text-align: justify;">
  <img src="http://d2huq83j2o5dyd.cloudfront.net/angular-not-only-spa/invalid.png" alt="" />
</div>

<p style="text-align: justify;">
  This functionality can be implemented using the <a href="https://docs.angularjs.org/api/ngMessages/directive/ngMessages">ng-messages</a> directive:
</p>

<pre class="lang:default decode:true">&lt;div class="pure-control-group"&gt;
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

<div class="fix-syntax-highlight" style="text-align: justify;">
</div>

<h3 id="controllingthestateofthesubmitbutton" style="text-align: justify;">
  Controlling the state of the submit button
</h3>

<p style="text-align: justify;">
  The submit button can be made enabled/disabled according to the form validity and the condition checkbox using the <code>ng-disabled</code> directive:
</p>

<pre class="lang:default decode:true">&lt;button type="submit" class="pure-button pure-button-primary" 
ng-disabled="frm.$invalid || !conditions"&gt;Submit&lt;/button&gt;</pre>

<div class="fix-syntax-highlight" style="text-align: justify;">
</div>

<h3 id="conclusions" style="text-align: justify;">
  Conclusions
</h3>

<p style="text-align: justify;">
  With the use of the Angular built-in directives and some additional modules, it&#8217;s possible to add a lot of commonly needed form validation features to an application, using a very small amount of code (see<a href="https://github.com/jhades/blog.jhades.org/tree/master/angular-not-only-spa">example</a>).
</p>

<p style="text-align: justify;">
  Used together with <code>ng-messages</code> and <code>ng-model</code>, the form validation built-in functionality makes building complex forms with Angular a much more approachable task.
</p>

<h3 id="references" style="text-align: justify;">
  References
</h3>

<p style="text-align: justify;">
  A thorough walk-through of Angular form validation can be found on this <a href="https://www.youtube.com/watch?v=t6XUPVmlYbY">screencast</a> (pre-ngMessages).
</p>
