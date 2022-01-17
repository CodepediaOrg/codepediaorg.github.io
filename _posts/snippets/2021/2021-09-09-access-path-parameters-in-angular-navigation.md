---
layout: post
title: Access path parameters in angular navigation
description: "Access path parameters in angular navigation  code snippet"
author: ama
permalink: /snippets/600a76e0ed39cd3a1f10b9a6/access-path-parameters-in-angular-navigation
published: true
snippetId: 600a76e0ed39cd3a1f10b9a6
categories: [snippets]
tags: [typescript, angular, angular-routing, codever-snippets]
---

At [Codever](https://www.codever.land) we use Angular extensively, and many time the use navigates to certain routes
where dynamically content based on an `id` for example is loaded. To access these **path params** in angular navigation
you have two possibilities.

 The **first one**, asynchronous, is to subscribe to the `Observable<ParamMap>` observable,
  which you can access via `paramMap` method of the `ActivatedRoute`.
   Then use the `get` method with parameter you want to get as argument,  as in the example below in the `ngOnInit` method:

```typescript
// other imports not included for brevity
import { ActivatedRoute } from '@angular/router';
import { switchMap } from 'rxjs/operators';

@Component({
  selector: 'app-public-snippet-details',
  templateUrl: './public-snippet-details.component.html'
})
export class PublicSnippetDetailsComponent implements OnInit {
  snippetId: string;
  snippet$: Observable<Codelet>;

  constructor(
    private publicSnippetsService: PublicSnippetsService,
    private userInfoStore: UserInfoStore,
    private route: ActivatedRoute) {
  }

  ngOnInit() {
    this.snippet$ = this.route.paramMap.pipe(
      switchMap(params => {
        this.snippetId = params.get('id');
        return this.publicSnippetsService.getPublicSnippetById(this.snippetId);
      })
    );
  }

}

```

The **second one**, synchronous, is to the `snapshot` of this route (`ActivatedRoute`),
 and directly access the parameter from the **paramMap**, `const bookmarkId = this.route.snapshot.paramMap.get('id');`

```typescript
export class BookmarkDetailsComponent implements OnInit {
  // constructor and other details ignored for brevity

  ngOnInit() {
    this.popup = this.route.snapshot.queryParamMap.get('popup');
    this.userInfoStore.getUserInfo$().subscribe(userInfo => {
      this.userData$ = this.userDataStore.getUserData$();
      this.bookmark = window.history.state.bookmark;
      if (!window.history.state.bookmark) {
        const bookmarkId = this.route.snapshot.paramMap.get('id');
        this.personalBookmarksService.getPersonalBookmarkById(userInfo.sub, bookmarkId).subscribe((response) => {
          this.bookmark = response;
        });
      }
    });
  }
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/router" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/router
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="600a76e0ed39cd3a1f10b9a6" %}
