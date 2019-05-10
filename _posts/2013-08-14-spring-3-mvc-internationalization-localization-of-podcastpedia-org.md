---
id: 387
title: 'Spring 3 MVC: Internationalization &#038; localization of Podcastpedia.org'
date: 2013-08-14T09:29:59+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=387
permalink: /ama/spring-3-mvc-internationalization-localization-of-podcastpedia-org/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 12
dsq_thread_id:
  - 1605138990
fsb_social_google:
  - 8
fsb_social_linkedin:
  - 1
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 1
categories:
  - java
  - spring
---
<a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a> can be accessed today in four languages &#8211; English, German, French and Romanian, with Spanish planned for the near future. In computing this is called internationalization (<a href="http://en.wikipedia.org/wiki/I18n" target="_blank">i18n</a>). The post presents how this is configured under the hood with the help of Spring 3 MVC.

### Application Context Configuration

#### Message Resource Files

Normally in the Java world, the <a title="Wikipedia - locale" href="http://en.wikipedia.org/wiki/Locale" target="_blank">locale</a>-specific data is stored in message resource files. In Spring you configure it by adding the following bean `org.springframework.context.support.ReloadableResourceBundleMessageSource` to the application context:

<pre><code class="xml">&lt;!-- Application Message Bundle --&gt;
&lt;bean id="messageSource"
  class="org.springframework.context.support.ReloadableResourceBundleMessageSource"&gt;
  &lt;property name="basename" value="classpath:messages/messages" /&gt;
  &lt;property name="defaultEncoding" value="UTF-8"/&gt;
&lt;/bean&gt;</code></pre>

<div>
  <!--more-->
</div>

<div>
  The configuration specifies that the message resource files should be named <em>messages_xx.properties (xx is the shortcut of the locale), </em>are stored in the <em>messages</em> folder in the classpath, and that the default encoding for the files is <a title="Wikipedia - utf8" href="http://en.wikipedia.org/wiki/Utf-8" target="_blank">UTF-8</a>.
</div>

<div>
</div>

<div>
  <div id="attachment_414" style="width: 299px" class="wp-caption alignnone">
    <a href="{{site.url}}/wp-content/uploads/2013/08/messages_classpath.png"><img class="size-medium wp-image-414" src="{{site.url}}/wp-content/uploads/2013/08/messages_classpath-289x300.png" alt="messages classpath" width="289" height="300" srcset="{{site.url}}/wp-content/uploads/2013/08/messages_classpath-289x300.png 289w, {{site.url}}/wp-content/uploads/2013/08/messages_classpath.png 398w" sizes="(max-width: 289px) 100vw, 289px" /></a>

    <p class="wp-caption-text">
      Message resource files in classpath
    </p>
  </div>
</div>

<div style="padding-left: 30px;">
  <em>Note: If the message resources files change often and you don&#8217;t want to restart the JVM, you should use <a href="http://static.springsource.org/spring/docs/3.2.4.RELEASE/javadoc-api/org/springframework/context/support/ReloadableResourceBundleMessageSource.html" target="_blank">org.springframework.context.support.ReloadableResourceBundleMessageSource </a></em>
</div>

<div style="padding-left: 30px;">
</div>

<div>
  <p>
    Spring&#8217;s <code>DispatcherServlet</code> enables you to automatically resolve messages using the client&#8217;s locale. This is done with <code>LocaleResolver</code> objects. You can select between
  </p>

  <ul>
    <li>
      an <code>AcceptHeaderLocaleResolver</code>, which inspects the <code>accept-language</code> header in the request that was sent by the client (e.g., a web browser). Usually this header field contains the locale of the client&#8217;s operating system.
    </li>
    <li>
      a <code>CookieLocaleResolver</code>,
    </li>
    <li>
      and a <code>SessionLocaleResolver</code>, which allows you to retrieve locales from the session that might be associated with the user&#8217;s reques
    </li>
  </ul>

  <h3>
    <code>CookieLocaleResolver</code>
  </h3>

  <p>
    The <code>CookieLocaleResolver</code> made the most sense for Podcastpedia.org . It inspects a <a title="HTTP cookie" href="http://en.wikipedia.org/wiki/HTTP_cookie" target="_blank">cookie</a> named <code>podcastpediaPreferredLanguage</code>, that might exist on the client to see if a locale is specified:
  </p>

<pre><code class="xml">&lt;bean id="localeResolver"&gt;
    &lt;property name="cookieName" value="podcastpediaPreferredLanguage"/&gt;
    &lt;property name="defaultLocale" value="en_US" /&gt;
    &lt;property name="cookieMaxAge" value="604800"/&gt;
&lt;/bean&gt;</code></pre>

  <p>
    If the cookie is not found, then the <code>defaultLocale</code> is set to American English. The <code>cookieMaxAge</code> (the maximum time a cookie will stay persistent on the client) is set to 604800 seconds (one month).
  </p>
</div>

### `LocaleChangeInterceptor`

<div>
  <p>
    The <code>LocaleResolver</code> is normally used in combination with the <code>LocaleChangeInterceptor</code>, which allows you to change of the current locale by using a defined parameter in the request (in this case the <code>lang</code> parameter). So, for example, a request for the following URL, <code>&lt;a title="Podcastpedia - categories" href="https://github.com/Codingpedia/podcastpedia/categories?lang=de" target="_blank">https://github.com/Codingpedia/podcastpedia/categories?&lt;strong>lang=de&lt;/strong>&lt;/a></code>, will change the site language to German:
  </p>

