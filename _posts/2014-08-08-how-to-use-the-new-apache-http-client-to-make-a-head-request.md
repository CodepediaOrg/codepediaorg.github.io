---
id: 1691
title: How to use the new Apache Http Client to make a HEAD request
date: 2014-08-08T20:47:44+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1691
permalink: /ama/how-to-use-the-new-apache-http-client-to-make-a-head-request/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:50:"https://github.com/podcastpedia/podcastpedia-batch";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 2911650145
fsb_social_facebook:
  - 2
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
tags:
  - apache http client
  - etag
  - http
  - http client
  - http header
  - last-modified
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Use_case_from_Podcastpediaorg">1. Use case from Podcastpedia.org</a>
    </li>
    <li>
      <a href="#2_Migration_to_the_43x_version">2. Migration to the 4.3.x version</a><ul>
        <li>
          <a href="#21_Software_dependencies">2.1. Software dependencies</a><ul>
            <li>
              <a href="#211_Before">2.1.1. Before</a>
            </li>
            <li>
              <a href="#212_After">2.1.2. After</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#22_HEAD_request_with_Apache_Http_Client">2.2. HEAD request with Apache Http Client</a><ul>
            <li>
              <a href="#221_Before_v42x">2.2.1. Before v4.2.x</a>
            </li>
            <li>
              <a href="#222_After_v_43x"> 2.2.2. After v 4.3.x</a>
            </li>
            <li>
              <a href="#223_Make_the_http_call_from_behind_a_proxy">2.2.3. Make the http call from behind a proxy</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a><ul>
        <li>
          <a href="#Source_Code_8211_GitHub">Source Code &#8211; GitHub</a>
        </li>
        <li>
          <a href="#Web">Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  If you&#8217;ve updated your Apache HTTP Client code to use the newest library (at the time of this writing it is version 4.3.5 for the httpclient and version 4.3.2 for httpcore) from the version 4.2.x you&#8217;ll notice that some classes, like <code>org.apache.http.impl.client.DefaultHttpClient</code> or <code>org.apache.http.params.HttpParams</code> have become deprecated. Well, I&#8217;ve been there, so in this post I&#8217;ll present how to get rid of the warnings by using the new classes.<!--more-->
</p>

<h2 style="text-align: justify;">
  <span id="1_Use_case_from_Podcastpediaorg">1. Use case from <a title="Podcastpedia, knowledge to go" href="http://www.codingpedia.org/ama/how-to-add-categories-to-menu-in-wordpress/" target="_blank">Podcastpedia.org</a></span>
</h2>

<p style="text-align: justify;">
  The use case I will use for demonstration is simple: I have a batch job to check if there are new episodes are available for podcasts. To avoid having to get and parse the feed if there are no new episodes, I verify before if the <code>eTag</code> or the <code>last-modified</code> headers of the feed resource have changed since the last call. This will work if the feed publisher supports these headers, which I highly recommend as it spares bandwidth and processing power on the consumers.
</p>

<p style="text-align: justify;">
  So how it works? Initially, when a new podcast is added to the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia">Podcastpedia.org </a>directory I check if the headers are present for the feed resource and if so I store them in the database. To do that, I execute an HTTP HEAD request against the URL of the feed with the help of Apache Http Client. According to the <a title="http://www.ietf.org/rfc/rfc2616.txt" href="http://www.ietf.org/rfc/rfc2616.txt" target="_blank">Hypertext Transfer Protocol &#8212; HTTP/1.1 rfc2616</a>, the meta-information contained in the HTTP headers in response to a HEAD request SHOULD be identical to the information sent in response to a GET request).
</p>

<p style="text-align: justify;">
  In the following sections I will present how the code actually looks in the Java, before and after the upgrade to the 4.3.x version of the Apache Http Client.
</p>

<h2 style="text-align: justify;">
  <span id="2_Migration_to_the_43x_version">2. Migration to the 4.3.x version</span>
</h2>

### <span id="21_Software_dependencies">2.1. Software dependencies</span>

<p style="text-align: justify;">
  To build my project, which by the way is now available on GitHub &#8211; <a title="https://github.com/podcastpedia/podcastpedia-batch" href="http://https://github.com/podcastpedia/podcastpedia-batch" target="_blank">Podcastpedia-batch</a>, I am using maven, so I listed below the dependencies required for the Apache Http Client:
