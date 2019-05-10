---
id: 2090
title: How to compress responses in Java REST API with GZip and Jersey
date: 2014-11-16T15:47:59+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2090
permalink: /ama/how-to-compress-responses-in-java-rest-api-with-gzip-and-jersey/
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3230719873
categories:
  - java
  - optimization
tags:
  - compress
  - compression
  - gzip
  - java
  - peformance
  - rest
---
<p style="text-align: justify;">
  There may be cases when your REST api provides responses that are very long, and we all know how important transfer speed and bandwidth still are on mobile devices/networks. I think this is the first performance optimization point one needs to address, when developing REST apis that support mobile apps. Guess what? Because responses are text, we can compress them. And with today&#8217;s power of smartphones and tablets uncompressing them on the client side should not be a big deal&#8230; So in this post I will present how you can SELECTIVELY compress your REST API responses, if you&#8217;ve built it in Java with <a title="https://jersey.java.net/documentation/latest/user-guide.html" href="https://jersey.java.net/documentation/latest/user-guide.html" target="_blank">Jersey</a>, which is  the JAX-RS Reference Implementation (and more)&#8230; 
</p>

<!--more-->

<h2 style="text-align: justify;">
  Short Version (compress every response)
</h2>

<p style="text-align: justify;">
  On the server side, you can easily compress every response with Jersey&#8217;s <code>EncodingFilter</code>, which is a container filter that supports enconding-based content configuration. The filter examines what content encodings are supported by the container  and decides what encoding should be chosen based on the encodings listed in the <code>Accept-Encoding</code> request header and their associated quality values. To use particular content en/decoding, you need to register one or more content encoding providers  that provide specific encoding support; currently there are providers for GZIP and DEFLATE coding.
</p>

<p style="text-align: justify;">
  All I had to do in this case is adding the following line of code to my JaxRs application class:
</p>

<pre><code class="java">public class RestDemoJaxRsApplication extends ResourceConfig {
	/**
	 * Register JAX-RS application components.
	 */
	public RestDemoJaxRsApplication() {

        packages("org.codingpedia.demo.rest");
		register(EntityFilteringFeature.class);
		EncodingFilter.enableFor(this, GZipEncoder.class);		

	}
}</code></pre>

<p style="text-align: justify; padding-left: 30px;">
  <em>Thank you Marek Potociar(<a class="ProfileHeaderCard-screennameLink u-linkComplex js-nav" style="color: #8899a6;" href="https://twitter.com/marek_potociar">@<span class="u-linkComplex-target">marek_potociar</span></a>) for pointing this out. </em>
</p>

<p class="note_normal" style="text-align: justify;">
  The only advantage I see now for my former solution presented below is having the possibility to compress responses more granularly, on a method(resource) level (but why would you do that?). Read on if you also want to learn how to use <code>WriterInterceptors</code>  with annotations.
</p>

<h2 style="text-align: justify;">
  Longer version &#8211; selectively compress responses
</h2>

<h3 style="text-align: justify;">
  1. Jersey filters and interceptors
</h3>

<p style="text-align: justify;">
  Well, thanks to Jersey&#8217;s powerful Filters and Interceptors features, the implementation is fairly easy.  Whereas filters are primarily intended to manipulate request and response parameters like HTTP headers, URIs and/or HTTP methods, interceptors are intended to manipulate entities, via manipulating entity input/output streams.
</p>

<p style="text-align: justify;">
  You&#8217;ve seen the power of filters in my posts
</p>

  * <a title="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" href="http://www.codingpedia.org/ama/how-to-add-cors-support-on-the-server-side-in-java-with-jersey/" target="_blank">How to add CORS support on the server side in Java with Jersey</a>, where I&#8217;ve shown how to CORS-enable a REST API

    **and **
  * <a title="http://www.codingpedia.org/ama/how-to-log-in-spring-with-slf4j-and-logback/" href="http://www.codingpedia.org/ama/how-to-log-in-spring-with-slf4j-and-logback/" target="_blank">How to log in Spring with SLF4J and Logback</a>, where I&#8217;ve shown how to log requests and responses from the REST API

