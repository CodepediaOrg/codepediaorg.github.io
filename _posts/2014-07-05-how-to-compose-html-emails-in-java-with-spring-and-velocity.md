---
id: 1649
title: How to compose html emails in Java with Spring and Velocity
date: 2014-07-05T09:22:19+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1649
permalink: /ama/how-to-compose-html-emails-in-java-with-spring-and-velocity/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:47:"https://github.com/Codingpedia/podcastpedia";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 2819059243
fsb_social_facebook:
  - 9
fsb_social_google:
  - 7
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - spring
tags:
  - apache
  - email
  - email template
  - open source
  - spring
  - velocity
---
<p style="text-align: justify;">
  In this post I will present how you can format and send automatic emails with Spring and Velocity. Spring offers alone the capability to create simple text emails, which is fine for simple cases, but in typical enterprise application you wouldn&#8217;t want to do that for a number of reasons:
</p>

  * creating HTML-based email content in Java code is tedious and error prone
  * there is no clear separation between display logic and business logic
  * changing the display structure of the email requires writing Java code, recompiling, redeploying etc

<p style="text-align: justify;">
  Typically the approach taken to address these issues is to use a template library such as FreeMarker or Velocity to define the display structure of email content. For Podcastpedia I chose Velocity, which is a free open source Java-based templating engine from Apache. In the end my only coding task will be to create the data that is to be rendered in the email template and sending the email.
</p>
<!--more-->

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#Scenario">Scenario</a>
      </li>
      <li>
        <a href="#1_Prerequisites">1. Prerequisites</a><ul>
          <li>
            <a href="#11_Spring_setup">1.1. Spring setup</a><ul>
              <li>
                <a href="#111_Library_depedencies">1.1.1. Library depedencies</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#12_Velocity_setup">1.2. Velocity setup</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#2_Email_notification_service">2. Email notification service</a><ul>
          <li>
            <a href="#21_JavaMailSender_and_MimeMessagePreparator">2.1. JavaMailSender and MimeMessagePreparator</a>
          </li>
          <li>
            <a href="#22_MimeMessageHelper">2.2. MimeMessageHelper</a>
          </li>
          <li>
            <a href="#23_VelocityEngine">2.3. VelocityEngine</a><ul>
              <li>
                <a href="#231_Create_velocity_template">2.3.1. Create velocity template</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#24_Beans_configuration">2.4. Beans configuration</a>
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

<p style="text-align: justify;">
  I will base the demonstration on a real scenario from <a title="http://velocity.apache.org/engine/releases/velocity-1.7/overview.html" href="http://velocity.apache.org/engine/releases/velocity-1.7/overview.html" target="_blank">Podcastpedia.org</a>
</p>

## <span id="Scenario">Scenario</span>

<p style="text-align: justify;">
  On Podcastpedia.org&#8217;s <a title="Podcastpedia.org, subtmit podcast" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help/add_podcast" target="_blank">Submit podcast</a> page, we encourage our visitors and podcast producers to submit their podcasts to be included in our podcast directory. Once a podcast is submitted, an automatic email will be generated to notify me (adrianmatei [AT] gmail DOT com ) and the Podcastpedia personnel ( contact [AT] podcastpedia DOT org) about it.
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>
</p>

Let’s see now how Spring and Velocity play together:

## <span id="1_Prerequisites">1. Prerequisites</span>

### <span id="11_Spring_setup">1.1. Spring setup</span>

<p style="text-align: justify; padding-left: 30px;">
  <span style="color: #333333;"><em>&#8220;The Spring Framework provides a helpful utility library for sending email that shields the user from the specifics of the underlying mailing system and is responsible for low level resource handling on behalf of the client.&#8221;</em>[1]</span>
</p>

#### <span id="111_Library_depedencies">1.1.1. Library depedencies</span>

<p style="color: #333333;">
  The following additional jars need to be on the classpath of your application in order to be able to use the Spring Framework’s email library.
</p>

<div class="itemizedlist" style="color: #333333;">
  <ul class="itemizedlist" type="disc">
    <li class="listitem">
      The <a class="ulink" style="color: #4183c4;" href="https://java.net/projects/javamail/pages/Home" target="_top">JavaMail</a> <code class="literal">mail.jar</code> library
    </li>
    <li class="listitem">
      The <a class="ulink" style="color: #4183c4;" href="http://www.oracle.com/technetwork/java/jaf11-139815.html" target="_top">JAF</a> <code class="literal">activation.jar</code> library
    </li>
  </ul>

  <p>
    I load these dependencies with Maven, so here&#8217;s the configuration snippet from pom.xml:
  </p>

