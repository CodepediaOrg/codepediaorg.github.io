---
id: 1429
title: Error handling in REST API with Jersey
date: 2014-06-06T07:22:10+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1429
permalink: /ama/error-handling-in-rest-api-with-jersey/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 21
fsb_social_google:
  - 9
fsb_social_linkedin:
  - 5
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2740629436
categories:
  - java
tags:
  - error handling
  - exception
  - jersey
  - rest
  - rest api
---

<p style="color: #222222; text-align: justify;">
  Error handling is one of the most procrastinated and least enjoyable parts when writing code&#8230; I mean, why should the application not always work as expected, when we&#8217;ve written it with so much passion and care, and, you know, the clients of the application always send the right requests, right?!? Unfortunately things do go wrong from time to time, and when it does we should be prepared to some extent at least&#8230; This is especially the case when writing REST APIs, because clients only get a black box with specification, having no clue what to do when the expected answer doesn&#8217;t come back, unless we do something about it&#8230;
</p>

<p style="color: #222222; text-align: justify; padding-left: 30px;">
  <strong>Bottom line:</strong>  error handling is essential when designing REST APIs.
</p>
 <!--more-->
<p style="color: #222222; text-align: justify;">
  In this post I will present how I&#8217;ve designed and implemented the error handling of a REST API with the help Jersey framework. The<span class="aioseop_option_input"><span id="aioseop_snippet_description"> code is based on the open source project presented in the <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>&#8220;</span></span>
</p>

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Design">1. Design</a>
    </li>
    <li>
      <a href="#2_Implementation">2. Implementation</a><ul>
        <li>
          <a href="#21_Checkedbusiness_exceptions">2.1. Checked(business) exceptions</a>
        </li>
        <li>
          <a href="#22_Uncheckedtechnical_exceptions">2.2. Unchecked(technical) exceptions</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Testing">3. Testing</a>
    </li>
    <li>
      <a href="#4_Custom_Reason_Phrase_in_HTTP_status_error_message_response_with_JAX-RS_Jersey">4. Custom Reason Phrase in HTTP status error message response with JAX-RS (Jersey)</a>
    </li>
    <li>
      <a href="#5_Summary">5. Summary</a>
    </li>
    <li>
      <a href="#6_Resources">6. Resources</a><ul>
        <li>
          <a href="#61_Source_Code">6.1. Source Code</a>
        </li>
        <li>
          <a href="#62_Codingpedia">6.2. Codingpedia</a>
        </li>
        <li>
          <a href="#63_Web">6.3. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<h2 style="color: #222222; text-align: justify;">
  <span id="1_Design">1. Design</span>
</h2>

<p style="text-align: justify;">
  There are usually two causes for error occurence: either something failed on the server or the client of the API sent an invalid request. In both cases I&#8217;ve decided to send back a response with some metadata that should be useful for client, and potentially the developers of the client application. But I also didn&#8217;t forget the developers of the API and the people monitoring it, because most likely they will be the ones that would have to react and debug when something goes wrong.
</p>

<p class="note_normal">
  Note: The design presented here is based on the great work and resources I stumbled upon from the post &#8211; <a title="http://www.codingpedia.org/ama/resources-on-how-to-design-error-handling-in-a-rest-api/" href="http://www.codingpedia.org/ama/resources-on-how-to-design-error-handling-in-a-rest-api/" target="_blank">Resources on how to design error handling in a REST API.</a>
</p>

The first way to let the client know that an error occurred, is by sending HTTP error codes in the response:

<li style="text-align: justify;">
  <strong>4xx Client error</strong> &#8211;  codes are used to tell the client that a fault has taken place on THEIR side. They should not re-transmit the same request again, but fix the error first.
</li>
<li style="text-align: justify;">
  <strong>5xx Server error</strong> &#8211; codes that tell the client the server failed to fulfill an apparently valid request.  The client can continue and try again with the request without modification.
</li>

