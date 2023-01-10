---
layout: post
title: Set value of input field in angular template
description: "Set value of input field in angular template code snippet"
author: ama
permalink: /snippets/638ceaf03b43b9521ecfab28/set-value-of-input-field-in-angular-template
published: true
snippetId: 638ceaf03b43b9521ecfab28
categories: [snippets]
tags: [angular, html, codever-snippets]
---

To set the initial value of the `input` control, just use the `value` attribute:

```typescript
<input type="text" class="form-control" placeholder="Generate Codever sharable Url"
       aria-describedby="basic-addon2"
value="{{ environment.HOST + '/bookmarks/shared/' + shareableId}}">
```

**Project**: `codever` - **File**:  `social-share-dialog.component.html`

To understand usage of angular environment used in this snippet (`environment.HOST`)
see the [Configure and use environment specific values in Angular and html template](https://www.codever.dev/snippets/6389997bb160cb1fab2430fe/details) snippet

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="638ceaf03b43b9521ecfab28" %}
