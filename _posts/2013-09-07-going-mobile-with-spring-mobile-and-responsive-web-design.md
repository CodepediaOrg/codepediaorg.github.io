---
id: 543
title: Going mobile with Spring mobile and responsive web design
date: 2013-09-07T06:57:51+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=543
permalink: /ama/going-mobile-with-spring-mobile-and-responsive-web-design/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 3
dsq_thread_id:
  - 1725153395
fsb_social_google:
  - 4
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
  - css
  - css3
  - mobile
  - responsive design
  - spring
  - spring mobile
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Dependency">1. Dependency</a>
    </li>
    <li>
      <a href="#2_Application_Context_configuration">2. Application Context configuration</a><ul>
        <li>
          <a href="#21_DeviceResolverHandlerInterceptor">2.1. DeviceResolverHandlerInterceptor</a>
        </li>
        <li>
          <a href="#22_SitePreferenceHandlerInterceptor">2.2. SitePreferenceHandlerInterceptor</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_CSS3_and_responsive_design">3. CSS3 and responsive design</a>
    </li>
    <li>
      <a href="#4_References">4. References</a>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  As mentioned in <a title="Story of Podcastpedia.org" href="http://www.codingpedia.org/ama/story-of-podcastpedia-org/" target="_blank">Story of Podcastpedia</a>, the next thing that needed urgent improvement on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, was to make the website more user friendly for mobile users. This post will present how, with the help of <a title="Spring Mobile " href="http://www.springsource.org/spring-mobile" target="_blank">Spring Mobile</a>, the website can detect now if the request is coming from a mobile device and act accordingly: it displays a mobile view employing some responsive design features, while also offering the possibility to choose the preferred way (desktop or mobile) for displaying the web pages.
</p>

## <span id="1_Dependency">1. Dependency</span>

<p style="text-align: justify;">
  You need to add the <code>spring-mobile-device-x.x.x.RELEASE.jar</code> to your classpath. If you are using Maven like me just add the following dependency to your <code>pom.xml</code> file:
</p>

<pre class="lang:default decode:true" title="Maven dependency spring mobile">&lt;dependency&gt;
	&lt;groupId&gt;org.springframework.mobile&lt;/groupId&gt;
	&lt;artifactId&gt;spring-mobile-device&lt;/artifactId&gt;
	&lt;version&gt;${spring-mobile-device-version}&lt;/version&gt;
&lt;/dependency&gt;</pre>

<!--more-->

## <span id="2_Application_Context_configuration">2. Application Context configuration</span>

Now you need to do a couple of configuration in your Spring application context. First introduce the DeviceWebArgumentResolver :

<pre class="lang:default mark:3 decode:true" title="Spring application context snippet - DeviceWebArgumentResolver configuration">&lt;mvc:annotation-driven&gt;
	&lt;mvc:argument-resolvers&gt;
		&lt;bean class="org.springframework.mobile.device.DeviceWebArgumentResolver" /&gt;
	&lt;/mvc:argument-resolvers&gt;
&lt;/mvc:annotation-driven&gt;</pre>

and secondly <span style="line-height: 1.5;"> a </span>`DeviceResolverHandlerInterceptor` <span style="line-height: 1.5;">and a </span>`SitePreferenceHandlerInterceptor`<span style="line-height: 1.5;">:</span>

<pre class="lang:default mark:8,11 decode:true" title="Spring application context snippet - interceptors for mobile">&lt;mvc:interceptors&gt;
    &lt;!-- Changes the locale when a 'lang' request parameter is sent; e.g. /?lang=de --&gt;
    &lt;bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"&gt;
        &lt;property name="paramName" value="lang"/&gt;
    &lt;/bean&gt;

    &lt;!-- Resolve the device which has generated the request --&gt;
    &lt;bean class="org.springframework.mobile.device.DeviceResolverHandlerInterceptor" /&gt;

    &lt;!-- On pre-handle, manage the user's site preference (declare after DeviceResolverHandlerInterceptor) --&gt;
    &lt;bean class="org.springframework.mobile.device.site.SitePreferenceHandlerInterceptor" /&gt;
&lt;/mvc:interceptors&gt;</pre>

### <span id="21_DeviceResolverHandlerInterceptor"><span style="font-family: Bitter, Georgia, serif; font-size: 22px; line-height: 1.3;">2.1. DeviceResolverHandlerInterceptor</span></span>

