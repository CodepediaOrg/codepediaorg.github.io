---
id: 1244
title: How to log in Spring with SLF4J and Logback
date: 2014-03-21T10:35:16+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1244
permalink: /ama/how-to-log-in-spring-with-slf4j-and-logback/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 4
fsb_social_google:
  - 13
fsb_social_linkedin:
  - 5
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2475652254
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/Codingpedia/demo-rest-jersey-spring";s:11:"ribbon-type";i:10;}'
categories:
  - java
  - spring
tags:
  - log
  - logback
  - logging
  - slf4j
---

<p style="text-align: justify;">
  <span style="text-align: justify; font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;">In this post I will present how to log in a Spring based application with the help of Simple Logging Facade for Java (SLF4J) and Logback. For demonstration I will use the application presented in the post <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a></span>, to which I will add now logging capabilities.
</p>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Introduction">1. Introduction</a><ul>
        <li>
          <a href="#11_Simple_Logging_Facade_for_JavaSL4J">1.1. Simple Logging Facade for Java(SL4J)</a>
        </li>
        <li>
          <a href="#12_Logback">1.2. Logback</a>
        </li>
        <li>
          <a href="#13_Spring">1.3.  Spring</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Configuration">2. Configuration</a><ul>
        <li>
          <a href="#21_Spring">2.1. Spring</a><ul>
            <li>
              <a href="#211_Exclude_commons-logging">2.1.1. Exclude commons-logging</a>
            </li>
            <li>
              <a href="#212_Required_libraries">2.1.2. Required libraries</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#22_Logback">2.2. Logback</a><ul>
            <li>
              <a href="#21_logbackxml">2.1. logback.xml</a><ul>
                <li>
                  <a href="#211_ConsoleAppender">2.1.1. ConsoleAppender</a>
                </li>
                <li>
                  <a href="#212_RollingFileAppender">2.1.2. RollingFileAppender</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Integration_in_code">3. Integration in code</a>
    </li>
    <li>
      <a href="#4_Resources">4. Resources</a><ul>
        <li>
          <a href="#41Source_code">4.1. Source code</a>
        </li>
        <li>
          <a href="#41_Codingpedia">4.1. Codingpedia</a>
        </li>
        <li>
          <a href="#43_Web">4.3. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="1_Introduction">1. Introduction</span>

### <span id="11_Simple_Logging_Facade_for_JavaSL4J">1.1. Simple Logging Facade for Java(SL4J)</span>

<p style="text-align: justify;">
  The  SLF4J serves as a simple facade or abstraction for various logging frameworks, such as java.util.logging, logback and log4j. SLF4J allows the end-user to plug in the desired logging framework at <em>deployment</em> time. Note that SLF4J-enabling your library/application implies the addition of only a single mandatory dependency, namely <em>slf4j-api-x.x.x.jar</em>.
</p>

<h3 style="text-align: justify;">
  <span id="12_Logback">1.2. Logback</span>
</h3>

From Logback&#8217;s official documentation:

_&#8220;Logback is intended as a successor to the popular log4j project, [picking up where log4j leaves off](http://logback.qos.ch/reasonsToSwitch.html)._

<p style="text-align: justify;">
  <em>Logback&#8217;s architecture is sufficiently generic so as to apply under different circumstances. At present time, logback is divided into three modules, logback-core, logback-classic and logback-access.</em>
</p>

<p style="text-align: justify;">
  <em>The logback-core module lays the groundwork for the other two modules. The logback-classic module can be assimilated to a significantly improved version of log4j. Moreover, logback-classic natively implements the <a href="http://www.slf4j.org/">SLF4J API</a> so that you can readily switch back and forth between logback and other logging frameworks such as log4j or java.util.logging (JUL).</em>
</p>

_The logback-access module integrates with Servlet containers, such as Tomcat and Jetty, to provide HTTP-access log functionality. Note that you could easily build your own module on top of logback-core.&#8221;_

### <span id="13_Spring">1.3.  Spring</span>