<pre><code class="xml">&lt;dependency&gt;
	&lt;groupId&gt;javax.mail&lt;/groupId&gt;
	&lt;artifactId&gt;mail&lt;/artifactId&gt;
	&lt;version&gt;1.4.7&lt;/version&gt;
	&lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;jaf&lt;/groupId&gt;
	&lt;artifactId&gt;activation&lt;/artifactId&gt;
	&lt;version&gt;1.0.2&lt;/version&gt;
	&lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;</code></pre>

  <h3 style="text-align: justify;">
    <span id="12_Velocity_setup">1.2. Velocity setup</span>
  </h3>

  <p>
    To use Velocity to create your email template(s), you will need to have the Velocity libraries available on your classpath in the first place.
  </p>

  <p style="text-align: justify;">
    With Maven you have the following dependencies in the pom.xml file:
  </p>

<pre><code class="xml">&lt;!-- velocity --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.velocity&lt;/groupId&gt;
	&lt;artifactId&gt;velocity&lt;/artifactId&gt;
	&lt;version&gt;1.7&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.apache.velocity&lt;/groupId&gt;
	&lt;artifactId&gt;velocity-tools&lt;/artifactId&gt;
	&lt;version&gt;2.0&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

  <h2>
    <span id="2_Email_notification_service">2. Email notification service</span>
  </h2>

  <p style="text-align: justify;">
    I defined the EmailNotificationService interface for email notification after a successful podcast submission. It has just one operation, namely to notify the Podcastpedia personnel about the proposed podcast.
  </p>
</div>

<p style="color: #333333;">
  The code bellow presents the EmailNotificationServiceImpl, which is the implementation of the interface mentioned above:
</p>

<pre><code class="java">package org.podcastpedia.web.suggestpodcast;

import java.util.Date;
import java.util.HashMap;
import java.util.Map;

import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

import org.apache.velocity.app.VelocityEngine;
import org.podcastpedia.common.util.config.ConfigService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.mail.javamail.MimeMessagePreparator;
import org.springframework.ui.velocity.VelocityEngineUtils;

public class EmailNotificationServiceImpl implements EmailNotificationService {

	@Autowired
	private ConfigService configService;
    private JavaMailSender mailSender;
    private VelocityEngine velocityEngine;

	public void sendSuggestPodcastNotification(final SuggestedPodcast suggestedPodcast) {
	      MimeMessagePreparator preparator = new MimeMessagePreparator() {
		        @SuppressWarnings({ "rawtypes", "unchecked" })
				public void prepare(MimeMessage mimeMessage) throws Exception {
		             MimeMessageHelper message = new MimeMessageHelper(mimeMessage);
		             message.setTo(configService.getValue("EMAIL_TO_SUGGEST_PODCAST"));
		             message.setBcc("adrianmatei@gmail.com");
		             message.setFrom(new InternetAddress(suggestedPodcast.getEmail()) );
		             message.setSubject("New suggested podcast");
		             message.setSentDate(new Date());
		             Map model = new HashMap();
		             model.put("newMessage", suggestedPodcast);

		             String text = VelocityEngineUtils.mergeTemplateIntoString(
		                velocityEngine, "velocity/suggestPodcastNotificationMessage.vm", "UTF-8", model);
		             message.setText(text, true);
		          }
		       };
		       mailSender.send(preparator);
	}

//getters and setters omitted for brevity

}</code></pre>

<p style="text-align: justify;">
  Let&#8217;s go a little bit through the code now:
</p>

<h3 style="text-align: justify;">
  <span id="21_JavaMailSender_and_MimeMessagePreparator">2.1. JavaMailSender and MimeMessagePreparator</span>
</h3>

<p style="text-align: justify;">
  The <code>org.springframework.mail</code> package is the root level package for the Spring Framework’s email support. The central interface for sending emails is the <code>MailSender</code> interface, but we are using the <code>org.springframework.mail.javamail.JavaMailSender</code>   interface (lines 22, 42), which adds specialized JavaMail features such as MIME message support to the MailSender interface (from which it inherits). <code>JavaMailSender</code> also provides a callback interface for preparation of JavaMail MIME messages, called <code>org.springframework.mail.javamail.MimeMessagePreparator (lines 26-42)&lt;br />
</code>
</p>

<h3 style="text-align: justify;">
  <span id="22_MimeMessageHelper">2.2. MimeMessageHelper</span>
</h3>

<p style="text-align: justify;">
  Another helpful class when dealing with JavaMail messages is the <code>org.springframework.mail.javamail.MimeMessageHelper</code> class, which shields you from having to use the verbose JavaMail API. As you can see by using the <code>MimeMessageHelper</code>, it becomes pretty easy to create a MimeMessage:
</p>

<pre><code class="java">MimeMessageHelper message = new MimeMessageHelper(mimeMessage);
 message.setTo(configService.getValue("EMAIL_TO_SUGGEST_PODCAST"));
 message.setBcc("adrianmatei@gmail.com");
 message.setFrom(new InternetAddress(suggestedPodcast.getEmail()) );
 message.setSubject("New suggested podcast");
 message.setSentDate(new Date());</code></pre>

### <span id="23_VelocityEngine">2.3. VelocityEngine</span>

The next thing to note is how the email text is being created:

<pre><code class="java">Map model = new HashMap();
model.put("newPodcast", suggestedPodcast);
String text = VelocityEngineUtils.mergeTemplateIntoString(
velocityEngine, "velocity/suggestPodcastNotificationMessage.vm", "UTF-8", model);
message.setText(text, true);</code></pre>

  * the `VelocityEngineUtils.mergeTemplateIntoString` method merges the specified template (`suggestPodcastNotificationMessage.vm` present in the velocity folder from the classpath) with the given model (model &#8211; &#8220;newPodcast&#8221;), which a map containing model names as keys and model objects as values.
  * you also need to the specify the velocityEngine you work with
  * and, finally, the result is returned as a string

#### <span id="231_Create_velocity_template">2.3.1. Create velocity template</span>

<p style="text-align: justify;">
  You can see below the Velocity template that is being used in this example. Note that it is HTML-based, and since it is plain text it can be created using your favorite HTML or text editor.
</p>

<pre><code class="html">&lt;html&gt;
	&lt;body&gt;
		&lt;h3&gt;Hi Adrian, you have a new suggested podcast!&lt;/h3&gt;
		&lt;p&gt;
			From - ${newMessage.name} / ${newMessage.email}
		&lt;/p&gt;
		&lt;h3&gt;
			Podcast metadataline
		&lt;/h3&gt;
		&lt;p&gt;
			${newMessage.metadataLine}
		&lt;/p&gt;
		&lt;h3&gt;
			With the message
		&lt;/h3&gt;
		&lt;p&gt;
			${newMessage.message}
		&lt;/p&gt;
	&lt;/body&gt;

&lt;/html&gt;</code></pre>

### <span id="24_Beans_configuration">2.4. Beans configuration</span>

Let&#8217;s see how everything is configured in the application context:

<pre><code class="xml">&lt;!-- ********************************* email service configuration ******************************* --&gt;
&lt;bean id="smtpSession" class="org.springframework.jndi.JndiObjectFactoryBean"&gt;
	&lt;property name="jndiName" value="java:comp/env/mail/Session"/&gt;
&lt;/bean&gt;
&lt;bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl"&gt;
	&lt;property name="session" ref="smtpSession" /&gt;
&lt;/bean&gt;
&lt;bean id="velocityEngine" class="org.springframework.ui.velocity.VelocityEngineFactoryBean"&gt;
  &lt;property name="velocityProperties"&gt;
	 &lt;value&gt;
	  resource.loader=class
	  class.resource.loader.class=org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
	 &lt;/value&gt;
  &lt;/property&gt;
&lt;/bean&gt;
&lt;bean id="emailNotificationServiceSuggestPodcast" class="org.podcastpedia.web.suggestpodcast.EmailNotificationServiceImpl"&gt;
  &lt;property name="mailSender" ref="mailSender"/&gt;
  &lt;property name="velocityEngine" ref="velocityEngine"/&gt;
&lt;/bean&gt;</code></pre>

  * the `JavaMailSender` has a JNDI reference to a smtp session. A generic example how to configure an email session with a google account can be found in the Jetty9-gmail-account.xml file
  * the `VelocityEngineFactoryBean` is a factory that configures the VelocityEngine and provides it as a bean reference.
  * the `ClasspathResourceLoader` is a simple loader that will load templates from the classpath

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="Summary">Summary</span>

<p style="text-align: justify;">
  You&#8217;ve learned in this example how to compose html emails in Java with Spring and Velocity. All you need are mail, spring and velocity libraries, compose your email template and use those simple Spring helper classes to add metadata to the email and send it.
</p>

## <span id="Resources">Resources</span>

  1. <a title="http://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/mail.html" href="http://docs.spring.io/spring/docs/4.0.x/spring-framework-reference/html/mail.html" target="_blank">Spring Email integration </a>
  2. <a title="http://velocity.apache.org/index.html" href="http://velocity.apache.org/index.html" target="_blank">Apache Velocity Project</a>

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
