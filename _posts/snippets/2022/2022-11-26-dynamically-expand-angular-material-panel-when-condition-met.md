---
layout: post
title: Dynamically expand angular material panel when condition met
description: "Dynamically expand angular material panel when condition met code snippet"
author: ama
permalink: /snippets/63820b10944cbd1faacdea21/dynamically-expand-angular-material-panel-when-condition-met
published: true
snippetId: 63820b10944cbd1faacdea21
categories: [snippets]
tags: [angular, angular-material, html, html-template, codever-snippets]
---

Use the `expanded` attribute of the `mat-expansion-panel` element and when condition is met it set to `true`. In the following example a filter pipe it is used and [the result is placed in the `filteredBookmarks` variable](https://www.codever.land/snippets/63806e79944cbd1faacdda84/details) which is checked in the condition - `[expanded]="filteredBookmarks.length === 1"` :

```html
<mat-expansion-panel
  *ngFor="let bookmark of bookmarks | bookmarkFilter: filterText as filteredBookmarks; index as i"
  [expanded]="filteredBookmarks.length === 1">
  <mat-expansion-panel-header *ngIf="i<15">
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
    <app-bookmark-text [bookmark]="bookmark" [showMoreText]="true"
                       (click)="addToHistoryService.onClickInDescription(true, $event, bookmark)"
                       (auxclick)="addToHistoryService.onMiddleClickInDescription(true, $event, bookmark)">
    </app-bookmark-text>
  </ng-template>
</mat-expansion-panel>
```

> **Project**: `codever` - **File**:  `hot-keys-dialog.component.html`

See it in action at [www.codever.land](https://www.codever.land):

![Copy-to-clipboard-demo](/images/posts/2022-11-26-expand-angular-material-accordeon-dynamically/codever-automatic-expander-filter-one-result.gif)

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="63820b10944cbd1faacdea21" %}