<p style="text-align: justify;">
  Spring is using by default Jakarta Commons Logging, but in the blog post <a title="http://spring.io/blog/2009/12/04/logging-dependencies-in-spring/" href="http://spring.io/blog/2009/12/04/logging-dependencies-in-spring/" target="_blank">Logging Dependencies in Spring</a> from Spring.io it says &#8220;<em>if we could turn back the clock and start Spring now as a new project it would use a different logging dependency. Probably the first choice would be the Simple Logging Facade for Java (<a href="http://www.slf4j.org/">SLF4J</a>)</em>&#8220;
</p>

## <span id="2_Configuration">2. Configuration</span>

<p style="text-align: justify;">
  To use SLF4J with Spring you need to replace the <code>commons-logging</code> dependency with the <code>SLF4J-JCL bridge</code>. Once you have done that, then logging calls from within Spring will be translated into logging calls to the SLF4J API, so if other libraries in your application use that API, then you have a single place to configure and manage logging.
</p>

### <span id="21_Spring">2.1. Spring</span>

#### <span id="211_Exclude_commons-logging">2.1.1. Exclude <code>commons-logging</code></span>

<p style="text-align: justify;">
  To switch off commons-logging, the commons-logging dependency mustn&#8217;t be present in the classpath at runtime. If you are using maven like me, you can simply exclude it from the spring-context artifact:
</p>


<pre>
  <code class="xml">
    &lt;dependency&gt;
    	&lt;groupId&gt;org.springframework&lt;/groupId&gt;
    	&lt;artifactId&gt;spring-context&lt;/artifactId&gt;
    	&lt;version&gt;${spring.version}&lt;/version&gt;
    	&lt;exclusions&gt;
    	   &lt;exclusion&gt;
    		  &lt;groupId&gt;commons-logging&lt;/groupId&gt;
    		  &lt;artifactId&gt;commons-logging&lt;/artifactId&gt;
    	   &lt;/exclusion&gt;
    	&lt;/exclusions&gt;			
    &lt;/dependency&gt;
  </code>
</pre>

#### <span id="212_Required_libraries"><span style="line-height: 1.5;">2.1.2. Required libraries</span></span>

Because I am using LogBack, which implements SLF4J directly, you will need to depend on two libraries (`jcl-over-slf4j` and `logback`):

<pre>
  <code class="xml">
    &lt;!-- LogBack dependencies --&gt;
    &lt;dependency&gt;
    	&lt;groupId&gt;ch.qos.logback&lt;/groupId&gt;
    	&lt;artifactId&gt;logback-classic&lt;/artifactId&gt;
    	&lt;version&gt;${logback.version}&lt;/version&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;                                    
    	&lt;groupId&gt;org.slf4j&lt;/groupId&gt;                
    	&lt;artifactId&gt;jcl-over-slf4j&lt;/artifactId&gt;     
    	&lt;version&gt;${jcloverslf4j.version}&lt;/version&gt;  
    &lt;/dependency&gt;
  </code>
</pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> You might also want to exclude the <code>slf4j-api</code> depedency from other external dependencies, besides Spring, to avoid having more than one version of that API on the classpath.
</p>

<p class="note_code" style="text-align: justify;">
  <strong>Code tip:</strong><br /> All dependencies required for the projects can be found on GitHub in the  <a title="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/469632984fd9bd19d758e98661f0253694fff104/pom.xml" href="https://github.com/Codingpedia/demo-rest-jersey-spring/blob/469632984fd9bd19d758e98661f0253694fff104/pom.xml" target="_blank">pom.xml </a> file.
</p>

### <span id="22_Logback"><span style="font-family: Bitter, Georgia, serif; font-size: 22px; line-height: 1.3;">2.2. Logback</span></span>

To configure Logback all you have to do is place the file `logback.xml` in the classpath.

#### <span id="21_logbackxml">2.1. logback.xml</span>

