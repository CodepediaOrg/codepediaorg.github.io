---
id: 994
title: How to handle session timeout exceptions in Spring MVC
date: 2013-12-01T13:37:09+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=994
permalink: /ama/how-to-handle-session-timeout-exceptions-in-spring-mvc/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
dsq_thread_id:
  - 2014641170
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - spring
  - troubleshooting
tags:
  - bug
  - debug
  - exception
  - session
  - timeout
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#The_challenge">The challenge</a>
    </li>
    <li>
      <a href="#The_solution">The solution</a><ul>
        <li>
          <a href="#Modify_session-timeout_in_webxml">Modify session-timeout in web.xml</a>
        </li>
        <li>
          <a href="#Meet_the_ExceptionHandler">Meet the @ExceptionHandler</a>
        </li>
        <li>
          <a href="#Exception_handling_configuration_in_the_application_context">Exception handling configuration in the application context</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a>
    </li>
  </ul>
</div>

## <span id="The_challenge">The challenge</span>

<p style="text-align: justify;">
  I have been getting complaints from people who were trying to <a title="Suggest podcast to Podcastpedia.org" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help/add_podcast" target="_blank">suggest a podcast</a> to <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org, </a>and that for a good reason. They were getting a page with the following message:
</p>

<pre class="lang:default decode:true" title="Unknow error message">"Unknown error. Please inform us about it with the Error indication form."</pre>

<p style="text-align: justify;">
  This is the default page rendered when an unknown exception occurs. Although this is prettier than displaying the error stacktrace, it should be avoided &#8211; it didn&#8217;t tell the visitors much and me neither.
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>

  <!--more-->
</p>

<p style="text-align: justify;">
  I have been struggling with this for some time and couldn&#8217;t find the reason, because everytime I would fill the form with the same input provided by the visitors it would work perfectly for me. Until today, when somehow the session expiration setting popped into my mind. I recalled setting the session timeout pretty short &#8211; 1 minute (default in Tomcat is 30 minutes) &#8211; in the <code>web.xml</code> file, for &#8220;performance&#8221; reasons:
</p>

<pre class="lang:default mark:3 decode:true" title="Original session timeout configuration in web.xml">&lt;!-- set session timeout for 1 minute &lt;url-pattern&gt;/*.atom&lt;/url-pattern&gt; --&gt;
&lt;session-config&gt;
	&lt;session-timeout&gt;1&lt;/session-timeout&gt;
	&lt;tracking-mode&gt;COOKIE&lt;/tracking-mode&gt;
&lt;/session-config&gt;</pre>

I have to admit one minute is extremly short. So I filled the form, waited 1 min and 13 seconds et voilà, I got the same error when trying to submit.

The Spring controller handling the podcast submission form uses the `@SessionAttributes("addPodcastForm")` annotation. This indicates the model object &#8211; `addPodcastForm` &#8211; to be held in a session, so that if a validation error ocurs, the visitor doesn&#8217;t have to fill the fields all over again. This annotation forces the existence of a valid session and triggered the error message.

## <span id="The_solution">The solution</span>

### <span id="Modify_session-timeout_in_webxml">Modify session-timeout in web.xml</span>

<p style="text-align: justify;">
  So the actual fix for this challenge was pretty easy, I&#8217;ve just increased the <code>session-timeout</code> value from 1 to 10 minutes. But what if a visitor starts filling the form, goes away, drinks a coffee and returns to submit the form after 15 minutes, I wouldn&#8217;t want her to see the same <code>"Unknown error..."</code> message &#8211; one pointing to the error cause and how the user might react would be much better.
</p>

### <span id="Meet_the_ExceptionHandler">Meet the @ExceptionHandler</span>

<p style="text-align: justify;">
  Well, Spring isn&#8217;t considered a mighty framework for nothing. To catch such an exception all I had to do is annotate a method in the controller, which handles the form submission requests, with <code>@ExceptionHandler</code> and specify which type of Exception to handle:
</p>

<pre class="lang:java mark:1 decode:true" title="Controller method to handle Session timeout exception">@ExceptionHandler(HttpSessionRequiredException.class)
@ResponseStatus(value = HttpStatus.UNAUTHORIZED, reason="The session has expired"))
public String handleSessionExpired(){
  return "sessionExpired";
}</pre>

<p style="text-align: justify;">
  The <code>HttpSessionRequiredException</code> requires a pre-existing session. The <code>@ResponseStatus</code> is optional and marks the method with the status code and the reason that should be returned. Several return types are supported for handler methods, but when I return a String &#8211; <code>sessionExpired</code> &#8211; this value is interpreted as a view name. In the Tiles configuration there is a definition which, based on this name, renders now a page with a corresponding message for session expiration.
</p>

<p class="note_normal">
  <strong>Note:</strong> Please see my post <a title="Spring MVC and Apache Tiles integration example" href="http://www.codingpedia.org/ama/spring-mvc-and-apache-tiles-integration-example/" target="_blank">Spring MVC and Apache Tiles integration example</a> for an explanation of using Tiles definitions with Spring MVC.
</p>

### <span id="Exception_handling_configuration_in_the_application_context">Exception handling configuration in the application context</span>

<pre class="lang:default mark:2,3 decode:true" title="Exception handling configuration in Application context">............
&lt;bean class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver" p:order="1" /&gt;
&lt;bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver" p:order="2" p:defaultErrorView="uncaughtException"&gt;
  &lt;property name="exceptionMappings"&gt;
	&lt;props&gt;
	  &lt;prop key="org.springframework.core.NestedRuntimeException.DataAccessException"&gt;dataAccessFailure&lt;/prop&gt;
	  &lt;prop key="org.springframework.web.servlet.mvc.multiaction.NoSuchRequestHandlingMethodException"&gt;resourceNotFound&lt;/prop&gt;
	  &lt;prop key="org.springframework.beans.TypeMismatchException"&gt;resourceNotFound&lt;/prop&gt;
	  &lt;prop key="org.springframework.web.bind.MissingServletRequestParameterException"&gt;resourceNotFound&lt;/prop&gt;
	&lt;/props&gt;
  &lt;/property&gt;
&lt;/bean&gt;
............</pre>

<p style="text-align: justify;">
  The exception handling is configured in the application context via
</p>

  * `ExceptionHandler` &#8211; resolves exceptions through `@ExceptionHandler` methods as our above method. It is configured to take precedence because the `p:order` to 1.
<li style="text-align: justify;">
  <code>SimpleMappingExceptionResolver</code> &#8211; allows for mapping exception class names to view names, either for a set of given handlers or for all handlers in the DispatcherServlet. Notice here the <code>defaultErrorView</code> attribute which sets the name of the default error view, if no specific mapping was found &#8211; this was the case for the session timeout exception.
</li>

Well, that&#8217;s it. I hope you could learn something from this as I did.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="Resources">Resources</span>

  1. <a title="Handling exceptions" href="http://docs.spring.io/spring/docs/3.2.5.RELEASE/spring-framework-reference/htmlsingle/#mvc-exceptionhandlers" target="_blank">Spring framework reference &#8211; MVC Handling exceptions</a>

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
