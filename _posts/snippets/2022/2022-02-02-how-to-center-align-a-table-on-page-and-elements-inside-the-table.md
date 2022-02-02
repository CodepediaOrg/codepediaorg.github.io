---
layout: post
title: How to center align a table on page and elements inside the table
description: "How to center align a table on page and elements inside the table code snippet"
author: ama
permalink: /snippets/60c0cf0c5eeee76d9163d4eb/how-to-center-align-a-table-on-page-and-elements-inside-the-table
published: true
snippetId: 60c0cf0c5eeee76d9163d4eb
categories: [snippets]
tags: [scss, css, html, codever-snippets]
---

**Project**: `codever` - **File**:  `howto-get-started.component.scss`

Set `margin-left` and `margin-right` to `auto` to align in center of the page/div and `text-align` to align the text and image inside of the table headers and cells:

```html
.extensions-table {
  margin-left: auto;
  margin-right: auto;
  th, td {
    width: 8rem;
    text-align: center;
  }
}
```

The html part

```html
<div class="container">
    <h2>Get started</h2>
    <p class="lead">Codever is a bookmark and code snippets manager created and designed for Developers & Co.
      The following helpers and extensions will assist you along the way:
    <table class="extensions-table">
      <thead>
      <tr>
        <th align="center"><a
          href="https://chrome.google.com/webstore/detail/codever/diofdblfhjbpgackifolmboaiccmebjb"
          rel="nofollow"><img src="./assets/img/bookmark-48.png" alt="Bookmark" ></a>
        </th>
        <th align="center" ><a
          href="https://chrome.google.com/webstore/detail/codever/diofdblfhjbpgackifolmboaiccmebjb"
          rel="nofollow"><img src="./assets/img/chrome-48.png" alt="Chrome logo" ></a>
        </th>
        <th align="center" ><a
          href="https://addons.mozilla.org/addon/codever/"
          rel="nofollow"><img src="./assets/img/firefox-48.png" alt="Firefox logo" ></a>
        </th>
        <th align="center" ><a
          href="https://addons.mozilla.org/addon/codever/"
          rel="nofollow"><img src="./assets/img/intellij-48.png" alt="IntelliJ Logo" ></a>
        </th>
      </tr>
      </thead>
      <tbody>
      <tr>
        <td align="center" ><a
          href="https://chrome.google.com/webstore/detail/codever/diofdblfhjbpgackifolmboaiccmebjb"
          rel="nofollow">Bookmarklet</a></td>
        <td align="center" ><a
          href="https://chrome.google.com/webstore/detail/codever/diofdblfhjbpgackifolmboaiccmebjb"
          rel="nofollow">Chrome</a></td>
        <td align="center" class="ml-2"><a
          href="https://addons.mozilla.org/addon/codever/"
          rel="nofollow">Firefox</a></td>
        <td align="center" class="ml-2"><a
          href="https://plugins.jetbrains.com/plugin/14456-codever-snippets/"
          rel="nofollow">IntelliJ plugin</a></td>
      </tr>
      </tbody>
    </table>
</div>
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://github.com/codeverland/codever" target="_blank" style="font-weight: lighter">
     https://github.com/codeverland/codever
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60c0cf0c5eeee76d9163d4eb" %} 