</p>

#### <span id="211_Before">2.1.1. Before</span>

<pre class="lang:default mark:5,10 decode:true" title="Apache Http Client dependencies 4.2.x">&lt;!-- Apache Http client --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
	&lt;artifactId&gt;httpclient&lt;/artifactId&gt;
	&lt;version&gt;4.2.5&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
	&lt;artifactId&gt;httpcore&lt;/artifactId&gt;
	&lt;version&gt;4.2.4&lt;/version&gt;
&lt;/dependency&gt;</pre>

#### <span id="212_After">2.1.2. After</span>

<pre class="lang:default mark:5,10 decode:true" title="Apache Http Client dependencies">&lt;!-- Apache Http client --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
	&lt;artifactId&gt;httpclient&lt;/artifactId&gt;
	&lt;version&gt;4.3.5&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
	&lt;artifactId&gt;httpcore&lt;/artifactId&gt;
	&lt;version&gt;4.3.2&lt;/version&gt;
&lt;/dependency&gt;</pre>

### <span id="22_HEAD_request_with_Apache_Http_Client">2.2. HEAD request with Apache Http Client</span>

#### <span id="221_Before_v42x">2.2.1. Before v4.2.x</span>

<pre class="lang:java decode:true" title="Example of executing a HEAD request with Apache HttpClient">private void setHeaderFieldAttributes(Podcast podcast) throws ClientProtocolException, IOException, DateParseException{

	HttpHead headMethod = null;
	headMethod = new HttpHead(podcast.getUrl());

	org.apache.http.client.HttpClient httpClient = new DefaultHttpClient(poolingClientConnectionManager);

	HttpParams params = httpClient.getParams();
	org.apache.http.params.HttpConnectionParams.setConnectionTimeout(params, 10000);
	org.apache.http.params.HttpConnectionParams.setSoTimeout(params, 10000);
	HttpResponse httpResponse = httpClient.execute(headMethod);
	int statusCode = httpResponse.getStatusLine().getStatusCode();

	if (statusCode != HttpStatus.SC_OK) {
		LOG.error("The introduced URL is not valid " + podcast.getUrl()  + " : " + statusCode);
	}

	//set the new etag if existent
	org.apache.http.Header eTagHeader = httpResponse.getLastHeader("etag");
	if(eTagHeader != null){
		podcast.setEtagHeaderField(eTagHeader.getValue());
	}

	//set the new "last modified" header field if existent
	org.apache.http.Header lastModifiedHeader= httpResponse.getLastHeader("last-modified");
	if(lastModifiedHeader != null) {
		podcast.setLastModifiedHeaderField(DateUtil.parseDate(lastModifiedHeader.getValue()));
		podcast.setLastModifiedHeaderFieldStr(lastModifiedHeader.getValue());
	}

	// Release the connection.
	headMethod.releaseConnection();
}</pre>

<p style="text-align: justify;">
  If you are using a smart IDE, it will tell you that <code>DefaultHttpClient</code>, <code>HttpParams</code> and <code>HttpConnectionParams</code> are deprecated. If you look now in their java docs, you&#8217;ll get a suggestion for their replacement, namely to use the <code>HttpClientBuilder</code> and classes provided by <code>org.apache.http.config </code>instead.
</p>

<p style="text-align: justify;">
  So, as you&#8217;ll see in the coming section, that&#8217;s exactly what I did.
</p>

#### <span id="222_After_v_43x"> 2.2.2. After v 4.3.x</span>

