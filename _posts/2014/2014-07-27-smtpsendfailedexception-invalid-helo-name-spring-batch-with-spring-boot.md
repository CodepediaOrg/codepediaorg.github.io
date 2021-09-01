---
id: 1707
title: 'SMTPSendFailedException &#8220;Invalid HELO name&#8221; &#8211; Spring Batch with Spring Boot'
date: 2014-07-27T06:46:52+00:00
author: ama
layout: post
permalink: /ama/smtpsendfailedexception-invalid-helo-name-spring-batch-with-spring-boot/
categories:
  - article
tags:
  - java
  - debugging
  - exception
  - java mail api
  - smtp
  - spring-batch
  - spring-boot
---
If you find yourself getting the following errror, when trying to send an email in Java:

## com.sun.mail.smtp.SMTPSendFailedException: 550 Access denied &#8211; Invalid HELO name (See RFC2821 4.1.1.1)

<pre class="lang:sh mark:1 decode:true" title="Error message">Failed message 1: com.sun.mail.smtp.SMTPSendFailedException: 550 Access denied - Invalid HELO name (See RFC2821 4.1.1.1)

	at org.springframework.mail.javamail.JavaMailSenderImpl.doSend(JavaMailSenderImpl.java:448)
	at org.springframework.mail.javamail.JavaMailSenderImpl.send(JavaMailSenderImpl.java:346)
	at org.springframework.mail.javamail.JavaMailSenderImpl.send(JavaMailSenderImpl.java:363)
	at org.springframework.mail.javamail.JavaMailSenderImpl.send(JavaMailSenderImpl.java:351)
	at org.podcastpedia.batch.jobs.addpodcast.service.EmailNotificationServiceImpl.sendPodcastAdditionConfirmation(EmailNotificationServiceImpl.java:53)
	at org.podcastpedia.batch.jobs.addpodcast.SuggestedPodcastItemWriter.write(SuggestedPodcastItemWriter.java:50)
	at org.springframework.batch.core.step.item.SimpleChunkProcessor.writeItems(SimpleChunkProcessor.java:175)
	at org.springframework.batch.core.step.item.SimpleChunkProcessor.doWrite(SimpleChunkProcessor.java:151)
	at org.springframework.batch.core.step.item.FaultTolerantChunkProcessor$3.doWithRetry(FaultTolerantChunkProcessor.java:329)
	at org.springframework.retry.support.RetryTemplate.doExecute(RetryTemplate.java:263)
	... 43 more</pre>

<p style="text-align: justify;">
  and cannot figure out why does it not work?!, EVEN THOUGH you can send emails via Telnet using the same configuration as the one set up for the Java client or you set the <code>mail.smtp.localhost</code> property to the  fully qualified domain name (FQDN) of the client host &#8211; that might be IP address of the client host &#8211;  as suggested in the <a title="https://www.oracle.com/technetwork/java/faq-135477.html#helo" href="https://www.oracle.com/technetwork/java/faq-135477.html#helo" target="_blank">JavaMail API FAQ</a>&#8230; THEN it might be that you are using an old version of the java mail api.<!--more-->
</p>

## Solution

<p style="text-align: justify;">
  I have configured a Spring Batch job with Spring Boot version 1.1.3.RELEASE by building on the <a title="https://spring.io/guides/gs/batch-processing/" href="https://spring.io/guides/gs/batch-processing/" target="_blank">Getting started guide for Spring Batch</a> on Spring.io, and for some reason the <code>spring-boot-starter-remote-shell</code> artifact comes with the java mail library in version 1.4. Replacing this with the version 1.4.7 (version which works for the batch jobs developed in my own kind of way&#8230;) solved the problem for me:
</p>

<pre class="lang:xhtml mark:18-32 decode:true " title="Snippet from pom.xml to solve the problem">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;project xmlns="https://maven.apache.org/POM/4.0.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="https://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
    &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;

    &lt;groupId&gt;org.podcastpedia.batch&lt;/groupId&gt;
    &lt;artifactId&gt;gs-batch-processing&lt;/artifactId&gt;
    &lt;version&gt;0.1.0&lt;/version&gt;

    &lt;parent&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-parent&lt;/artifactId&gt;
        &lt;version&gt;1.1.3.RELEASE&lt;/version&gt;
    &lt;/parent&gt;

    &lt;dependencies&gt;
	........
 		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
			&lt;artifactId&gt;spring-boot-starter-remote-shell&lt;/artifactId&gt;
		    &lt;exclusions&gt;
		        &lt;exclusion&gt;
					&lt;groupId&gt;javax.mail&lt;/groupId&gt;
					&lt;artifactId&gt;mail&lt;/artifactId&gt;
		        &lt;/exclusion&gt;
		    &lt;/exclusions&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;javax.mail&lt;/groupId&gt;
			&lt;artifactId&gt;mail&lt;/artifactId&gt;
			&lt;version&gt;1.4.7&lt;/version&gt;
		&lt;/dependency&gt;
	........
    &lt;/dependencies&gt;

    &lt;repositories&gt;
        &lt;repository&gt;
            &lt;id&gt;spring-snapshots&lt;/id&gt;
            &lt;url&gt;https://repo.spring.io/libs-snapshot&lt;/url&gt;
            &lt;snapshots&gt;&lt;enabled&gt;true&lt;/enabled&gt;&lt;/snapshots&gt;
        &lt;/repository&gt;
    &lt;/repositories&gt;

    &lt;pluginRepositories&gt;
        &lt;pluginRepository&gt;
            &lt;id&gt;spring-snapshots&lt;/id&gt;
            &lt;url&gt;https://repo.spring.io/libs-snapshot&lt;/url&gt;
            &lt;snapshots&gt;&lt;enabled&gt;true&lt;/enabled&gt;&lt;/snapshots&gt;
        &lt;/pluginRepository&gt;
    &lt;/pluginRepositories&gt;

&lt;/project&gt;</pre>

## Conclusion

If you get this error in any circumstances make sure you use a newer version of the java mail api. Version 1.4.7 worked for me.

<p class="note_normal">
  P.S. A complete Spring Batch tutorial with Spring Boot will follow soon, so stay tuned&#8230;
</p>

&nbsp;

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/CodepediaOrg/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="CodepediaOrg, share code knowledge" href="https://www.codepedia.org" target="_blank">Codepedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/CodepediaOrg" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/CodepediaOrg" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codepediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adi-matei" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
