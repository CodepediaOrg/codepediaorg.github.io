---
layout: post
title: How to export a constant in typescript
description: "How to export a constant in typescript code snippet"
author: ama
permalink: /snippets/5fcf18bf781081728180314b/how-to-export-a-constant-in-typescript
published: true
snippetId: 5fcf18bf781081728180314b
categories: [snippets]
tags: [typescript]
---

Use the `export` keyword:

```typescript
import { SearchDomain } from './search-domain.enum';

export const searchDomains: any = new Map([
  [SearchDomain.MY_BOOKMARKS, 'My Bookmarks'],
  [SearchDomain.PUBLIC_BOOKMARKS, 'Public Bookmarks'],
  [SearchDomain.MY_SNIPPETS, 'My Snippets'],
  [SearchDomain.PUBLIC_SNIPPETS, 'Public Snippets']
]);
```

In the consumer file we `import` it:

```typescript
import { searchDomains } from '../../core/model/search-domains-map';

export class SearchbarComponent implements OnInit {
    searchDomains = searchDomains;
    //...
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://www.typescriptlang.org/docs/handbook/modules.html" target="_blank" style="font-weight: lighter">
     https://www.typescriptlang.org/docs/handbook/modules.html
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fcf18bf781081728180314b" %}
