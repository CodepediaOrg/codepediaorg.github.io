---
id: 717
title: reCAPTCHA and Spring MVC integration
date: 2013-10-17T09:46:42+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=717
permalink: /ama/recaptcha-and-spring-mvc-integration/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 7
dsq_thread_id:
  - 1866837901
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - spring
tags:
  - captcha
  - java
  - javascript
  - recaptcha
  - spring
  - spring mvc
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Why_use_reCAPTCHA">Why use reCAPTCHA?</a><ul>
        <li>
          <a href="#Sign_up_for_a_reCAPTCHA_account">Sign up for a reCAPTCHA account</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#reCAPTCHA_Spring_MVC_integration">reCAPTCHA Spring MVC integration</a><ul>
        <li>
          <a href="#Maven_dependency">Maven dependency</a>
        </li>
        <li>
          <a href="#Client_Side_How_to_make_the_CAPTCHA_image_show_up">Client Side (How to make the CAPTCHA image show up)</a>
        </li>
        <li>
          <a href="#Server_Side_How_to_test_if_the_user_entered_the_right_answer">Server Side (How to test if the user entered the right answer)</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#10x">10x</a>
    </li>
    <li>
      <a href="#References">References</a>
    </li>
  </ul>
</div>

## <span id="Why_use_reCAPTCHA">Why use reCAPTCHA?</span>

<p style="text-align: justify;">
  We have a section on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, called <a title="Recommend podcasts on Podcastpedia.org" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help/add_podcast" target="_blank">Recommend podcast</a>, that allows visitors to submit podcasts. Lately we have received very good suggestions &#8211; thank you all you Guys for that, but also lots of spam. A way to signifactly reduce the amount of spam, is to use <a title="Wikipedia, captcha definition" href="http://en.wikipedia.org/wiki/CAPTCHA" target="_blank">captchas</a>, which is a type of challenge-response test used in computing to determine whether or not the user is human. One popular implementaiton of captchas, is <a title="ReCAPTCHA homepage" href="http://www.google.com/recaptcha" target="_blank">reCAPTCHA</a>, now owned by Google. You might thing that solving catpchas is annoying, but by using reCAPTCHA you help to digitize books, newspapers and old time radio shows &#8211; <a title="Ted talk cOnline Collaboration" href="http://www.ted.com/talks/luis_von_ahn_massive_scale_online_collaboration.html" target="_blank">here</a> is a greatd TED talk from Luis von Ahn on massive-scale online collaboration explaining how this works:<br /> <br /> Last but not least, you help us avoid <a title="Email spam" href="http://en.wikipedia.org/wiki/Email_spam" target="_blank">email spam</a>, which I guess you know by now how annoying and dangerous that can be.
</p>

This post presents how reCAPTCHA is integrated with a Spring MVC form to recommend podcasts.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>



<!--more-->

### <span id="Sign_up_for_a_reCAPTCHA_account">Sign up for a reCAPTCHA account</span>

Before using reCAPTCHA you have to sign up for an account. For your domain, you will receive then a **Public Key**, to use in the JavaScript code that is served to your users and a **Private Key** to use when communicating between your server and reCAPTCHA server &#8211; make sure to keep the latter secret.

## <span id="reCAPTCHA_Spring_MVC_integration">reCAPTCHA Spring MVC integration</span>

### <span id="Maven_dependency">Maven dependency</span>

Add the following dependency to your `pom.xml` file

<pre class="lang:default decode:true" title="reCAPTCHA maven dependency">&lt;!-- reCaptcha --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;net.tanesha.recaptcha4j&lt;/groupId&gt;
	&lt;artifactId&gt;recaptcha4j&lt;/artifactId&gt;
	&lt;version&gt;0.0.7&lt;/version&gt;
&lt;/dependency&gt;</pre>

The current Spring version used for Podcastpedia is `<spring.version>3.2.3.RELEASE</spring.version>`

If you don&#8217;t use Maven to build your application you can download the [reCAPTCHA Java Library here](http://code.google.com/p/recaptcha/downloads/list?q=label:java-Latest) (contributed by Soren) and unzip it. Typically the only thing you&#8217;ll need is the jar file (recaptcha4j-X.X.X.jar), which you have to copy to a place where it can be loaded by your java application. For example, if you are using Tomcat to run JSP, you may put the jar file in a directory called _WEB-INF/lib/_.

