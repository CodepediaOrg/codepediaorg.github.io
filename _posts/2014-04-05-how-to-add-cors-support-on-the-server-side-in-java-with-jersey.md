---
id: 1299
title: How to add CORS support on the server side in Java with Jersey
date: 2014-04-05T14:50:56+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1299
permalink: /ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/
fsb_show_social:
  - 0
dsq_thread_id:
  - 2588437371
fsb_social_facebook:
  - 11
fsb_social_google:
  - 13
fsb_social_linkedin:
  - 3
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 1
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
categories:
  - java
tags:
  - ajax
  - cors
  - http
  - http header
  - jersey
  - resource
  - rest
  - XMLHttpRequest
---
<p style="text-align: justify;">
  In this post I will present how easy it is to enable HTTP response headers on the server sidein Java with Jersey, as defined by the <a title="http://www.w3.org/TR/cors/" href="http://www.w3.org/TR/cors/" target="_blank">Cross-Origing Resource Sharing (CORS)</a> specification. For that I have extended the REST API  built in the post <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>, with CORS support.
</p>
<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Quick_intro_to_CORS">1. Quick intro to CORS</a><ul>
        <li>
          <a href="#11_Overview">1.1. Overview</a>
        </li>
        <li>
          <a href="#12_CORS_HTTP_headers">1.2. CORS HTTP headers</a><ul>
            <li>
              <a href="#121_ClientBrowser_side_8211_HTTP_request_headers">1.2.1. Client/Browser side &#8211; HTTP request headers</a>
            </li>
            <li>
              <a href="#122_Server_side_8211_HTTP_response_headers">1.2.2. Server side &#8211; HTTP response headers</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Adding_HTTP_headers_to_resources_with_Jersey">2. Adding HTTP headers to resources with Jersey</a><ul>
        <li>
          <a href="#21_Response_and_ResponseBuilder">2.1. Response and ResponseBuilder</a>
        </li>
        <li>
          <a href="#22_ContainerResponseFilter">2.2.  ContainerResponseFilter</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Resources">3. Resources</a><ul>
        <li>
          <a href="#31_Source_code_8211_GitHub">3.1. Source code &#8211; GitHub</a>
        </li>
        <li>
          <a href="#32_Web">3.2. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  But before we delve into that, I would like to start with a quick introduction to CORS as found on various great website like Wikipedia or Mozilla Development Network (MDN), I would myself like to reference later.
</p>

## <span id="1_Quick_intro_to_CORS">1. Quick intro to CORS</span>

Well, according to Wikipedia:

<p style="padding-left: 30px; text-align: justify;">
  <em><strong style="text-align: justify; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;">&#8220;Cross-origin resource sharing (CORS)</strong><span style="text-align: justify; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;"> is a mechanism that allows JavaScript on a web page to make XMLHttpRequests to another domain, not the domain the JavaScript originated from. Such &#8220;cross-domain&#8221; requests would otherwise be forbidden by web browsers, per the </span><a style="text-align: justify; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;" title="http://en.wikipedia.org/wiki/Same-origin_policy" href="http://en.wikipedia.org/wiki/Same-origin_policy" target="_blank">same origin security policy</a><span style="text-align: justify; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;">. CORS defines a way in which the browser and the server can interact to determine whether or not to allow the cross-origin request. It is more useful than only allowing same-origin requests, but it is more secure than simply allowing all such cross-origin requests.&#8221; [1]</span></em>
</p>

<h3 style="text-align: justify;">
  <span id="11_Overview"><span style="font-family: Bitter, Georgia, serif; font-size: 30px; line-height: 1.3;">1.1. Overview</span></span>
</h3>

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;The Cross-Origin Resource Sharing standard works by adding new HTTP headers that allow servers to describe the set of origins that are permitted to read that information using a web browser.  Additionally, for HTTP request methods that can cause side-effects on user data (in particular, for HTTP methods other than <code>GET</code>, or for <code>POST</code> usage with certain MIME types), the specification mandates that browsers &#8220;preflight&#8221; the request, soliciting supported methods from the server with an HTTP <code>OPTIONS</code> request method, and then, upon &#8220;approval&#8221; from the server, sending the actual request with the actual HTTP request method.  Servers can also notify clients whether &#8220;credentials&#8221; (including Cookies and HTTP Authentication data) should be sent with requests.&#8221; [2]</em>
</p>

<p class="alert_note" style="text-align: justify;">
  <span style="line-height: 1.5;">The OPTIONS method represents a request for information about the communication options available on the request/response chain identified by the Request-URI. This method allows the client to determine the options and/or requirements associated with a resource, or the capabilities of a server, without implying a resource action or initiating a resource retrieval.[7]</span>
</p>

