---
id: 983
title: Start two or more Tomcat instances on localhost machine
date: 2013-11-27T21:16:29+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=983
permalink: /ama/start-two-or-more-tomcat-instances-on-localhost-machine/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
dsq_thread_id:
  - 2005344231
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - development
  - tomcat
  - troubleshooting
tags:
  - debug
  - deployment
  - instance
  - tomcat
format: aside
---
<p style="text-align: justify;">
  There are to main applications, that power <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, one that is actually the web application and another one where I do administrative stuff and podcast updates. Experience has shown that if I want to run the podcasts updates in the background and do some testing/debugging/redployment on the front-end application it&#8217;s better to have them running on different Tomcat instances on the development machine.<!--more-->
</p>

**How do I do that?** It&#8217;s very easy. First make a copy of one instance and name it as you please:

<div id="attachment_984" style="width: 721px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/clone-tomcat-directory.png"><img class="size-full wp-image-984" src="{{site.url}}/wp-content/uploads/2013/11/clone-tomcat-directory.png" alt="Clone Tomcat directory" width="711" height="192" srcset="{{site.url}}/wp-content/uploads/2013/11/clone-tomcat-directory.png 711w, {{site.url}}/wp-content/uploads/2013/11/clone-tomcat-directory-300x81.png 300w" sizes="(max-width: 711px) 100vw, 711px" /></a>

  <p class="wp-caption-text">
    Clone Tomcat directory
  </p>
</div>

After that go to the `conf/server.xml` file and modify the `port`s from the `Connector` entries:

original

<pre class="lang:default mark:2,4,7 decode:true" title="Original port configuration">.......
&lt;Connector port="8080" protocol="HTTP/1.1"
		   connectionTimeout="20000"
		   redirectPort="8443" /&gt;
......
&lt;!-- Define an AJP 1.3 Connector on port 8010 --&gt;
&lt;Connector port="8009" protocol="AJP/1.3" redirectPort="8443" /&gt;
....</pre>

modified

<pre class="lang:default mark:2,4,7 decode:true" title="Modified port numbers in server.xml ">.......
&lt;Connector port="8081" protocol="HTTP/1.1"
		   connectionTimeout="20000"
		   redirectPort="8444" /&gt;
......
&lt;!-- Define an AJP 1.3 Connector on port 8010 --&gt;
&lt;Connector port="8010" protocol="AJP/1.3" redirectPort="8444" /&gt;
....</pre>

And then start your two instances. Because I still get an `java.net.BindException: Address already in use: JVM_Bind error`:

<pre class="lang:default decode:true" title="Address already in use Error">27.11.2013 06:56:47 org.apache.catalina.core.StandardServer await
SEVERE: StandardServer.await: create[localhost:8005]:
java.net.BindException: Address already in use: JVM_Bind
	at java.net.PlainSocketImpl.socketBind(Native Method)
	at java.net.PlainSocketImpl.bind(PlainSocketImpl.java:383)
	at java.net.ServerSocket.bind(ServerSocket.java:328)
	at java.net.ServerSocket.&lt;init&gt;(ServerSocket.java:194)
	at org.apache.catalina.core.StandardServer.await(StandardServer.java:427)
	at org.apache.catalina.startup.Catalina.await(Catalina.java:779)
	at org.apache.catalina.startup.Catalina.start(Catalina.java:725)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at java.lang.reflect.Method.invoke(Method.java:597)
	at org.apache.catalina.startup.Bootstrap.start(Bootstrap.java:322)
	at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:456)</pre>

it means I have forgot to change the port number from the `Server` entry:

default

<pre class="lang:default decode:true" title="Default Server entry in server.xml">&lt;Server port="8005" shutdown="SHUTDOWN"&gt;</pre>

modified

<pre class="lang:default decode:true" title="Modified Server entry in server.xml">&lt;Server port="8006" shutdown="SHUTDOWN"&gt;</pre>

<p style="text-align: justify;">
  That&#8217;s it. You should now be able to run the two instances of Tomcat 7 with no trouble. If you want to start a third one and so one, follow the same procedure and use different port numbers.
</p>

<p class="note_normal">
  Don&#8217;t forget to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you might find it really interesting. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

<p class="note_normal">
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
</p>
