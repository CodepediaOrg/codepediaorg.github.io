---
layout: post
title: Pass data through window object history in Angular navigation
description: "Code snippet showing how to add and retrieve data from window object history in Angular"
author: ama
permalink: /snippets/5fe47dd246ff736c274b92c9/add-and-retrieve-data-from-window-object-history-in-angular
published: true
snippetId: 5fe47dd246ff736c274b92c9
categories: [snippets]
tags: [typescript, angular]
---

**Project**: [`bookmarks.dev`](https://github.com/codeverland/codever) - **File**:  `snippet-form.base.component.ts`

In the `router.navigate` method place the object /data in the `state` field of the navigation `extras` parameter

```typescript
//  constructor( protected router: Router) {}

  navigateToSnippetDetails(snippet: Codelet, queryParams: Params): void {
    const link = [`./my-snippets/${snippet._id}/details`];
    this.router.navigate(link, {
      state: {snippet: snippet},
      queryParams: queryParams
    });
  }
```

To receive on the the other end of the line, just look for it in `window.history.state`

```typescript
  ngOnInit(): void {
    this.popup = this.route.snapshot.queryParamMap.get('popup');
    this.snippet$ = of(window.history.state.snippet);
    if (!window.history.state.snippet) {
      this.userInfoStore.getUserInfo$().subscribe(userInfo => {
        this.userId = userInfo.sub;
        this.codeletId = this.route.snapshot.paramMap.get('id');
        this.snippet$ = this.personalCodeletsService.getPersonalCodeletById(this.userId, this.codeletId);
      });
    }
  }
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/api/router/NavigationExtras" target="_blank" style="font-weight: lighter">
     https://angular.io/api/router/NavigationExtras
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fe47dd246ff736c274b92c9" %}
