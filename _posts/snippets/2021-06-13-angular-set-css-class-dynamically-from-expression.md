---
layout: post
title: Set css class dynamically in Angular from expression
description: "Code snippets showing two ways you can dynamically add css classes to html elements in Angular"
author: ama
permalink: /snippets/60c5de1876edda2bb1381cba/set-css-class-dynamically-in-angular-from-expression
snippetId: 60c5de1876edda2bb1381cba
published: true
categories: [snippets]
tags: [angular, css, html, codever-snippets]
---

There are times when you need to set a `css` class of a html element dynamically in angular, given a simple or complex condition.
This was the case for me when I had to set a different `backround-color` and `color` attributes for an anchor `a` html element
to display the **latest searches** on the landing page of [Codever](https://www.codever.land)

<!--more-->

## Use `ngClass` (before)

You can do it by setting the desired css class directly to the [`ngClass`](https://angular.io/api/common/NgClass) input
 of the element. The thing is if the condition gets complex, as in my case where I used initially combined `?:` construct
 it can get hard to read and understand:

```html
<a
    (mouseover)="hoveringLastSearches[i]=true"
    (mouseout)="hoveringLastSearches[i]=false"
    [routerLink]="['/search']"
    [queryParams]="{q: mySearch.text, sd: mySearch.searchDomain }"
    class="badge badge-secondary mb-1  ml-1"
    [ngClass]="mySearch.searchDomain === 'my-snippets' ? 'snippet-last-search' : mySearch.searchDomain === 'my-bookmarks' ? 'my-bookmarks-last-search' : 'public-bookmarks-last-search' "
    title="{{'Search in ' + mySearch.searchDomain + ': ' + mySearch.text}}"
> <i class="fa fa-xs fa-search mr-1"></i>
    <span *ngIf="!hovering(i); else longVersion">{{mySearch.text.length > 20 ? mySearch.text.substring(0,20) + '...' : mySearch.text}}</span>
    <ng-template #longVersion>{{mySearch.text}}</ng-template>
    <i *ngIf="mySearch.searchDomain === 'my-snippets' || mySearch.searchDomain === 'public-snippets'"
       class="fa fa-xs fa-code ml-1"></i>
    <i *ngIf="mySearch.searchDomain === 'my-bookmarks' || mySearch.searchDomain === 'public-bookmarks'"
       class="fa fa-xs fa-bookmark ml-1"></i>
</a>
```

## Set the css class directly to `[class]` input (after)

The second option, which I used eventually is to set your css class to the `[class]` attribute of the html element
with the following syntax:

```html
<a
    (mouseover)="hoveringLastSearches[i]=true"
    (mouseout)="hoveringLastSearches[i]=false"
    [routerLink]="['/search']"
    [queryParams]="{q: mySearch.text, sd: mySearch.searchDomain }"
    class="badge badge-secondary mb-1  ml-1"
    [class.my-snippets-last-search]="mySearch.searchDomain === 'my-snippets'"
    [class.public-snippets-last-search]="mySearch.searchDomain === 'public-snippets'"
    [class.my-bookmarks-last-search]="mySearch.searchDomain === 'my-bookmarks'"
    [class.public-bookmarks-last-search]="mySearch.searchDomain === 'public-bookmarks'"
    title="{{'Search in ' + mySearch.searchDomain + ': ' + mySearch.text}}"
> <i class="fa fa-xs fa-search mr-1"></i>
    <span *ngIf="!hovering(i); else longVersion">{{mySearch.text.length > 20 ? mySearch.text.substring(0,20) + '...' : mySearch.text}}</span>
    <ng-template #longVersion>{{mySearch.text}}</ng-template>
    <i *ngIf="mySearch.searchDomain === 'my-snippets' || mySearch.searchDomain === 'public-snippets'"
       class="fa fa-xs fa-code ml-1"></i>
    <i *ngIf="mySearch.searchDomain === 'my-bookmarks' || mySearch.searchDomain === 'public-bookmarks'"
       class="fa fa-xs fa-bookmark ml-1"></i>
</a>
```

In my opinion the latter is easier to grasp.

You can see the how it looks in action in the following animation:

{% include image.html url="https://raw.githubusercontent.com/codeverland/codever/master/documentation/gif/how-to/my-last-searches-quick-access.gif" description="Quick access to My Last Searches animation" %}

 {% include snippet-post-recommendation-ending.html snippetId="60c5de1876edda2bb1381cba" %}
