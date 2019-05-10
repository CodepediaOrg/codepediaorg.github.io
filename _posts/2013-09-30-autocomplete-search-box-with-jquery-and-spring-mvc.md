---
id: 609
title: Autocomplete search box with jQuery and Spring MVC
date: 2013-09-30T21:15:30+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=609
permalink: /ama/autocomplete-search-box-with-jquery-and-spring-mvc/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 8
dsq_thread_id:
  - 1812057858
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 1
fsb_social_google:
  - 3
categories:
  - java
  - spring
tags:
  - autocomplete
  - javascript
  - jQuery
  - MyBatis
  - spring
  - spring mvc
---
## <span id="Functionality">Functionality</span>

Each podcast on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> has one or several associated keywords. When you go to a specific podcast you will see all the associated keywords :

<div id="attachment_610" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/09/podcast_keywords.png"><img class="size-medium wp-image-610" title="Podcast keywords sample" src="{{site.url}}/wp-content/uploads/2013/09/podcast_keywords-300x146.png" alt="Podcast keywords sample" width="300" height="146" srcset="{{site.url}}/wp-content/uploads/2013/09/podcast_keywords-300x146.png 300w, {{site.url}}/wp-content/uploads/2013/09/podcast_keywords-1024x499.png 1024w, {{site.url}}/wp-content/uploads/2013/09/podcast_keywords-624x304.png 624w, {{site.url}}/wp-content/uploads/2013/09/podcast_keywords.png 1065w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Podcast keywords sample
  </p>
</div>

<p style="text-align: justify;">
  There is also a special entry in the main menu &#8211; <a title="Podcastpedia.org, keywords" href="https://github.com/Codingpedia/podcastpedia/tags/all/0" target="_blank">Keywords</a> &#8211; that displays all the keywords associated to podcasts, ordered by the number of podcasts associated with. But the really cool part of the page is the <strong>autocomplete box</strong> &#8211; you can easily find podcasts related to a topic of your interest by typing in the first characters of the topic&#8217;s name. Let&#8217;s say you would like to see if there any podcasts related to Java, you would type &#8220;Ja&#8221;, the autocomplete functionality will display the keywords that start with &#8220;ja&#8221; and you can see &#8220;Java&#8221; exist and select it:
</p>

