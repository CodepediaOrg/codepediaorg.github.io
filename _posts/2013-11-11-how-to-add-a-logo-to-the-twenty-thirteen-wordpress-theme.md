---
id: 915
title: How to add a logo to the Twenty Thirteen WordPress theme
date: 2013-11-11T21:09:59+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=915
permalink: /ama/how-to-add-a-logo-to-the-twenty-thirteen-wordpress-theme/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 7
dsq_thread_id:
  - 1957130292
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
fsb_social_google:
  - 4
categories:
  - WordPress
tags:
  - 2013
  - logo
  - theme
  - twenty thirteen
  - wordpress
---
<p style="text-align: justify;">
  In this post I will share my experience of adding a logo in front of the site title when using the <a title="Wordpress - Twenty Thirteen theme" href="http://wordpress.org/themes/twentythirteen" target="_blank">Twenty Thirteen</a> theme for WordPress. I&#8217;ve been learning WordPress basics in the last couple of months, but I am no expert to create a custom theme, so I will stick to default ones for the moment. Up until today I have been using the <a title="Wordpress - Twenty Twelve theme" href="http://wordpress.org/themes/twentytwelve" target="_blank">Twenty Twelve</a> theme &#8211; this is fully a responsive theme and with the right plugins you get a pretty good functionality, at least for a technical blog, &#8230;but I find it a little dull. So I said why not try some more colour? Well, the <a title="Wordpress - Twenty Thirteen theme" href="http://wordpress.org/themes/twentythirteen" target="_blank">Twenty Thirteen</a> theme is certainly an answer for that. I gave it a try on my localhost, I liked it, so I decided to give it a try in production. This is so cool about WordPress, you can change the look of your pages pretty fast, as long as you haven&#8217;t invested much too effort in customizing one theme. <!--more-->If by the time of your reading, Codingpedia.org does not use the Twenty Thirteen anymore this is how it looked like :
</p>

<div id="attachment_918" style="width: 614px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/Preview-2013.png"><img class="size-large wp-image-918" alt="Preview Twenty Thirteen Theme" src="{{site.url}}/wp-content/uploads/2013/11/Preview-2013-1024x616.png" width="604" height="363" srcset="{{site.url}}/wp-content/uploads/2013/11/Preview-2013-1024x616.png 1024w, {{site.url}}/wp-content/uploads/2013/11/Preview-2013-300x180.png 300w, {{site.url}}/wp-content/uploads/2013/11/Preview-2013.png 1439w" sizes="(max-width: 604px) 100vw, 604px" /></a>

  <p class="wp-caption-text">
    Preview Twenty Thirteen Theme
  </p>
</div>

## Create a Child Theme

<p style="text-align: justify;">
  Well, because I need to modify something on a standard theme, it&#8217;s best practice to use a child theme. Otherwise if I made an update on theme, all my changes would be lost. A WordPress child theme is a theme that inherits the functionality of another theme, called the parent theme. Child theme allows you to modify, or add to the functionality of that parent theme. A child theme is the safest and easiest way to modify an existing theme, whether you want to make a few tiny changes or extensive changes. Instead of modifying the theme files directly, you can create a child theme and override within.
</p>

<p style="padding-left: 30px;">
  <em><strong>Note:</strong> Please follow <a title="Child Themes on WordPress " href="http://codex.wordpress.org/Child_Themes" target="_blank">Child Themes on WordPress</a> to find out what&#8217;s the exact process to create a child theme.</em>
</p>

<p style="text-align: justify;">
  To add the logo to the website I had to create the <code>twentythirteen-child</code> directory under <code>WP_HOME/wp-content/themes</code>, where I only store the <code>header.php</code> and <code>style.css</code> files.
</p>

### header.php

<p style="text-align: justify;">
  If you want to modify anything to the site header, the first place you would want to look in the <code>header.php</code> file. After doing a HTML inspection I came to the conclusion that, indeed, this is the place where I have to add the logo image. For html and css inspection I use <a title="Firebug" href="https://getfirebug.com/" target="_blank">Firebug </a>:
</p>

