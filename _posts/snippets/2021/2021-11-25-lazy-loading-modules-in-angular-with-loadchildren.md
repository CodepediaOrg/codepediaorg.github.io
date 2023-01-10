---
layout: post
title: Lazy loading modules in Angular with loadChildren
description: "Lazy loading modules in Angular with loadChildren code snippet"
author: ama
permalink: /snippets/602675a61a93236e029a1f79/lazy-loading-modules-in-angular-with-loadchildren
published: true
snippetId: 602675a61a93236e029a1f79
categories: [snippets]
tags: [typescript, angular, angular-routing, codever-snippets]
---

**Project**: [`codever`](https://github.com/CodeverDotDev/codever) - **File**:  `app.routing.ts`

To lazy load Angular modules, use `loadChildren` (instead of component) in your AppRoutingModule routes configuration as follows.


```typescript
import { RouterModule, Routes } from '@angular/router';
import { NgModule } from '@angular/core';
import { PageNotFoundComponent } from './not-found.component';
import { SnippetNotFoundComponent } from './not-found/snippet-not-found.component';

const routes: Routes = [
  {
    path: 'personal',
    loadChildren: () => import('app/personal/personal-bookmarks.module').then(m => m.PersonalBookmarksModule)
  },
  {
    path: 'dashboard',
    loadChildren: () => import('app/user/dashboard/user-dashboard.module').then(m => m.UserDashboardModule)
  },
  {
    path: 'settings',
    loadChildren: () => import('app/user-settings/user-settings.module').then(m => m.UserSettingsModule)
  },
  {
    path: 'public',
    loadChildren: () => import('app/public/public.module').then(m => m.PublicBookmarksModule)
  },
  {
    path: 'my-snippets',
    loadChildren: () => import('app/codelet/codelet.module').then(m => m.CodeletModule)
  },
  {
    path: 'my-codelets',
    redirectTo: 'my-snippets',
  },
  {
    path: 'search',
    loadChildren: () => import('app/search-results/search-results.module').then(m => m.SearchResultsModule)
  },
  {
    path: '',
    redirectTo: 'public',
    pathMatch: 'full'
  },
  {path: '404-snippet', component: SnippetNotFoundComponent},
  {path: '**', component: PageNotFoundComponent}
];


/**
 * See App routing @https://angular.io/docs/ts/latest/guide/ngmodule.html
 */
@NgModule({
  imports: [
    RouterModule.forRoot(
      routes
    )
  ],
  exports: [RouterModule]
})
export class AppRoutingModule {
}
```

Where a child module might look something like the `SearchResultsModule`:

```typescript
const searchResultsRoutes: Routes = [
  {
    path: '',
    component: SearchResultsComponent
  }
];

@NgModule({
  declarations: [SearchResultsComponent],
  imports: [
    RouterModule.forChild(searchResultsRoutes),
    CommonModule,
    CodeletModule,
    SharedModule,
    MatTabsModule,
  ]
})
export class SearchResultsModule { }
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/lazy-loading-ngmodules" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/lazy-loading-ngmodules
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="602675a61a93236e029a1f79" %}
