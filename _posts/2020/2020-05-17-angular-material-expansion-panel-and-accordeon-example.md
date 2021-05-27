---
layout: post
title: Angular material expansion panel and accordion example
description: "Presents angular material expansion panel and accordion in action at https://www.codever.land with
source code and notes"
author: ama
permalink: /ama/angular-material-expansion-panel-and-accordion-example
published: true
categories: [snippets]
tags:
    - angular-material
    - angular
---

I find accordions pretty well suited for FAQs or HowTo pages. That's why I chose one for the [HowTo](https://www.codever.land/howto) page of
[www.codever.land](https://www.codever.land/), which is implemented with [angular material expansion panel and
accordion](https://material.angular.io/components/expansion/overview).

 ![HowTo Accordion Showcase](/images/posts/2020-05-17-angular-material-expansion-panel-and-accordeon-example/howto-accordeon-showcase.gif)

This blog post presents the source code for that with a couple of notes.

{% include source-code-bookmarks.dev.html %}

<!--more-->

## The source code

```html
  <mat-accordion>
    <mat-expansion-panel>
      <mat-expansion-panel-header>
        <h4><i class="fas fa-xs fa-info-circle"></i> Get started</h4>
      </mat-expansion-panel-heade}r>

      <ng-template matExpansionPanelContent>
        <app-howto-get-started></app-howto-get-started>
      </ng-template>
    </mat-expansion-panel>

    <mat-expansion-panel>
      <mat-expansion-panel-header>
        <h4><i class="fas fa-xs fa-info-circle"></i> Save bookmarks</h4>
      </mat-expansion-panel-header>
      <ng-template matExpansionPanelContent>
        <app-howto-save></app-howto-save>
      </ng-template>
    </mat-expansion-panel>

    <mat-expansion-panel>
      <mat-expansion-panel-header>
        <h4><i class="fas fa-xs fa-info-circle"></i> Search bookmarks</h4>
      </mat-expansion-panel-header>
      <ng-template matExpansionPanelContent>
        <app-howto-search></app-howto-search>
      </ng-template>
    </mat-expansion-panel>

    <mat-expansion-panel>
      <mat-expansion-panel-header>
        <h4><i class="fas fa-xs fa-info-circle"></i> Bookmarklets</h4>
      </mat-expansion-panel-header>
      <ng-template matExpansionPanelContent>
        <app-howto-bookmarklets></app-howto-bookmarklets>
      </ng-template>
    </mat-expansion-panel>

    <mat-expansion-panel>
      <mat-expansion-panel-header>
        <h4><i class="fas fa-xs fa-info-circle"></i> Codelets</h4>
      </mat-expansion-panel-header>
      <ng-template matExpansionPanelContent>
        <app-howto-codelets></app-howto-codelets>
      </ng-template>
    </mat-expansion-panel>
  </mat-accordion>
```

## Notes

 * the `mat-expansion-panel` components are encapsulated in an `mat-accordion` element
 * the code itself for the several sections is encapsulated in own component, for better code readability and to access
 them directly
   * [HowTo - Get started](https://www.codever.land/howto/get-started)
   * [HowTo - Save bookmarks](https://www.codever.land/howto/save)
   * [HowTo - Search bookmarks](https://www.codever.land/howto/search)
   * [HowTo - Bookmarklets](https://www.codever.land/howto/bookmarklets)
   * [HowTo - Codelets](https://www.codever.land/howto/codelets)
 * the construct `ng-template` with the `matExpansionPanelContent` attribute in the  is used to defer initialization until the panel is open.
  By default, the expansion panel content will be initialized even when the panel is closed
 * by setting the input ``multi="true"`` (default `false`) on `mat-accordion` you could allow the expansions state to
 be set independently of each other

{% include action-to-star-bookmarksdev-on-github.html %}
