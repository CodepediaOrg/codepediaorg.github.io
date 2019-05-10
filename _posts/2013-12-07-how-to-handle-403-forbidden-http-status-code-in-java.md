---
id: 1020
title: How to handle 403 Forbidden HTTP status code in Java
date: 2013-12-07T13:58:07+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1020
permalink: /ama/how-to-handle-403-forbidden-http-status-code-in-java/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 4
dsq_thread_id:
  - 2032757576
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - troubleshooting
tags:
  - bug
  - error
  - http
---
## The challenge &#8211; <code style="font-size: 0.8em;">java.io.IOException: Server returned HTTP response code: 403 for URL</code>

Remember my post <a title="Reading/Parsing RSS and Atom feeds in Java with Rome" href="http://www.codingpedia.org/ama/reading-parsing-rss-and-atom-feeds-with-rome/#more-585" target="_blank">Reading/Parsing RSS and Atom feeds in Java with Rome</a>, remember how I build `SyndFeed` objects out of a feed&#8217;s URL:

<pre class="lang:java decode:true" title="Build SyndFeed from URL">public SyndFeed getSyndFeedForUrl(String url) throws MalformedURLException, IOException, IllegalArgumentException, FeedException {

	SyndFeed feed = null;
	InputStream is = null;

	try {

		URLConnection openConnection = new URL(url).openConnection();
		is = new URL(url).openConnection().getInputStream();
		if("gzip".equals(openConnection.getContentEncoding())){
			is = new GZIPInputStream(is);
		}
		InputSource source = new InputSource(is);
		SyndFeedInput input = new SyndFeedInput();
		feed = input.build(source);

	} catch (Exception e){
		LOG.error("Exception occured when building the feed object out of the url", e);
	} finally {
		if( is != null)	is.close();
	}

	return feed;
}</pre>

<!--more-->It works pretty well, but one bug managed to mangle through &#8211;

`java.io.IOException: Server returned HTTP response code: 403 for URL http://happysomeone.com/feed/podcast`:

<pre class="lang:java decode:true" title="Http 403 Error stack trace">java.io.IOException: Server returned HTTP response code: 403 for URL: http://happysomeone.com/feed/podcast
	at sun.net.www.protocol.http.HttpURLConnection.getInputStream(HttpURLConnection.java:1459)
	at org.podcastpedia.admin.service.utils.impl.UtilsImpl.getSyndFeedForUrl(UtilsImpl.java:529)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.getSyndFeedForUpdate(UpdateServiceImpl.java:472)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.getNewEpisodes(UpdateServiceImpl.java:389)
	at org.podcastpedia.admin.service.impl.UpdateServiceImpl.updatePodcastById(UpdateServiceImpl.java:221)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:317)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:198)
	at sun.proxy.$Proxy23.updatePodcastById(Unknown Source)
	at org.podcastpedia.admin.controllers.UpdateController.updatePodcastsByIds(UpdateController.java:164)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.springframework.web.method.support.InvocableHandlerMethod.invoke(InvocableHandlerMethod.java:219)
	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:132)
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:104)
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandleMethod(RequestMappingHandlerAdapter.java:745)
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:686)
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:80)
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:925)
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:856)
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:936)
	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:838)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:647)
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:812)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:728)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:305)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:210)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:330)
	at org.springframework.security.web.access.intercept.FilterSecurityInterceptor.invoke(FilterSecurityInterceptor.java:118)
	at org.springframework.security.web.access.intercept.FilterSecurityInterceptor.doFilter(FilterSecurityInterceptor.java:84)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.access.ExceptionTranslationFilter.doFilter(ExceptionTranslationFilter.java:113)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.session.SessionManagementFilter.doFilter(SessionManagementFilter.java:103)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.authentication.AnonymousAuthenticationFilter.doFilter(AnonymousAuthenticationFilter.java:113)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.authentication.rememberme.RememberMeAuthenticationFilter.doFilter(RememberMeAuthenticationFilter.java:146)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter.doFilter(SecurityContextHolderAwareRequestFilter.java:54)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.savedrequest.RequestCacheAwareFilter.doFilter(RequestCacheAwareFilter.java:45)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.authentication.www.BasicAuthenticationFilter.doFilter(BasicAuthenticationFilter.java:150)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter.doFilter(AbstractAuthenticationProcessingFilter.java:183)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.authentication.logout.LogoutFilter.doFilter(LogoutFilter.java:105)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.session.ConcurrentSessionFilter.doFilter(ConcurrentSessionFilter.java:125)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.context.SecurityContextPersistenceFilter.doFilter(SecurityContextPersistenceFilter.java:87)
	at org.springframework.security.web.FilterChainProxy$VirtualFilterChain.doFilter(FilterChainProxy.java:342)
	at org.springframework.security.web.FilterChainProxy.doFilterInternal(FilterChainProxy.java:192)
	at org.springframework.security.web.FilterChainProxy.doFilter(FilterChainProxy.java:160)
	at org.springframework.web.filter.DelegatingFilterProxy.invokeDelegate(DelegatingFilterProxy.java:346)
	at org.springframework.web.filter.DelegatingFilterProxy.doFilter(DelegatingFilterProxy.java:259)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:243)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:210)
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:88)
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:243)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:210)
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:222)
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:123)
	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:502)
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:171)
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:100)
	at org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:953)
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:118)
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:408)
	at org.apache.coyote.http11.AbstractHttp11Processor.process(AbstractHttp11Processor.java:1041)
	at org.apache.coyote.AbstractProtocol$AbstractConnectionHandler.process(AbstractProtocol.java:603)
	at org.apache.tomcat.util.net.JIoEndpoint$SocketProcessor.run(JIoEndpoint.java:312)
	at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
	at java.lang.Thread.run(Thread.java:662)</pre>

