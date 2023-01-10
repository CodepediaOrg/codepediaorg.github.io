---
layout: post
title: Sanitize url in angular example
description: "Sanitize url in angular example code snippet"
author: ama
permalink: /snippets/6060d95b967b5a363dfa8f75/sanitize-url-in-angular-example
published: true
snippetId: 6060d95b967b5a363dfa8f75
categories: [snippets]
tags: [typescript, angular, href, codever-snippets]
---

**Project**: [`codever`](https://www.codever.dev) - **File**:  `backup-bookmarks-dialog.component.ts`

Use the `DomSanitizer` and its method `bypassSecurityTrustUrl` as shown in the example below.
 Thus, you don't get the `unsafe` prefix in your generated html

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
```

In the html component the `sanitizedBlogUrl` is injected in the `href` attribute of the `a` html element


```html
  <a [href]="sanitizedBlobUrl" [download]="filename" type="button" class="btn btn-primary btn-sm mr-2" (click)="download()"><i class="fas fa-download"></i> Download
  </a>
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/api/platform-browser/DomSanitizer" target="_blank" style="font-weight: lighter">
     https://angular.io/api/platform-browser/DomSanitizer
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6060d95b967b5a363dfa8f75" %}