<pre><code class="xml">&lt;mvc:interceptors&gt;
&lt;!-- Changes the locale when a 'lang' request parameter is sent; e.g. /?lang=de --&gt;
  &lt;bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"&gt;
      &lt;property name="paramName" value="lang"/&gt;
   &lt;/bean&gt;
&lt;/mvc:interceptors&gt;</code></pre>
</div>

### Relevant code in Podcastpedia.org

#### JSP

On <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a> you can change the language by selecting the corresponding flag :

[<img class="alignnone  wp-image-417" src="{{site.url}}/wp-content/uploads/2013/08/Language_selection-300x44.png" alt="Language_selection" width="500" height="75" srcset="{{site.url}}/wp-content/uploads/2013/08/Language_selection-300x44.png 300w, {{site.url}}/wp-content/uploads/2013/08/Language_selection-1024x153.png 1024w, {{site.url}}/wp-content/uploads/2013/08/Language_selection-624x93.png 624w, {{site.url}}/wp-content/uploads/2013/08/Language_selection.png 1082w" sizes="(max-width: 500px) 100vw, 500px" />]({{site.url}}/wp-content/uploads/2013/08/Language_selection.png)

Behind the scenes, the jsp file, which common to all pages, looks something like the following:

<pre><code class="xml">&lt;div id="flags"&gt;
    &lt;a href="?lang=en"&gt;
        &lt;img alt="en" title="English" src="&lt;c:url value="/static/images/flags/en.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;a href="?lang=de"&gt;
        &lt;img alt="de" title="German" src="&lt;c:url value="/static/images/flags/de.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;a href="?lang=fr"&gt;
        &lt;img alt="fr" title="French" src="&lt;c:url value="/static/images/flags/fr.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;!--  TODO uncomment when the translation in Spanish is available
    &lt;a href="?lang=es"&gt;
        &lt;c:url value="/static/images/flags/es.png" var="url_flag_image" /&gt;
        &lt;img alt="es" title="Spanish" src="${url_flag_image}"&gt;
    &lt;/a&gt;
    --&gt;
    &lt;a href="?lang=ro"&gt;
        &lt;img alt="ro" title="Romanian" src="&lt;c:url value="/static/images/flags/ro.png"/&gt;"&gt;
    &lt;/a&gt;
&lt;/div&gt;</code></pre>

Basically by clicking on the flag the page is reloaded with the corresponding `lang` parameter, as presented above.

#### Access the Locale in Spring

If you need to access the current locale in Spring you can use the `org.springframework.context.i18n.LocaleContextHolder.LocaleContextHolder.getLocale()`, which returns the _Locale_ associated with the current thread, if any, or the system default Locale else. In the following code snippet you can see how I use this in the start page&#8217;s controller to display the newest and most popular podcasts based on the language selected:

<pre><code class="java">@ModelAttribute
public void addDataToModel(ModelMap model){
    ...............
    Locale locale = LocaleContextHolder.getLocale();
    String  language = locale.getLanguage();
    List&lt;String&gt; preferredLanguagesList =  Arrays.asList(preferredLanguages);
    if(preferredLanguagesList.contains(language)){
        model.put("newestPodcasts", startPageService.getNewestPodcasts(LanguageCode.get(language)));
        model.put("topRatedPodcasts", startPageService.getTopRatedPodcastsWithLanguage(LanguageCode.get(language), NUMBER_OF_PODCASTS_IN_CHART));
    } else {
        model.put("newestPodcasts", startPageService.getNewestPodcasts());
        model.put("topRatedPodcasts", startPageService.getTopRatedPodcasts(NUMBER_OF_PODCASTS_IN_CHART));
    }
}</code></pre>

### Watch out for&#8230;

#### Browser Caching optimization

If you have enabled browser caching, as specified for example in the post <a title="How To: Enable compression and leverage browser caching with Apache Server" href="http://www.codingpedia.org/ama/how-to-enable-compression-and-leverage-browser-caching-with-apache-server/" target="_blank">How To: Enable compression and leverage browser caching with Apache Server</a>, make sure you set the `expiring` and `cache control` for the html pages to 0 seconds:

<pre><code class="xml">ExpiresByType text/html                 "access plus 0 seconds"
....
Header set Cache-Control "max-age=0, private"</code></pre>

, so that changing the locale is effective immediately for the page &#8211; otherwise if you come back later to the same page, but without the locale parameter in the url, you would have the page displayed in the old locale, if the expiration time was not reached yet.

Well, that&#8217;s All Folks! If you would like to have Podcastpedia.org localized in your language, you can download the message resource file for English &#8211; [messages_en.properties]({{site.url}}/wp-content/uploads/2013/08/messages_en.zip), and contact me at _ama [AT] codingpedia DOT org_ &#8211; thanks

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

### Resources

  * <a title="Spring Documentation" href="http://www.springsource.org/spring-framework#documentation" target="_blank">Spring Framework documentation </a>
  * <a href="http://static.springsource.org/spring/docs/3.2.x/spring-framework-reference/html/" target="_blank">Spring 3.2 documentation </a>
  * <a title="Spring MVC locale resolver" href="http://static.springsource.org/spring/docs/3.2.x/spring-framework-reference/html/mvc.html#mvc-localeresolver" target="_blank">Spring MVC locale resolver</a>
  * <a title="Wikipedia - Internationalization and localization" href="http://en.wikipedia.org/wiki/I18n" target="_blank">http://en.wikipedia.org/wiki/I18n</a>
  * <a title="Java Resource Bundle" href="http://en.wikipedia.org/wiki/Java_resource_bundle" target="_blank">Java resource bundle </a>
  * <a title="Wikipedia - locale" href="http://en.wikipedia.org/wiki/Locale" target="_blank">Locale</a>

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