<h3 style="text-align: justify;">
  <span id="12_CORS_HTTP_headers"><span style="font-family: Bitter, Georgia, serif; font-size: 22px; line-height: 1.3;">1.2. CORS HTTP headers</span></span>
</h3>

The HTTP headers as defined by CORS are:

#### <span id="121_ClientBrowser_side_8211_HTTP_request_headers">1.2.1. Client/Browser side &#8211; HTTP request headers</span>

These are headers that clients may use when issuing HTTP requests in order to make use of the cross-sharing feature:

<li style="text-align: justify;">
  <strong>Origin:</strong> URI indicating the server from which the request initiated.  It does not include any path information, but only the server name.
</li>
<li style="text-align: justify;">
  <strong>Access-Control-Request-Headers:</strong>  used when issuing a preflight request to let the server know what HTTP headers will be used when the actual request is made.
</li>
<li style="text-align: justify;">
  <strong>Access-Control-Request-Method:</strong> used when issuing a preflight request to let the server know what HTTP method will be used when the actual request is made.
</li>

#### <span id="122_Server_side_8211_HTTP_response_headers">1.2.2. Server side &#8211; HTTP response headers</span>

<p style="text-align: justify;">
  These are the HTTP headers that the server sends back for access control requests as defined by the Cross-Origin Resource Sharing specification:
</p>