<p style="text-align: justify;">
  The second thing is<strong> to give the API clients as much information as possible about the error</strong> &#8211; based on reason, necessity and best practices found in the resources mentioned above, I&#8217;ve come up with the following structure for error messages:
</p>

<pre>
  <code class="json">
    {
       "status": 400,
       "code": 4000,
       "message": "Provided data not sufficient for insertion",
       "link": "http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-with-jersey-and-spring",
       "developerMessage": "Please verify that the feed is properly generated/set"
    }
  </code>
</pre>

Here&#8217;s the explanation for each of the properties in the response:

<li style="text-align: justify;">
  <strong>status</strong> &#8211; holds redundantly the HTTP error status code, so that the developer can &#8220;see&#8221; it without having to analyze the response&#8217;s header
</li>
   <strong>code</strong> &#8211; this is an internal code specific to the API (should be more relevant for business exceptions)
<li style="text-align: justify;">
  <strong>message</strong> &#8211; short description of the error, what might have cause it and possibly a &#8220;fixing&#8221; proposal
</li>
<li style="text-align: justify;">
  <strong>link</strong> &#8211; points to an online resource, where more details can be found about the error
</li>
<li style="text-align: justify;">
  <strong>developerMessage</strong> &#8211; detailed message, containing additional data that might be relevant to the developer. This should only be available when the &#8220;debug&#8221; mode is switched on and could potentially contain stack trace information or something similar
</li>

<h2 style="color: #222222; text-align: justify;">
  <span id="2_Implementation">2. Implementation</span>
</h2>

Now that I know how errors should look like, let&#8217;s write some code to make this a reality. In <a title="https://jersey.java.net/documentation/latest/user-guide.html#d0e5171" href="https://jersey.java.net/documentation/latest/user-guide.html#d0e5171" target="_blank">Jersey</a> you have currently to possibilities to handle exceptions via

  * WebApplicationExceptionOR/AND
  * mapping exceptions to responses via ExceptionMappers

<p style="color: #222222;">
  I like the second approach because it gives more control on the error entity I might send back. This is the approach I used throughout the tutorial.
</p>

<h3 style="color: #222222;">
  <span id="21_Checkedbusiness_exceptions">2.1. Checked(business) exceptions</span>
</h3>

<p style="color: #222222; padding-left: 30px;">
  <em>&#8220;<strong style="color: #000000;">Checked Exceptions</strong><span style="color: #000000;"> should be used to declare for </span><strong style="color: #000000;">expected</strong><span style="color: #000000;">, but </span><strong style="color: #000000;">unpreventable</strong><span style="color: #000000;"> errors that are </span><strong style="color: #000000;">reasonable to recover</strong><span style="color: #000000;"> from.</span>&#8220;[<a title="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" href="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" target="_blank">3</a>]</em>
</p>

<p style="color: #222222; text-align: justify;">
  First I packed the checked(business) exceptions under the <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/AppException.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/AppException.java" target="_blank"><code>AppException</code></a> class. The structure of the class has exactly the properties mentioned for errors in the Design section, which are also mirrored in the <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/ErrorMessage.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/ErrorMessage.java" target="_blank">ErrorMessage</a> class holding the JSON entity in the response. With the help of the <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/AppExceptionMapper.java" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/java/org/codingpedia/demo/rest/errorhandling/AppExceptionMapper.java" target="_blank"><code>AppExceptionMapper</code></a>:
</p>

<pre>
  <code class="java">
    package org.codingpedia.demo.rest.errorhandling;

    import javax.ws.rs.core.MediaType;
    import javax.ws.rs.core.Response;
    import javax.ws.rs.ext.ExceptionMapper;

    public class AppExceptionMapper implements ExceptionMapper&lt;AppException&gt; {

    	public Response toResponse(AppException ex) {
    		return Response.status(ex.getStatus())
    				.entity(new ErrorMessage(ex))
    				.type(MediaType.APPLICATION_JSON).
    				build();
    	}

    }
  </code>
