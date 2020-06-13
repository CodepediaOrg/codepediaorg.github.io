---
layout: post
title: Keyboard shortcuts example with Angular
description: "Setup of hot keys with Angular to access recent and pinned bookmarks on www.bookmarks.dev"
author: ama
permalink: /ama/keyboard-shortcuts-example-with-angular
published: true
categories: [productivity]
tags: [angular, plugins, bookmarksdev]
---

You know how good ideas come out of the blue? Recently it hit me that I could more easily access my bookmarks
history and the pinned bookmarks on [www.bookmarks.dev](https://www.bookmarks.dev) with hot keys. So I sat down and implemented
this feature. This post details how.

 ![Hot Keys on Bookmarks.dev demo](/images/posts/2020-06-12-hotkeys-angular/angular-hotkeys-showcase-1440x900.gif)

<!--more-->

It's not very much. The actual magic happens in this piece of code:
```typescript
import { Component, HostListener } from '@angular/core';

import 'styles.scss';
import { UserDataHistoryStore } from './core/user/userdata.history.store';
import { MatDialog, MatDialogConfig } from '@angular/material/dialog';
import { HotKeysDialogComponent } from './shared/history-dialog/hot-keys-dialog.component';
import { UserDataPinnedStore } from './core/user/userdata.pinned.store';

export class AppComponent {

  url = 'https://www.bookmarks.dev';
  innerWidth: any;
  constructor(private userDataHistoryStore: UserDataHistoryStore,
              private userDataPinnedStore: UserDataPinnedStore,
              private historyDialog: MatDialog) {
    this.innerWidth = 100;
  }

  @HostListener('window:keydown.control.p', ['$event'])
  showPinned(event: KeyboardEvent) {
    event.preventDefault();
    const dialogConfig = new MatDialogConfig();

    dialogConfig.disableClose = false;
    dialogConfig.autoFocus = true;
    dialogConfig.width = this.getRelativeWidth();

    dialogConfig.data = {
      bookmarks$: this.userDataPinnedStore.getPinnedBookmarks$(1),
      title: '<i class="fas fa-thumbtack"></i> Pinned'
    };

    const dialogRef = this.historyDialog.open(HotKeysDialogComponent, dialogConfig);
    dialogRef.afterClosed().subscribe(
      data => {
        console.log('Dialog output:', data);
      }
    );
  }
}
```

and to be more precise in the following three lines of code:

```typescript
  @HostListener('window:keydown.control.p', ['$event'])
  showPinned(event: KeyboardEvent) {
    event.preventDefault();
    //...
    }
```

The `HostListener`[^1] decorator declares a DOM event to listen for. Angular will invoke the `showPinned()` method when
the host emits the key-press event - **Ctrl + P**.

[^1]: <https://angular.io/api/core/HostListener>

The `event.preventDefault()`[^2] method stops the default action of an element from happening, which in this case on Windows
would be print the page, and instead launches an [Angular dialog](https://www.codepedia.org/ama/how-to-embed-a-youtube-video-in-an-angular-material-dialog)
with the pinned bookmarks.

[^2]: <https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault>

The same mechanism applies for the **Ctrl + H** shortcut to show the bookmarks from **History**.


## Conclusion
I've told you it wasn't much, but it's definetely a [piece of code I will bookmark for later](https://dev.to/ama/bookmarking-code-snippets-with-codelets-3d44).

## References
