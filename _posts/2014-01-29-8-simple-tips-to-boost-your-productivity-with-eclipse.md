---
id: 1121
title: 8+ simple tips to boost your productivity with Eclipse
date: 2014-01-29T08:36:12+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1121
permalink: /ama/8-simple-tips-to-boost-your-productivity-with-eclipse/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 6
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2189326121
categories:
  - development
  - ide
tags:
  - development
  - eclipse
  - productivity
  - shortcuts
  - tips
  - tricks
---
## <span id="1_References_of_used_classmethod_in_code">1. References of used class/method in code</span>

<p style="text-align: justify;">
  This is a very quick and useful way to find out where a class or method is used throughout the workspace/project. How to do it &#8211;<strong> select class/method </strong>><strong> right click </strong>><strong> References </strong>><strong> Workspace (Ctrl+Shift+G) || Project</strong>
</p>

Once you&#8217;ve done that, the results will be displayed in the **Search view**:

<div id="attachment_1130" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/1-show-references.png" target="_blank"><img class="size-medium wp-image-1130 " src="{{site.url}}/wp-content/uploads/2014/01/1-show-references-300x271.png" alt="Show-References" width="300" height="271" srcset="{{site.url}}/wp-content/uploads/2014/01/1-show-references-300x271.png 300w, {{site.url}}/wp-content/uploads/2014/01/1-show-references.png 939w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Show references
  </p>
</div>

Another alternative for that is by selecting the class/method > start Java Search (**Ctrl+H**) > select your search criteria (in this case **Type**) and limit to **References**:

<div id="attachment_1132" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/1-1-show-references.png" target="_blank"><img class="size-medium wp-image-1132 " src="{{site.url}}/wp-content/uploads/2014/01/1-1-show-references-300x251.png" alt="show references - java search" width="300" height="251" srcset="{{site.url}}/wp-content/uploads/2014/01/1-1-show-references-300x251.png 300w, {{site.url}}/wp-content/uploads/2014/01/1-1-show-references.png 735w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Show References via Java Search
  </p>
</div>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_References_of_used_classmethod_in_code">1. References of used class/method in code</a>
    </li>
    <li>
      <a href="#2_Bookmarks">2. Bookmarks</a>
    </li>
    <li>
      <a href="#3_Use_the_8220search_in_file8221_function">3. Use the &#8220;search in file&#8221; function</a>
    </li>
    <li>
      <a href="#4_Open_declarationimplementation">4. Open declaration/implementation</a>
    </li>
    <li>
      <a href="#5_Use_multiple_workspaces">5.  Use multiple workspaces</a>
    </li>
    <li>
      <a href="#6_Use_shortcuts">6. Use shortcuts</a>
    </li>
    <li>
      <a href="#7_Use_code_templates">7. Use code templates</a>
    </li>
    <li>
      <a href="#8_Save_actions">8. Save actions</a>
    </li>
    <li>
      <a href="#Summary">Summary</a>
    </li>
    <li>
      <a href="#Resources">Resources</a><ul>
        <li>
          <a href="#Web">Web</a>
        </li>
        <li>
          <a href="#Codingpedia">Codingpedia</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="2_Bookmarks"><strong>2. Bookmarks</strong></span>

<p style="text-align: justify;">
  So I&#8217;ve found out where the method is used or where I want to modify some code. For sure I might want to go back to that location several times before I am finished. I don&#8217;t want to search for the location in code every time so I just bookmark it! How to do it &#8211; go to the <strong>left margin of the code view</strong> (where you set the breakpoints) > <strong>right click </strong>> <strong>Add bookmark&#8230;</strong>
</p>

<div id="attachment_1131" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/2-add-bookmark.png" target="_blank"><img class="size-medium wp-image-1131 " src="{{site.url}}/wp-content/uploads/2014/01/2-add-bookmark-300x246.png" alt="Add bookmark" width="300" height="246" srcset="{{site.url}}/wp-content/uploads/2014/01/2-add-bookmark-300x246.png 300w, {{site.url}}/wp-content/uploads/2014/01/2-add-bookmark.png 809w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Add bookmark
  </p>
</div>

## <span id="3_Use_the_8220search_in_file8221_function">3. Use the &#8220;search in file&#8221; function</span>

