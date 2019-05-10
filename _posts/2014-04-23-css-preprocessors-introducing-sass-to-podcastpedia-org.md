---
id: 1324
title: 'CSS Preprocessors &#8211; Introducing Sass to Podcastpedia.org'
date: 2014-04-23T12:09:23+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1324
permalink: /ama/css-preprocessors-introducing-sass-to-podcastpedia-org/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:55:"https://github.com/podcastpedia/css-generator-sass-gulp";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 2632817389
fsb_social_facebook:
  - 4
fsb_social_google:
  - 6
fsb_social_linkedin:
  - 3
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
  - less
  - preprocessor
  - sass
  - web design
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_CSS_Preprocessors">1. CSS Preprocessors</a><ul>
        <li>
          <a href="#11_Sass">1.1. Sass</a><ul>
            <li>
              <a href="#12_SCSS">1.2. SCSS</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Advantages_of_using_preprocessors_Sass">2. Advantages of using preprocessors (Sass)</a><ul>
        <li>
          <a href="#21_Partials_8211_break_the_style_sheet_into_meaningful_separate_sheets">2.1. Partials &#8211; break the style sheet into meaningful separate sheets</a>
        </li>
        <li>
          <a href="#22_Variables">2.2. Variables</a>
        </li>
        <li>
          <a href="#23_Nesting">2.3. Nesting</a>
        </li>
        <li>
          <a href="#24ExtendInheritance">2.4. Extend/Inheritance</a>
        </li>
        <li>
          <a href="#25_Mixins">2.5. Mixins</a>
        </li>
        <li>
          <a href="#26_Operators">2.6. Operators</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Generate_the_CSS">3. Generate the CSS</a>
    </li>
    <li>
      <a href="#4_Resources">4. Resources</a><ul>
        <li>
          <a href="#41_Web">4.1. Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

<p style="text-align: justify;">
  The Cascade Style Sheet (CSS) file of <a title="Podcastpedia.org, knwoledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> had grown to over 2000 lines and it had become hard to manage. New CSS elements used to, most likely, go to the end of the file. Something had to be done&#8230; Well, CSS does have an import option that lets you split your CSS into smaller, more maintainable portions. The only MAJOR drawback is that each time you use <code>@import</code> in CSS it creates another HTTP request. In addition to that, this could have prevented style sheets from being downloaded concurrently. So, what to do? <strong>Ta-da, CSS preprocessors to the rescue.</strong>
</p>

<p style="text-align: justify;">
  <p class="note_normal">
    <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
  </p>

  <!--more-->
</p>

<h2 style="text-align: justify;">
  <span id="1_CSS_Preprocessors">1. CSS Preprocessors</span>
</h2>

<p style="text-align: justify;">
  CSS preprocessors take code written in a preprocessed language (most popular now are Less and Sass) and then convert that code into the same old CSS we&#8217;ve been writing for years.
</p>

Some Sass vs. LESS resources

  * <a title="Sass vs. LESS" href="http://css-tricks.com/sass-vs-less/" target="_blank">Sass vs. LESS</a>
  * <a title="https://gist.github.com/chriseppstein/674726" href="https://gist.github.com/chriseppstein/674726" target="_blank">Sass/Less Comparison</a>

### <span id="11_Sass"><span style="font-family: Bitter, Georgia, serif; font-size: 22px; line-height: 1.3;">1.1. Sass</span></span>

<p style="text-align: justify;">
  For now, I chose <a title="Official website for Sass" href="http://sass-lang.com/" target="_blank"><b>Sass</b> (<b>S</b>yntactically <b>A</b>wesome <b>S</b>tyle<b>s</b>heets)</a>, also called &#8220;CSS with super powers&#8221;. Sass is a scripting language that is interpreted into Cascading Style Sheets (CSS). SassScript is the scripting language itself.
</p>

#### <span id="12_SCSS">1.2. SCSS</span>

<p style="text-align: justify;">
  Now that I&#8217;ve decided to go with Sass, there are two different syntaxes to choose from: Sass or Sassy CSS(SCSS). I chose SCSS because it&#8217;s more alike to CSS, and thus provides a more lower barrier to entry (remember I am still earning my living as a back-end developer ):
</p>

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;In <a href="http://sass-lang.com/docs/yardoc/file.SASS_CHANGELOG.html#3-0-0" target="_blank">version 3 of Sass</a>, the SCSS (Sassy CSS) syntax was introduced as &#8220;the new main syntax&#8221; for Sass and builds on the existing syntax of CSS. It uses brackets and semi-colons just like CSS. It doesn&#8217;t care about indentation levels or white-space. In fact, Sass&#8217;s SCSS syntax is a <a href="http://encyclopedia2.thefreedictionary.com/superset" target="_blank">superset</a> of CSS – which means SCSS contains all the features of CSS, but has been expanded to include the features of Sass as well. In layman&#8217;s terms, any valid CSS is valid SCSS. And in the end, SCSS has the exact same features as the Sass syntax, minus the opinionated syntax.&#8221; [3]</em>
</p>

