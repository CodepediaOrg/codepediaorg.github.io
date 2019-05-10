---
id: 2051
title: Custom Reason Phrase in HTTP status error message response with JAX-RS (Jersey)
date: 2014-10-28T20:15:08+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2051
permalink: /ama/custom-reason-phrase-in-http-status-error-message-response-with-jax-rs-jersey/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 3166737579
fsb_social_facebook:
  - 2
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - errors
  - java
tags:
  - error handling
  - http
  - http header
  - http reasponse
  - jax-rs
  - jersey
  - rest
  - rest api
---
<p style="text-align: justify;">
  In some of my recent work I got the request to produce a custom Reason Phrase in the  HTTP status response delivered to one of our REST API consuming clients when an error occurs. In this post I will demonstrate how you can achieve that with Jersey.<!--more-->
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#1_Define_checked_exception_and_exception_mapper">1. Define checked exception and exception mapper</a>
      </li>
      <li>
        <a href="#2_Implement_custom_StatusType">2. Implement custom StatusType</a>
      </li>
      <li>
        <a href="#3_Test_the_custom_Reason_Phrase_in_the_HTTP_status_response"> 3. Test the custom Reason Phrase in the HTTP status response</a><ul>
          <li>
            <a href="#31_Request">3.1. Request</a>
          </li>
          <li>
            <a href="#32_Response">3.2. Response</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Summary">Summary</a>
      </li>
      <li>
        <a href="#Resources">Resources</a>
      </li>
    </ul>
  </div>
</p>

<h2 style="text-align: justify;">
  <span id="1_Define_checked_exception_and_exception_mapper">1. Define checked exception and exception mapper</span>
</h2>

<p style="text-align: justify;">
  As you might have found out from my post <a title="http://www.codingpedia.org/ama/error-handling-in-rest-api-with-jersey/" href="http://www.codingpedia.org/ama/error-handling-in-rest-api-with-jersey/" target="_blank">Error handling in REST API with Jersey</a>, I like handling the checked exceptions using Jersey&#8217;s <a title="https://jersey.java.net/documentation/latest/user-guide.html#d0e5206" href="https://jersey.java.net/documentation/latest/user-guide.html#d0e5206" target="_blank">ExceptionMapper </a>capability.
</p>

For the purpose of this demonstration I defined a `CustomReasonPhraseException`:

<pre class="lang:java decode:true" title="CustomReasonPhraseException">package org.codingpedia.demo.rest.errorhandling;

public class CustomReasonPhraseException extends Exception {

	private static final long serialVersionUID = -271582074543512905L;

	private final int businessCode;

	public CustomReasonPhraseException(int businessCode, String message) {
		super(message);
		this.businessCode = businessCode;
	}

	public int getBusinessCode() {
		return businessCode;
	}

}</pre>

and a  `CustomReasonPhraseExceptionMapper` to handle the mapping to a response if a `CustomReasonPhraseException` occurs:

<pre class="lang:java mark:12 decode:true" title="CustomReasonPhraseExceptionMapper">package org.codingpedia.demo.rest.errorhandling;

import javax.ws.rs.core.Response;
import javax.ws.rs.core.Response.Status;
import javax.ws.rs.ext.ExceptionMapper;
import javax.ws.rs.ext.Provider;

@Provider
public class CustomReasonPhraseExceptionMapper implements ExceptionMapper&lt;CustomReasonPhraseException&gt; {

	public Response toResponse(CustomReasonPhraseException bex) {
		return Response.status(new CustomReasonPhraseExceptionStatusType(Status.BAD_REQUEST))
				.entity("Custom Reason Phrase exception occured : " + bex.getMessage())
				.build();
	}

}
</pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Reminder: </strong>When the application throws a <code>CustomReasonPhraseException</code> the <code>toResponse</code> method of the <code>CustomReasonPhraseExceptionMapper</code> instance will be invoked.
</p>

In the `ExceptionMapper` code note line 12:

<pre class="lang:java decode:true " title="CustomReasonPhraseExceptionStatusType">return Response.status(new CustomReasonPhraseExceptionStatusType(Status.BAD_REQUEST))</pre>

In Jersey&#8217;s `ResponseBuilder` you have the possibility to define your own status types, by implementing the `javax.ws.rs.core.Response.StatusType` interface.

## <span id="2_Implement_custom_StatusType">2. Implement custom StatusType</span>

To make a little more extensible I&#8217;ve created an `AbstractStatusType` class:

<pre class="lang:java decode:true" title="AbstractStatusType">package org.codingpedia.demo.rest.errorhandling;

import javax.ws.rs.core.Response.Status;
import javax.ws.rs.core.Response.Status.Family;
import javax.ws.rs.core.Response.StatusType;

/**
 * Class used to provide custom StatusTypes, especially for the the Reason Phrase that appears in the HTTP Status Response
 */
public abstract class AbstractStatusType implements StatusType {

	public AbstractStatusType(final Family family, final int statusCode,
                          final String reasonPhrase) {
	    super();

	    this.family = family;
	    this.statusCode = statusCode;
	    this.reasonPhrase = reasonPhrase;
	}

	protected AbstractStatusType(final Status status,
	                             final String reasonPhrase) {
	    this(status.getFamily(), status.getStatusCode(), reasonPhrase);
	}

	@Override
	public Family getFamily() { return family; }

	@Override
	public String getReasonPhrase() { return reasonPhrase; }

	@Override
	public int getStatusCode() { return statusCode; }

	private final Family family;
	private final int statusCode;
	private final String reasonPhrase;

}
</pre>

, which I extend afterwards with the `CustomReasonPhraseExceptionStatusType` to provide the custom `Reason Phrase` I desire (_e.g. &#8220;Custom error message&#8221;_) in the response:

<pre class="lang:java decode:true " title="CustomReasonPhraseExceptionStatusType extends AbstractStatusType">package org.codingpedia.demo.rest.errorhandling;

import javax.ws.rs.core.Response.Status;

/**
 * Implementation of StatusType for CustomReasonPhraseException.
 * The Reason Phrase is set in this case to "Custom error message"
 */
public class CustomReasonPhraseExceptionStatusType extends AbstractStatusType{

	private static final String CUSTOM_EXCEPTION_REASON_PHRASE = "Custom error message";

	public CustomReasonPhraseExceptionStatusType(Status httpStatus) {
		super(httpStatus, CUSTOM_EXCEPTION_REASON_PHRASE);
	}

}
</pre>

&nbsp;

## <span id="3_Test_the_custom_Reason_Phrase_in_the_HTTP_status_response"> 3. Test the custom Reason Phrase in the HTTP status response</span>

### <span id="31_Request">3.1. Request</span>

<pre class="lang:default mark:1 decode:true" title="Request example">GET http://localhost:8888/demo-rest-jersey-spring/mocked-custom-reason-phrase-exception HTTP/1.1
Accept-Encoding: gzip,deflate
Host: localhost:8888
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)</pre>

### <span id="32_Response">3.2. Response</span>

Et voila:

<pre class="lang:default mark:2 decode:true" title="Response example">HTTP/1.1 400 Custom error message
Content-Type: text/plain
Content-Length: 95
Server: Jetty(9.0.7.v20131107)

Custom Reason Phrase exception occured : message attached to the Custom Reason Phrase Exception</pre>

, the custom Reason Phrase appears in the response as expected.

<p style="text-align: justify;">
  <strong>Tip:</strong> If you want really learn how to design and implement REST API in Java read the following <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
</p>

## <span id="Summary">Summary</span>

<p style="text-align: justify;">
  You&#8217;ve seen in this post how to create a custom Reason Phrase in a HTTP status response when you want to flag a &#8220;special&#8221; error. Of course you can use this mechanism to define your own Reason Phrase-s for other HTTP statuses as well.  Actually you should not abuse this Reason Phrase feature as in the HTTP 1.1 rfc2616 is stated the following:
</p>

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;The Status-Code element is a 3-digit integer result code of the attempt to understand and satisfy the request. These codes are fully defined in section 10. The Reason-Phrase is intended to give a short textual description of the Status-Code. The Status-Code is intended for use by automata and the Reason-Phrase is intended for the human user. The client is not required to examine or display the Reason- Phrase.&#8221;[1]</em>
</p>

<p style="text-align: justify;">
  Well, that&#8217;s it. Keep on coding and keep on sharing coding knowledge.
</p>

## <span id="Resources">Resources</span>

  1. <a title="http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html" target="_blank">Hypertext Transfer Protocol &#8212; HTTP/1.1 &#8211; Response</a>
  2. <a title="https://jersey.java.net/documentation/latest/user-guide.html" href="https://jersey.java.net/documentation/latest/user-guide.html" target="_blank">Jersey User Guide</a>

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
