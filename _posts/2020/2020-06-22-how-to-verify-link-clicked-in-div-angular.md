---
layout: post
title: How to verify element click in div with Angular
description: "Demo and code showing how to verify an anchor was clicked in Angular. Based on use case for www.codever.dev"
author: ama
permalink: /ama/how-to-verify-element-click-in-div-with-angular
published: true
categories: [tutorial]
tags:
    - javascript
    - angular
    - html
    - codever
---

Being on a constant lookout for bookmarks management optimization on [www.codever.dev](https://www.codever.dev),
I had this idea lately to add a bookmark to my history not only when I click the title of the bookmark (main URL),
but also when I click hyperlinks in the description of the bookmark - sometimes I tend to bookmark the "parent" url and
add "child" or related bookmarks in the description (an example would be bookmarking the same "resource" which is deployed
 on different environments like dev, test and prod):

 ![Click on link in bookmark description](/images/posts/2020-06-22-how-to-verify-link-clickec-in-div-angular/click-link-in-description-demo-1440x900.gif)

> This plays really well now with the recently introduced [hot keys to access the history and pinned bookmarks]({{ site.baseurl }}/ama/keyboard-shortcuts-example-with-angular)

{% include source-code-codever.html %}

Well, let's see how this looks like in code:

<!--more-->

## The HTML template
```html
    <app-bookmark-text
      [bookmark]="bookmark"
      [queryText]="queryText"
      (click)="onClickInDescription($event, bookmark)"
      (auxclick)="onMiddleClickInDescription($event, bookmark)"
      (contextmenu)="onRightClickInDescription($event, bookmark)">
    </app-bookmark-text>
```

In the template we listen to the following events the Angular API provides:
* `onClick` - fires when the user clicks the left mouse button on the object
* `onauxclick` - to cover the middle button click of the mouse I use the auxclick[^1] event, which is fired at an Element
 when a non-primary pointing device button (any mouse button other than the primary—usually leftmost—button) has been pressed
    and released both within the same element.fires when the user clicks the left mouse button on the object
* `oncontextmenu` - fires when the user clicks the right mouse button in the client area, opening the context menu.

[^1]: <https://developer.mozilla.org/en-US/docs/Web/API/Element/auxclick_event>

## The Angular component
Let's see how this is implemented in the component:

```typescript
  onClickInDescription($event: any, bookmark: Bookmark) {
    if (this.isHtmlAnchorElement($event)) {
      $event.target.setAttribute('target', '_blank');
      this.addToHistoryIfLoggedIn(bookmark);
    }
  }

  onRightClickInDescription($event: any, bookmark: Bookmark) {
    if (this.isHtmlAnchorElement($event)) {
      this.addToHistoryIfLoggedIn(bookmark);
    }
  }

  onMiddleClickInDescription($event: any, bookmark: Bookmark) {
    if (this.isHtmlAnchorElement($event)) {
      this.addToHistoryIfLoggedIn(bookmark);
    }
  }

  private isHtmlAnchorElement($event: any) {
    return $event.target.matches('a');
  }
```

The pattern is the same: <span class="highlight-yellow">verify that the object onto which the event was dispatched
 is an HTML Anchor - <code>isHtmlAnchorElement()</code></span>. If that's the case it will be added to my bookmarks history.

 To achieve that, we use the `target`[^2] property of the `Event` interface with the `matches(selector)`[^3] method. In this particular
 case we use `a` as `selectorString`. For other html elements use a corresponding selector.

[^2]: <https://developer.mozilla.org/en-US/docs/Web/API/Event/target>
[^3]: <https://developer.mozilla.org/en-US/docs/Web/API/Element/matches>

**Note**:
* **on left click** we force opening the URL in a new tab by setting the `target` attribute to `_blank` - `$event.target.setAttribute('target', '_blank');`
* **on right click** I haven't found a way yet to verify if the user clicked "Open Link in New Tab"
 or "Open Link in New Window" - suggestions are more than welcomed; but for the time being I think it is enough,
 what else would you do a right click on a link for?


## Conclusion
This is definitely a [piece of code I will bookmark for later](https://dev.to/ama/bookmarking-code-snippets-with-codelets-3d44).

{% include action-to-star-bookmarksdev-on-github.html %}

## References