<div id="attachment_920" style="width: 596px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/11/Firebug-inspection.png"><img class=" wp-image-920  " alt="Firebug HTML inspector" src="{{site.url}}/wp-content/uploads/2013/11/Firebug-inspection.png" width="586" height="287" srcset="{{site.url}}/wp-content/uploads/2013/11/Firebug-inspection.png 837w, {{site.url}}/wp-content/uploads/2013/11/Firebug-inspection-300x146.png 300w" sizes="(max-width: 586px) 100vw, 586px" /></a>

  <p class="wp-caption-text">
    Firebug HTML inspector
  </p>
</div>

#### Before:

<pre class="lang:php mark:4,5 decode:true" title="Snippet original header.php Twenty Thirteen">..........
&lt;header id="masthead" class="site-header" role="banner"&gt;
	&lt;a class="home-link" href="&lt;?php echo esc_url( home_url( '/' ) ); ?&gt;" title="&lt;?php echo esc_attr( get_bloginfo( 'name', 'display' ) ); ?&gt;" rel="home"&gt;
		&lt;h1 class="site-title"&gt;&lt;?php bloginfo( 'name' ); ?&gt;&lt;/h1&gt;
		&lt;h2 class="site-description"&gt;&lt;?php bloginfo( 'description' ); ?&gt;&lt;/h2&gt;
	&lt;/a&gt;

	&lt;div id="navbar" class="navbar"&gt;
		&lt;nav id="site-navigation" class="navigation main-navigation" role="navigation"&gt;
			&lt;h3 class="menu-toggle"&gt;&lt;?php _e( 'Menu', 'twentythirteen' ); ?&gt;&lt;/h3&gt;
			&lt;a class="screen-reader-text skip-link" href="#content" title="&lt;?php esc_attr_e( 'Skip to content', 'twentythirteen' ); ?&gt;"&gt;&lt;?php _e( 'Skip to content', 'twentythirteen' ); ?&gt;&lt;/a&gt;
			&lt;?php wp_nav_menu( array( 'theme_location' =&gt; 'primary', 'menu_class' =&gt; 'nav-menu' ) ); ?&gt;
			&lt;?php get_search_form(); ?&gt;
		&lt;/nav&gt;&lt;!-- #site-navigation --&gt;
	&lt;/div&gt;&lt;!-- #navbar --&gt;
&lt;/header&gt;&lt;!-- #masthead --&gt;
..........</pre>

#### After:

<pre class="lang:php mark:4,5,6,10 decode:true" title="Code snippet modified header.php file">..........
&lt;header id="masthead" class="site-header" role="banner"&gt;
	&lt;a class="home-link" href="&lt;?php echo esc_url( home_url( '/' ) ); ?&gt;" title="&lt;?php echo esc_attr( get_bloginfo( 'name', 'display' ) ); ?&gt;" rel="home"&gt;
		&lt;div id="logo_and_title"&gt;
				&lt;img class="header-logo" src="{{site.url}}/wp-content/uploads/2013/11/logo.png"/&gt;
				&lt;div id="title_subtitle"&gt;
					&lt;h1 class="site-title"&gt;&lt;?php bloginfo( 'name' ); ?&gt;&lt;/h1&gt;
					&lt;h2 class="site-description"&gt;&lt;?php bloginfo( 'description' ); ?&gt;&lt;/h2&gt;
				&lt;/div&gt;
				&lt;div class="clear"&gt;&lt;/div&gt;
		&lt;/div&gt;
	&lt;/a&gt;
	&lt;div id="navbar" class="navbar"&gt;
		&lt;nav id="site-navigation" class="navigation main-navigation" role="navigation"&gt;
			&lt;h3 class="menu-toggle"&gt;&lt;?php _e( 'Menu', 'twentythirteen' ); ?&gt;&lt;/h3&gt;
			&lt;a class="screen-reader-text skip-link" href="#content" title="&lt;?php esc_attr_e( 'Skip to content', 'twentythirteen' ); ?&gt;"&gt;&lt;?php _e( 'Skip to content', 'twentythirteen' ); ?&gt;&lt;/a&gt;
			&lt;?php wp_nav_menu( array( 'theme_location' =&gt; 'primary', 'menu_class' =&gt; 'nav-menu' ) ); ?&gt;
			&lt;?php get_search_form(); ?&gt;
		&lt;/nav&gt;&lt;!-- #site-navigation --&gt;
	&lt;/div&gt;&lt;!-- #navbar --&gt;