### <span id="Client_Side_How_to_make_the_CAPTCHA_image_show_up">Client Side (How to make the CAPTCHA image show up)</span>

To use the Java plugin to display the reCAPTCHA widget, you&#8217;ll need to import the appropriate reCAPTCHA classes. In JSP, you would do this by inserting these lines near the top of the file with the form element where the reCAPTCHA widget will be displayed:

<pre class="lang:xhtml decode:true" title="Import Java packages in JSP file">&lt;%@ page import="net.tanesha.recaptcha.ReCaptcha" %&gt;
&lt;%@ page import="net.tanesha.recaptcha.ReCaptchaFactory" %&gt;</pre>

Then, you need to create an instance of reCAPTCHA:

<pre class="lang:java decode:true" title="reCAPTCHA instance">ReCaptcha c = ReCaptchaFactory.newReCaptcha("your_public_key", "your_private_key", false);</pre>

Finally, the HTML to display the reCAPTCHA widget can be obtained from the following function call:

<pre class="lang:default decode:true" title="createHTML to display reCAPTCHA widget">c.createRecaptchaHtml(null, null)</pre>

I wanted the reCAPTCHA widget customized, so I used the &#8220;clean&#8221; theme and also introduced support to internationalize the reCAPTCHA snippet with my own messages. For that the following JavaScript code had to be included in the jsp file:

<pre class="lang:js decode:true" title="reCAPTCHA custom options">&lt;script type="text/javascript"&gt;
	var strings = new Array();
	strings['recaptcha.instructions_visual'] = "&lt;spring:message code='recaptcha.instructions_visual' javaScriptEscape='true'/&gt;";
	strings['recaptcha.instructions_audio'] = "&lt;spring:message code='recaptcha.instructions_audio' javaScriptEscape='true'/&gt;";
	strings['recaptcha.play_again'] = "&lt;spring:message code='recaptcha.play_again' javaScriptEscape='true'/&gt;";
	strings['recaptcha.cant_hear_this'] = "&lt;spring:message code='recaptcha.cant_hear_this' javaScriptEscape='true'/&gt;";
	strings['recaptcha.visual_challenge'] = "&lt;spring:message code='recaptcha.visual_challenge' javaScriptEscape='true'/&gt;";
	strings['recaptcha.audio_challenge'] = "&lt;spring:message code='recaptcha.audio_challenge' javaScriptEscape='true'/&gt;";
	strings['recaptcha.refresh_btn'] = "&lt;spring:message code='recaptcha.refresh_btn' javaScriptEscape='true'/&gt;";
	strings['recaptcha.help_btn'] = "&lt;spring:message code='recaptcha.help_btn' javaScriptEscape='true'/&gt;";
	strings['recaptcha.incorrect_try_again'] = "&lt;spring:message code='recaptcha.incorrect_try_again' javaScriptEscape='true'/&gt;";

	var RecaptchaOptions = {
	custom_translations : {
		 instructions_visual :  strings['recaptcha.instructions_visual'] ,
		 instructions_audio : strings['recaptcha.instructions_audio'],
		 play_again : strings['recaptcha.play_again'],
		 cant_hear_this : strings['recaptcha.cant_hear_this'],
		 visual_challenge : strings['recaptcha.visual_challenge'],
		 audio_challenge : strings['recaptcha.audio_challenge'],
		 refresh_btn : strings['recaptcha.refresh_btn'],
		 help_btn : strings['recaptcha.help_btn'],
		 incorrect_try_again : strings['recaptcha.incorrect_try_again']
	},
	theme : 'clean'
	};
&lt;/script&gt;</pre>

The strings array contains the values of the localized messages I want displayed in the captcha widget. See the post <a title="Spring 3 MVC: Internationalization & localization of Podcastpedia.org" href="http://www.codingpedia.org/ama/spring-3-mvc-internationalization-localization-of-podcastpedia-org/" target="_blank">Spring 3 MVC: Internationalization & localization of Podcastpedia.org</a> to find out how internationalization and localization is achieved for Podcastpedia.org

