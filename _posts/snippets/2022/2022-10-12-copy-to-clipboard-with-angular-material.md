---
layout: post
title: Copy to clipboard with angular material
description: "Copy to clipboard with angular material code snippet"
author: ama
permalink: /snippets/63465a59a89ae15f46e41c65/copy-to-clipboard-with-angular-material
published: true
snippetId: 63465a59a89ae15f46e41c65
categories: [snippets]
tags: [angular, angular-material, copy, clipboard, codever-snippets]
---

Use the `click` event to pass the text to the handling function, in this case `copyToClipboard(bookmark.location)`

```typescript
<span class="btn-light btn-sm" (click)="copyToClipboard(bookmark.location)" title="Copy link to clipboard">
  <i class="far fa-copy copy-link"></i><span class="copy-btn-text">{{copyLinkButtonText}}</span>
</span>
```

To programmatically copy a string use the `Clipboard` which service copies text to the user's clipboard.
We use a `setTimeout` to visually inform the user for a brief moment that the **copy** was successful


```typescript
import { Clipboard } from '@angular/cdk/clipboard';

export class BookmarkListElementComponent {

copyLinkButtonText = '';

  constructor(private router: Router,
              private clipboard: Clipboard) {}

  copyToClipboard(location: string) {
    const copied = this.clipboard.copy(location);
    if (copied) {
      this.copyLinkButtonText = ' Copied';
      setTimeout(() => this.copyLinkButtonText = '', 1300);
    }

  }
}
```

See it in action at [www.codever.dev](https://www.codever.dev):

![Copy-to-clipboard-demo](/images/posts/2022-10-12-angular-copy-to-clipboard-snippet/codever-copy-to-clipboard-demo.gif)

See the **reference link** (official docs) how to use it for longer texts.

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://material.angular.io/cdk/clipboard/overview" target="_blank" style="font-weight: lighter">
     https://material.angular.io/cdk/clipboard/overview
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="63465a59a89ae15f46e41c65" %}
