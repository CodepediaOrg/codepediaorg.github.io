---
layout: post
title: How to filter results list in Angular with input text
description: "Real life example showing how to filter results list in Angular and its implementation with Angular pipes."
author: ama
permalink: /ama/how-to-filter-results-in-angular-example-with-input-text
published: true
categories: [article]
tags:
    - bookmarks.dev
    - angular
---

I find myself lately looking for recent visited bookmarks via the [Ctrl+h - History dialog]({% post_url 2020-06-12-hotkeys-angular-example %})
on [Bookmarks.dev](https://www.codever.land). To make my life even easier, I added a filter box in the dialog.
 You can now add one or more keywords to filter the <span class="highlight-yellow">displayed results</span> even further.
  One thing let to another and I have added the filter box to other bookmarks lists, like [Pinned](https://www.codever.land/?tab=history),
  [ReadLater](https://www.codever.land/?tab=read-later) or [My Dashboard](https://www.codever.land/dashboard):

![show filter animation](https://i.ibb.co/sjsZ5NL/show-filter-possibilities-800x500-from-ezgif.gif)

In this blog post I will present the implementation in Angular required to achieve this new feature.

{% include source-code-bookmarks.dev.html %}

<!--more-->

## The bookmarks filter pipe code
The easiest way to achieve the filtering functionality is to use an angular pipe[^1]:

```html
<mat-expansion-panel *ngFor="let bookmark of bookmarks | bookmarkFilter: filterText">
```

[^1]: <https://angular.io/guide/pipes>

The complete implementation of the pipe is:

```typescript
// bookmarks-filter.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';
import { Bookmark } from '../core/model/bookmark';

@Pipe({name: 'bookmarkFilter'})
export class BookmarksFilterPipe implements PipeTransform {
  /**
   * Bookmarks in, bookmarks out that contain all the terms in the filterText
   *
   * @param {Bookmark[]} bookmarks
   * @param {string} filterText
   * @returns {Bookmark[]}
   */
  transform(bookmarks: Bookmark[], filterText: string): Bookmark[] {
    if (!bookmarks) {
      return [];
    }
    if (!filterText) {
      return bookmarks;
    }

    return bookmarks.filter(bookmark => {
      return this.bookmarkContainsFilterText(bookmark, filterText);
    });
  }

  private bookmarkContainsFilterText(bookmark: Bookmark, filterText): boolean {
    filterText = filterText.toLocaleLowerCase();
    const filterTerms = filterText.split(' ');
    for (const filterTerm of filterTerms) {
      const hasFilterTerm = this.bookmarkContainsFilterTerm(bookmark, filterTerm);
      if (hasFilterTerm === false) {
        return false;
      }
    }

    return true;
  }

  private tagsHaveFilterText(tags: string[], filterText: string): boolean {
    for (const tag of tags) {
      if (tag.includes(filterText)) {
        return true;
      }
    }

    return false;
  }

  private bookmarkContainsFilterTerm(bookmark: Bookmark, filterTerm: string) {
    return bookmark.name.toLocaleLowerCase().includes(filterTerm)
      || bookmark.location.toLocaleLowerCase().includes(filterTerm)
      || bookmark.description.toLocaleLowerCase().includes(filterTerm)
      || this.tagsHaveFilterText(bookmark.tags, filterTerm);
  }
}
```

It checks if the bookmarks contains **all** the filter terms provided in the `filterText` either in title, location, tags or description
of the bookmark.

> The `filterText` it's split into terms by spaces and **all terms must be present**

### Usage in the Angular History Dialog Component
Below is the complete usage of the `bookmarkFilter` in the history dialog html component:

```html
<!--
 hot-keys-dialog.component.html
-->
<div class="dialog-title">
  <h2 mat-dialog-title [innerHTML]="title"></h2>
  <div class="form-group has-search">
    <span class="fas fa-filter form-control-feedback"></span>
    <input type="search" [(ngModel)]="filterText" class="form-control" placeholder="Filter...">
  </div>
</div>
<mat-dialog-content *ngIf="(bookmarks$ | async) as bookmarks" class="mt-2 pt-1 pb-1">
  <mat-accordion>
    <mat-expansion-panel *ngFor="let bookmark of bookmarks | bookmarkFilter: filterText">
      <mat-expansion-panel-header>
        <div class="p-3">
          <h5 class="card-title">
            <a href="{{bookmark.location}}"
               [innerHTML]="bookmark.name | slice:0:100 | highlightHtml: filterText"
               target="_blank"
               (click)="addToHistoryService.promoteInHistoryIfLoggedIn(true, bookmark)"
               (auxclick)="addToHistoryService.onMiddleClickInDescription(true, $event, bookmark)"
            >
              {{"see innerhtml"}}
            </a>
            <sup class="external-link-hint"><i class="fas fa-external-link-alt"></i></sup>
          </h5>
          <h6 class="card-subtitle mb-2 text-muted url-under-title"
              [innerHTML]="bookmark.location | slice:0:120 | highlightHtml: filterText"
          >
            {{"see innerhtml"}}
          </h6>
        </div>
      </mat-expansion-panel-header>

      <ng-template matExpansionPanelContent>
        <app-bookmark-text [bookmark]="bookmark"
                           (click)="addToHistoryService.onClickInDescription(true, $event, bookmark)"
                           (auxclick)="addToHistoryService.onMiddleClickInDescription(true, $event, bookmark)">
        </app-bookmark-text>
      </ng-template>
    </mat-expansion-panel>
  </mat-accordion>
</mat-dialog-content>
```

The `filterText` variable is a two-way bounded variable - `<input type="search" [(ngModel)]="filterText" class="form-control" placeholder="Filter...">`.
The value of the html input is filter parameter filter of the bookmark filter pipeline as seen
before - `transform(bookmarks: Bookmark[], filterText: string): Bookmark[]`.

> Note the `highlight` pipe chained to highlight the search terms: `[innerHTML]="bookmark.name | slice:0:100 | highlightHtml: filterText"`

In the component `filterText` is defined as a simple string variable:

```typescript
export class HotKeysDialogComponent implements OnInit {

  bookmarks$: Observable<Bookmark[]>;
  title: string;
  filterText: '';

  constructor(
    private dialogRef: MatDialogRef<HotKeysDialogComponent>,
    public addToHistoryService: AddToHistoryService,
    @Inject(MAT_DIALOG_DATA) data
  ) {
    this.bookmarks$ = data.bookmarks$;
    this.title = data.title;
  }

  ngOnInit() {
  }
}
```

## Bonus: the Highlight Pipe
You can find below the implementation of the Highlight pipe, which highlights the filter terms in the history dialog:
```typescript
import {Pipe} from '@angular/core';
import {PipeTransform} from '@angular/core';

@Pipe({ name: 'highlightHtml' })
export class HighLightHtmlPipe implements PipeTransform {

  transform(text: string, search): string {
    if (!search || search === undefined) {
      return text;
    } else {
      let pattern = search.replace(/[\-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, '\\$&');
      pattern = pattern.split(' ').filter((t) => {
        return t.length > 0;
      }).join('|');
      pattern = '(' + pattern + ')' + '(?![^<]*>)';
      const regex = new RegExp(pattern, 'gi');

      return search ? text.replace(regex, (match) => `<span class="highlight">${match}</span>`) : text;
    }
  }

}
```

## Conclusion
In this post you've seen a way to dynamically filter a list of elements in Angular with the helps of pipes.

{% include action-to-star-bookmarksdev-on-github.html %}

## References


