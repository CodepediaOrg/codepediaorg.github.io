---
layout: post
title: How to create a map from a double value array in Javascript
description: "How to create a map from a double value array in Javascript code snippet"
author: ama
permalink: /snippets/5fcf17eb6fd7966de9f1270c/how-to-create-a-map-from-double-value-array-in-javascript
published: true
snippetId: 5fcf17eb6fd7966de9f1270c
categories: [snippets]
tags: [typescript, javascript]
---

Use the regular Map constructor to transform a 2D key-value Array into a map

```javascript
const searchDomains: any = new Map([
  [SearchDomain.MY_BOOKMARKS, 'My Bookmarks'],
  [SearchDomain.PUBLIC_BOOKMARKS, 'Public Bookmarks'],
  [SearchDomain.MY_SNIPPETS, 'My Snippets'],
  [SearchDomain.PUBLIC_SNIPPETS, 'Public Snippets']
]);

searchDomains.get(SearchDomain.MY_BOOKMARKS) // returns "My Bookmarks"
```

<hr/>

Use `Array.from()` to transform a map into a 2D key-value Array

```javascript
console.log(Array.from(searchDomains)) // Will show you exactly the same Array as kvArray
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map" target="_blank" style="font-weight: lighter">
     https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map
  </a>
</span>

<hr/>

{% include snippet-post-recommendation-ending.html snippetId="5fcf17eb6fd7966de9f1270c" %}






