---
layout: post
title: Angular router navigation with query parameters example
description: "Example showing how to pass angular router navigation with query parameters and how to receive them"
author: ama
permalink: /snippets/5fdb92eeb32264729e506b1e/angular-router-navigation-with-query-parameters-example
published: true
snippetId: 5fdb92eeb32264729e506b1e
categories: [snippets]
tags: [typescript, angular, routing]
---

[**Project**: `codever`](https://github.com/codeverland/codever)

Use the `queryParams` extra parameter of the router `navigate` method:

```typescript
  constructor(private router: Router,
              private httpClient: HttpClient) {
    this.publicSnippetsApiBaseUrl = environment.API_URL + '/public/snippets';
  }

  getPublicSnippetById(snippetId: string): Observable<Codelet> {
    return this.httpClient
      .get<Codelet>(`${this.publicSnippetsApiBaseUrl}/${snippetId}`).pipe(
        catchError(() => {
          this.router.navigate(['/404-snippet'],
            {
              queryParams: {snippetId: snippetId}
            });
          return throwError('Error 404');
        }));
  }
```

On the receiving side you can extract the parameter from the `ActivatedRoute`'s `queryParamMap` property

```typescript
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  template: '  <div id="about-content" class="jumbotron"><h5>Snippet with the id "{{snippetId}}" was not found - the submitter might have deleted it</h5> </div>'
})
export class SnippetNotFoundComponent {
  snippetId: string;

  constructor(private route: ActivatedRoute) {
    this.snippetId = this.route.snapshot.queryParamMap.get('snippetId');
  }
}

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/router#activated-route" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/router#activated-route
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="5fdb92eeb32264729e506b1e" %}