<p style="text-align: justify;">
  The <code>DeviceResolverHandlerInterceptor</code> is a <code>HandlerInterceptor</code> that, on <code>preHandle</code>, delegates to a <code>DeviceResolver</code>. The resolved Device is indexed under a request attribute named <code>'currentDevice'</code>, making it available to handlers throughout request processing. The default <code>DeviceResolver</code> implementation used for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is based on the &#8220;lite&#8221; <a href="http://plugins.trac.wordpress.org/browser/wordpress-mobile-pack/trunk/plugins/wpmp_switcher/lite_detection.php" target="_top">detection algorithm</a> implemented as part of the <a href="http://wordpress.org/extend/plugins/wordpress-mobile-pack" target="_top">WordPress Mobile Pack</a>. This resolver only detects the presence of a mobile or tablet device, and does not detect specific capabilities. No special configuration is required to enable this resolver.
</p>

### <span id="22_SitePreferenceHandlerInterceptor">2.2. SitePreferenceHandlerInterceptor</span>

The `SitePreferenceHandlerInterceptor` enables SitePreference management before requests are processed. The indicated SitePreference is then used to vary control and view rendering logic, as shown in the `getPodcastDetails` method of the `PodcastController`:

<pre class="lang:java mark:21,22 decode:true" title="Snippet - detect mobile ">@RequestMapping(value="{podcastId}/*", method=RequestMethod.GET)
public String getPodcastDetails(@PathVariable("podcastId") int podcastId,
                             ModelMap model,
                             HttpServletRequest httpRequest) throws BusinessException{

    LOG.debug("------ getPodcastDetails : Received request to show details for podcast id " + podcastId + " ------");

    Podcast  podcast = podcastService.getPodcastById(podcastId);

    //add the last episodes to be displayed under the podcast metadata
    List&lt;Episode&gt; lastEpisodes = null;
    if(podcast.getEpisodes().size() &gt; 6){
        lastEpisodes = podcast.getEpisodes().subList(1, 6);
    } else {
        lastEpisodes = podcast.getEpisodes();
    }
    model.addAttribute("lastEpisodes", lastEpisodes);
    model.addAttribute("nr_divs_with_ratings", lastEpisodes.size());
    model.addAttribute("podcast", podcast);
    model.addAttribute("roundedRatingScore", Math.round(podcast.getRating() == null? 0 : podcast.getRating()));
    SitePreference currentSitePreference = SitePreferenceUtils.getCurrentSitePreference(httpRequest);
    if(currentSitePreference.isMobile()){
        return "m_podcastDetails";
    } else {
        return "podcastDetails";
    }
}</pre>

To obtain the reference to the current site preference, the `SitePreferenceUtils` (line 21) was used, by calling the `getCurrentSitePreference` method on the `HttpServletRequest` parameter. If the currentSitePreference is mobile a `mobile view (m_podcastDetails)` is selected, otherwise it will default to the `normal (podcastDetails)` desktop view. In <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, you can choose your site preference by selecting the mobile or desktop icon at the top left corner, right after the social media follow buttons:

<div id="attachment_550" style="width: 166px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/09/switch-to-desktop.png"><img class="size-medium wp-image-550" src="{{site.url}}/wp-content/uploads/2013/09/switch-to-desktop-156x300.png" alt="switch to desktop" width="156" height="300" srcset="{{site.url}}/wp-content/uploads/2013/09/switch-to-desktop-156x300.png 156w, {{site.url}}/wp-content/uploads/2013/09/switch-to-desktop.png 355w" sizes="(max-width: 156px) 100vw, 156px" /></a>

  <p class="wp-caption-text">
    Mobile version &#8211; switch to desktop
  </p>
</div>

<div id="attachment_551" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/09/switch-to-mobile.png"><img class="size-medium wp-image-551" src="{{site.url}}/wp-content/uploads/2013/09/switch-to-mobile-300x230.png" alt="switch to mobile" width="300" height="230" srcset="{{site.url}}/wp-content/uploads/2013/09/switch-to-mobile-300x230.png 300w, {{site.url}}/wp-content/uploads/2013/09/switch-to-mobile-624x478.png 624w, {{site.url}}/wp-content/uploads/2013/09/switch-to-mobile.png 800w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Desktop version &#8211; switch to mobile
  </p>
</div>

Behind the image the `currentSitePreference` parameter is set to either _mobile_ or _normal_:

<pre class="lang:xhtml mark:11 decode:true" title="HTML snippet - Switch to desktop">&lt;div id="logos"&gt;
    &lt;a href="https://www.facebook.com/Podcastpedia" target="_blank"&gt;
        &lt;img alt="Facebook" title="Follow us on Facebook" src="&lt;c:url value="/static/images/logos/fb.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;a href="https://twitter.com/podcastpedia" target="_blank"&gt;
        &lt;img alt="Twitter" title="Follow us on Twitter" src="&lt;c:url value="/static/images/logos/twitter.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;a href="//plus.google.com/101757667729624824161" target="_blank"&gt;
        &lt;img alt="Google+" title="Follow us on Google+" src="&lt;c:url value="/static/images/logos/gplus.png"/&gt;"&gt;
    &lt;/a&gt;
    &lt;a href="?site_preference=normal"&gt;
        &lt;img alt="Desktop" title="Desktop" src="&lt;c:url value="/static/images/logos/desktop_24.png"/&gt;"&gt;
    &lt;/a&gt;