<pre>
  <code class="xml">
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;configuration&gt;
    	&lt;appender name="consoleAppender" class="ch.qos.logback.core.ConsoleAppender"&gt;
    		&lt;encoder&gt;
    			&lt;Pattern&gt;.%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg %n
    			&lt;/Pattern&gt;
    		&lt;/encoder&gt;
    		&lt;filter class="ch.qos.logback.classic.filter.ThresholdFilter"&gt;
    			&lt;level&gt;TRACE&lt;/level&gt;
    		&lt;/filter&gt;
    	&lt;/appender&gt;

      	&lt;appender name="dailyRollingFileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
    		&lt;File&gt;c:/tmp/rest-demo.log&lt;/File&gt;
    		&lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
    		    &lt;!-- daily rollover --&gt;
    			&lt;FileNamePattern&gt;rest-demo.%d{yyyy-MM-dd}.log&lt;/FileNamePattern&gt;

    			&lt;!-- keep 30 days' worth of history --&gt;
    			&lt;maxHistory&gt;30&lt;/maxHistory&gt;			
    		&lt;/rollingPolicy&gt;

    		&lt;encoder&gt;
    			&lt;Pattern&gt;%d{HH:mm:ss.SSS} [%thread] %-5level %logger{35} - %msg %n&lt;/Pattern&gt;
    		&lt;/encoder&gt; 	    
      	&lt;/appender&gt;
      	&lt;appender name="minuteRollingFileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender"&gt;
    		&lt;rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy"&gt;
    		    &lt;!-- rollover every minute --&gt;
    			&lt;FileNamePattern&gt;c:/tmp/minutes/rest-demo-minute.%d{yyyy-MM-dd_HH-mm}.log&lt;/FileNamePattern&gt;

    			&lt;!-- keep 30 minutes' worth of history --&gt;
    			&lt;maxHistory&gt;30&lt;/maxHistory&gt;			
    		&lt;/rollingPolicy&gt;

    		&lt;encoder&gt;
    			&lt;Pattern&gt;%-4relative [%thread] %-5level %logger{35} - %msg %n&lt;/Pattern&gt;
    		&lt;/encoder&gt; 	    
      	&lt;/appender&gt;  	

    	&lt;logger name="org.codingpedia" additivity="false"&gt;
    	    &lt;level value="DEBUG" /&gt;
    		&lt;appender-ref ref="dailyRollingFileAppender"/&gt;
    		&lt;appender-ref ref="minuteRollingFileAppender"/&gt;
    		&lt;appender-ref ref="consoleAppender" /&gt;
    	&lt;/logger&gt;

    	&lt;root&gt;
    		&lt;level value="INFO" /&gt;
    		&lt;appender-ref ref="consoleAppender" /&gt;
    	&lt;/root&gt;
    &lt;/configuration&gt;
  </code>
</pre>

<span style="font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;">Logback uses </span><a style="font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;" title="http://logback.qos.ch/manual/appenders.html" href="http://logback.qos.ch/manual/appenders.html" target="_blank">appenders</a><span style="font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;">, which are components Logback delegates the task of writing logging events to. </span>

##### <span id="211_ConsoleAppender">2.1.1. ConsoleAppender</span>

<p style="text-align: justify;">
  As the name says it, this appender logs to the console. It has an encoder property &#8211; <a title="Logback encoders" href="http://logback.qos.ch/manual/encoders.html" target="_blank">encoders</a> are responsible for transforming an incoming event into a byte array <b>and</b> writing out the resulting byte array onto the appropriate <code>OutputStream</code>.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> Until logback version 0.9.19, many appenders relied on the Layout instances to control the format of log output. Since then encoders are the standard &#8211; encoders are assigned the type of <code>ch.qos.logback.classic.encoder.PatternLayoutEncoder</code> by default.
</p>

<p style="text-align: justify;">
  The <code>PatternLayoutEncoder</code> wraps instances of <a title="Logback - layouts" href="http://logback.qos.ch/manual/layouts.html" target="_blank"><code>PatternLayout</code></a> &#8211; this takes a logging event and returns a String, which you can customize by using PatternLayout&#8217;s conversion pattern.
</p>

<p style="text-align: justify;">
  From Logaback&#8217;s documentation:<br /> &#8220;<em>The conversion pattern of <code>PatternLayout</code> is closely related to the conversion pattern of the <code>printf()</code> function in the C programming language. A conversion pattern is composed of literal text and format control expressions calledconversion specifiers. You are free to insert any literal text within the conversion pattern. Each conversion specifier starts with a percent sign &#8216;%&#8217; and is followed by optional format modifiers, a conversion word and optional parameters between braces. The conversion word controls the data field to convert, e.g. logger name, level, date or thread name. The format modifiers control field width, padding, and left or right justification.&#8221;</em>