<div id="attachment_611" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery.png"><img class="size-medium wp-image-611" src="{{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery-300x173.png" alt="Autocomplete jQuery" width="300" height="173" srcset="{{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery-300x173.png 300w, {{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery-1024x591.png 1024w, {{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery-624x360.png 624w, {{site.url}}/wp-content/uploads/2013/09/autocomplete_jQuery.png 1237w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Autocomplete keywords box
  </p>
</div>

As shown in the picture above, you can select several keywords on the same page. But enough talking&#8230; let&#8217;s see how the magic happens behind the scenes, because this is actually the topic of this post.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#Functionality">Functionality</a>
    </li>
    <li>
      <a href="#The_Model">The Model</a>
    </li>
    <li>
      <a href="#The_View">The View</a><ul>
        <li>
          <a href="#JSP">JSP</a>
        </li>
        <li>
          <a href="#jQuery_code">jQuery code</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#The_Controller">The Controller</a><ul>
        <li>
          <a href="#References">References</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="The_Model">The Model</span>

In the model we need a `Keyword/Tag` class, that has an `Id`, a `Name` and a `NumberOfPodcasts` property associated with the it.

<pre><code class="java">package org.podcastpedia.domain;

import java.io.Serializable;
import net.sf.ehcache.pool.sizeof.annotations.IgnoreSizeOf;

@IgnoreSizeOf
public class Tag implements Serializable{

    private static final long serialVersionUID = -2370292880165225805L;

    /** id of the tag - BIGINT in MySQL DB */
    private long tagId;

    /** name of the tag */
    private String name;

    /** number of podcasts the tag is associated with */
    private Integer nrOfPodcasts;

    public Integer getNrOfPodcasts() {
        return nrOfPodcasts;
    }

    public void setNrOfPodcasts(Integer nrOfPodcasts) {
        this.nrOfPodcasts = nrOfPodcasts;
    }

    public long getTagId() {
        return tagId;
    }

    public void setTagId(long tagId) {
        this.tagId = tagId;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
</code></pre>

The `IgnoreSizeOf` annotation is set to ignore the Tag as a referenced object when calculating the size of the object graph in <a title="Ehcache.org" href="http://ehcache.org/" target="_blank">Ehcache</a>. This was a measure taken to fix an EhCache `net.sf.ehcache.pool.sizeof.ObjectGraphWalker` warning :

<pre>WARN 2013-02-04 10:09:14,632 net.sf.ehcache.pool.sizeof.ObjectGraphWalker: The configured limit of 1,000 object references was reached while attempting to calculate the size of the object graph. Severe performance degradation could occur if the sizing operation continues. This can be avoided by setting the
CacheManger or Cache  elements maxDepthExceededBehavior to "abort" or adding stop points with
@IgnoreSizeOf annotations. If performance degradation is NOT an issue at the configured limit, raise the limit value using the CacheManager or
Cache  elements maxDepth attribute. For more information, see the Ehcache configuration documentation.
</pre>

But more about caching with Ehcache in a future post.

Also at the model layer, we need a user interaction service, which returns a list of Tags, given the first characters of the search term:

<pre><code class="java">public class UserInteractionServiceImpl implements UserInteractionService{

    @Autowired
    private UserInteractionDao userInteractionDao;
    ....
	public List getTagList(String query) {
		return userInteractionDao.getTagList(query + "%");
	}
}
</code></pre>

Behind the `UserInteractionDao`, there is a <a title="MyBatis" href="http://mybatis.github.io/mybatis-3/" target="_blank">MyBatis</a> mapping that uses the following SQL statement:

<pre><code class="sql">SELECT
   t.tag_id,
   t.name,
   count(pt.podcast_id) as number_of_tags
FROM
   tags t,
   podcasts_tags pt,
   podcasts p
WHERE
   t.name like #{value}
   AND
   t.tag_id=pt.tag_id
   AND
   p.podcast_id = pt.podcast_id
   AND
   p.availability = 200
GROUP BY t.tag_id
ORDER BY
   t.name ASC
LIMIT 0,21
</code></pre>

The statement will return the first 21 keywords starting with input value (line 10) ordered in alphabetial &#8211; we don&#8217;t want too many displayed at once.

I won&#8217;t insist on the MyBatis mapping, because it is sort of irrelevant for this post and this will be explained in detail in a future MyBatis post.

## <span id="The_View">The View</span>

### <span id="JSP">JSP</span>

The `html/jsp` code behind the input box is quite simple &#8211; it is a regular `input` tag inside of a div tag, that has an associated `class` of type `ui-widget`. Make sure to also include the jQuery and jQuery-ui libraries:

<pre><code class="javascript">&lt;script type="text/javascript" src="http://code.jquery.com/jquery-1.9.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="http://code.jquery.com/ui/1.10.3/jquery-ui.js"&gt;&lt;/script&gt;&lt;
&lt;div id="find_keyword"&gt;
&lt;div class="ui-widget"&gt;&lt;input id="tagQuery" type="text" name="tagQuery" value="&lt;spring:message code=" /&gt;" onFocus="inputFocus(this)" onBlur="inputBlur(this)"&gt;&lt;/div&gt;
&lt;/div&gt;
</code></pre>

The `inputFocus` and `onBlur` JavaScript-functions will just change the coloring of the input text when `onFocus` and `onBlur` events occur on the input field:

<pre><code class="javascript">&lt;script type="text/javascript"&gt;// &lt;![CDATA[
	function inputFocus(i){
		if(i.value==i.defaultValue){ i.value=""; i.style.color="#000"; }
	}
	function inputBlur(i){
		if(i.value==""){ i.value=i.defaultValue; i.style.color="#848484"; }
	}
// ]]&gt;&lt;/script&gt;
</code></pre>

### <span id="jQuery_code">jQuery code</span>

<pre><code class="javascript">$(document).ready(function() {
    //attach autocomplete
    $("#tagQuery").autocomplete({
        minLength: 1,
        delay: 500,
        //define callback to format results
        source: function (request, response) {
            $.getJSON("/tags/get_tag_list", request, function(result) {
                response($.map(result, function(item) {
                    return {
                        // following property gets displayed in drop down
                        label: item.name + "(" + item.nrOfPodcasts + ")",
                        // following property gets entered in the textbox
                        value: item.name,
                        // following property is added for our own use
                        tag_url: "http://" + window.location.host + "/tags/" + item.tagId + "/" + item.name
                    }
                }));
            });
        },

        //define select handler
        select : function(event, ui) {
            if (ui.item) {
                event.preventDefault();
                $("#selected_tags span").append('&lt;a href=" + ui.item.tag_url + " target="_blank"&gt;'+ ui.item.label +'&lt;/a&gt;');
                //$("#tagQuery").value = $("#tagQuery").defaultValue
                var defValue = $("#tagQuery").prop('defaultValue');
                $("#tagQuery").val(defValue);
                $("#tagQuery").blur();
                return false;
            }
        }
    });
});
</code></pre>

The real magic happens in the `jQuery UI autocomplete` function &#8211; once the document is ready, the autocomplete function is associated to the `tagQuery` input field (line 3). After that a couple of parameters specific to the function need to be set:

  * `minLength = 1` &#8211; the minimum number of characters a user must type before a search is performed
  * `delay = 500` &#8211; the delay in milliseconds between when a keystroke occurs and when a search is performed (you want to give the database some time to rest 🙂 if you have many records)
  * `source` &#8211; must be specified and defines the data to use. In our case the data is provided by a callback function that, over `AJAX`, will get `JSON` data from the Server (the following step will present how this is implemented in Spring ). The result from the server is, as specified in the model, a list of Tag objects, which, with the help of <a title="jQuery map function" href="http://api.jquery.com/jQuery.map/" target="_blank">jQuery.map()</a>, gets translated to jQuery UI elements that are understood by autocomplete ui select handler
  * `select` &#8211; this is triggered when a keyword is selected from the proposed list. The action in this case will be to append the selected keyword to the existing ones (line 29), and restore the input text field value to the default one(lines 31 -33).

## <span id="The_Controller">The Controller</span>

Starting with version 3.0 Spring has significantly improved its support for AJAX calls. In our case there is a single method in the `TagController`, that will make use of an `UserInteractionService` defined in the model to return a list of Tag objects:

<pre><code class="java">package org.podcastpedia.controllers;
...
/**
 * Annotation-driven controller that handles requests to display tags categories in different forms.
 * @author Ama
 */
@Controller
@RequestMapping("/tags")
public class TagController {
...
    @RequestMapping(value = "/get_tag_list",  method = RequestMethod.GET)
    public @ResponseBody List getTagList(@RequestParam("term") String query) {
        List tagList = userInteractionService.getTagList(query);
        return tagList;
    }
}
</code></pre>

The `@ResponseBody` annotation instructs Spring MVC to serialize the list of Tags to the client and bind it to the web response body. Spring MVC automatically serializes to JSON because the client accepts that content type.

Underneath the covers, Spring MVC delegates to a [`HttpMessageConverter`](http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/http/converter/HttpMessageConverter.html) to perform the serialization. In this case, Spring MVC invokes a [`MappingJacksonHttpMessageConverter`](http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/http/converter/json/MappingJacksonHttpMessageConverter.html) built on the [Jackson](http://jackson.codehaus.org/) JSON processor. This happens automatically but you need to use the [`mvc:annotation-driven`](https://src.springframework.org/svn/spring-samples/mvc-ajax/trunk/src/main/webapp/WEB-INF/spring/mvc-config.xml) configuration element and have with Jackson present in your classpath. In the project, the Jackson libraries are loaded with maven using the following dependencies in the `pom.xml` file:

<pre><code class="xml"><!-- Jackson JSON Mapper -->
&lt;dependency&gt;
    &lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
    &lt;artifactId&gt;jackson-mapper-asl&lt;/artifactId&gt;
    &lt;version&gt;1.9.12&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
    &lt;artifactId&gt;jackson-jaxrs&lt;/artifactId&gt;
    &lt;version&gt;1.9.12&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;org.codehaus.jackson&lt;/groupId&gt;
    &lt;artifactId&gt;jackson-core-asl&lt;/artifactId&gt;
    &lt;version&gt;1.9.12&lt;/version&gt;
&lt;/dependency&gt;
</code></pre>

The controller method is called asynchronously every time the user tips a new character, no sooner than every 500ms as set in the View.

<p style="text-align: justify;">
  Well, this is how the magic happens behind the scenes. If you notice any room for optimization please <a href="mailto:contact@codingepdia.org?Subject=jQuery%20autocomplete" target="_top">contact us</a> or leave a message. Many thanks to the jQuery and Spring creators and contributers, to the open source communities, to Google, Stackoverflow and to all the great people out there.
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

### <span id="References">References</span>

  1. <a title="jQueryUI autocomplete" href="http://jqueryui.com/autocomplete/" target="_blank">jQuery UI autocomplete </a>
  2. <a title="jQuery.getJSON - jQuery API Documentation" href="http://api.jquery.com/jQuery.getJSON/" target="_blank">jQuery.getJSON &#8211; jQuery API Documentation</a>
  3. <a title="jQuery Core - $(document).ready()" href="http://learn.jquery.com/using-jquery-core/document-ready/" target="_blank">jQuery Core &#8211; $(document).ready()</a>
  4. <a title="jQuery map function" href="http://api.jquery.com/jQuery.map/" target="_blank">jQuery.map()</a>
  5. <a title="Ajax, Wikipedia" href="http://en.wikipedia.org/wiki/Ajax_%28programming%29" target="_blank">AJAX</a>
  6. <a title="Ajax Simplifications in Spring 3.0" href="http://spring.io/blog/2010/01/25/ajax-simplifications-in-spring-3-0/" target="_blank">Ajax Simplifications in Spring 3.0</a>

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

Adrian&#8217;s favorite Spring and jQuery books

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
