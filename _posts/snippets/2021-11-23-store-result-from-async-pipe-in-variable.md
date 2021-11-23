---
layout: post
title: Store result from async pipe in variable
description: "Store result from async pipe in variable  code snippet"
author: ama
permalink: /snippets/602664d8cd228421f08d6d9d/store-result-from-async-pipe-in-variable
published: true
snippetId: 602664d8cd228421f08d6d9d
categories: [snippets]
tags: [angular, html-template, codever-snippets]
---

**Project**: [`codever`](https://github.com/codeverland/codever) - **File**:  `snippet-details.component.html`

Use the `async` pipe and store it in variable for later use `*ngIf="(snippet$ | async) as snippet"`. Below the complete snippet example:

```html
<div *ngIf="(snippet$ | async) as snippet" class="card">
  <div class="card-body show-hide">
    <div class="header-wrapper">
      <div class="titles">
        <h4 *ngIf='inlist; else hyperLinkTitle' class="card-title">
             <span *ngIf="snippet.public; else linkToPrivateSnippet">
                <i class="fas fa-eye fa-xs mr-2" title="Public snippet"></i>
                <a routerLink="/snippets/{{snippet._id}}/details"
                   [innerHtml]="snippet.title | highlight: queryText"></a> </span>
          <ng-template #linkToPrivateSnippet>
                <span>
                  <i class="fas fa-eye-slash fa-xs mr-2" title="Private snippet"></i>
                  <a routerLink="/my-snippets/{{snippet._id}}/details"
                     [innerHtml]="snippet.title | highlight: queryText"></a>
                </span>
          </ng-template>
        </h4>
        <ng-template #hyperLinkTitle>
          <h4 class="card-title">
            <i *ngIf="snippet.public; else linkToPrivateSnippet" class="fas fa-eye fa-xs mr-1"
               title="Public snippet"></i>
            <ng-template #linkToPrivateSnippet>
              <i class="fas fa-eye-slash fa-xs mr-1" title="Private snippet"></i>
            </ng-template>
            {{snippet.title}} <span *ngIf="snippet.public === false; else publicPill"
                                    class="badge badge-pill badge-light ml-3 font-weight-normal">Private</span>
            <ng-template #publicPill>
              <span class="badge badge-pill badge-light ml-3  font-weight-normal">Public</span>
            </ng-template>
          </h4>
        </ng-template>
        <h6 class="card-subtitle mb-2 text-muted url-under-title">
          <span *ngIf="snippet.sourceUrl"><strong>Ref</strong> -
            <span
              *ngIf="snippet.sourceUrl.startsWith('http:') || snippet.sourceUrl.startsWith('https:'); else justText">
              <a href="{{snippet.sourceUrl}}" target="_blank">{{snippet.sourceUrl}}</a>
              <sup class="ml-1"><i class="fas fa-external-link-alt"></i></sup>
            </span>
            <ng-template #justText>
              <span>{{snippet.sourceUrl}}</span>
            </ng-template>
            </span>
        </h6>
      </div>
    </div>

    <hr class="title-content-separator">

    <app-codelet-card-body [codelet]="snippet" [queryText]="queryText" [inList]="inlist"></app-codelet-card-body>

  </div>

</div>

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/api/common/AsyncPipe" target="_blank" style="font-weight: lighter">
     https://angular.io/api/common/AsyncPipe
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="602664d8cd228421f08d6d9d" %}