If put together, all these relevant things look somewhat like the following in the jsp file:

<pre class="lang:xhtml decode:true" title="relevant reCAPTCHA code in JSP file">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %&gt;
&lt;%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%&gt;
&lt;%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%&gt;
&lt;%@ page import="net.tanesha.recaptcha.ReCaptcha" %&gt;
&lt;%@ page import="net.tanesha.recaptcha.ReCaptchaFactory" %&gt;
&lt;%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%&gt;

&lt;%@ include file="/WEB-INF/jsp/common/recaptcha_options.jsp" %&gt;

.....
	&lt;form:form id="add_podcast_form" class="vertical_style_form"
		method="POST" modelAttribute="addPodcastForm"&gt;
		.....
		&lt;div id="captcha_paragraph"&gt;
			&lt;c:if test="${invalidRecaptcha == true}"&gt;
				&lt;span class="error_form_validation"&gt;&lt;spring:message code="invalid.captcha" text="Invalid captcha please try again"/&gt;&lt;/span&gt;
			&lt;/c:if&gt;
		    &lt;%
		        ReCaptcha c = ReCaptchaFactory.newReCaptcha("6LcW3OASAAAAAKEJTHMmp_bo5kny4lZXeDtgcMqC",
		        					"6LcW3OASAAAAAKVX2duVsSy2uMMHL105-jPDrHMD", false);
		        out.print(c.createRecaptchaHtml(null, null));
		    %&gt;
		&lt;/div&gt;
		.....
	&lt;/form:form&gt;
	&lt;span style="color:#A73030;margin-left:30px;font-weight: bold;"&gt;*&lt;spring:message code="required" text="required"/&gt;&lt;/span&gt;
&lt;/div&gt;</pre>

where `/WEB-INF/jsp/common/recaptcha_options.jsp` contains the customization options mentioned above. The result looks similar to the following:

[<img class="alignnone size-medium wp-image-741" src="{{site.url}}/wp-content/uploads/2013/10/recommend-podcast-form-focus-recaptcha-300x178.png" alt="recaptcha widget" width="300" height="178" srcset="{{site.url}}/wp-content/uploads/2013/10/recommend-podcast-form-focus-recaptcha-300x178.png 300w, {{site.url}}/wp-content/uploads/2013/10/recommend-podcast-form-focus-recaptcha-624x372.png 624w, {{site.url}}/wp-content/uploads/2013/10/recommend-podcast-form-focus-recaptcha.png 880w" sizes="(max-width: 300px) 100vw, 300px" />]({{site.url}}/wp-content/uploads/2013/10/recommend-podcast-form-focus-recaptcha.png)

### <span id="Server_Side_How_to_test_if_the_user_entered_the_right_answer">Server Side (How to test if the user entered the right answer)</span>

First the `reCAPTCHA bean` has to be configured in the Spring application context:

<pre class="lang:default mark:27-29 decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;beans  xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:jaxws="http://cxf.apache.org/jaxws"
	    xmlns:p="http://www.springframework.org/schema/p"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:tx="http://www.springframework.org/schema/tx"
	    xmlns:mvc="http://www.springframework.org/schema/mvc"
	    xmlns:cache="http://www.springframework.org/schema/cache"
	    xsi:schemaLocation="
			http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/mvc
			http://www.springframework.org/schema/mvc/spring-mvc.xsd
			http://www.springframework.org/schema/context
			http://www.springframework.org/schema/context/spring-context.xsd
			http://cxf.apache.org/jaxws
			http://cxf.apache.org/schemas/jaxws.xsd
			http://www.springframework.org/schema/tx
			http://www.springframework.org/schema/tx/spring-tx.xsd
			http://www.springframework.org/schema/cache
			http://www.springframework.org/schema/cache/spring-cache.xsd"&gt;

	&lt;context:component-scan base-package="org.podcastpedia.service, org.podcastpedia.controllers"/&gt;
	&lt;mvc:annotation-driven/&gt;
	......
	&lt;bean id="reCaptcha" class="net.tanesha.recaptcha.ReCaptchaImpl"&gt;
		&lt;property name="privateKey" value="your_recaptcha_private_key"/&gt;
	&lt;/bean&gt;
	......
