---
layout: post
title: Define and use variables in sass
description: "Define and use variables in sass code snippet"
author: ama
permalink: /snippets/61caeb32c25ba5706f39610e/define-and-use-variables-in-sass
published: true
snippetId: 61caeb32c25ba5706f39610e
categories: [snippets]
tags: [scss, css, codever-snippets]
---

Use the `$` sign to define the variable `$variable_name: variable_value`.
Below we define `anthracite-gray` colour in the `_colors.scss` file used in the [Codever](https://www.codever.land) project.
Then we reference the variable in the class definition:

```scss
$anthracite-gray: #4A5054;

.anthracite-gray {
  color: $anthracite-gray;
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://sass-lang.com/documentation/variables" target="_blank" style="font-weight: lighter">
     https://sass-lang.com/documentation/variables
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="61caeb32c25ba5706f39610e" %}