</pre>

when an `AppException` occurs, let&#8217;s say for example you requested a podcast-resource that might have been deleted in the mean time:

<pre>
  <code class="http">
    GET http://localhost:8888/demo-rest-jersey-spring/podcasts/22 HTTP/1.1
    Accept-Encoding: gzip,deflate
    Accept: application/json
    Host: localhost:8888
    Connection: Keep-Alive
    User-Agent: Apache-HttpClient/4.1.1 (java 1.5)
  </code>
</pre>

the exception mapper encapsulates the error message as JSON, with the HTTP status you desire and you might get a response like the following:

<pre>
  <code class="http">
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Methods: GET, POST, DELETE, PUT
    Access-Control-Allow-Headers: X-Requested-With, Content-Type, X-Codingpedia
    Content-Length: 231
    Server: Jetty(9.0.7.v20131107)

    {
       "status": 404,
       "code": 404,
       "message": "The podcast you requested with id 22 was not found in the database",
       "link": "http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/",
       "developerMessage": "Verify the existence of the podcast with the id 22 in the database"
    }
  </code>
</pre>

<p class="note_normal">
  <strong>Note: </strong>After receiving this message, it should be clear to the client that the resource is not available anymore and can react to that accordingly.
</p>

### <span id="22_Uncheckedtechnical_exceptions">2.2. Unchecked(technical) exceptions</span>

<p style="color: #222222; padding-left: 30px;">
  <em><strong style="color: #000000;">&#8220;Unchecked Exceptions</strong><span style="color: #000000;"> should be used for everything else.&#8221;<em style="color: #222222;">[<a title="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" href="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" target="_blank">3</a>]</em></span></em>
</p>

<p style="color: #222222;">
  I&#8217;ve decided to catch all other exception that are not of type <code>AppException</code>, by implementing an <code>ExceptionMapper</code> on <code>Throwable</code>:
</p>

<pre>
  <code class="java">
    package org.codingpedia.demo.rest.errorhandling;

    //imports omitted for brevity

    import org.codingpedia.demo.rest.filters.AppConstants;

    public class GenericExceptionMapper implements ExceptionMapper&lt;Throwable&gt; {

    	@Override
    	public Response toResponse(Throwable ex) {

    		ErrorMessage errorMessage = new ErrorMessage();		
    		setHttpStatus(ex, errorMessage);
    		errorMessage.setCode(AppConstants.GENERIC_APP_ERROR_CODE);
    		errorMessage.setMessage(ex.getMessage());
    		StringWriter errorStackTrace = new StringWriter();
    		ex.printStackTrace(new PrintWriter(errorStackTrace));
    		errorMessage.setDeveloperMessage(errorStackTrace.toString());
    		errorMessage.setLink(AppConstants.BLOG_POST_URL);

    		return Response.status(errorMessage.getStatus())
    				.entity(errorMessage)
    				.type(MediaType.APPLICATION_JSON)
    				.build();
    	}

    	private void setHttpStatus(Throwable ex, ErrorMessage errorMessage) {
    		if(ex instanceof WebApplicationException ) {
    			errorMessage.setStatus(((WebApplicationException)ex).getResponse().getStatus());
    		} else {
    			errorMessage.setStatus(Response.Status.INTERNAL_SERVER_ERROR.getStatusCode()); //defaults to internal server error 500
    		}
    	}
    }
  </code>
</pre>

**Note:**

<li style="text-align: justify;">
  this is not an invasive way of catching and presenting runtime exceptions; if one happens it will be presented to the client without cluttering the code&#8230;
</li>
<li style="text-align: justify;">
  if the error comes from Jersey (hence there is of type WebApplicationException), I inherit the HTTP status in from Jersey (line 28)
</li>
<li style="text-align: justify;">
  in the <strong>developerMessage</strong> property I set the error&#8217;s stack trace, thus giving the API developers a nice way to see, in browsers for example, what was the technical problem; the developerMessage should only be visible in debug-mode, or not at all if the security constraints impose it
