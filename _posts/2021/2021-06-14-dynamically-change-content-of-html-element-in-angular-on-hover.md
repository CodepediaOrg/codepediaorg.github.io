---
layout: post
title: How to change the content of a html element on hover in Angular
description: "Example showing how to dynamically change the displayed content of an anchor html element when hovering "
author: ama
permalink: /ama/how-to-change-the-content-of-html-element-on-hover-in-angular
published: true
categories: [tutorial]
tags: [angular, css, html, codever]
---

## Use case
I find my self often use the autocomplete function of my last searches in the search box to find and access bookmarks and snippets I recently
looked for. I figured out since I do that, why not make it easier. So I did - I added sort of quick access to my
last searches directly from the side menu in the landing page. See it in action below:

{% include image.html url="https://raw.githubusercontent.com/codeverland/codever/master/documentation/gif/how-to/my-last-searches-quick-access.gif" description="Quick access to My Last Searches animation" %}

One tricky part was to show the whole content of the search when its length passes a certain limit. That meant
when hovering dynamically changing the content of the html anchor `a` element. This was even more difficult, because
the list is dynamically generated out of the last searches. Let's see how I implemented that.

<!--more-->

## Implementation

First I initialise the elements of the booleans list `hoveringLastSearches` to `false`.
We also need a method - `hovering()`, which returns the boolean value of an element from the list given its index:

```typescript
export class AppComponent implements OnInit {

 //... other code parts removed for brevity

  private hoveringLastSearches: boolean[] = [];

  ngOnInit(): void {
    this.keycloakService.isLoggedIn().then(isLoggedIn => {
        //... other code parts removed for brevity
        this.latestSearches$ = this.userDataStore.getUserData$().pipe(
          map(userData => {
            for (let i = 0; i < 10; i++) {
              this.hoveringLastSearches.push(false);
            }
            return userData.searches.slice(0, 10);
          })
        )
      }
    });
  }

 //... other code parts removed for brevity

  hovering(i: number): boolean {
    return this.hoveringLastSearches[i];
  }

}
```

In the html template, when hovering over the element (`mouseover` event) or when exiting the element (`mouseout`) I set
the corresponding `boolean` value to `true`, respectively to `false` of the corresponding element in the list.
 Then when displaying the content, depending on the result of the `hovering(index)` result I use the longer,
  respectively the short version of the text:

```html
    <ng-container *ngFor="let mySearch of latestSearches; let i = index" class="mt-1">
        <span class="mt-1 mr-2">
            <a
              (mouseover)="hoveringLastSearches[i]=true"
              (mouseout)="hoveringLastSearches[i]=false"
              [routerLink]="['/search']"
              [queryParams]="{q: mySearch.text, sd: mySearch.searchDomain }"
              class="badge badge-secondary mb-1  ml-2"
              [class.my-snippets-last-search]="mySearch.searchDomain === 'my-snippets'"
              [class.public-snippets-last-search]="mySearch.searchDomain === 'public-snippets'"
              [class.my-bookmarks-last-search]="mySearch.searchDomain === 'my-bookmarks'"
              [class.public-bookmarks-last-search]="mySearch.searchDomain === 'public-bookmarks'"
              title="{{'Search in ' + mySearch.searchDomain + ': ' + mySearch.text}}"
            >  <i class="fa fa-xs fa-search mr-1"></i>
              <span
                *ngIf="!hovering(i); else longVersion">{{mySearch.text.length > 20 ? mySearch.text.substring(0, 20) + '...' : mySearch.text}}</span>
              <ng-template #longVersion>{{mySearch.text}}</ng-template>
              <i *ngIf="mySearch.searchDomain === 'my-snippets' || mySearch.searchDomain === 'public-snippets'"
                 class="fa fa-xs fa-code ml-1"></i>
              <i *ngIf="mySearch.searchDomain === 'my-bookmarks' || mySearch.searchDomain === 'public-bookmarks'"
                 class="fa fa-xs fa-bookmark ml-1"></i>
            </a>
        </span>
    </ng-container>
```



