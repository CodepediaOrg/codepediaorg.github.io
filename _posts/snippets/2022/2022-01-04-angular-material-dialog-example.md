---
layout: post
title: Angular material dialog example
description: "Angular material dialog example code snippet"
author: ama
permalink: /snippets/602bb538cd228421f08d83a0/angular-material-dialog-example
published: true
snippetId: 602bb538cd228421f08d83a0
categories: [snippets]
tags: [typescript, angular-material, angular, codever-snippets]
---

**Project**: [`codever`](https://github.com/CodeverDotDev/codever)

The following example shows the elements required to implement an angular material dialog to ask the user to log in
on [Codever](https://www.codever.dev), when this is needed (like _following tags_).

First thing add the `MatDialogModule` in the Angular module where you intend to use the dialog,
 and the component holding the dialog body (`LoginRequiredDialogComponent`) to the `entryComponents`

```typescript
@NgModule({
  imports:      [
    //...
    MatDialogModule,
    RouterModule
  ],
  declarations: [
   //....
    LoginRequiredDialogComponent
  ],
  exports: [],
  entryComponents: [
    //....
    LoginRequiredDialogComponent,
  ]
})
export class SharedModule { }
```

Then in a component, where the dialog is launched, e.g `TagComponent`, inject a MatDialog service to open Material Design modal dialogs. Then configure the dialog with the help of [`MatDialogConfig`](https://material.angular.io/components/dialog/api#MatDialogConfig) and `open` it with the component holding the content of the dialog:

```typescript
export class TagComponent implements OnInit {
  constructor(private tagService: TagService,
              //....
              private loginDialog: MatDialog) {
  }

  watchTag() {
    if (!this.userIsLoggedIn) {
      const dialogConfig = new MatDialogConfig();

      dialogConfig.disableClose = true;
      dialogConfig.autoFocus = true;
      dialogConfig.data = {
        message: 'You need to be logged in to follow tags'
      };

      this.loginDialog.open(LoginRequiredDialogComponent, dialogConfig);
    } else {
      this.userDataWatchedTagsStore.watchTag(this.tag);
    }
  }
}
```

Below is the component, `LoginRequiredDialogComponent`, holdingcontent (body) of the dialog. You can reference and access the provided the calling component using the `MAT_DIALOG_DATA` injectable:

```typescript
import { Component, Inject, OnInit } from '@angular/core';
import { MAT_DIALOG_DATA, MatDialogRef } from '@angular/material/dialog';
import { KeycloakService } from 'keycloak-angular';
import { Router } from '@angular/router';
import { KeycloakServiceWrapper } from '../../core/keycloak-service-wrapper.service';

@Component({
  selector: 'app-delete-bookmark-dialog',
  templateUrl: './login-required-dialog.component.html',
  styleUrls: ['./login-required-dialog.component.scss']
})
export class LoginRequiredDialogComponent implements OnInit {

  message: string;

  constructor(
    private keycloakService: KeycloakService,
    private keycloakServiceWrapper: KeycloakServiceWrapper,
    private dialogRef: MatDialogRef<LoginRequiredDialogComponent>,
    private router: Router,
    @Inject(MAT_DIALOG_DATA) data
  ) {
    this.message = data.message || 'You need to be logged in to be able execute this action';
  }

  ngOnInit() {
  }

  login() {
    this.dialogRef.close('LOGIN_CONFIRMED');
    this.keycloakServiceWrapper.login();
  }

  cancel() {
    this.dialogRef.close();
  }

}

```

The `login()` and `cancel()` methods seen in the component before, are triggered from the angular html template:

```typescript
<h2 mat-dialog-title>Login required</h2>

<hr>

<mat-dialog-content>
  <p>{{message}}</p>
</mat-dialog-content>

<hr>

<mat-dialog-actions class="app-dialog-actions">
  <button type="button" class="btn btn-primary btn-sm mr-2" (click)="login()"><i class="fas fa-unlock"></i> Login / Register
  </button>
  <button type="button" class="btn btn-secondary btn-sm" (click)="cancel()">Cancel</button>
</mat-dialog-actions>

```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://material.angular.io/components/dialog/overview" target="_blank" style="font-weight: lighter">
     https://material.angular.io/components/dialog/overview
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="602bb538cd228421f08d83a0" %}