<pre class="lang:default mark:11-15 decode:true" title="HEAD request example with Apache Http Client v 4.3.x">private void setHeaderFieldAttributes(Podcast podcast) throws ClientProtocolException, IOException, DateParseException{

	HttpHead headMethod = null;
	headMethod = new HttpHead(podcast.getUrl());

	RequestConfig requestConfig = RequestConfig.custom()
			.setSocketTimeout(TIMEOUT * 1000)
			.setConnectTimeout(TIMEOUT * 1000)
			.build();

	CloseableHttpClient httpClient = HttpClientBuilder
								.create()
								.setDefaultRequestConfig(requestConfig)
								.setConnectionManager(poolingHttpClientConnectionManager)
								.build();

	HttpResponse httpResponse = httpClient.execute(headMethod);
	int statusCode = httpResponse.getStatusLine().getStatusCode();

	if (statusCode != HttpStatus.SC_OK) {
		LOG.error("The introduced URL is not valid " + podcast.getUrl()  + " : " + statusCode);
	}

	//set the new etag if existent
	Header eTagHeader = httpResponse.getLastHeader("etag");
	if(eTagHeader != null){
		podcast.setEtagHeaderField(eTagHeader.getValue());
	}

	//set the new "last modified" header field if existent
	Header lastModifiedHeader= httpResponse.getLastHeader("last-modified");
	if(lastModifiedHeader != null) {
		podcast.setLastModifiedHeaderField(DateUtil.parseDate(lastModifiedHeader.getValue()));
		podcast.setLastModifiedHeaderFieldStr(lastModifiedHeader.getValue());
	}

	// Release the connection.
	headMethod.releaseConnection();
}</pre>

Notice

<li style="text-align: justify;">
  how the <code>HttpClientBuilder</code> has been used to build a <code>ClosableHttpClient</code> [lines 11-15], which is a base implementation of <code>HttpClient</code> that also implements <code>Closeable</code>
</li>
<li style="text-align: justify;">
  the <code>HttpParams</code> from the previous version have been replaced by <code>org.apache.http.client.config.RequestConfig</code> [lines 6-9] where I can set the socket and connection timeouts. This configuration is later used (line 13) when building the <code>HttpClient</code>
</li>

The remaining of the code is quite simple:

  * the HEAD request is executed (line 17)
  * if existant, the `eTag` and `last-modified` headers are persisted.
  * in the end the internal state of the request is reset, making it reusable &#8211; `headMethod.releaseConnection()`

#### <span id="223_Make_the_http_call_from_behind_a_proxy">2.2.3. Make the http call from behind a proxy</span>

If you are behind a proxy you can easily configure the HTTP call by setting a `org.apache.http.HttpHost` proxy host on the `RequestConfig`:

<pre class="lang:java decode:true" title="HTTP call behind a proxy ">HttpHost proxy = new HttpHost("xx.xx.xx.xx", 8080, "http");
RequestConfig requestConfig = RequestConfig.custom()
		.setSocketTimeout(TIMEOUT * 1000)
		.setConnectTimeout(TIMEOUT * 1000)
		.setProxy(proxy)
		.build();</pre>

## <span id="Resources">Resources</span>

### <span id="Source_Code_8211_GitHub">Source Code &#8211; GitHub</span>

<li style="text-align: justify;">
  <a title="https://github.com/podcastpedia/podcastpedia-batch" href="https://github.com/podcastpedia/podcastpedia-batch" target="_blank">podcastpedia-batch </a>&#8211; the job for adding new podcasts from a file to the podcast directory, uses the code presented in the post to persist the eTag and lastModified headers; it is still work in progress. Please make a pull request if you have any improvement proposals
</li>

### <span id="Web">Web</span>

  * <a title="http://www.ietf.org/rfc/rfc2616.txt" href="http://www.ietf.org/rfc/rfc2616.txthttp://" target="_blank"> Hypertext Transfer Protocol &#8212; HTTP/1.1</a>
  * Maven Repository
      * <a title="http://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient" href="http://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient" target="_blank">HttpComponents Client </a>
      * <a title="http://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore" href="http://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore" target="_blank">HttpComponents Core (blocking I/O)</a>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/wp-content/uploads/2014/01/AdrianMatei.png" alt="Podcastpedia image" />

  <div id="author_details" style="text-align: justify;">
    <a title="Codingpedia.org, let's code the better world" href="http://www.codingpedia.org/" target="_blank">Codingpedia.org</a> was founded by Adrian Matei (ama [AT] codingpedia DOT org), a computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy.
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-googleplus" href="https://plus.google.com/+AdrianMatei" target="_blank"> </a><a class="icon-twitter" href="https://twitter.com/adrianimatei" target="_blank"> </a><a class="icon-linkedin" href="http://www.linkedin.com/in/adrianmatei1983" target="_blank"> </a><a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