## <span id="2_Advantages_of_using_preprocessors_Sass">2. Advantages of using preprocessors (Sass)</span>

I will present in the following sections some features of Sass, and how they made my css-writing-life easier.

### <span id="21_Partials_8211_break_the_style_sheet_into_meaningful_separate_sheets">2.1. Partials &#8211; break the style sheet into meaningful separate sheets</span>

<p style="text-align: justify;">
  You can create partial Sass files that contain little snippets of CSS that you can include in other Sass files. This is a great way to modularize your CSS and help keep things easier to maintain. A partial is simply a Sass file named with a leading underscore . You might name it something like <em>_partial.scss</em>. The underscore lets Sass know that the file is only a partial file and that it should not be generated into a CSS file[3]. Sass builds on top of the current CSS <code>@import</code> but instead of requiring an HTTP request, Sass will take the file that you want to import and combine it with the file you&#8217;re importing into so you can serve a single CSS file to the web browser.
</p>

There are many &#8220;best practices&#8221; on how to structure a Sass project with partials. If you analyze the main style sheet file _podcastpedia.scss_, that combines all the partials:

<pre class="lang:default decode:true" title="podcastpedia.scss - ">//first import variables that might be used throughout all the other files
@import "common/_variables";
@import "common/_mixins";

//initializing css
@import "_init";

//elements
@import "common/_icon_fonts";
@import "common/_languageSelection";
@import "common/_responsiveGrid";
@import "common/_navigation";
@import "common/_header";
@import "common/_pagination";
@import "common/_resultsList";
@import "common/_forms";
@import "common/_footer";
@import "common/_social_media_share";
@import "common/_button_links";

//pages
@import "pages/_podcastDetails";
@import "pages/_episodeDetails";
@import "pages/_allCategoriesPage";
@import "pages/_allKeywordsPage";
@import "pages/_homePage";
@import "pages/_advancedSearchForm";

// what's left from the original css that I could not put in someplace reasonable
@import "_leftovers";</pre>

you can see what suits me for now:

  * _common_ &#8211; folder that stores css snippets that might be present all over the website; \_variables and \_mixins are placed at the beginning, as they might be used all over
  * _pages_ &#8211; folder in which every file contains page-specific scss code
  * __init.scss_ &#8211; contains css property initialization
  * __leftovers.scss_ &#8211; what&#8217;s left from the original css that I could not place in a someplace reasonable. I could also use it if I need to make a quick fix.

<p class="alert_note">
  In the <code>@import</code> directive you don&#8217;t have to specify <em>.scss</em> extension of the imported files.
</p>

### <span id="22_Variables">2.2. Variables</span>

<p style="text-align: justify;">
  Variables are a way to store information that you want to reuse throughout your style sheet. You can store things like colors, breakpoints, font stacks, or any CSS value you think you&#8217;ll want to reuse. Sass uses the <code>$</code> symbol to make something a variable. Here are some examples from the <em>_variables.scss</em> file:
</p>

<pre class="lang:sass mark:3,12,15,18,23 decode:true" title="Variables samples">$border-radius-base: 5px !default;
#podcast_metadata {
  border-radius: $border-radius-base;
  padding: 8px 8px 3px 8px;
  position: relative;
  h2 {
    font-size: 2em;
    text-transform: normal;
    font-weight: 700;
    margin-bottom: 15px;
    padding-top: 8px;
    @include until-mq($break-point-normal){
      font-size:1.9em;
    }
    @include until-mq($break-point-smaller){
      font-size:1.6em;
    }
    @include until-mq($break-point-small){
      font-size: 1.3em;
      margin-bottom:0px;
      padding-top:0px;
    }
    @include until-mq($break-point-very-small){
      font-size: 14px;
		}
  }
  ...............
}
</pre>

<p class="alert_code_tip">
  You can find the complete file where the snippet is extracted from, on Github &#8211; <em>_podcastDetails.scss</em>.
</p>

<p class="alert_note" style="text-align: justify;">
  With variable defaults &#8211; <code>!default</code> &#8211; you can assign to variables if they aren’t already assigned by adding the <code>!default</code> flag to the end of the value. This means that if the variable has already been assigned to, it won’t be re-assigned, but if it doesn’t have a value yet, it will be given one.
</p>