&lt;/header&gt;&lt;!-- #masthead --&gt;
..........</pre>

Note that the only difference to the original file is adding the image (`line 5`) and some extra `div` elements to get better control with the style sheet file.

### style.css

Talking about style sheet files, create one &#8211; `style.css` &#8211; in the child theme directory (`twentythirteen-child`) :

<pre class="lang:css mark:2,7,11,41-47 decode:true" title="style.css - child style sheet file">/*
 Theme Name:     Twenty Thirteen Child
 Theme URI:      http://example.com/twenty-thirteen-child/
 Description:    Twenty Thirteen Child Theme
 Author:         John Doe
 Author URI:     http://example.com
 Template:       twentythirteen
 Version:        1.0.0
*/

@import url("../twentythirteen/style.css");

.entry-content code, .comment-content code {
    font-family: Consolas,Monaco,Lucida Console,monospace;
    font-size: 0.857143rem;
    line-height: 2;
    background-color:#E8E8E8;
    padding: 0px 3px;
}
#social_logos {margin-top:-15px;}
.social_logo { float: left; margin-right: 15px; height: 34px; width: 34px;}
.clear {clear:both;margin:0;padding:0;}
#author_portrait {box-shadow:0 0 0 rgba(0, 0, 0, 0.2);}
.amazon_book {float:left; padding:0 10px;}

.header-logo {float:left; margin-top:50px;}
#title_subtitle {float:left;}
.clear {clear:both;margin:0;padding:0;}
.entry-thumbnail img {
    border-radius: 3px;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.2)
}
.entry-title {font-size:34px;}
h2 {font-size:25px;margin:20px 0}
p {margin:0 0 17px}
.sidebar .entry-header, .sidebar .entry-content, .sidebar .entry-summary, .sidebar .entry-meta {
    max-width: 1140px;
    padding: 0 376px 0 60px;
}
@media (max-width: 720px) {
.header-logo {height:90px; width:90px;}
.site-title {font-size:30px;}
}
@media (max-width: 420px) {
.header-logo {display:none}
}</pre>

<p style="text-align: justify;">
  Note that the style sheet <strong>must</strong> start with the comment lines (1-9). You can change each of these lines to suit your theme. The only required lines are the <code>Theme Name</code> (line 2), and the <code>Template</code> (line 7). The Template is the directory name of the parent theme. In this case, the parent theme is the TwentyThirteen theme, so the Template is <code>twentythirteen</code>, which is the name of the directory where the TwentyThirteen theme resides. If you want to make a child of a theme with the directory name <code>example-theme-name</code>, then you would use <code>Template: example-theme-name</code>.
</p>

<p style="padding-left: 30px;">
  <em><strong>Note</strong> : the child theme’s stylesheet is included after the parent theme&#8217;s and styles will therefore override those in the parent theme’s stylesheet (line 11). </em>
</p>

Now to the **logo adding** part, I floated both the image (line 27) and the `title_subtitle` div (line 28) to the left, and after that I cleared the floating (line 29).

<p style="text-align: justify;">
  To keep the responsivness of the theme I used media queries (lines 41-47). This makes the image and texts smaller when the size shrinks (I based the sizing on the original <code>style.css</code> file, where <code>max-width</code> is a little bit smaller, but unless I set it bigger, it would have moved the title under the logo for some widths). You can see this in action by resizing the browser window of this post.
</p>

<p style="text-align: justify;">
  Well, that&#8217;s it. Easy, wasn&#8217;t it? I have to thank the WordPress guys for that and for creating this amazing product &#8211; keep up the good work guys!!!
</p>

<p style="text-align: justify;">
  If you liked this, please show your support by following us and <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">joining us</a> on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org. </a>We promise to only share high quality podcasts and episodes.
</p>

## Resources

  1. <a title="WordPress - Child Themes" href="http://codex.wordpress.org/Child_Themes" target="_blank">WordPress &#8211; Child Themes</a>

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
