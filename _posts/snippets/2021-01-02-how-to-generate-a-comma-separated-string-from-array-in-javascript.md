---
layout: post
title: How to generate a comma separated string from array in JavaScript
description: "How to generate a comma separated string from array in JavaScript. Tagged with typescript, javascript"
author: TODO-change-me
permalink: /snippets/5feef3220b79fd210e120d3b/how-to-generate-a-comma-separated-string-from-array-in-javascript
published: true
snippetId: 5feef3220b79fd210e120d3b
categories: [snippets]
tags: [typescript, javascript]
---

Use the `arr.join([separator])` function.  The separator specifies a string to separate each pair of adjacent elements of the array. The separator is converted to a string if necessary. If omitted, the array elements are separated with a comma (","). If separator is an empty string, all elements are joined without any characters in between them.

```typescript
    private generateBlogPostHeader(commaSeparatedListOfTags: string, snippet: Snippet): string {
        let header: string = '';

        header += 'categories: [snippets]\n';
        header += `tags: [${snippet.tags.join(',')}]\n`;
        header += '---\n';

        return header;
    }
```

<hr/>
You can also use the `toString()` method in this particular case with the same result

```typescript
header += `tags: [${snippet.tags.toString()}]\n`;
```

<hr/>
<span style="font-size: 0.9rem"><strong>Reference - </strong><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join" target="_blank" style="font-weight: lighter">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join</a></span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="5feef3220b79fd210e120d3b" %} 
