---
layout: post
title: Highlight liquid code in Jekyll blog post
description: "Highlight liquid code in Jekyll blog post code snippet"
author: ama
permalink: /snippets/601138a3e78329226ca53868/highlight-liquid-code-in-jekyll-blog-post
published: true
snippetId: 601138a3e78329226ca53868
categories: [snippets]
tags: [markdown, jekyll, liquid, html, blogging, codever-snippets]
---

**Project**: [`codepediaorg.github.io`](https://github.com/CodepediaOrg/codepediaorg.github.io)

You need to place `{% raw %}` and  `{% endraw %}`  tags around your code. Since Jekyll 4.0 , you can add `render_with_liquid: false` in your front matter to disable Liquid entirely for a particular document. Use the standard Code snippet highlighting from Jekyll:

```liquid
{% highlight liquid %}
{% raw %}
      {% if page.categories[0] != "snippets" %}
        {% include promote-bookmarks.dev.html %}
      {% endif %}
{% endraw %}
{% endhighlight %}
```

Or place it in markdown code markers:

```liquid
```liquid
{% raw %}
  {% if page.categories[0] != "snippets" %}
    {% include promote-bookmarks.dev.html %}
  {% endif %}
{% endraw %}
```
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://jekyllrb.com/docs/liquid/tags/" target="_blank" style="font-weight: lighter">
     https://jekyllrb.com/docs/liquid/tags/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="601138a3e78329226ca53868" %}