<p style="text-align: justify;">
  , but for compressing will be using a <code>GZip WriterInterceptor</code>. A writer interceptor is used for cases where entity is written to the &#8220;wire&#8221;, which on the server side as in this case, means when writing out a response entity.
</p>

#### 1.1. GZip Writer Interceptor

So let&#8217;s have a look at our GZip Writer Interceptor:

<pre><code class="java">package org.codingpedia.demo.rest.interceptors;

import java.io.IOException;
import java.io.OutputStream;
import java.util.zip.GZIPOutputStream;

import javax.ws.rs.WebApplicationException;
import javax.ws.rs.core.MultivaluedMap;
import javax.ws.rs.ext.WriterInterceptor;
import javax.ws.rs.ext.WriterInterceptorContext;

@Provider
@Compress
public class GZIPWriterInterceptor implements WriterInterceptor {

    @Override
    public void aroundWriteTo(WriterInterceptorContext context)
                    throws IOException, WebApplicationException {

    	MultivaluedMap&lt;String,Object&gt; headers = context.getHeaders();
    	headers.add("Content-Encoding", "gzip");

        final OutputStream outputStream = context.getOutputStream();
        context.setOutputStream(new GZIPOutputStream(outputStream));
        context.proceed();
    }
}</code></pre>

**Note:**

<li style="text-align: justify;">
  it implements the <code>WriterInterceptor</code>,  which is an interface for message body writer interceptors that wrap around calls to <code>javax.ws.rs.ext.MessageBodyWriter.writeTo</code>
</li>
<li style="text-align: justify;">
  providers implementing <code>WriterInterceptor</code> contract must be either programmatically registered in a JAX-RS runtime or must be annotated with @Provider annotation to be automatically discovered by the JAX-RS runtime during a provider scanning phase.
</li>
<li style="text-align: justify;">
  <code>@Compress</code>  is the name binding annotation, which we will discuss more detailed in the coming paragraph
</li>
<li style="text-align: justify;">
  <em>&#8220;The interceptor gets a output stream from the WriterInterceptorContext and sets a new one which is a GZIP wrapper of the original output stream. After all interceptors are executed the output stream lastly set to the WriterInterceptorContext will be used for serialization of the entity. In the example above the entity bytes will be written to the GZIPOutputStream which will compress the stream data and write them to the original output stream. The original stream is always the stream which writes the data to the &#8220;wire&#8221;. When the interceptor is used on the server, the original output stream is the stream into which writes data to the underlying server container stream that sends the response to the client.&#8221;</em> [2]
</li>
<li style="text-align: justify;">
  <em>&#8220;The overridden method aroundWriteTo() gets WriterInterceptorContext as a parameter. This context contains getters and setters for header parameters, request properties, entity, entity stream and other properties.&#8221; </em>[2]; when you compress your response you should set the &#8220;Content-Encoding&#8221; header to &#8220;gzip&#8221;
</li>

#### 1.2. Compress annotation

<p style="text-align: justify;">
  Filters and interceptors can be <span class="emphasis"><em>name-bound</em></span>. Name binding is a concept that allows to say to a JAX-RS runtime that a specific filter or interceptor will be executed only for a specific resource method. When a filter or an interceptor is limited only to a specific resource method we say that it is <span class="emphasis"><em>name-bound</em></span>. Filters and interceptors that do not have such a limitation are called <span class="emphasis"><em>global</em></span>. In our case we&#8217;ve built the @Compress annotation:
</p>

<pre><code class="xml">package org.codingpedia.demo.rest.interceptors;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

import javax.ws.rs.NameBinding;

//@Compress annotation is the name binding annotation
@NameBinding
@Retention(RetentionPolicy.RUNTIME)
public @interface Compress {}</code></pre>

