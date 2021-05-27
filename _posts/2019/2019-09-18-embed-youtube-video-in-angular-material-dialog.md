---
layout: post
title: How to embed a youtube video in an angular material dialog
description: "A simple solution to embed a youtube video in an angular material dialog, as currently used on bookmarks.dev"
author: ama
permalink: /ama/how-to-embed-a-youtube-video-in-an-angular-material-dialog
published: true
categories: [tutorial]
tags:
    - angular
    - angular-material
    - youtube
---

It's easy now to recognise the youtube video bookmarks on [www.codever.land](https://www.codever.land), by placing the youtube logo before the title
of the bookmark:
<figure>
  <img src="{{site.url}}/images/posts/embed-youtube-video-in-angular-material-dialog/youtube-bookmark-printscreen.png" alt="Youtube logo before bookmark title"/>
  <figcaption>Bookmarks list with youtube logo</figcaption>
</figure>

Also, when you click on the youtube logo, a dialog will pop where the video is embedded and you can play it directly there.
In this blog post I will show how it is implemented.

> As a reminder, the [www.codever.land](https://www.codever.land) website
uses Angular, with angular material and bootstrap for styling.

<!--more-->

* TOC
{:toc}


## The Bookmarks List Component

### HTML Template

In the bookmarks list's html template I check if the bookmark is a youtube video (a `youtubeVideoId` must be present), to display the youtube icon

```html
...
  <h5 class="card-title">
    <i *ngIf="bookmark.youtubeVideoId" class="fab fa-youtube youtube-icon" (click)="playYoutubeVideo(bookmark)" title="Play youtube video"></i>
    <a href="{{bookmark.location}}" target="_blank" [innerHtml]="bookmark.name | highlight: queryText" (click)="onBookmarkLinkClick(bookmark)"></a>
    <sup class="external-link-hint"><i class="fas fa-external-link-alt"></i></sup>
  </h5>
...
```

### The Angular Bookmarks List Component - **AsyncBookmarkListComponent**

If the youtube icon is present, when clicked it triggers the method `playYoutubeVideo` int the `AsyncBookmarkListComponent` component:

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

The configuration of the dialog is quite strait forward. For smaller screens (`width < 1500px`), the dialog
will take up to 80% of the width and the height corresponds to a **16:9** ratio, to which you would add a 120px to properly display
the close button. For bigger screen we have fixed values which have the same ratio.

The window width (here `innerWidth`) is set in the `ngOnInit()` method of the component:

```typescript
  ngOnInit(): void {
    this.innerWidth = window.innerWidth;
    ...
  }
```

## Angular Material Dialog Integration

Let's focus now on the angular material component that displays the youtube video.

### HTML Template

The youtube video is embedded via an iframe.

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

> To systematically block XSS bugs, Angular treats all values as untrusted by default. So the iframe url needs to be [marked as safe](https://angular.io/guide/security#bypass-security-apis), to avoid being sanitized by Angular.

### Dialog Component - `PlayYoutubeVideoDialogComponent`

The iframe's url (`safeUrl`) is marked as trusted by injecting the `DomSanitizer` and calling its `bypassSecurityTrustResourceUrl` method:

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

### CSS

To display the video in a "fluid" manner in the iframe, we use a solution pioneered by Thierry Koblentz
 and presented on A List Apart in 2009: [Creating Intrinsic Ratios for Video](https://www.alistapart.com/articles/creating-intrinsic-ratios-for-video/):

> "The idea is to create a box with the proper ratio ( 16:9, etc.), then make the video inside that box stretch to fit the dimensions of the box. It’s that simple."
I recommend you read the whole article to understand the details.

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

## Conclusion
Well it as simple as that. The whole code source for this for [www.codever.land](https://www.codever.land) is available on [Github](https://github.com/codeverland/codever).

I would really appreciate if you gave [www.codever.land](https://www.codever.land) a try
and have a look at the generated public bookmarks at [https://github.com/codeverland/bookmarks](https://github.com/codeverland/bookmarks).