<li style="text-align: justify;">
  <strong>Access-Control-Allow-Origin:</strong> specifies the authorized domains to make cross-domain request (you should include the domains of your REST clients or &#8220;*&#8221; if you want the resource public and available to everyone &#8211; the latter is not an option if credentials are allowed during CORS requests)
</li>
  * **Access-Control-Expose-Headers:** lets a server white list headers that browsers are allowed to access
  * **Access-Control-Max-Age:**  indicates how long the results of a preflight request can be cached.
  * **Access-Control-Allow-Credentials:** indicates if the server allows credentials during CORS requests
  * **Access-Control-Allow-Methods:** indicates the methods allowed when accessing the resource
  * **Access-Control-Allow-Headers:**  used in response to a preflight request to indicate which HTTP headers can be used when making the actual request.

<h2 style="text-align: justify;">
  <span id="2_Adding_HTTP_headers_to_resources_with_Jersey">2. Adding HTTP headers to resources with Jersey</span>
</h2>

<p style="text-align: justify;">
  In this post, because we are concerned with the server side of things, I will only use the <em>Access-Control-Allow-Origin</em>, <em>Access-Control-Allow-Methods</em> and <em>Access-Control-Allow-Headers</em> response headers.
</p>

<p style="text-align: justify;">
  There are two ways to add headers to a response with Jersey:
</p>

<h3 style="text-align: justify;">
  <span id="21_Response_and_ResponseBuilder">2.1. Response and ResponseBuilder</span>
</h3>

The first one is by using the `header` method of the `javax.ws.rs.core.Response`. This method adds an arbitrary header to the `ResponseBuilder` (_`ResponseBuilder` is a class used to build `Response` instances that contain metadata instead of or in addition to an entity_):

<pre>
<code class="java">@GET
@Path("{id}")
@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
public Response getPodcastById(@PathParam("id") Long id, @QueryParam("detailed") boolean detailed)
		throws IOException,	AppException {
	Podcast podcastById = podcastService.getPodcastById(id);
	return Response.ok() //200
			.entity(podcastById, detailed ? new Annotation[]{PodcastDetailedView.Factory.get()} : new Annotation[0])
			.header("Access-Control-Allow-Origin", "*")
			.header("Access-Control-Allow-Methods", "GET, POST, DELETE, PUT")
			.allow("OPTIONS").build();
}</code>
</pre>

<p style="text-align: justify;">
  As you can see on lines 9 and 10, the <code>header</code> method extends the existing <code>ResponseBuilder</code> and adds the HTTP headers <code>Access-Control-Allow-Origin</code>  and <code>Access-Control-Allow-Methods</code>, suggesting that GET requests coming from any domain will be allowed for the resource identified by this method &#8211; <code>/podcasts/{id}</code>
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> The <code>javax.ws.rs.core.Response</code> has also a <code>ResponseBuilder</code> <code>allow(String... methods)</code> method, that you could also use to set the list of allowed methods for the resource.
</p>

### <span id="22_ContainerResponseFilter"><span style="font-family: Bitter, Georgia, serif; font-size: 22px; line-height: 1.3; text-align: justify;">2.2.  ContainerResponseFilter</span></span>

<p style="text-align: justify;">
  Another way to add the headers to the response is by using <em>Jersey filters</em>, which can modify inbound and outbound requests and responses including modification of headers, entity and other request/response parameters.
</p>

<p style="text-align: justify;">
  I think this is the better way to do it, especially if you want to expose the same HTTP headers in the response, for all the resources of the API &#8211; this is a sort of a cross-cutting concern capability powered by Jersey filters.
</p>

<p style="text-align: justify;">
  Ok, so let&#8217;s see how this works:
</p>

<pre>
<code class="java">package org.codingpedia.demo.rest.util;

import java.io.IOException;

import javax.ws.rs.container.ContainerRequestContext;
import javax.ws.rs.container.ContainerResponseContext;
import javax.ws.rs.container.ContainerResponseFilter;
import javax.ws.rs.core.MultivaluedMap;

public class CORSResponseFilter
implements ContainerResponseFilter {

	public void filter(ContainerRequestContext requestContext, ContainerResponseContext responseContext)
			throws IOException {

		MultivaluedMap&lt;String, Object&gt; headers = responseContext.getHeaders();

		headers.add("Access-Control-Allow-Origin", "*");
		//headers.add("Access-Control-Allow-Origin", "http://podcastpedia.org"); //allows CORS requests only coming from podcastpedia.org		
		headers.add("Access-Control-Allow-Methods", "GET, POST, DELETE, PUT");			
		headers.add("Access-Control-Allow-Headers", "X-Requested-With, Content-Type, X-Codingpedia");
	}

}</code>
</pre>

In the example above the `CORSResponseFilter` always  adds

<li style="text-align: justify;">
  <code>Access-Control-Allow-Origin</code> header to the response(line 18). The  &#8220;*&#8221; means the request can come from any domain &#8211; this is the way to set this header if you want to make your REST API public where everyone can access it
</li>
<li style="text-align: justify;">
  <code>Access-Control-Allow-Methods</code> header to the response (line 20), which indicates that <code>GET</code>, <code>POST</code>, <code>DELETE</code>, <code>PUT</code> methods are allowed when accessing the resource
</li>
<li style="text-align: justify;">
  <code>Access-Control-Allow-Headers</code> header to the response (line 21), which indicates that the  <code>X-Requested-With</code>, <code>Content-Type</code>, <code>X-Codingpedia</code> headers can be used when making the actual request.
</li>

<p style="text-align: justify;">
  The filter must inherit from the <code>ContainerResponseFilter</code> interface and must be registered as a provider:
</p>

<pre>
<code class="java">/**
* Register JAX-RS application components.
*/
public RestDemoJaxRsApplication(){
   register(CORSResponseFilter.class);
   //other registrations omitted for brevity
}</code>
</pre>

Well, that&#8217;s it &#8211; you&#8217;ve learned how easy it is to add CORS support to the server side with Jersey.

<p class="alert_note_no_header">
  Thanks for sharing and connecting with us.
</p>

## <span id="3_Resources">3. Resources</span>

<h3 title="https://github.com/adrianmatei-me/demo-restWS-spring-jersey-tomcat-mybatis">
  <span id="31_Source_code_8211_GitHub">3.1. Source code &#8211; GitHub</span>
</h3>

<ul style="box-sizing: border-box; margin: 16px 0px; padding: 0px 0px 0px 40px; list-style-type: square; color: #141412; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; line-height: 19.200000762939453px; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffffff;">
  <li style="box-sizing: border-box;">
    <a style="box-sizing: border-box; color: #bc360a; text-decoration: none;" title="https://github.com/Codingpedia/demo-rest-jersey-spring" href="https://github.com/Codingpedia/demo-rest-jersey-spring" target="_blank">GitHub – Codingpedia/demo-rest-jersey-spring<span class="Apple-converted-space"> </span></a>(instructions on how to install and run the project)
  </li>
</ul>

### <span id="32_Web">3.2. Web</span>

  1. Wikipedia &#8211; <a title="http://en.wikipedia.org/wiki/Cross-origin_resource_sharing" href="http://en.wikipedia.org/wiki/Cross-origin_resource_sharing" target="_blank">Cross-origin resource sharing</a>
  2. MDN &#8211; <a title="https://developer.mozilla.org/en-US/docs/HTTP/Access_control_CORS" href="https://developer.mozilla.org/en-US/docs/HTTP/Access_control_CORS" target="_blank">HTTP access control (CORS)</a>
  3. W3C Recommendation &#8211; <a title="http://www.w3.org/TR/cors/" href="http://www.w3.org/TR/cors/" target="_blank">Cross-Origin Resource Sharing</a>
  4. <a title="http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/" href="http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/" target="_blank">Cross-domain Ajax with Cross-Origin Resource Sharing</a>
  5. MDN &#8211; <a title="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest" href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest" target="_blank">XMLHttpRequest</a>
  6. <a title="http://stackoverflow.com/questions/17478731/whats-the-point-of-x-requested-with-header" href="http://stackoverflow.com/questions/17478731/whats-the-point-of-x-requested-with-header" target="_blank">What&#8217;s the point of X-Requested-With header?</a>
  7. <a title="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html" target="_blank">HTTP 1.1 &#8211; RFC2616</a>

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
