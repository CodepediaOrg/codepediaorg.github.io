---
layout: post
title: How to save api json response as file in angular
description: "Shows the angular implementation of how to save your bookmarks or snippets on Bookmarks.dev"
author: ama
permalink: /ama/save-file-as-example-in-angular
published: true
categories: [article]
tags: [angular, bookmarks.dev]
---

You can now export your bookmarks and snippets from [Bookmarks.dev](https://www.codever.land) for backup or other purposes.
 You can do that by going to your [dashboard](https://www.codever.land) and click **Export my bookmarks** button.
 A dialog will pop up to ask whether you want to see your bookmarks or snippets in the browser(this opens a new window)
  or save them as a json file:

![Export my snippets](https://i.ibb.co/pZrpBMh/export-my-snippets-from-dashboard-1440x900-optimized.gif)

<!--more-->

### How it is implemented behind the scenes?

> I will show you the implementation for snippets export case. For bookmarks is basically the same.

When you click the **Export my snippets** button:
```html
  <button type="button" class="btn btn-outline-primary" title="Export all my snippets"
          (click)="exportMySnippets()">
    Export my snippets
  </button>
```

the `exportMySnippts()` function is called which calls the backend REST API, where we wait for the response
to then invoke the `downloadFile` with it:
```typescript
  exportMySnippets() {
    this.personalSnippetsService.getAllMySnippets(this.userId).subscribe(data =>
      this.downloadFile(data)
    );
  }
```

In the `downloadFile` function we create a blob[^1] from the data, get the blob's url via `window.URL.createObjectURL(blob)`
 and pass it to the dialog presented to the user:
```typescript
  private downloadFile(data: Snippet[]) {
    const blob = new Blob([JSON.stringify(data, null, 2)], {type: 'application/json'});
    const dialogConfig = new MatDialogConfig();

    dialogConfig.autoFocus = true;
    dialogConfig.data = {
      blobUrl: window.URL.createObjectURL(blob),
      backupType: 'snippets'
    };

    this.backupBookmarksDialog.open(BackupBookmarksDialogComponent, dialogConfig);
  }
```

[^1]: <https://developer.mozilla.org/en-US/docs/Web/API/Blob>

### Angular dialog

In the dialog we have the two buttons:
- one to download the file ("save as" functionality) which is triggered by providing input to the `href` and `download` attributes
of the `a` anchor element
- the **View in browser** functionality which opens a new window with the blob's url we created earlier - `window.open(this.blobUrl);`

```html
<h2 mat-dialog-title>Download personal {{backupType}}</h2>

<hr>

<mat-dialog-actions class="app-dialog-actions">
  <a [href]="sanitizedBlobUrl" [download]="filename" type="button" class="btn btn-primary btn-sm mr-2" (click)="download()"><i class="fas fa-download"></i> Download
  </a>
  <button type="button" class="btn btn-primary btn-sm mr-2" (click)="viewInBrowser()"><i class="fas fa-eye"></i> View in browser
  </button>
  <button type="button" class="btn btn-secondary btn-sm" (click)="close()">Close <i class="fas fa-window-close"></i></button>
</mat-dialog-actions>
```


I had to sanitize the url passed to the `href` attribute of the `a` anchor element in angular, via the `DomSanitizer`
and its method `bypassSecurityTrustUrl`:

```typescript
export class BackupBookmarksDialogComponent implements OnInit {

  backupType: string; // 'bookmarks' | 'snippets';
  blobUrl: any;
  sanitizedBlobUrl: any;
  filename: string;

  constructor(
    private dialogRef: MatDialogRef<BackupBookmarksDialogComponent>,
    private router: Router,
    @Inject(MAT_DIALOG_DATA) data,
    private sanitizer: DomSanitizer
  ) {
    this.sanitizedBlobUrl = this.sanitizer.bypassSecurityTrustUrl(data.blobUrl);
    this.blobUrl = data.blobUrl;
    this.backupType = data.backupType;
    const currentDate = new Date();
    this.filename = `${this.backupType}_${currentDate.toISOString()}.json`;
  }

  ngOnInit() {
  }

  close() {
    this.dialogRef.close();
  }

  download() {
  }

  viewInBrowser() {
    window.open(this.blobUrl);
  }
}
```

> Bypassing built-in security has to be well thought. See [Angular's security guide](https://angular.io/guide/security)
> for more information.


> As an alternative you could also use [FileSave.js](https://github.com/eligrey/FileSaver.js#readme), which ads an HTML5 `saveAs()` FileSave
> implementation, but I wanted to avoid adding another library to the project

## References