&lt;/beans&gt;</pre>

The captcha verification occurs directly in the controller:

<pre class="lang:java mark:3,4,12,19,20,27,28,30 decode:true" title="reCAPTCHA server side verification - Controller">package org.podcastpedia.controllers;

import net.tanesha.recaptcha.ReCaptchaImpl;
import net.tanesha.recaptcha.ReCaptchaResponse;
....

@Controller
@RequestMapping("/how_can_i_help")
@SessionAttributes("addPodcastForm")
public class HowCanIhelpController {
	@Autowired
	ReCaptchaImpl reCaptcha;
	.....
	@RequestMapping(value="add_podcast", method=RequestMethod.POST)
	public String processAddPodcastForm(
			@ModelAttribute("addPodcastForm") SuggestedPodcast addPodcastFormData,
			BindingResult result,
			Model model,
			@RequestParam("recaptcha_challenge_field") String challangeField,
			@RequestParam("recaptcha_response_field") String responseField,
			ServletRequest servletRequest,
			SessionStatus sessionStatus){

		LOG.debug("------ processAddPodcastForm : form is being validated and processed -----");
		suggestPodcastValidator.validate(addPodcastFormData, result);

		String remoteAddress = servletRequest.getRemoteAddr();
		ReCaptchaResponse reCaptchaResponse = this.reCaptcha.checkAnswer(remoteAddress, challangeField, responseField);

		if(reCaptchaResponse.isValid() && !result.hasErrors()){

        	userInteractionService.addSuggestedPodcast(addPodcastFormData);
        	emailNotificationService.sendSuggestPodcastNotification(addPodcastFormData);
        	sessionStatus.setComplete();

        	return "redirect:/how_can_i_help/add_podcast?tks=true";
		} else {
			model.addAttribute("addPodcastForm", addPodcastFormData);
    		if(!reCaptchaResponse.isValid()) {
    			result.rejectValue("invalidRecaptcha", "invalid.captcha");
    			model.addAttribute("invalidRecaptcha", true);
    		}
			return "add_podcast_form_def";
		}

	}
	.....
}</pre>

First you need to import the necessary classes (`lines 3 and 4`), autowire the reCaptcha bean defined in the application context (`line 12`), initialize the passed captcha parameter fields (`lines 19 and 20`) and validate the recaptcha challenge and response (`line 28`).

The `remoteAddress` (`line 27`) is the user&#8217;s IP address or last proxy, which is passed to the reCAPTCHA servers.

If reCAPTCHA response is valid and all the other form fields are properly filled (`line 30`) a notification email is sent with the recommended podcast and the current handler&#8217;s session processing is marked as complete, allowing for cleanup of session attributes. If there are errors corresponding error messages are displayed.

Well, that&#8217;s it. After reading this I hope you feel more positive about resolving captchas and integrating them on your website.

## <span id="10x">10x</span>

  * Many thanks to the guys who created captcha/reCaptcha
  * very much to the guys who recommended podcasts for the directory and remember if you know any good podcast worth sharing <a title="Recommend podcasts on Podcastpedia.org" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help/add_podcast" target="_blank">tell us</a> about it and we will add it to the directory.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="References">References</span>

  1. <a title="ReCAPTCHA homepage" href="http://www.google.com/recaptcha" target="_blank">reCAPTCHA</a>
  2. <a title="Using reCAPTCHA with Java/JSP" href="https://developers.google.com/recaptcha/docs/java" target="_blank">Using reCAPTCHA with JAVA/JSP </a>
  3. <a title=" Customizing the Look and Feel of reCAPTCHA" href="https://developers.google.com/recaptcha/docs/customization" target="_blank">Customizing the Look and Feel of reCAPTCHA</a>
  4. <a title="Spring Framework Reference Documentation" href="http://docs.spring.io/spring/docs/3.2.3.RELEASE/spring-framework-reference/html/" target="_blank">Spring Framework Reference Documentation</a>

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

Adrian&#8217;s favorite Spring books

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="amazon_book">
</div>

<div class="clear">
</div>
