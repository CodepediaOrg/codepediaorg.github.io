---
layout: post
title: How to embed a youtube video in angular material dialog
description: "Embed a youtube video in an angular material dialog a la bookmarks.dev way"
author: ama
permalink: /ama/how-to-embed-a-youtube-video-in-angular-material-dialog
published: false
categories: [angular]
tags: [angular-material, youtube]
---

I've made it easy recently to recognise the youtube video bookmarks on [www.bookmarks.dev](https://www.bookmarks.dev), by placing the youtube logo before the title 
of the bookmark:
<figure>
  <img src="{{site.url}}/images/posts/embed-youtube-video-in-angular-material-dialog/youtube-bookmark-printscreen.png" alt="Youtube logo before bookmark title"/>
  <figcaption>Or didn't you?</figcaption>
</figure>

Also, when you click on the youtube logo, a dialog will pop where the video is embedded and you can play it directly there.
In this blog post I will show what was required to achieve this. The [www.bookmarks.dev](https://www.bookmarks.dev) website
uses Angular, with angular material and bootstrap.   

<!--more-->

* TOC
{:toc} 



## HTML Template
In the html template I check if the bookmark is a youtube video (`youtubeVideoId` must be present), to display the youtube icon

```html
...
  <h5 class="card-title">
    <i *ngIf="bookmark.youtubeVideoId" class="fab fa-youtube youtube-icon" (click)="playYoutubeVideo(bookmark)" title="Play youtube video"></i>
    <a href="{{bookmark.location}}" target="_blank" [innerHtml]="bookmark.name | highlight: queryText" (click)="onBookmarkLinkClick(bookmark)"></a>
    <sup class="external-link-hint"><i class="fas fa-external-link-alt"></i></sup>
  </h5>
...          
```

## Angular component

Below you can see the method `playYoutubeVideo` from the `AsyncBookmarkListComponent` component, which is triggered when the youtube icon is clicked:

```typescript
  playYoutubeVideo(bookmark: Bookmark) {
    const dialogConfig = new MatDialogConfig();

    dialogConfig.disableClose = false;
    dialogConfig.autoFocus = true;

    let relativeWidth = (this.innerWidth * 80) / 100; // take up to 80% of the screen size
    if (this.innerWidth > 1500) {
      relativeWidth = (1500 * 80 ) / 100;
    } else {
      relativeWidth = (this.innerWidth * 80 ) / 100;
    }

    const relativeHeight = (relativeWidth * 9) / 16 + 120; // 16:9 to which we add 120 px for the dialog action buttons ("close")
    dialogConfig.width = relativeWidth + 'px';
    dialogConfig.height = relativeHeight + 'px';

    dialogConfig.data = {
      bookmark: bookmark
    };

    const dialogRef = this.deleteDialog.open(PlayYoutubeVideoDialogComponent, dialogConfig);
  }
```

As you can see the configuration of the dialog is quite strait forward. For smaller screens (width < 1500px), the dialog
will take up to 80% of the width and the height corresponds to a 16:9 ratio, to which you would add a 120px to properly display
the close button.

The window width (here `innerWidth`) is set in the `ngOnInit()` method of the component:

```typescript
  ngOnInit(): void {
    this.innerWidth = window.innerWidth;
    ...
  }
```

https://angular.io/guide/security#bypass-security-apis

## Angular Material Dialog Integration

Let's focus now on the angular material component. 

### HTML Template

```html
<mat-dialog-content>
  <div class="videoWrapper">
    <iframe [src]='safeUrl' frameborder="0" allowfullscreen></iframe>
  </div>
</mat-dialog-content>

<hr>

<mat-dialog-actions class="app-dialog-actions">
  <button type="button" class="btn btn-primary btn-sm" (click)="close()">Close</button>
</mat-dialog-actions>
```

To systematically block XSS bugs, Angular treats all values as untrusted by default.
 When a value is inserted into the DOM from a template, via property, attribute, style, class binding, or interpolation, Angular sanitizes and escapes untrusted values.

### CSS
```scss
.videoWrapper {
  position: relative;
  padding-bottom: 56.25%; /* 16:9 */
  padding-top: 25px;
  height: 0;
}
.videoWrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```


Links - https://alistapart.com/article/creating-intrinsic-ratios-for-video/
https://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php

### PlayYoutubeVideoDialogComponent

```typescript
import { Component, Inject, OnInit } from '@angular/core';
import { MAT_DIALOG_DATA, MatDialogRef } from '@angular/material';
import { DomSanitizer } from '@angular/platform-browser';
import { Bookmark } from '../../core/model/bookmark';

@Component({
  selector: 'app-play-youtube-video-dialog',
  templateUrl: './play-youtube-video-dialog.component.html',
  styleUrls: ['./play-youtube-video-dialog.component.scss']
})
export class PlayYoutubeVideoDialogComponent implements OnInit {

  bookmark: Bookmark;
  safeUrl: any;

  constructor(
    private dialogRef: MatDialogRef<PlayYoutubeVideoDialogComponent>,
    @Inject(MAT_DIALOG_DATA) data,
    private _sanitizer: DomSanitizer
  ) {
    this.bookmark = data.bookmark;
    this.safeUrl = this._sanitizer.bypassSecurityTrustResourceUrl(`https://www.youtube.com/embed/${this.bookmark.youtubeVideoId}`);
  }

  ngOnInit() {
  }

  close() {
    this.dialogRef.close('Play Youtube Video Closed');
  }

}
```


## Conclusion
 

I would really appreciate if you had a look at the [www.bookmarks.dev](https://www.bookmarks.dev) application and give it a try (you might cannot not use it) 
and star the generated public bookmarks at [https://github.com/CodepediaOrg/bookmarks](https://github.com/CodepediaOrg/bookmarks).