### <span id="23_Nesting">2.3. Nesting</span>

<p style="text-align: justify;">
  The nested syntax allows a cleaner method of targeting DOM elements. It prevents the need to rewrite selectors multiple times, additionally reducing the time it takes to make changes to style sheets. The inner rule then only applies within the outer rule’s selector. For example:
</p>

<pre class="lang:sass mark:6,7,27,30 decode:true" title="Nesting example - navigation">#nav {
  height: 38px;
  overflow: hidden;
  zoom: 1;
  margin: 0 auto;
  max-width: $page-max-width;
  min-width: $page-min-width;
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    li {
      float: left;
      width: 12em;
    }
  }
  li a {
    display: block;
    border-bottom: none;
    margin-right: 5px;
    color: #fff;
    text-align: center;
    text-decoration: none;
    font-size: 13px;
    height: 38px;
    background-color: #343435;
    border-radius: $border-radius-base;
    text-shadow: 4px 4px 3px #2C2D2E;
    padding: 5px;
    &:hover {
      background-color: #185B8B;
      color: #fff;
    }
  }
}</pre>

is compiled to :

<pre class="lang:css decode:true" title="Compiled CSS from nesting example">#nav {
  height: 38px;
  overflow: hidden;
  zoom: 1;
  margin: 0 auto;
  max-width: 1060px;
  min-width: 300px;
}
#nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
#nav ul li {
  float: left;
  width: 12em;
}
#nav li a {
  display: block;
  border-bottom: none;
  margin-right: 5px;
  color: #fff;
  text-align: center;
  text-decoration: none;
  font-size: 13px;
  height: 38px;
  background-color: #343435;
  border-radius: 5px;
  text-shadow: 4px 4px 3px #2C2D2E;
  padding: 5px;
}
#nav li a:hover {
  background-color: #185B8B;
  color: #fff;
}</pre>

<p class="alert_code_tip">
  You can find the complete file from the example on GitHub &#8211; <em>_navigation.scss</em>.
</p>

<p class="alert_note" style="text-align: justify;">
  With the help of the <code>ampersand(&)</code> you can reference the parent selector. In the example above, line 30, the ampersand is used to modify the layout of the menu items when hovering over them.
</p>

### <span id="24ExtendInheritance">2.4. Extend/Inheritance</span>

<p style="text-align: justify;">
  This should one of the most useful features of Sass, which I have not used yet :). Using <code>@extend</code> lets you share a set of CSS properties from one selector to another. It helps keep your Sass very DRY.
</p>

<p class="alert_note" style="text-align: justify;">
  At the time of this writing the gulp-sass plugin does not support properly this feature, but the <a title="https://github.com/sindresorhus/gulp-ruby-sass" href="https://github.com/sindresorhus/gulp-ruby-sass" target="_blank">gulp-ruby-sass</a> should.
</p>

### <span id="25_Mixins">2.5. Mixins</span>

<p style="text-align: justify;">
  A mixin lets you make groups of CSS declarations that you want to reuse throughout the website. You can pass values to make the mixin more flexible. Here is an example of using mixin for media queries breakpoints:
</p>

<pre class="lang:sass mark:2 decode:true" title="Mixin - media query naming">/* media query mixin */
@mixin until-mq($device-width) {
  @media screen and (max-width: $device-width - 1) {
    @content
  }
}</pre>

<p style="text-align: justify;">
  You use the <code>@mixin</code> directive to created a mixin and give it a name. The one presented here is named <code>until-mq</code>. As input it receives the <code>$device-width</code> parameter inside the parentheses, so that  I can pass the width of the device, in pixels, until which the media query rule is valid. I use it all over the website where I need responsive design. Based on the same example used in sections <em>2.2. Variables,</em> the mixin is used when setting the title of the podcast depending on the display&#8217;s width:
</p>

<pre class="lang:sass mark:11,14,17,22 decode:true" title="Mixin inclusion in sass code">#podcast_metadata {
  border-radius: $border-radius-base;
  padding: 8px 8px 3px 8px;
  position: relative;
  h2 {
    font-size: 2em;
    text-transform: normal;
    font-weight: 700;
    margin-bottom: 15px;
    padding-top: 8px;
    @include until-mq($break-point-normal){
      font-size:1.9em;
    }
    @include until-mq($break-point-smaller){
      font-size:1.6em;
    }
    @include until-mq($break-point-small){
      font-size: 1.3em;
      margin-bottom:0px;
      padding-top:0px;
    }
    @include until-mq($break-point-very-small){
      font-size: 14px;
	}
  }
  .......
}</pre>

<p style="text-align: justify;">
  So after the mixin has been created, you can use it then as a CSS declaration starting with <code>@include</code> followed by the name of the mixin.
