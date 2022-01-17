---
layout: post
title: Izitoast usage angular example
description: "Izitoast usage angular example code snippet"
author: ama
permalink: /snippets/60e57efca9a4e07a75cbd777/izitoast-usage-angular-example
published: true
snippetId: 60e57efca9a4e07a75cbd777
categories: [snippets]
tags: [typescript, javascript, notifications, popup, dialog, modal, angular, codever-snippets]
---

**Project**: `codever` - **File**:  `searchbar.component.ts`

Here used the `success` method with `IziToastSettings`

```typescript
import iziToast, { IziToastSettings } from 'izitoast';

removeSearch(search: Search, e: MouseEvent) {
  e.preventDefault();
  e.stopPropagation();

  const indexSavedSearch = this.getIndexSavedSearch(search.searchDomain, search.text);

  const deletedSearch = this._userData.searches.splice(indexSavedSearch, 1)[0];
  this.userDataStore.updateUserData$(this._userData).subscribe(() => {
    console.log('Removed search ' + search.text + ' from domain ' + search.searchDomain);
    const iziToastSettings: IziToastSettings = {
      title: `"${deletedSearch.text}" deleted from "${deletedSearch.searchDomain}" search history `,
      timeout: 3000,
      position: 'bottomRight'
    }
    iziToast.success(iziToastSettings);
  });
}


```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://izitoast.marcelodolza.com/#Methods" target="_blank" style="font-weight: lighter">
     https://izitoast.marcelodolza.com/#Methods
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="60e57efca9a4e07a75cbd777" %}