&lt;/div&gt;</pre>

<p style="text-align: justify;">
   The indicated site preference is then stored in a <code>SitePreferenceRepository</code> so it is remembered in future requests made by the user. <code>CookieSitePreferenceRepository</code> is the default implementation used and stores the user&#8217;s&#8217; preference in a client-side cookie.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong> Note:</strong><br /> In addition, if no <code>SitePreference</code> has been explicitly indicated by the user, a <strong>default</strong> will be derived based on the user&#8217;s Device (MOBILE for mobile devices, and NORMAL otherwise).
</p>

## <span id="3_CSS3_and_responsive_design">3. CSS3 and responsive design</span>

<p style="text-align: justify;">
  In the end I want to present some modifications I made to the original css file to allow responsive design capabilities. The main difference was to use <a title="Mediy query" href="http://en.wikipedia.org/wiki/Media_queries" target="_blank">media queries</a> to shrink image sizes, modify widths, widths percentages, hide some elements:
</p>

<pre class="lang:css decode:true" title="CSS snippet with media query">@media screen and (max-width: 480px) {
....
}
@media screen and (max-width: 640px) {
    .col {
        margin: 1% 0 1% 0%;
    }
    .span_2_of_2 {
        width: 100%;
    }
    .span_1_of_2 {
        width: 100%;
    }
.....
}</pre>

On the <a title="Podcastpedia.org, knowledge to go - mobile" href="https://github.com/Codingpedia/podcastpedia/?site_preference=mobile" target="_blank">home page, </a>the <a title="Responsive Grid System" href="http://www.responsivegridsystem.com/" target="_blank">Responsive Grid System</a> was used to allow the podcast charts to slide under each other when the screen-width becomes small enough. Thank you very much Graham Miller for this:

<div id="attachment_565" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/09/home-page-responsiveness.png"><img class="size-medium wp-image-565  " src="{{site.url}}/wp-content/uploads/2013/09/home-page-responsiveness-300x165.png" alt="home page responsiveness" width="300" height="165" srcset="{{site.url}}/wp-content/uploads/2013/09/home-page-responsiveness-300x165.png 300w, {{site.url}}/wp-content/uploads/2013/09/home-page-responsiveness-624x345.png 624w, {{site.url}}/wp-content/uploads/2013/09/home-page-responsiveness.png 1020w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Podcastpedia.org&#8217;s home page responsiveness
  </p>
</div>

You can download the complete css file <a title="CSS file mobile" href="http://podcastpedia.org/static/css/style.min.m.css" target="_blank">here</a> (media queries can be found right at the end of the file).

Apart from just modifying the css file, second `.jsp` files for mobile (e.g. `podcastDetails<strong>_m</strong>.jsp`) were added, in which some parts, present in the destkop version, were removed to speed up things on mobile. As mentioned in the post <a title="How To: Enable compression and leverage browser caching with Apache Server" href="http://www.codingpedia.org/ama/how-to-enable-compression-and-leverage-browser-caching-with-apache-server/" target="_blank">How To: Enable compression and leverage browser caching with Apache Server</a>, using Google&#8217;s <a title="PageSpeed Insights" href="https://developers.google.com/speed/pagespeed/insights/" target="_blank">PageSpeed Insights</a> to test how your web pages perform on mobile and desktop is highly recommended.

This is just the beginning of improving the mobile experience, so if you notice any room for improvement, PLEASE <a href="mailto:contact@codingepdia.org?Subject=Apache%20optimization" target="_top">contact us</a> or leave a message.

If you liked this, please show your support by <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">helping us</a> with <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>

We promise to only share high quality podcasts and episodes.

## <span id="4_References">4. References</span>

  1. <a title="Spring Mobile" href="http://www.springsource.org/spring-mobile" target="_blank">Spring Mobile</a>
  2. <a title="Spring Mobile Hello World Example" href="http://www.srccodes.com/p/article/25/spring-mobile-hello-world-example-that-includes-deviceresolver-sitepreference-urlpath-siteswitcher-and-litedevicedelegatingviewresolver" target="_blank">Spring Mobile Hello World Example that includes DeviceResolver, SitePreference, urlPath SiteSwitcher and LiteDeviceDelegatingViewResolver</a> (Abhijit Ghosh)
  3. <a title="Media queries" href="http://css-tricks.com/snippets/css/media-queries-for-standard-devices/" target="_blank">Media queries for standard devices</a>
  4. <a title="Responsive Grid System" href="http://www.responsivegridsystem.com/" target="_blank">Responsive Grid System</a>

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