</p>

<p class="alert_note" style="text-align: justify;">
  <span style="text-align: justify; line-height: 1.5;">To avoid code duplication you should only use mixins if you need to pass custom parameters. If you see yourself using the same mixing multiple times passing the same values than you should create a base “type” that is inherited by other selectors. A good article on this topic is  </span><a style="text-align: justify; line-height: 1.5;" title="http://www.zacharybrady.com/mixins-and-selector-inheritance-in-sass-when-should-you-use-one-or-the-other/" href="http://www.zacharybrady.com/mixins-and-selector-inheritance-in-sass-when-should-you-use-one-or-the-other/" target="_blank">Mixins and Selector Inheritance in SASS: When Should You Use One Or the Other?</a>
</p>

<p class="alert_note" style="text-align: justify;">
  I started working with <code>@media</code> queries by adding blocks of them at the bottom of your main style sheet. That works, but it leads to mental disconnect between the original styling and the responsive styles.
</p>

<p class="alert_note" style="text-align: justify;">
  A good use of mixins is for vendor prefixes. But you can avoid all this tedious writing by using the <a title="https://www.npmjs.org/package/gulp-autoprefixer" href="https://www.npmjs.org/package/gulp-autoprefixer" target="_blank">gulp-autoprefixer plugin</a> to do that for you. Check the following post <post name> on how to do that.
</p>

### <span id="26_Operators">2.6. Operators</span>

<p style="text-align: justify;">
  Doing math in your CSS is very helpful. Sass has a handful of standard math operators like +, -, *, /, and %.
</p>

<p style="text-align: justify;">
  There might be some &#8220;disadvantages&#8221; in using preprocessors, like the ones mentioned in <a title="http://blog.millermedeiros.com/the-problem-with-css-pre-processors/" href="http://blog.millermedeiros.com/the-problem-with-css-pre-processors/" target="_blank">The problem with CSS pre-processors</a>, but I for one, wouldn&#8217;t go back writing plain CSS again.
</p>

<p style="text-align: justify;">
  Well, those are some of the &#8220;sassy&#8221; features I&#8217;ve used so far to style <a title="Podcastpedia, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia</a>, but I am sure there are other goodies left to be discovered and put to use (e.g. <a title="http://sass-lang.com/documentation/file.SASS_CHANGELOG.html#sassbased_functions" href="http://sass-lang.com/documentation/file.SASS_CHANGELOG.html#sassbased_functions" target="_blank">sass-based functions &#8230;</a>) &#8211; maybe you could give me a helping hand and leave a comment regarding this.
</p>

<h2 style="text-align: justify;">
  <span id="3_Generate_the_CSS">3. Generate the CSS</span>
</h2>

<p style="text-align: justify;">
  Now that I have everything set in place, it&#8217;s time to generate the CSS file. Initially I wanted to present how I did that in this one post, but since the it got pretty long and the topics can be discussed separately I&#8217;ve decided to move it to a new post. So in the post &#8220;How to use Gulp to generate CSS from Sass(scss)&#8221; (coming next week) you&#8217;ll find out how o generate the CSS file used for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org.</a>
</p>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="4_Resources">4. Resources</span>

### <span id="41_Web">4.1. Web</span>

  1. <a title="http://en.wikipedia.org/wiki/Cascading_Style_Sheets" href="http://en.wikipedia.org/wiki/Cascading_Style_Sheets" target="_blank">Cascading Style Sheets &#8211; Wikipedia</a>
  2. <a title="http://sass-lang.com/" href="http://sass-lang.com/" target="_blank">Sass: Syntactically Awesome Style Sheets</a>
  3. <a title="http://sass-lang.com/guide" href="http://sass-lang.com/guide" target="_blank">Sass Guide </a>
  4. <a title="http://lesscss.org/" href="http://lesscss.org/" target="_blank">LESS (Leaner CSS)</a>
  5. <a title="http://www.zacharybrady.com/mixins-and-selector-inheritance-in-sass-when-should-you-use-one-or-the-other/" href="http://www.zacharybrady.com/mixins-and-selector-inheritance-in-sass-when-should-you-use-one-or-the-other/" target="_blank">Mixins and Selector Inheritance in SASS: When Should You Use One Or the Other?</a>
  6. <a title="http://css-tricks.com/sass-style-guide/" href="http://css-tricks.com/sass-style-guide/" target="_blank">CSS tricks &#8211; Sass Style Guide</a>
  7. <a title="http://css-tricks.com/naming-media-queries/" href="http://css-tricks.com/naming-media-queries/" target="_blank">CSS tricks &#8211; Naming Media Queries</a>

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
