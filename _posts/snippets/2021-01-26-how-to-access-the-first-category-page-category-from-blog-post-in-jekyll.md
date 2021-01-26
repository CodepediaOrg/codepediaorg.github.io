---
layout: post
title: Access the first category page category from blog post in Jekyll
description: "Code snippet example showing how to access the first category page category from blog post in Jekyll"
author: ama
permalink: /snippets/600016dade81142f0e80f435/how-to-access-the-first-category-page-category-from-blog-post-in-jekyll
published: true
snippetId: 600016dade81142f0e80f435
categories: [snippets]
tags: [html, jekyll, liquid, ruby]
---

Use the `page.categories` variable and select index `0` of the array:

```
      {% if page.categories[0] != "snippets" %}
        {% include promote-bookmarks.dev.html %}
      {% endif %}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://jekyllrb.com/docs/variables/" target="_blank" style="font-weight: lighter">
     https://jekyllrb.com/docs/variables/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="600016dade81142f0e80f435" %}