<p style="text-align: justify;">
  By now we all know how important search is, otherwise you wouldn&#8217;t have probably found my post in the first place&#8230; The same holds true for code &#8211; we don&#8217;t have to remember everything. As long as we remember something to put in the search box we can look for it
</p>

 **Menu > Search >  File&#8230;**

<p style="text-align: justify;">
  For example I know I have a <code>select</code> identifier in the MyBatis mapper (<code>.xml</code> file) called <code>getPodcastById</code>. The faster way to navigate to it is to search it in <code>.xml</code> files, instead of clicking myself through the <strong>Package Explorer</strong> view:
</p>

<div id="attachment_1134" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/3-search-in-file.png" target="_blank"><img class="size-medium wp-image-1134 " src="{{site.url}}/wp-content/uploads/2014/01/3-search-in-file-300x231.png" alt="Search in files" width="300" height="231" srcset="{{site.url}}/wp-content/uploads/2014/01/3-search-in-file-300x231.png 300w, {{site.url}}/wp-content/uploads/2014/01/3-search-in-file.png 606w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Search in files
  </p>
</div>

## <span id="4_Open_declarationimplementation">4. Open declaration/implementation</span>

<p style="text-align: justify;">
  Long gone are the times when we didn&#8217;t have to look in the source code of the frameworks/products we were using :). Provided that you downloaded the java sources/docs (e.g. in Maven projects : <strong>Eclipse Menu</strong> > <strong>Window</strong> > <strong>Preferences</strong> ><strong> Maven</strong> > <strong>Download Artifact Sources/JavaDoc</strong>),  you can quickly go to the implementation/declaration, by holding the <strong>Ctrl</strong> key and then hovering with the mouse over the class/method:
</p>

<dl id="attachment_1136" class="wp-caption alignnone" style="width: 310px;">
  <dt class="wp-caption-dt" style="display: inline !important;">
    <a href="{{site.url}}/wp-content/uploads/2014/01/4-open-declaration.png" target="_blank"><img class="size-medium wp-image-1136 alignnone" src="{{site.url}}/wp-content/uploads/2014/01/4-open-declaration-300x112.png" alt="open declaration" width="300" height="112" srcset="{{site.url}}/wp-content/uploads/2014/01/4-open-declaration-300x112.png 300w, {{site.url}}/wp-content/uploads/2014/01/4-open-declaration.png 883w" sizes="(max-width: 300px) 100vw, 300px" /></a>
  </dt>
</dl>

<dl id="attachment_1136" class="wp-caption alignnone" style="width: 310px;">
  <dd class="wp-caption-dd">
    Open Declaration/Implementation
  </dd>
</dl>

<p style="text-align: justify;">
  Of course you can use the same functionality to quickly navigate to the declarations/implementations in your own projects.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> A quicker way to open the declaration of the class/method is by selecting the method/class and pressing <strong>F3.</strong>
</p>

## <span id="5_Use_multiple_workspaces">5.  Use multiple workspaces</span>

<p style="text-align: justify;">
  Don&#8217;t clutter your workspace with many projects, that might not have any correlation with each other. It&#8217;s more efficient to group related projects (could be just one project) in their own workspace and create different Eclipse shortcuts for each workspace.
</p>

**Sub-tips:**

