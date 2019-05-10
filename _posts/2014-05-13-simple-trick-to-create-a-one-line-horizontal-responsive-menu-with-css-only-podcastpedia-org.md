---
id: 1402
title: 'Simple trick to create a one line horizontal responsive menu with CSS only &#8211; Podcastpedia.org'
date: 2014-05-13T22:14:02+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1402
permalink: /ama/simple-trick-to-create-a-one-line-horizontal-responsive-menu-with-css-only-podcastpedia-org/
fsb_show_social:
  - 0
dsq_thread_id:
  - 2682832643
fsb_social_facebook:
  - 3
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - Podcastpedia.org
  - web design
tags:
  - css
  - css3
  - podcastpedia
  - responsive
  - responsive design
  - sass
  - scss
  - web design
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_The_trick">1. The trick</a><ul>
        <li>
          <a href="#11_Initial_design_of_the_menu">1.1. Initial design of the menu</a>
        </li>
        <li>
          <a href="#12_Requirements">1.2. Requirements</a>
        </li>
        <li>
          <a href="#13_Result">1.3. Result</a>
        </li>
        <li>
          <a href="#14_The_magic">1.4. The magic</a><ul>
            <li>
              <a href="#141_HTMLJSP_Code">1.4.1. HTML/JSP Code</a>
            </li>
            <li>
              <a href="#142_SCSSCSS_code">1.4.2. SCSS/CSS code</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Resources">2. Resources</a><ul>
        <li>
          <a href="#21_Web">2.1. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>



In this post I will present a trick I used to create the responsive menu on <a title="https://github.com/Codingpedia/podcastpedia/" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a> :

<div id="attachment_1408" style="width: 643px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/05/Responsive-menu-support.png"><img class="wp-image-1408 size-full" src="{{site.url}}/wp-content/uploads/2014/05/Responsive-menu-support.png" alt="Responsive menu on Podcastpedia.org" width="633" height="372" srcset="{{site.url}}/wp-content/uploads/2014/05/Responsive-menu-support.png 633w, {{site.url}}/wp-content/uploads/2014/05/Responsive-menu-support-300x176.png 300w" sizes="(max-width: 633px) 100vw, 633px" /></a>

  <p class="wp-caption-text">
    Responsive menu on Podcastpedia.org
  </p>
</div>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

<!--more-->

<p style="text-align: justify;">
  There are lots of resources out there on how to design horizontal menus with sub-menus, so I won&#8217;t insist on that. My favorite so far is <em>How To Create a Pure CSS Dropdown Menu</em>[1] by <a title="https://twitter.com/chrisspooner" href="https://twitter.com/chrisspooner" target="_blank">@chrisspooner</a>
</p>

## <span id="1_The_trick">1. The trick</span>

<h3 style="text-align: justify;">
  <span id="11_Initial_design_of_the_menu">1.1. Initial design of the menu</span>
</h3>

<p style="text-align: justify;">
  Initially the items in the menu used to disappear if the size of screen got smaller, because of CSS <code>overflow</code> property that was set to <code>hidden</code> for the elements in list of the menu. I considered that a satisfying &#8220;responsive&#8221; solution for the beginning, but later it crossed my mind not to deprive the visitors with small screen resolutions from all the wonderful links you can access directly on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> via the main menu&#8230;
</p>

<h3 style="text-align: justify;">
  <span id="12_Requirements">1.2. Requirements</span>
</h3>

<p style="text-align: justify;">
  So a better solution was need and it had to satisfy the following requirements:
</p>

<li style="text-align: justify;">
  <strong> all</strong> the links from the menu should be easily accesible even on small screens
</li>
<li style="text-align: justify;">
  showing and hidding of menu elements should happen gradually based on the screen width and not the Bootstrap classic approach, where once a certain width is reached everything is reduced to a Menu button
</li>
<li style="text-align: justify;">
  and of course it should look nice
</li>

### <span id="13_Result">1.3. Result</span>

<p style="text-align: justify;">
  You can see what came out in the initial picture or by visiting the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org </a>website. If by the time of your reading I will have already changed the design of the menu, you can have a look at this recorded video :
</p>



### <span id="14_The_magic">1.4. The magic</span>

#### <span id="141_HTMLJSP_Code">1.4.1. HTML/JSP Code</span>

Well, the trick I used was to have the links from the menu inserted twice in the html code but with differend `ids` for the list elements containing them:

<pre class="lang:xhtml mark:25-49 decode:true" title="Navigation HTML/JSP code">&lt;div id="nav"&gt;
	&lt;ul&gt;
		&lt;li id="nav-homepage"&gt;
			&lt;a href="/"&gt;Home&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-tags"&gt;
			&lt;a href="/tags/all/0"&gt;Keywords&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-categories"&gt;
			&lt;a href="/categories"&gt;Categories&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-add-podcast"&gt;
			&lt;a href="/how_can_i_help/add_podcast"&gt;Add Podcast&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-support"&gt;

			&lt;a href="/how_can_i_help"&gt;Support&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-contact"&gt;
			&lt;a href="/contact"&gt;Contact&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-podcasting"&gt;
			&lt;a href="/podcasting"&gt;Podcasting&lt;/a&gt;
		&lt;/li&gt;
		&lt;li id="nav-responsive"&gt;
			&lt;a href="#"&gt;&lt;/a&gt;
			&lt;ul&gt;
				&lt;li id="nav-tags-resp"&gt;
					&lt;a href="/tags/all/0"&gt;Keywords&lt;/a&gt;
				&lt;/li&gt;
				&lt;li id="nav-categories-resp"&gt;
					&lt;a href="/categories"&gt;Categories&lt;/a&gt;
				&lt;/li&gt;
				&lt;li id="nav-add-podcast-resp"&gt;
					&lt;a href="/how_can_i_help/add_podcast"&gt;Add Podcast&lt;/a&gt;
				&lt;/li&gt;
				&lt;li id="nav-support-resp"&gt;

					&lt;a href="/how_can_i_help"&gt;Support&lt;/a&gt;
				&lt;/li&gt;
				&lt;li id="nav-contact-resp"&gt;
					&lt;a href="/contact"&gt;Contact&lt;/a&gt;
				&lt;/li&gt;
				&lt;li id="nav-podcasting-resp"&gt;
					&lt;a href="/podcasting"&gt;Podcasting&lt;/a&gt;
				&lt;/li&gt;
			&lt;/ul&gt;
		&lt;/li&gt;
	&lt;/ul&gt;
&lt;/div&gt;</pre>

#### <span id="142_SCSSCSS_code">1.4.2. SCSS/CSS code</span>

Now that the menu item are present twice, I can show and hide them based on the width of the display with media queries:

<pre class="lang:sass decode:true" title="Media queries to hide and show elements">/** media queries to hide and show menu items, based on the display width */
@media screen and (min-width: 1040px) {
  #nav-responsive {
    display: none;
  }
}

@media screen and (max-width: 1039px) {
  #nav-podcasting-resp, #nav-contact-resp {
    display: block;
  }
  #nav-podcasting, #nav-contact {
    display: none;
  }
}

@media screen and (max-width: 900px) {
  #nav-support-resp {
    display: block;
  }
  #nav-support {
    display: none;
  }
}

@media screen and (max-width: 750px) {
  #nav-add-podcast-resp {
    display: block;
  }
  #nav-add-podcast {
    display: none;
  }
}

@media screen and (max-width: 610px) {
  #nav-categories-resp {
    display: block;
  }
  #nav-categories {
    display: none;
  }
}

@media screen and (max-width: 465px) {
  #nav-tags-resp {
    display: block;
  }
  #nav-tags {
    display: none;
  }
}</pre>

<p style="text-align: justify;">
  As you can see in the code for big screens (width over 1040px) the elements marked with <code>resp(onsive)</code> are not shown at all, but once the width gets smaller, elements from the menu are hidden and corresponding elements from the second <code>resp(onsive)</code> menu list. These are now shown when you hover (tip on mobile) over the standard menu icon <img class="alignnone size-full wp-image-1412" src="{{site.url}}/wp-content/uploads/2014/05/standard-menu-icon.png" alt="standard menu icon" width="33" height="30" />
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="2_Resources">2. Resources</span>

### <span id="21_Web">2.1. Web</span>

  1. <a title="http://line25.com/tutorials/how-to-create-a-pure-css-dropdown-menu" href="http://line25.com/tutorials/how-to-create-a-pure-css-dropdown-menu" target="_blank">How To Create a Pure CSS Dropdown Menu</a>
  2. <a title="http://www.w3schools.com/cssref/pr_class_display.asp" href="http://www.w3schools.com/cssref/pr_class_display.asp" target="_blank">CSS display Property</a>

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