</p>

You can find below a short explanation of the the conversion specifiers used in the example:

  * `%d{HH:mm:ss.SSS}` &#8211; Used to output the date of the logging event.
  * `%thread`&#8211; outputs the name of the thread that generated the logging event
  * `%-5level` &#8211; means the level of the logging event should be left justified to a width of five characters
  * `%logger{36}`  &#8211; outputs the name of the logger at the origin of the logging event. It takes an integer as parameter. This specifies the length the converter&#8217;s algorithm  will shorten the logger name to.
  * `%msg` &#8211; outputs the application-supplied message associated with the logging event.
  * `%n` &#8211; outputs the platform dependent line separator character or characters.
  * `%relative` &#8211; outputs the number of milliseconds elapsed since the start of the application until the creation of the logging event

The result looks something like this:

<pre>
  <code class="nohighlight">
    10:12:51.012 [qtp231719230-45] DEBUG o.c.d.r.util.LoggingResponseFilter
  </code>
</pre>

<p style="padding-left: 30px;">
   or
</p>

<pre>
  <code class="nohighlight">
    102662 [qtp1790106082-48] DEBUG o.c.d.r.dao.PodcastDao.getPodcasts
  </code>
</pre>

##### <span id="212_RollingFileAppender">2.1.2. RollingFileAppender</span>

The <a style="text-align: justify; line-height: 1.5;" href="http://logback.qos.ch/xref/ch/qos/logback/core/rolling/RollingFileAppender.html"><code>RollingFileAppender</code></a><span style="text-align: justify; line-height: 1.5;"> extends </span><code style="text-align: justify;">FileAppender</code><span style="text-align: justify; line-height: 1.5;"> with the capability to rollover log files. In the example, the </span><code style="text-align: justify;">RollingFileAppender</code><span style="text-align: justify; line-height: 1.5;"> will log to a file named </span><em style="text-align: justify; line-height: 1.5;">rest-demo.log</em><span style="text-align: justify; line-height: 1.5;"> file and, once a certain condition (every day and every minute) is met, change its logging target to another file.</span>

<p style="text-align: justify;">
  There are two important sub-components that interact with <code>RollingFileAppender</code>. The first <code>RollingFileAppender</code> sub-component, namely <code>RollingPolicy</code>, is responsible for undertaking the actions required for a rollover. A second sub-component of <code>RollingFileAppender</code>, namely <code>TriggeringPolicy</code>, will determine if and exactly when rollover occurs. Thus, <code>RollingPolicy</code> is responsible for the <em>what</em> and <code>TriggeringPolicy</code> is responsible for the <em>when</em>.
</p>

<p style="text-align: justify;">
  To be of any use, a <code>RollingFileAppender</code> must have both a <code>RollingPolicy</code> and a <code>TriggeringPolicy</code> set up. However, if its  <code>RollingPolicy</code> also implements the <code>TriggeringPolicy</code> interface, then only the former needs to be specified explicitly &#8211; this is exactly the case in the example, where <code>TimeBasedRollingPolicy</code> is used. This assumes the responsibility for rollover as well as for the triggering of said rollover, by implementing <em>both</em> <code>RollingPolicy</code> and <code>TriggeringPolicy </code>interfaces.
</p>

<p style="text-align: justify;">
  In the example the rollover occurs daily and every minute.  For the &#8220;daily&#8221;-appender, the file property is set to <em>rest-demo.log</em>: During today (March 21th, 2014), logging will go to the file <em>c:/temp/rest-demo.log</em>. At midnight, the rest-demo.log will be renamed to <em>c:/rest-demo.2014-03-21.log</em> and a new <em>c:/temp/rest-demo.log</em> will be created and for the rest of March 22th logging output will be redirected to rest-<em>demo.log</em>.
</p>

<p class="note_normal">
  <span style="line-height: 1.5;"><strong>Note:</strong><br /> The <code>file</code> property in </span><code>RollingFileAppender</code><span style="line-height: 1.5;"> (the parent of </span><code>TimeBasedRollingPolicy</code><span style="line-height: 1.5;">) can be either set or omitted. By setting the file property of the containing </span><code>FileAppender</code><span style="line-height: 1.5;">, you can decouple the location of the active log file and the location of the archived log files.</span>