<p style="text-align: justify;">
  This tells us that the <a title="Web server" href="http://en.wikipedia.org/wiki/Web_server">web server</a> may return a <b>403 Forbidden</b> <a title="List of HTTP status codes" href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes">HTTP status code</a> in response to the request I made and indicates that the server can be reached and understood the request, but refuses to take any further action. Status code 403 responses are the result of the web server being configured to deny access, for some reason, to the requested resource by the client.
</p>

## The solution

<p style="text-align: justify;">
  At the same, if I access the feed url via the web browser, I don&#8217;t get any error messages and the feed data is displayed properly. Hmm, so what could the problem be? Let&#8217;s see what the GET request looks like via web browser:
</p>

<pre class="lang:default mark:3 decode:true" title="HTTP request via browser">GET /feed/podcast/ HTTP/1.1
Host: happysomeone.com
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:25.0) Gecko/20100101 Firefox/25.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-gb,en;q=0.5
Accept-Encoding: gzip, deflate
Cookie: _ga=GA1.2.989262483.1379652403; __qca=P0-1317688601-1379652402575
Connection: keep-alive</pre>

<p class="note_normal">
  <strong>Note:</strong> I am using the firefox plugin <a title="Live HTTP Headers add-on" href="https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/" target="_blank">Live HTTP Headers</a> to see this &#8211; very useful tool by debugging.
</p>

<p style="text-align: justify;">
  This led me to ask myself how can I make my request from Java more &#8220;browsery-like&#8221;. The first thing that comes to mind is trying to add an <code>user agent</code> to the request just like the one in line 3 from the web browser request. In Java this is possible by adding a request property to the <code>URL connection</code> object. The modified code looks like this:
</p>

<pre class="lang:java mark:9 decode:true" title="Build SyndFeed from URL modified">public SyndFeed getSyndFeedForUrl(String url) throws MalformedURLException, IOException, IllegalArgumentException, FeedException {

	SyndFeed feed = null;
	InputStream is = null;

	try {

		URLConnection openConnection = new URL(url).openConnection();
		openConnection.addRequestProperty("User-Agent", "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:25.0) Gecko/20100101 Firefox/25.0");
		is = openConnection.getInputStream();
		if("gzip".equals(openConnection.getContentEncoding())){
			is = new GZIPInputStream(is);
		}
		InputSource source = new InputSource(is);
		SyndFeedInput input = new SyndFeedInput();
		feed = input.build(source);

	} catch (ParsingFeedException e){
		LOG.error("************* ParsingFeedException *************\n ", e);
		throw e;
	} catch (MalformedByteSequenceException e) {
		LOG.error("************* MalformedByteSequenceException *************\n ", e);
		throw e;
	} catch (IOException e) {
		LOG.error("************* IOException *************\n ", e);
		throw e;
	} finally {
		if( is != null)	is.close();
	}

	return feed;
}</pre>

<p style="text-align: justify;">
  Notice the method <code>addRequestProperty</code> in <code>line 9</code>. This adds a general request property specified by a key-value pair. This method will not overwrite existing values associated with the same key. In our case I set the user agent that Firefox uses.
</p>

Et voilà, this solved the problem. I hope you could learn something from this as I did.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>


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