<li style="text-align: justify;">
  Create individual Eclipse shortcuts for each workspace. In the target of the shortcut specify the path to the workspace folder via the <code>-data</code> parameter (e.g. <em>&#8220;C:\eclipse\eclipse.exe <strong>-data C:\workspace\git</strong>&#8220;</em>)
</li>
  1. Name your workspaces  **Window** > **Preferences** >  **General** > **Workspace:**

<div id="attachment_1139" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/5-name-workspace.png"><img class="size-medium wp-image-1139" src="{{site.url}}/wp-content/uploads/2014/01/5-name-workspace-300x148.png" alt="Name workspace" width="300" height="148" srcset="{{site.url}}/wp-content/uploads/2014/01/5-name-workspace-300x148.png 300w, {{site.url}}/wp-content/uploads/2014/01/5-name-workspace-1024x507.png 1024w, {{site.url}}/wp-content/uploads/2014/01/5-name-workspace.png 1151w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Name workspace
  </p>
</div>

It will help you navigate easier between open Eclipse instances in the taskbar.</li>

  * <span style="line-height: 1.5;">Reuse </span>configuration<span style="line-height: 1.5;"> when creating a new workspace &#8211; you can keep the configuration of one work space (Java, Maven settings etc.) when you create another one, by just copying  <code>.metadata</code> folder created by Eclipse of your current work space in the new work space&#8217;s folder.  </span>
  * Store the projects outside of the workspace if you want to share them across different Eclipse versions.</ol>

## <span id="6_Use_shortcuts"><span style="line-height: 1.5;">6. Use shortcuts</span></span>

<p style="text-align: justify;">
  For faster development you should use more of the keyboard and less of the mouse. Apart from the classic shortcuts <strong>Ctrl+c</strong> (copy text/file to clipboard), <strong>Ctrl+v</strong> (paste text/file from clipboard), <strong>Ctrl+z</strong> (undo last action), <strong>Ctrl+y</strong> (redo last action), and <strong>Ctrl+s</strong> (save),  Eclipse supports keyboard shortcuts for the most common actions. There are dozens of them, but if you know the &#8220;important&#8221; ones, you should be fine.
</p>

I&#8217;ve listed below the ones I use most frequently :

<table border="1" cellspacing="0" cellpadding="0">
  <tr>
    <th>
      Shortcut
    </th>

    <th>
      Description
    </th>
  </tr>

  <tr>
    <td width="186">
       Alt+Shift+R
    </td>

    <td width="588">
       Rename selected element and all references in project
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+Space
    </td>

    <td width="588">
       Content assist/code completion
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+1
    </td>

    <td width="588">
       Quick Fix – Suggest possible fixes for a problem, or shows possible actions (let Eclipse create local variables, constants, choose their name and realize the necessary imports / initialize local variables / assign the result of a method to a new local variable)
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+Shift+T
    </td>

    <td width="588">
       Open Type – Open a type in a Java editor
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+Q
    </td>

    <td width="588">
       Goes to the Last edit location
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+7
    </td>

    <td width="588">
       Toggle comment the selected linets
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+O
    </td>

    <td width="588">
       Shows quick outline of a class
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+Shift+O
    </td>

    <td width="588">
       Organize imports – Evaluate all required imports and replace the current imports
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+F
    </td>

    <td width="588">
       Opens the find dialog
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+k/Ctrl+Shift+k
    </td>

    <td width="588">
       Find previous / next occurence of search item (close find window first)
    </td>
  </tr>

  <tr>
    <td width="186">
       F3
    </td>

    <td width="588">
       Open declaration – Jump to declaration of selected class, method or parameter
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+M
    </td>

    <td width="588">
       Maximize active editor or view
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+Shift+F
    </td>

    <td width="588">
       Format source code
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+E
    </td>

    <td width="588">
       Show a list of open editors
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+F6
    </td>

    <td width="588">
       Move between open editors &#8211; a slower alternative to Ctrl+E
    </td>
  </tr>

  <tr>
    <td width="186">
       Alt+<
    </td>

    <td width="588">
       Go to the previous opened editor. Cursor is placed where it was before you  opened the next editor
    </td>
  </tr>

  <tr>
    <td width="186">
       Alt+>
    </td>

    <td width="588">
       Similar Alt+< but opens the next editor
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+PageUp
    </td>

    <td width="588">
       Switch to previous opened editor
    </td>
  </tr>

  <tr>
    <td width="186">
       Ctrl+PageDown
    </td>

    <td width="588">
       Switch to next opened editor
    </td>
  </tr>
</table>

## <span id="7_Use_code_templates"><span style="line-height: 1.5;">7. Use code templates</span></span>

<p style="text-align: justify;">
  Templates are a structured description of coding patterns that reoccur in source code. The Java editor supports the use of templates to fill in commonly used source patterns. Templates are inserted using content assist (<strong>Ctrl+Space</strong>).
</p>

<p style="text-align: justify;">
  Typing the code for a loop to run through a collection of Podcasts so that I can perform some sort of action with every Podcast can be a &#8220;tedious&#8221; task. But using templates in Eclipse this can be very easy. I just type <code>for</code> in the editor and prest <strong>Ctrl+Space:</strong>
</p>

<div id="attachment_1143" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/7-templates-in-action.png" target="_blank"><img class="size-medium wp-image-1143 " src="{{site.url}}/wp-content/uploads/2014/01/7-templates-in-action-300x131.png" alt="Templates in action" width="300" height="131" srcset="{{site.url}}/wp-content/uploads/2014/01/7-templates-in-action-300x131.png 300w, {{site.url}}/wp-content/uploads/2014/01/7-templates-in-action.png 947w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Templates in action
  </p>
</div>

<p style="text-align: justify;">
  Templates are not only Java-specific and are defined in the Preferences pane under the specific plug-in with which they&#8217;re associated — for example, <strong>Eclipse > Preferences > <em>Plug-inName</em> > <em>EditorName</em> > Templates</strong>. If you do a search on Templates in Preferences you might get something like the following:
</p>

<div id="attachment_1142" style="width: 296px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/7-pref-templates.png" target="_blank"><img class="size-medium wp-image-1142 " src="{{site.url}}/wp-content/uploads/2014/01/7-pref-templates-286x300.png" alt="Templates in Preferences" width="286" height="300" srcset="{{site.url}}/wp-content/uploads/2014/01/7-pref-templates-286x300.png 286w, {{site.url}}/wp-content/uploads/2014/01/7-pref-templates.png 846w" sizes="(max-width: 286px) 100vw, 286px" /></a>

  <p class="wp-caption-text">
    Templates in Preferences
  </p>
</div>

You can also define your own code templates, but this is a story for a dedicated post. There are plenty of resources on the web for that, some of which I listed under Resources.

## <span id="8_Save_actions">8. Save actions</span>

With **Window** > **Preferences** > **Java** > **Editor** > **Save actions**

<div id="attachment_1144" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/01/8-save-actions.png" target="_blank"><img class="size-medium wp-image-1144 " src="{{site.url}}/wp-content/uploads/2014/01/8-save-actions-300x276.png" alt="Save actions" width="300" height="276" srcset="{{site.url}}/wp-content/uploads/2014/01/8-save-actions-300x276.png 300w, {{site.url}}/wp-content/uploads/2014/01/8-save-actions.png 917w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Save actions
  </p>
</div>

you can automate some of the boring tasks like format source code, organize imports and even add additional actions, when saving your work.

## <span id="Summary">Summary</span>

<p style="text-align: justify;">
  Well, these are the initail eight simple tips. I hope I could help you speed up your development in Eclipse. If you have been using them already, even better and if you see I am missing something major here, please leave a comment and I will add it to the post &#8211; thanks!
</p>

<p class="note_normal">
  If you&#8217;ve found some usefulness in this post this, I&#8217;d be very grateful if you&#8217;d help it spread by sharing it on Twitter, Google+ or Facebook. Thank you! Don&#8217;t forget also to check out <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> &#8211; you&#8217;ll find for sure interesting podcasts and episodes. We are grateful for <a title="Podcastpedia.org - how can I contribute" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">your support. </a>
</p>

## <span id="Resources">Resources</span>

### <span id="Web">Web</span>

  * <a title="http://www.vogella.com/tutorials/EclipseShortcuts/article.html" href="http://www.vogella.com/tutorials/EclipseShortcuts/article.html" target="_blank">http://www.vogella.com/tutorials/EclipseShortcuts/article.html</a>
  * <a title="http://eclipse.dzone.com/news/effective-eclipse-shortcut-key" href="http://eclipse.dzone.com/news/effective-eclipse-shortcut-key" target="_blank">Effective Eclipse: Shortcut keys</a>
  * <a title="http://eclipse.dzone.com/news/visual-guide-templates-eclipse" href="http://eclipse.dzone.com/news/visual-guide-templates-eclipse" target="_blank">Visual Guide to Templates in Eclipse</a>
  * <a title="http://help.eclipse.org/juno/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fpreferences%2Fjava%2Feditor%2Fref-preferences-save-actions.htm" href="http://help.eclipse.org/juno/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fpreferences%2Fjava%2Feditor%2Fref-preferences-save-actions.htm" target="_blank">Java Save Actions Preferences</a>

### <span id="Codingpedia">Codingpedia</span>

  * <a title="http://www.codingpedia.org/ama/install-eclipse-kepler-64-bit-on-windows-7-64-bit/" href="http://www.codingpedia.org/ama/install-eclipse-kepler-64-bit-on-windows-7-64-bit/" target="_blank">Install Eclipse Kepler 64-bit on Windows 7 64-bit</a>

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
