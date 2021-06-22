---
layout: post
title: How to display an element in full screen in Angular with screenfull.js
description: "Use screenfull.js to display code snippets in full screen in Angular."
author: ama
permalink: /ama/how-to-display-element-in-fullscreen-in-angular
published: true
categories: [tutorial]
tags: [angular, javascript, codever, productivity]
---

## Use case
Some code snippets I save are really long or maybe too wide, due to comments of course, to read them easily in the code text
area. In this case it is very practical to view them in full screen, a feature I implemented over the passing weekend:

{% include image.html url="https://raw.githubusercontent.com/codeverland/codever/master/documentation/gif/how-to/codever-snippets-show-code-in-fullscreen-1680x1050.gif" description="Toggle fullscreen for snippets" %}

For the implementation I used screenfull.js[^1], which is
a simple wrapper for cross-browser usage of the JavaScript Fullscreen API[^2].

[^1]: <https://github.com/sindresorhus/screenfull.js/>
[^2]: <https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API>

Let's see how this goes exactly.

<!--more-->

## Implementation

I added an extra `button`, besides the **Copy Snippet** one, which on `click` will triggered the `toggleFullScreen` method.
This method has as parameter the `code` html element, referenced via the `#codePart` template variable:

```html
  <div class="code-snippet-wrapper">
    <app-button-copy-snippet [codeSnippet]="codeSnippet.code"></app-button-copy-snippet>
    <button
      class="btn btn-sm btn-light float-right mr-1"
      (click)="toggleFullScreen(codePart)"
      placeholder="Click to copy code snippet" >
       Fullscreen <i class="fas fa-expand"></i>
    </button>
    <div class="clear"></div>
    <pre class="mb-0"><code [highlight]="codeSnippet.code" #codePart></code></pre>
  </div>
```

In the typescript angular component, we verify if **fullscreen** is enabled for the document with the help
of the `screenfull.isEnabled` method and when this is true we request fullscreen for the html element via
the `screenfull.toggle` method:

```typescript
import * as screenfull from 'screenfull';

export class SnippetCardBodyComponent  implements  AfterViewInit, AfterViewChecked {
  // rest not displayed for brevity

  toggleFullScreen(codePart: HTMLElement) {
    if (screenfull.isEnabled) {
      screenfull.toggle(codePart);
    }
  }

}
```

