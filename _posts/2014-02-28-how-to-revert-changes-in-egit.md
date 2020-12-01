---
id: 1221
title: How to revert changes in EGit
date: 2014-02-28T16:42:33+00:00
author: ama
layout: post
permalink: /ama/how-to-revert-changes-in-egit/
categories:
  - article
tags:
  - eclipse
  - egit
  - git
  - version-control
  - subversive
---
One thing I immediately needed when using GIT/<a title="EGit" href="https://www.eclipse.org/egit/" target="_blank">EGit </a>instead of Subversion/<a title="https://www.eclipse.org/subversive/" href="https://www.eclipse.org/subversive/" target="_blank">Subversive</a> in Eclipse,  was the possibility to quickly revert the changes I made to a file. The good news is that with EGit it goes just as fast&#8230;

Revert changes:

  * **Subversive  **&#8211; _**right click on file** > **Team** > **Revert&#8230;**_
  * **EGit** &#8211; _**right click on file**_ > _**Replace With**_ > _**File in Git Index**_

<div id="attachment_1222" style="width: 614px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2014/02/Replace-with-file-from-Git.png"><img class="size-large wp-image-1222" alt="Replace with file from Git index" src="{{site.url}}/wp-content/uploads/2014/02/Replace-with-file-from-Git-847x1024.png" width="604" height="730" srcset="{{site.url}}/wp-content/uploads/2014/02/Replace-with-file-from-Git-847x1024.png 847w, {{site.url}}/wp-content/uploads/2014/02/Replace-with-file-from-Git-248x300.png 248w, {{site.url}}/wp-content/uploads/2014/02/Replace-with-file-from-Git.png 976w" sizes="(max-width: 604px) 100vw, 604px" /></a>

  <p class="wp-caption-text">
    Replace with file from Git index
  </p>
</div>
