---
id: 2253
title: Is IntelliJ IDEA shining through Eclipse?
date: 2015-03-26T14:16:47+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2253
permalink: /ama/is-intellij-idea-shining-through-eclipse/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 7
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 4
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3628245122
categories:
  - development
tags:
  - comparison
  - eclipse
  - ide
  - intellij
---
<p style="text-align: justify;">
  As a long time Eclipse user, I want to give a more serious look to <a title="https://www.jetbrains.com/idea/" href="https://www.jetbrains.com/idea/" target="_blank">IntelliJ IDEA</a>. The people from <a title="https://www.jetbrains.com/" href="https://www.jetbrains.com/" target="_blank">JetBrains</a> were very nice and granted me an open source license for the <a title="https://github.com/Codingpedia/podcastpedia" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>  and <a title="http://www.codingpedia.org" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a> projects. In the post I listed some of the things I use often in Eclipse and their equivalent in IntelliJ. I wrote this post so I can bookmark it and come back to, whenever I forget something, and if it helps others the better.
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#Keyboard_shortcuts">Keyboard shortcuts</a>
      </li>
      <li>
        <a href="#Link_to_editor">Link to editor</a><ul>
          <li>
            <a href="#Eclipse">Eclipse</a>
          </li>
          <li>
            <a href="#IntelliJ">IntelliJ</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#What_I_really_like_about_IntelliJ">What I really like about IntelliJ</a><ul>
          <li>
            <a href="#Lots_of_things_by_default">Lots of things by default</a>
          </li>
          <li>
            <a href="#Change_Font_size_using_the_mouse_wheel">Change Font size using the mouse wheel</a>
          </li>
          <li>
            <a href="#Launch_terminal_directly_in_the_IDE">Launch terminal directly in the IDE</a>
          </li>
          <li>
            <a href="#Find_action_by_name">Find action by name</a>
          </li>
          <li>
            <a href="#Coollive_templates">Cool live templates</a>
          </li>
          <li>
            <a href="#Great_JavaScriptHTML5_support">Great JavaScript/HTML5 support</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#What_I_miss_from_Eclipse">What I miss from Eclipse</a><ul>
          <li>
            <a href="#Cannot_maximize_console">Cannot maximize console</a>
          </li>
          <li>
            <a href="#Show_javadoc_when_hovering_with_the_mouse">Show javadoc when hovering with the mouse</a>
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
  <!--more-->
</p>

## <span id="Keyboard_shortcuts">Keyboard shortcuts</span>

So firt things first. Here&#8217;s a list of the shortcuts I use or ought to be using the most:

<table width="603">
  <tr>
    <td width="290">
      <strong>Description</strong>
    </td>

    <td width="156">
      <strong>Eclipse</strong>
    </td>

    <td width="157">
      <strong>IntelliJ</strong>
    </td>
  </tr>

  <tr>
    <td width="290">
       Code completion
    </td>

    <td width="156">
       Ctrl+Space
    </td>

    <td width="157">
       Ctrl+Space
    </td>
  </tr>

  <tr>
    <td width="290">
       Open class or interface<br /> (in both cases you can ease the search by filtering the lookup list with the help of the &#8220;camel words&#8221; prefixes <em>e.g. &#8220;PoDI&#8221; will also list the <strong>Po</strong>dcast<strong>D</strong>ao<strong>I</strong>mpl class</em>)
    </td>

    <td width="156">
       Ctrl+Shif +T
    </td>

    <td width="157">
       Ctrl+N
    </td>
  </tr>

  <tr>
    <td width="290">
       Open file/resource quickly
    </td>

    <td width="156">
       Ctrl+Shift+R
    </td>

    <td width="157">
       Ctrl+Shift +N
    </td>
  </tr>

  <tr>
    <td width="290">
       Refactor/Rename classes
    </td>

    <td width="156">
       Alt+Shift+R
    </td>

    <td width="157">
       Shift+F6
    </td>
  </tr>

  <tr>
    <td width="290">
       Open declaration
    </td>

    <td width="156">
       F3
    </td>

    <td width="157">
       Ctrl+B
    </td>
  </tr>

  <tr>
    <td width="290">
       View Javadoc / Details
    </td>

    <td width="156">
       Mouse Over (F2 focus)
    </td>

    <td width="157">
       Ctrl+Q
    </td>
  </tr>

  <tr>
    <td width="290">
       Quick fix
    </td>

    <td width="156">
       Ctrl+1
    </td>

    <td width="157">
       Alt+Enter
    </td>
  </tr>

  <tr>
    <td width="290">
       Organize imports
    </td>

    <td width="156">
       Ctrl+Shift+O
    </td>

    <td width="157">
      Ctrl+Alt+O
    </td>
  </tr>

  <tr>
    <td width="290">
       Save file / Save all files
    </td>

    <td width="156">
       Ctrl+S / Ctrl+Shift+S
    </td>

    <td width="157">
      Don&#8217;t need to. Happens automatically
    </td>
  </tr>

  <tr>
    <td width="290">
       File Structure pop-up for quick navigation through the current file (members, methods)
    </td>

    <td width="156">
       Ctrl+O
    </td>

    <td width="157">
      Ctrl+F12
    </td>
  </tr>

  <tr>
    <td width="290">
       Source (Generate getters/setters, constructor etc.)
    </td>

    <td width="156">
       Alt+Up /Alt+Down
    </td>

    <td width="157">
       Alt+Insert
    </td>
  </tr>

  <tr>
    <td width="290">
       Maximize Editor
    </td>

    <td width="156">
       Ctrl+M (works for any current view with focus on)
    </td>

    <td width="157">
       Ctrl+Shift+F12
    </td>
  </tr>

  <tr>
    <td width="290">
       Source (Generate getters/setters, constructor etc.)
    </td>

    <td width="156">
       Alt+Up /Alt+Down
    </td>

    <td width="157">
       Alt+Insert
    </td>
  </tr>

  <tr>
    <td width="290">
       Complete current statement such as if, do-while, try-catch, return (or a method call) into a syntactically correct construct (e.g. add curly braces)
    </td>

    <td width="156">
    </td>

    <td width="157">
       Ctrl+Shift+Enter
    </td>
  </tr>

  <tr>
    <td width="290">
       Extract constant
    </td>

    <td width="156">
       Ctrl+1 -> Extract to constant
    </td>

    <td width="157">
       Ctrl+Alt+C
    </td>
  </tr>

  <tr>
    <td width="290">
       Extract variable
    </td>

    <td width="156">
       Ctrl+1 -> Extract to variable
    </td>

    <td width="157">
       Ctrl+Alt+V
    </td>
  </tr>

  <tr>
    <td colspan="3" width="603">
      <p style="text-align: center;">
        <strong>Adding, Deleting and Moving Lines</strong>
      </p>
    </td>
  </tr>

  <tr>
    <td width="290">
      Add new line after the one at caret
    </td>

    <td width="156">
      Shift+Enter
    </td>

    <td width="157">
      Shift+Enter
    </td>
  </tr>

  <tr>
    <td width="290">
      Duplicate line or code fragment
    </td>

    <td width="156">
      Ctrl+Alt+Up/Down
    </td>

    <td width="157">
      Ctrl+D
    </td>
  </tr>

  <tr>
    <td width="290">
      Remove line
    </td>

    <td width="156">
      Ctrl+D
    </td>

    <td width="157">
      Ctrl+Y
    </td>
  </tr>

  <tr>
    <td width="290">
      Move row or entire selection up or down
    </td>

    <td width="156">
      Alt+Up/Down
    </td>

    <td width="157">
      Shift+Alt+Up/Down
    </td>
  </tr>

  <tr>
    <td colspan="3" width="603">
      <p style="text-align: center;">
        <strong>Find/Search</strong>
      </p>
    </td>
  </tr>

  <tr>
    <td width="290">
       Find usage of class/variable in workspace / project
    </td>

    <td width="156">
       Ctrl+Shift+G
    </td>

    <td width="157">
       Ctrl+Alt+F7 (popup)<br /> Alt+F7 (Found console)
    </td>
  </tr>

  <tr>
    <td width="290">
       Find text in project/workspace project
    </td>

    <td width="156">
       Ctrl+H (choose File Search)
    </td>

    <td width="157">
       Ctrl+Shift+F
    </td>
  </tr>

  <tr>
    <td colspan="3" width="603">
      <p style="text-align: center;">
        <strong>Navigation</strong>
      </p>
    </td>
  </tr>

  <tr>
    <td width="290">
       Back (Undo last navigation operation)
    </td>

    <td width="156">
       Alt+Left
    </td>

    <td width="157">
       Ctrl+Alt+Left
    </td>
  </tr>

  <tr>
    <td width="290">
       Back (Undo last navigation operation)
    </td>

    <td width="156">
       Alt+ Left
    </td>

    <td width="157">
       Ctrl + Alt + Left
    </td>
  </tr>

  <tr>
    <td width="290">
       Navigate between tabs/editors
    </td>

    <td width="156">
       Ctrl + Page Down / Up
    </td>

    <td width="157">
       Alt + Left/Alt + Right
    </td>
  </tr>

  <tr>
    <td width="290">
       Go to line
    </td>

    <td width="156">
       Ctrl + L
    </td>

    <td width="157">
       Ctrl + G
    </td>
  </tr>

  <tr>
    <td width="290">
       Navigate to recent files
    </td>

    <td width="156">
       Ctrl + E
    </td>

    <td width="157">
       Ctrl + E
    </td>
  </tr>

  <tr>
    <td width="290">
       Quickly move between methods in the editor
    </td>

    <td width="156">
    </td>

    <td width="157">
       Alt + Up / Down
    </td>
  </tr>

  <tr>
    <td colspan="3" width="603">
      <p style="text-align: center;">
        <strong>Debug</strong>
      </p>
    </td>
  </tr>

  <tr>
    <td width="290">
       Step over
    </td>

    <td width="156">
       F6
    </td>

    <td width="157">
       F8
    </td>
  </tr>

  <tr>
    <td width="290">
       Step into
    </td>

    <td width="156">
       F5
    </td>

    <td width="157">
       F7
    </td>
  </tr>

  <tr>
    <td width="290">
       Step out
    </td>

    <td width="156">
       F7
    </td>

    <td width="157">
       Shift + F8
    </td>
  </tr>

  <tr>
    <td width="290">
       Resume
    </td>

    <td width="156">
       F8
    </td>

    <td width="157">
       F9
    </td>
  </tr>

  <tr>
    <td colspan="3" width="603">
      <p style="text-align: center;">
        <strong>Run tests</strong>
      </p>
    </td>
  </tr>

  <tr>
    <td width="290">
       Run as JUnit Test (valid both for test classes and methods)
    </td>

    <td width="156">
       Alt+Shift+X,T
    </td>

    <td width="157">
       Ctrl+Shift+F10
    </td>
  </tr>

  <tr>
    <td width="290">
       Debug as JUnit Test
    </td>

    <td width="156">
       Alt+Shift+D,T
    </td>

    <td width="157">
    </td>
  </tr>
</table>

<div class="box nota_bene" style="font-weight: normal; color: #474747;">
  <h2>
    <span id="Link_to_editor">Link to editor</span>
  </h2>

  <p style="text-align: justify;">
    I often find myself when editing a file and to need to edit others files in the same context. For example ff it is a class usually, I also work on others classes in the same package &#8211; you can quickly navigate to the other class in the package by having the Link to Editor feature turned on.  What does it do? Whenever I edit a file it diplays it instantly in the package explorer/project. If you take vertical packaging approach, which holds classes together based on functionality rather than on layers (dao, service etc.), which I highly recommend, this comes pretty handy.
  </p>

  <h3>
    <span id="Eclipse">Eclipse</span>
  </h3>

  <p>
    Go to the Project Explorer or Package Explorer View and click on the <strong>Link to Editor</strong> button
  </p>

  <p>
    <a href="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-eclipse.png"><img class="alignnone size-full wp-image-2304" src="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-eclipse.png" alt="link-with-editor-eclipse" width="784" height="393" srcset="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-eclipse.png 784w, {{site.url}}/wp-content/uploads/2015/02/link-with-editor-eclipse-300x150.png 300w" sizes="(max-width: 784px) 100vw, 784px" /></a>
  </p>

  <p style="text-align: justify;">
    If you don&#8217;t want to enable this feature, you can still navigate to the package/project explorer hierarchy by using the keys combination <strong>Alt + Shift + W</strong> and select where you want to show it:
  </p>

  <p>
    <a href="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-eclipse.png"><img class="alignnone size-full wp-image-2306" src="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-eclipse.png" alt="show in package explorer eclipse" width="909" height="431" srcset="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-eclipse.png 909w, {{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-eclipse-300x142.png 300w" sizes="(max-width: 909px) 100vw, 909px" /></a>
  </p>

  <h3>
    <span id="IntelliJ">IntelliJ</span>
  </h3>

  <p>
    In the Project or Packages view select Settings and then <strong>Autoscroll From Source</strong>
  </p>

  <p>
    <a href="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-intellij.png"><img class="alignnone size-full wp-image-2305" src="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-intellij.png" alt="link-with-editor-intellij" width="698" height="602" srcset="{{site.url}}/wp-content/uploads/2015/02/link-with-editor-intellij.png 698w, {{site.url}}/wp-content/uploads/2015/02/link-with-editor-intellij-300x259.png 300w" sizes="(max-width: 698px) 100vw, 698px" /></a>
  </p>

  <p>
    If you don&#8217;t want to enable this feature you can still navigate by using the keys combination <strong>Alt + F1</strong> and select where you want to show it:<a href="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-intellij.png"><img class="alignnone size-full wp-image-2307" src="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-intellij.png" alt="show in package explorer intellij" width="891" height="624" srcset="{{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-intellij.png 891w, {{site.url}}/wp-content/uploads/2015/02/show-in-package-explorer-intellij-300x210.png 300w" sizes="(max-width: 891px) 100vw, 891px" /></a>
  </p>

  <h2>
    <span id="What_I_really_like_about_IntelliJ">What I really like about IntelliJ</span>
  </h2>

  <h3>
    <span id="Lots_of_things_by_default">Lots of things by default</span>
  </h3>
</div>

<p style="text-align: justify;">
  IntelliJ comes with a lots of features provided by default (e.g. GitHub integration &#8211; <em>have to check that one 🙂</em>). Of course in Eclipse you can also get lots of functionality, by selecting on of the more specialized versions, but most likely you&#8217;ll still have to configure this or that plugin&#8230;
</p>

<h3 style="font-weight: normal; color: #474747;">
  <span id="Change_Font_size_using_the_mouse_wheel">Change Font size using the mouse wheel</span>
</h3>

<p style="font-weight: normal; color: #474747;">
  You can change font size using the mouse wheel (feature I heavily use in browser). But you need first to enable it:
</p>

  1. <span style="color: #474747;">Open the IDE Settings (Ctrl+Shift+S or over the Menu File > Settings)</span>
  2. On the Editor page (type Editor in the search bar), make sure that the setting **<span class="control" style="color: #404853;">Change font size (Zoom) with Ctrl+MouseWheel</span>**<span style="color: #404853;"> is enabled.</span>

<img class="alignnone" src="http://drive.google.com/uc?export=download&id=0B9xyFgZ3ENhFNlJrazNSa3hOSFE" alt="" width="1218" height="875" />

### <span id="Launch_terminal_directly_in_the_IDE">Launch terminal directly in the IDE</span>

Alt + F12

### <span id="Find_action_by_name">Find action by name</span>

<p style="text-align: justify;">
  If you cannot remember a shortcut, you can find the actions by name as well as the settings, using the <strong>Ctrl+Shift+A</strong> &#8211; well, this one you need to remember though&#8230;
</p>

<p style="text-align: justify;">
  Lets&#8217;s say you&#8217;d want to find the shortcut for Find usages for example, then click <strong>Ctrl+Shift+A</strong> and then start typing &#8220;<em>Find usage</em>&#8221; in the search box:
</p>

[<img class="alignnone size-full wp-image-2361" src="{{site.url}}/wp-content/uploads/2015/03/intellij-find-action-by-name.png" alt="intellij - find action by name" width="471" height="384" srcset="{{site.url}}/wp-content/uploads/2015/03/intellij-find-action-by-name.png 471w, {{site.url}}/wp-content/uploads/2015/03/intellij-find-action-by-name-300x245.png 300w" sizes="(max-width: 471px) 100vw, 471px" />]({{site.url}}/wp-content/uploads/2015/03/intellij-find-action-by-name.png)

### <span id="Coollive_templates">Cool live templates</span>

Type p and press Ctrl + J, then you get following options:

  * psf &#8211; public static final
  * psfi &#8211; public static final int
  * psfs &#8211; public static final String
  * psvm &#8211; main method declaration

### <span id="Great_JavaScriptHTML5_support">Great JavaScript/HTML5 support</span>

<p style="text-align: justify;">
  The commercial version (IntelliJ Ultimate), should include ultimate code assistance for <a href="https://www.jetbrains.com/idea/features/html_css.html">HTML5</a>, <a href="https://www.jetbrains.com/idea/features/html_css.html">CSS3</a>, <a href="https://www.jetbrains.com/idea/features/html_css.html">SASS</a>, <a href="https://www.jetbrains.com/idea/features/html_css.html">LESS</a>, <a href="https://www.jetbrains.com/idea/features/javascript_editor.html">JavaScript</a>, <a href="https://www.jetbrains.com/idea/features/javascript_editor.html">CoffeeScript</a>, <a href="https://www.jetbrains.com/idea/features/nodejs.html">Node.js</a>, <a href="https://www.jetbrains.com/idea/features/flex_ide.html">ActionScript</a> and other languages. Promise to check this pretty soon.
</p>

## <span id="What_I_miss_from_Eclipse">What I miss from Eclipse</span>

### <span id="Cannot_maximize_console">Cannot maximize console</span>

<p style="text-align: justify;">
  Ctrl + M on the console. I haven&#8217;t found an easy way to maximize the console output, like a double click on the tab or Ctrl+M from Eclipse.
</p>

### <span id="Show_javadoc_when_hovering_with_the_mouse">Show javadoc when hovering with the mouse</span>

<p style="text-align: justify;">
  Of course you have the Ctrl + Q to get the same effect, but it&#8217;s so nice in Eclipse when you just hover of class and receive a snippet of its javadoc&#8230;
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong><br /> Actually you can have this feature in IntelliJ also, only that is disabled by default. Go to File > Settings > General > Editor and check &#8220;Show quick documentation on mouse move&#8221; (You can also set the delay). Thank you Крис for pointing this out.
</p>

## <span id="Summary">Summary</span>

<p style="text-align: justify;">
  The feeling &#8211; I find both IDEs great, IntelliJ looks more modern, but  on the same time I like the &#8220;classicness&#8221; of Eclipse, but that&#8217;s maybe because I so much more used to it. Will relate about this later&#8230;
</p>

Well, that&#8217;s all I have so far, but I&#8217;ll try to add features to the comparison should I come accross them and the time will allow it, so stay tuned.

## <span id="Resources">Resources</span>

  1. <a title="https://www.jetbrains.com/idea/documentation/migration_faq.html" href="https://www.jetbrains.com/idea/documentation/migration_faq.html" target="_blank">IntelliJ IDEA Q&A for Eclipse Users</a>
  2. <a title="https://www.jetbrains.com/idea/help/keyboard-shortcuts-you-cannot-miss.html" href="https://www.jetbrains.com/idea/help/keyboard-shortcuts-you-cannot-miss.html" target="_blank">IntelliJ IDEA &#8211; Keyboard Shortcuts You Cannot Miss</a>
  3. <a title="https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard.pdf" href="https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard.pdf" target="_blank">IntelliJ IDEA Default Keymap</a>
  4. <a title="http://www.vogella.com/tutorials/EclipseShortcuts/article.html" href="http://www.vogella.com/tutorials/EclipseShortcuts/article.html" target="_blank">Eclipse Shortcuts &#8211; Tutorial</a>

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
