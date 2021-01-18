---
layout: post
title: Use wildcard to redirect to main sub-route component in angular
description: "Code snippet showin how to use a wildcard to redirect to main sub-route component in angular"
author: ama
permalink: /snippets/5fd650d57810817281804847/use-wildcard-to-redirect-to-main-sub-route-component-in-angular
published: true
snippetId: 5fd650d57810817281804847
categories: [snippets]
tags: [typescript, angular, angular-routing]
---

[**Project**: `bookmarks.dev`](https://github.com/BookmarksDev/bookmarks.dev) - **File**:  `public-routing.module.ts`

The `**` wildcard has to be defined as a child of the root you want redirected back to. Thus URLs like [https://www.bookmarks.dev/snippets/60046e4554aac471e0799da7/some-mongo-title-here](https://www.bookmarks.dev/snippets/60046e4554aac471e0799da7/some-mongo-title-here) will still "point" to [https://www.bookmarks.dev/snippets/60046e4554aac471e0799da7](https://www.bookmarks.dev/snippets/60046e4554aac471e0799da7):

```typescript
  {
    path: 'snippets/:id',
    component: PublicSnippetDetailsComponent,
    children: [
      // This is a WILDCARD CATCH-ALL route that is scoped to the "/snippets/:snippetid"
      // route prefix. It will only catch non-matching routes that live
      // within this portion of the router tree.
      {
        path: '**',
        component: PublicSnippetDetailsComponent
      }
    ]
  },
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/router#setting-up-redirects" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/router#setting-up-redirects
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fd650d57810817281804847" %}
