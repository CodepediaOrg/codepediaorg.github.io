---
layout: post
title: Angular http client with query parameters example
description: "Angular http client with query parameters example code snippet"
author: ama
permalink: /snippets/5ede90e7116a5754dabe6900/angular-http-client-with-query-parameters-example
published: true
snippetId: 5ede90e7116a5754dabe6900
categories: [snippets]
tags: [typescript, angular, angular-httpclient, codever-snippets]
---

In [Codever](https://www.codever.land) we use extensively the [Angular Http Client](https://angular.io/guide/http)
to make REST calls against a NodeJs/ExpressJS API - [source code on Github](https://github.com/codeverland/codever).

In the following snippet you can see hot to set http query parameters to the rest api calls.

 Use the `HttpParams` class with the `params` request option to add URL query strings in your HttpRequest:

```typescript
  getFilteredPersonalBookmarks(searchText: string, limit: number, page: number, userId: string, include: string): Observable<Bookmark[]> {
    const params = new HttpParams()
      .set('q', searchText)
      .set('page', page.toString())
      .set('limit', limit.toString())
      .set('include', include);
    return this.httpClient.get<Bookmark[]>(`${this.personalBookmarksApiBaseUrl}/${userId}/bookmarks`,
      {params: params})
      .pipe(shareReplay(1));
  }

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/http#url-params" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/http#url-params
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5ede90e7116a5754dabe6900" %}
