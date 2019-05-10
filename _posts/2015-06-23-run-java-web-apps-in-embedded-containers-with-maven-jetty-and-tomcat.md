---
id: 2392
title: Run java web apps in embedded containers with Maven, Jetty and Tomcat
date: 2015-06-23T21:24:12+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2392
permalink: /ama/run-java-web-apps-in-embedded-containers-with-maven-jetty-and-tomcat/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3872856946
fsb_social_facebook:
  - 3
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 7
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - Java EE
tags:
  - jetty
  - maven
  - tomcat
---
<p style="text-align: justify;">
  While developing java web applications is very practical to have quick feedback from a &#8220;real&#8221; environment. In this post I&#8217;ll explore how to run a java web application with Maven in an embedded container be it Jetty or Tomcat.  I&#8217;ll show how I have configured them for the development of <a title="https://github.com/Codingpedia/podcastpedia" href="https://github.com/Codingpedia/podcastpedia" target="_blank">podcastpedia</a> project backing the <a title="Podcastpedia, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> website.
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>

  <!--more-->
</p>

## Prerequisites

<p style="text-align: justify;">
  You should have <a title="http://maven.apache.org/download.cgi" href="http://maven.apache.org/download.cgi" target="_blank">Maven</a> and  at least <a title="http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" href="http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java 7</a> installed. Ideally you could <a title="https://github.com/Codingpedia/podcastpedia" href="https://github.com/Codingpedia/podcastpedia" target="_blank">setup up the podcastpedia</a> project yourself to see it in action.
</p>

## Jetty Maven Plugin