</li>
<li style="text-align: justify;">
  in the online resource specified by the <strong>link</strong> property you could have a email/phone number listed where the clients of the API can give more details about the context of the error
</li>

## <span id="3_Testing">3. Testing</span>

Check out our video tutorial <a title="https://www.youtube.com/watch?v=XV7WW0bDy9c" href="https://www.youtube.com/watch?v=XV7WW0bDy9c" target="_blank">How to test a REST API with SoapUI</a>, there are tests cases there covering error handling:



<p class="note_code" style="text-align: justify;">
  You can find the complete test suite on <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/tree/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/resources/soapui" href="https://github.com/Codingpedia/demo-rest-jersey-spring/tree/9d13e664da1a04aa67dfe5e02ec45531219806af/src/main/resources/soapui" target="_blank">GitHub</a>.
</p>

<h2 style="text-align: justify;">
  <span id="4_Custom_Reason_Phrase_in_HTTP_status_error_message_response_with_JAX-RS_Jersey">4. Custom Reason Phrase in HTTP status error message response with JAX-RS (Jersey)</span>
</h2>

<p style="text-align: justify;">
  If for some reason you need to produce a custom Reason Phrase in the HTTP status response delivered when an error occurs than you might want to check the following post
</p>

<p style="padding-left: 30px;">
  <a title="http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/" href="http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/" target="_blank">http://www.codingpedia.org/ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/</a>
</p>

## <span id="5_Summary">5. Summary</span>

<p style="text-align: justify;">
  Well, that&#8217;s it. In this post you&#8217;ve learned one way to design error handling for a REST API and how you could implement it with Jersey.  I&#8217;d more than happy to hear about your approach on treating exceptions in REST APIs so if you have a suggestion please leave a message.
</p>

## <span id="6_Resources"><span id="10_Resources">6. </span><span id="10_Resources">Resources</span></span>

### <span id="61_Source_Code"><span id="101_Source_Code">6.1. Source Code</span></span>

  * <a style="color: #bc360a;" title="https://github.com/Codingpedia/demo-rest-jersey-spring" href="https://github.com/Codingpedia/demo-rest-jersey-spring" target="_blank">GitHub – Codingpedia/demo-rest-jersey-spring </a>(instructions on how to install and run the project)

### <span id="62_Codingpedia">6.2. Codingpedia</span>

  * <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
  * Compilation &#8211; <a title="http://www.codingpedia.org/ama/resources-on-how-to-design-error-handling-in-a-rest-api/" href="http://www.codingpedia.org/ama/resources-on-how-to-design-error-handling-in-a-rest-api/" target="_blank">Resources on how to design error handling in a REST API</a>
  * Video &#8211; <a title="https://www.youtube.com/watch?v=XV7WW0bDy9c" href="https://www.youtube.com/watch?v=XV7WW0bDy9c" target="_blank">How to test a REST API with SoapUI </a>

### <span id="63_Web">6.3. Web</span>

  1. <a title="http://www.hacktrix.com/checked-and-unchecked-exceptions-in-java" href="http://www.hacktrix.com/checked-and-unchecked-exceptions-in-java" target="_blank">Checked and Unchecked Exceptions in Java</a>
  2. <a title="http://restcookbook.com/HTTP%20Methods/400-vs-500/" href="http://restcookbook.com/HTTP%20Methods/400-vs-500/" target="_blank">When should we return 4xx or 5xx status codes to the client?</a>
  3. Stackoverflow &#8211; <a title="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" href="http://stackoverflow.com/questions/27578/when-to-choose-checked-and-unchecked-exceptions" target="_blank">When to choose checked and unchecked exceptions</a>

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

<p style="text-align: center;">
  “Featured Image courtesy of Stuart Miles / FreeDigitalPhotos.net”
</p>