</p>

## <span id="3_Integration_in_code">3. Integration in code</span>

<p style="text-align: justify;">
  To use SLF4J in code is pretty easy &#8211; you just have to get a logger, and then on that logger you can log messages at the different logging levels (TRACE, DEBUG, INFO, WARN, ERROR).
</p>

<p style="text-align: justify;">
  <span style="line-height: 1.5;">For demonstration purposes I created a special logging filter LoggingResponseFilter (didn&#8217;t want the REST facade cluttered with logging code) in the REST content, that will log</span>
</p>

  * **input** &#8211; the HTTP method used + the relative path called of the REST resource
  * **output** &#8211; a pretty print of the response entities in JSON format

  <pre>
    <code class="java">
    package org.codingpedia.demo.rest.util;

    import java.io.IOException;

    import javax.ws.rs.container.ContainerRequestContext;
    import javax.ws.rs.container.ContainerResponseContext;
    import javax.ws.rs.container.ContainerResponseFilter;

    import org.codehaus.jackson.map.ObjectMapper;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    public class LoggingResponseFilter
    		implements ContainerResponseFilter {

    	private static final Logger logger = LoggerFactory.getLogger(LoggingResponseFilter.class);

    	public void filter(ContainerRequestContext requestContext,
    			ContainerResponseContext responseContext) throws IOException {
    		String method = requestContext.getMethod();

    		logger.debug("Requesting " + method + " for path " + requestContext.getUriInfo().getPath());
    		Object entity = responseContext.getEntity();
    		if (entity != null) {
    			logger.debug("Response " + new ObjectMapper().writerWithDefaultPrettyPrinter().writeValueAsString(entity));
    		}

    	}

    }
  </code>
</pre>

So you I am just getting the &#8220;LoggingResponseFilter &#8221; logger  and on that, I will log messages at DEBUG level and upwards.

<p style="text-align: justify;">
  Well, that&#8217;s it! I would like to use this opportunity once again to thank the people who have developed and documented these wonderful frameworks. To name a few Dave Syer, Ceki Gülcü, Sébastien Pennec, Carl Harris . Thank you guys and keep up the good work!
</p>

## <span id="4_Resources">4. Resources</span>

### <span id="41Source_code">4.1. Source code</span>

  * <a style="color: #bc360a;" title="https://github.com/Codingpedia/demo-rest-jersey-spring" href="https://github.com/Codingpedia/demo-rest-jersey-spring" target="_blank">GitHub – Codingpedia/demo-rest-jersey-spring </a><span style="line-height: 1.5;">(instructions on how to install and run the project)</span>

### <span id="41_Codingpedia">4.1. Codingpedia</span>

  * <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>

### <span id="43_Web">4.3. Web</span>

  * <a title="Simple Logging Facade for Java (SLF4J)" href="http://www.slf4j.org/" target="_blank">Simple Logging Facade for Java (SLF4J)</a>
  * <a title="SLF4J user manual" href="http://www.slf4j.org/manual.html" target="_blank">SLF4J user manual</a>
  * <a title="Logback Project " href="http://logback.qos.ch/" target="_blank">Logback Project </a>
  * <a title="Logback documentation" href="http://logback.qos.ch/documentation.html" target="_blank">Logback documentation</a>
  * <a title="Logback Appenders" href="http://logback.qos.ch/manual/appenders.html" target="_blank">Logback Appenders</a>
  * <a title="Logback layouts" href="http://logback.qos.ch/manual/layouts.html" target="_blank">Logback Layouts</a>
  * <a title="Logging Dependencies in Spring" href="http://spring.io/blog/2009/12/04/logging-dependencies-in-spring/" target="_blank">Logging Dependencies in Spring</a> &#8211; spring.io
  * <a title="JUnit, Logback, Maven with Spring 3" href="http://techforenterprise.blogspot.de/2012/07/junit-logback-maven-with-spring-3.html" target="_blank">JUnit, Logback, Maven with Spring 3</a>
  * <a title="Logback or Log4j Additivity Explained" href="http://blog.idleworx.com/2010/07/logback-log4j-additivity-explained.html" target="_blank">Logback or Log4j Additivity Explained</a>

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

<div class="clear">
</div>