<pre class="lang:default decode:true" title="Plugin Configuration">&lt;!-- http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html --&gt;
&lt;plugin&gt;
	&lt;groupId&gt;org.eclipse.jetty&lt;/groupId&gt;
	&lt;artifactId&gt;jetty-maven-plugin&lt;/artifactId&gt;
	&lt;version&gt;${jetty.version}&lt;/version&gt;
	&lt;configuration&gt;
		&lt;jettyConfig&gt;${project.basedir}/src/main/resources/config/jetty9.xml&lt;/jettyConfig&gt;
		&lt;stopKey&gt;STOP&lt;/stopKey&gt;
		&lt;stopPort&gt;9999&lt;/stopPort&gt;
		&lt;scanIntervalSeconds&gt;5&lt;/scanIntervalSeconds&gt;
		&lt;scanTargets&gt;
			&lt;scanTarget&gt;${project.basedir}/src/main&lt;/scanTarget&gt;
			&lt;scanTarget&gt;${project.basedir}/src/test&lt;/scanTarget&gt;
		&lt;/scanTargets&gt;
		&lt;contextXml&gt;${project.basedir}/src/test/resources/jetty-context.xml&lt;/contextXml&gt;
		&lt;webAppConfig&gt;
			&lt;contextPath&gt;/&lt;/contextPath&gt;
		&lt;/webAppConfig&gt;
	&lt;/configuration&gt;
	&lt;dependencies&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;mysql&lt;/groupId&gt;
			&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
			&lt;version&gt;${mysql.connector.java.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;javax.mail&lt;/groupId&gt;
			&lt;artifactId&gt;mail&lt;/artifactId&gt;
			&lt;version&gt;${java.mail.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tomcat&lt;/groupId&gt;
			&lt;artifactId&gt;tomcat-jdbc&lt;/artifactId&gt;
			&lt;version&gt;${tomcat.jdbc.version}&lt;/version&gt;
		&lt;/dependency&gt;
	&lt;/dependencies&gt;
&lt;/plugin&gt;</pre>

**Notes:**

  * **jettyConfig** points to the Jetty configuration file; see next section for more explanations
  * defined folders (**scanTargets**) where Jetty looks for changes every 5 seconds (**scanInterval**)
  * defined external **dependencies** to connect to database and send email

### Jetty.xml configuration file

<pre class="lang:default decode:true" title="Jetty xml configuration file">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;Configure class="org.eclipse.jetty.webapp.WebAppContext"&gt;
	&lt;New id="pcmdbDS" class="org.eclipse.jetty.plus.jndi.Resource"&gt;
		&lt;Arg&gt;jdbc/pcmDB&lt;/Arg&gt;
		&lt;Arg&gt;
			&lt;New class="com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource"&gt;
				&lt;Set name="Url"&gt;jdbc:mysql://localhost:3307/pcmDB?allowMultiQueries=true
				&lt;/Set&gt;
				&lt;Set name="User"&gt;pcm&lt;/Set&gt;
				&lt;Set name="Password"&gt;pcm_pw&lt;/Set&gt;
			&lt;/New&gt;
		&lt;/Arg&gt;
	&lt;/New&gt;
	&lt;New id="mailSessionId" class="org.eclipse.jetty.plus.jndi.Resource"&gt;
		&lt;Arg&gt;mail/Session&lt;/Arg&gt;
		&lt;Arg&gt;
			&lt;New class="org.eclipse.jetty.jndi.factories.MailSessionReference"&gt;
				&lt;Set name="user"&gt;test-dev@podcastpedia.org&lt;/Set&gt;
				&lt;Set name="password"&gt;test-dev&lt;/Set&gt;
				&lt;Set name="properties"&gt;
					&lt;New class="java.util.Properties"&gt;
						&lt;Put name="mail.host"&gt;mail.podcastpedia.org&lt;/Put&gt;
						&lt;Put name="mail.debug"&gt;true&lt;/Put&gt;
						&lt;Put name="mail.transport.protocol"&gt;smtp&lt;/Put&gt;
						&lt;Put name="mail.smtp.port"&gt;25&lt;/Put&gt;
						&lt;Put name="mail.smtp.auth"&gt;true&lt;/Put&gt;
					&lt;/New&gt;
				&lt;/Set&gt;
			&lt;/New&gt;
		&lt;/Arg&gt;
	&lt;/New&gt;
&lt;/Configure&gt;</pre>

In the <a title="http://www.eclipse.org/jetty/documentation/current/jetty-xml-config.html" href="http://www.eclipse.org/jetty/documentation/current/jetty-xml-config.html" target="_blank">Jetty configuration file (jetty.xml)</a> you have the following configured:

  * The Server class (or subclass if extended) and global options.
  * A ThreadPool (min and max thread).
  * Connectors (ports, timeouts, buffer sizes, protocol).
  * The handler structure (default handlers and/or a contextHandlerCollections).
  * The deployment manager that scans for and deploys webapps and contexts.
  * Login services that provide authentication checking.
  * A request log.

## Apache Tomcat Maven Plugin

<pre class="lang:default decode:true" title="Apache Tomcat Maven Plugin Configuration">&lt;!-- https://tomcat.apache.org/maven-plugin-trunk/index.html --&gt;
&lt;plugin&gt;
	&lt;groupId&gt;org.apache.tomcat.maven&lt;/groupId&gt;
	&lt;artifactId&gt;tomcat7-maven-plugin&lt;/artifactId&gt;
	&lt;version&gt;2.2&lt;/version&gt;
	&lt;configuration&gt;
		&lt;!-- http port --&gt;
		&lt;port&gt;8080&lt;/port&gt;
		&lt;!-- application path always starts with /--&gt;
		&lt;path&gt;/&lt;/path&gt;
		&lt;!-- optional path to a context file --&gt;
		&lt;contextFile&gt;context.xml&lt;/contextFile&gt;
		&lt;!-- optional system propoerties you want to add --&gt;
		&lt;systemProperties&gt;
			&lt;appserver.base&gt;${project.build.directory}/appserver-base&lt;/appserver.base&gt;
			&lt;appserver.home&gt;${project.build.directory}/appserver-home&lt;/appserver.home&gt;
			&lt;derby.system.home&gt;${project.build.directory}/appserver-base/logs&lt;/derby.system.home&gt;
			&lt;java.io.tmpdir&gt;${project.build.directory}&lt;/java.io.tmpdir&gt;
		&lt;/systemProperties&gt;
		&lt;!-- if you want to use test dependencies rather than only runtime --&gt;
		&lt;useTestClasspath&gt;false&lt;/useTestClasspath&gt;
		&lt;!-- optional if you want to add some extra directories into the classloader --&gt;
		&lt;additionalClasspathDirs&gt;
			&lt;additionalClasspathDir&gt;&lt;/additionalClasspathDir&gt;
		&lt;/additionalClasspathDirs&gt;
	&lt;/configuration&gt;
	&lt;!-- For any extra dependencies needed when running embedded Tomcat (not WAR dependencies) add them below --&gt;
	&lt;dependencies&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;mysql&lt;/groupId&gt;
			&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
			&lt;version&gt;${mysql.connector.java.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;javax.mail&lt;/groupId&gt;
			&lt;artifactId&gt;mail&lt;/artifactId&gt;
			&lt;version&gt;${java.mail.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.tomcat&lt;/groupId&gt;
			&lt;artifactId&gt;tomcat-jdbc&lt;/artifactId&gt;
			&lt;version&gt;${tomcat.jdbc.version}&lt;/version&gt;
		&lt;/dependency&gt;
	&lt;/dependencies&gt;
&lt;/plugin&gt;</pre>

**Notes**

  * specify **port** where Tomcat runs
  * specify **contextFile** where Tomcat looks for configuration
  * defined external **dependencies** to connect to database and send email

### Context.xml

<pre class="lang:default decode:true ">&lt;Context&gt;
  &lt;Resource
            name="jdbc/pcmDB"
            auth="Container"
            type="javax.sql.DataSource"
            factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
            initialSize="5"
            maxActive="55"
            maxIdle="21"
            minIdle="13"
            timeBetweenEvictionRunsMillis="34000"
            minEvictableIdleTimeMillis="55000"
            validationQuery="SELECT 1"
            validationInterval="34"
            testOnBorrow="true"
            removeAbandoned="true"
            removeAbandonedTimeout="233"
            username="pcm"
            password="pcm_pw"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://localhost:3307/pcmDB?allowMultiQueries=true"
   /&gt;

	&lt;Resource name="mail/Session" auth="Container"
	            type="javax.mail.Session"
		        username="test-dev@podcastpedia.org"
		        password="test-dev"
	            mail.smtp.host="mail.podcastpedia.org"
	            mail.smtp.port="25"
	            mail.smtp.user="test-dev@podcastpedia.org"
	            mail.transport.protocol="smtp"
	            mail.smtp.auth="true"
	/&gt;
&lt;/Context&gt;</pre>

In context.xml there are defined the database and email resources.

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong><br /> These are simple configurations, but suffice for current development. My advice is to read the corresponding documentation for more advanced options and capabilities.
</p>

* * *

<p style="text-align: justify;">
  There you go&#8230; Java webapps powered by <a title="http://projects.spring.io/spring-framework/" href="http://projects.spring.io/spring-framework/" target="_blank">Spring Framework</a> running light servlet containers posing a true alternative to JAVA EE servers and all the costs that come with them.
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## References

  1. <a title="http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html" href="http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html" target="_blank">Jetty Maven Plugin</a>
  2. <a title="https://tomcat.apache.org/maven-plugin-trunk/index.html" href="https://tomcat.apache.org/maven-plugin-trunk/index.html" target="_blank">Apache Tomcat Maven Plugin</a>

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