and used it to mark methods on resources which should be gzipped (_e.g. when GET-ing all the podcasts with the `PodcastsResource`_):

<pre><code class="java">@Component
@Path("/podcasts")
public class PodcastsResource {

	@Autowired
	private PodcastService podcastService;

    ...........................

	/*
	 * *********************************** READ ***********************************
	 */
	/**
	 * Returns all resources (podcasts) from the database
	 *
	 * @return
	 * @throws IOException
	 * @throws JsonMappingException
	 * @throws JsonGenerationException
	 * @throws AppException
	 */
	@GET
	@Compress
	@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
	public List&lt;Podcast&gt; getPodcasts(
			@QueryParam("orderByInsertionDate") String orderByInsertionDate,
			@QueryParam("numberDaysToLookBack") Integer numberDaysToLookBack)
			throws IOException,	AppException {
		List&lt;Podcast&gt; podcasts = podcastService.getPodcasts(
				orderByInsertionDate, numberDaysToLookBack);
		return podcasts;
	}

    ...........................
}</code></pre>

### 2. Testing

#### 2.1. SOAPui

<p style="text-align: justify;">
  Well, if you are testing with SOAPui, you can issue the following request against the <code>PodcastsResource</code>
</p>

##### Request:

<pre><code class="http">GET http://localhost:8888/demo-rest-jersey-spring/podcasts/?orderByInsertionDate=DESC HTTP/1.1
Accept-Encoding: gzip,deflate
Accept: application/json, application/xml
Host: localhost:8888
Connection: Keep-Alive
User-Agent: Apache-HttpClient/4.1.1 (java 1.5)</code></pre>

##### Response:

<pre><code class="http">HTTP/1.1 200 OK
Content-Type: application/json
Content-Encoding: gzip
Content-Length: 409
Server: Jetty(9.0.7.v20131107)

[
   {
      "id": 2,
      "title": "Quarks & Co - zum Mitnehmen",
      "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/quarks",
      "feed": "http://podcast.wdr.de/quarks.xml",
      "description": "Quarks & Co: Das Wissenschaftsmagazin",
      "insertionDate": "2014-10-29T10:46:13.00+0100"
   },

   {
      "id": 1,
      "title": "- The Naked Scientists Podcast - Stripping Down Science",
      "linkOnPodcastpedia": "https://github.com/Codingpedia/podcastpedia/podcasts/792/-The-Naked-Scientists-Podcast-Stripping-Down-Science",
      "feed": "feed_placeholder",
      "description": "The Naked Scientists flagship science show brings you a lighthearted look at the latest scientific breakthroughs, interviews with the world top scientists, answers to your science questions and science experiments to try at home.",
      "insertionDate": "2014-10-29T10:46:02.00+0100"
   }
]</code></pre>

<p style="text-align: justify;">
  SOAPui recognizes the <code>Content-Type: gzip</code> header, we&#8217;ve added in the <code>GZIPWriterInterceptor</code> and automatically uncompresses the response and displays it readable to the human eye.
</p>

<p style="text-align: justify;">
  Well, that&#8217;s it. You&#8217;ve learned how Jersey makes it straightforward to compress the REST api responses.
</p>

<p class="note_code" style="text-align: justify;">
  <strong> Tip:</strong> If you want really learn how to design and implement REST API in Java read the following <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
</p>

<p style="text-align: center;">
  <strong>Keep on coding!!!</strong>
</p>

## Resources

  1. <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>
  2. <a title="https://jersey.java.net/documentation/latest/user-guide.html" href="https://jersey.java.net/documentation/latest/user-guide.html" target="_blank">Jersey User Guide </a>
      1.
      2. <a title="https://jersey.java.net/documentation/latest/user-guide.html#filters-and-interceptors" href="https://jersey.java.net/documentation/latest/user-guide.html#filters-and-interceptors" target="_blank">Filters and interceptors</a>
  3. <a title="http://www.gzip.org/" href="http://www.gzip.org/" target="_blank">Gzip.org</a>

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
