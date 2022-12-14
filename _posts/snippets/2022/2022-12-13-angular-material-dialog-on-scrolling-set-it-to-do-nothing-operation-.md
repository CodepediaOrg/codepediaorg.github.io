---
layout: post
title: Do nothing on Angular material dialog scrolling
description: "Do nothing on Angular material dialog scrolling code snippet"
author: ama
permalink: /snippets/63983f3ed00726522c6d0b5a/angular-material-dialog-on-scrolling-set-it-to-do-nothing-operation-
published: true
snippetId: 63983f3ed00726522c6d0b5a
categories: [snippets]
tags: [angular, angular-material, codever-snippets]
---

If you are using Angular Material Dialogs for example and when it pops up the page seems to move a bit, well that
might be due to the scrolling strategy that the dialog is using. If you don't want to see this annoying effect you can
use the `NoopScrollStrategy` as `ScrollStrategy`, thus there is no influence on scrolling when the angular material dialog is opened.

One way to do that, is to set it via `ScrollStrategyOptions` to replace the default behaviour.
First define a `scrollStrategy` variable and set the value to `NoopScrollStrategy` by calling the `noop()` of the `ScrollStrategyOptions` in `OnInit`:

```typescript
export class AppComponent implements OnInit {
  scrollStrategy: ScrollStrategy;
  constructor(private keycloakService: KeycloakService,
              //...
              private historyDialog: MatDialog,
              private loginDialog: MatDialog,
              private loginDialogHelperService: LoginDialogHelperService,
              private readonly  scrollStrategyOptions: ScrollStrategyOptions) {}

  ngOnInit(): void {
      this.scrollStrategy = this.scrollStrategyOptions.noop();
  }
```

Then in the dialog configuration (`MatDialogConfig`) set the `scrollStrategy` property to this component variable
- `dialogConfig.scrollStrategy = this.scrollStrategy;` :

```typescript
  @HostListener('window:keydown.control.h', ['$event'])
  showHistory(event: KeyboardEvent) {
    if (!this.userIsLoggedIn) {
      const dialogConfig = this.loginDialogHelperService.loginDialogConfig('You need to be logged in to see the History Bookmarks popup');

      this.loginDialog.open(LoginRequiredDialogComponent, dialogConfig);
    } else {
      event.preventDefault();
      const dialogConfig = new MatDialogConfig();

      dialogConfig.disableClose = false;
      dialogConfig.autoFocus = true;
      dialogConfig.width = this.getRelativeWidth();
      dialogConfig.height = this.getRelativeHeight();
      dialogConfig.scrollStrategy = this.scrollStrategy;
      dialogConfig.data = {
        bookmarks$: this.userDataHistoryStore.getAllHistory$(this.userId),
        title: '<i class="fas fa-history"></i> History'
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

For the **login required dialog**,
this option is injected via a service - `const dialogConfig = this.loginDialogHelperService.loginDialogConfig('You need to be logged in to see the History Bookmarks popup');`,
but it follows basically the same principle:

```typescript
@Injectable()
export class LoginDialogHelperService {

  scrollStrategy: ScrollStrategy;

  constructor(private readonly scrollStrategyOptions: ScrollStrategyOptions) {
    this.scrollStrategy = this.scrollStrategyOptions.noop();
  }

  loginDialogConfig(message: string) {
    const dialogConfig = new MatDialogConfig();

    dialogConfig.disableClose = true;
    dialogConfig.autoFocus = true;
    dialogConfig.scrollStrategy = this.scrollStrategy;
    dialogConfig.data = {
      message: message
    };

    return dialogConfig;
  }
}
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://material.angular.io/cdk/overlay/api#ScrollStrategyOptions" target="_blank" style="font-weight: lighter">
     https://material.angular.io/cdk/overlay/api#ScrollStrategyOptions
  </a>
</span>

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="63983f3ed00726522c6d0b5a" %}
