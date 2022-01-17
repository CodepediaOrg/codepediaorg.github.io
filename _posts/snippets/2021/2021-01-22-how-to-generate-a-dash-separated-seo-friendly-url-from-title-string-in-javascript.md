---
layout: post
title: Generate a dash separated seo friendly URL from title in JavaScript
description: "Code snippet showing how to generate a dash separated seo friendly URL from title (string) in JavaScript"
author: ama
permalink: /snippets/5feee2990b79fd210e120c4e/how-to-generate-a-dash-separated-seo-friendly-url-from-title-string-in-javascript
published: true
snippetId: 5feee2990b79fd210e120c4e
categories: [snippets]
tags: [typescript, javascript]
---

Use a global `replace` with `\W+` option ( `\W+` means any sequence of non-word characters)

```sql

    private generateDashBasedUrlPathFromTitle(title: string) {
        return title.trim().replace(/\W+/g, '-').toLowerCase();
    }
```

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="5feee2990b79fd210e120c4e" %}
