---
layout: post
title: How to use the if else clause in a Jekyll liquid page
description: "Code snippet example showing how to use if else clause in a Jekyll html liquid template page"
author: ama
permalink: /snippets/60006016e6eb82224c98e4ba/how-to-use-if-else-in-a-jekyll-html-page
published: true
snippetId: 60006016e6eb82224c98e4ba
categories: [snippets]
tags: [html, liquid, jekyll]
---

`if` is a control flow tag, that can change the information Liquid shows using programming logic:

```liquid
{% raw %}
{% if page.title %}
  {% if page.jsonld %}
    {% include {{page.jsonld}}.html %}
  {% else %}
    {% include postJSONLD.html %}
  {% endif %}
{% else %}
  {% include homeJSONLD.html %}
{% endif %}
{% endraw %}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://shopify.github.io/liquid/tags/control-flow/" target="_blank" style="font-weight: lighter">
     https://shopify.github.io/liquid/tags/control-flow/
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60006016e6eb82224c98e4ba" %}
