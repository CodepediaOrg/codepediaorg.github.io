---
layout: post
title: Angular inheritance example
description: ""
author: ama
permalink: /ama/making-a-chrome-extension-to-save-bookmarks-cross-browser-compatible
published: false
categories: [tutorial]
tags:
    - angular
    - inheritance
    - refactoring
    - bookmarks.dev
---

For the create and update form of code snippets I had previously only one class. But when introducing the **Copy to mine**
functionality the logic has become too intertwined for me to bear so I decided to split the functionality in two parts
- one for handling updating and copy to mine, and the second for creating new snippets. Because there is some common
functionality in both, like handling autocompletion of tags, I decided to use Angular component inheritance to avoid
code duplication.


So I have defined a snippet base form class - `to avoid code duplication `
```typescript
import { Component, ElementRef, Input, OnInit, ViewChild } from '@angular/core';
import { Observable } from 'rxjs';
import { FormArray, FormBuilder, FormControl, FormGroup } from '@angular/forms';
import { codelet_common_tags } from '../shared/codelet-common-tags';
import { COMMA, ENTER } from '@angular/cdk/keycodes';
import { Codelet, CodeSnippet } from '../core/model/codelet';
import { map, startWith } from 'rxjs/operators';
import { MatChipInputEvent } from '@angular/material/chips';
import { MatAutocompleteActivatedEvent, MatAutocompleteSelectedEvent } from '@angular/material/autocomplete';
import { SuggestedTagsStore } from '../core/user/suggested-tags.store';
import { UserInfoStore } from '../core/user/user-info.store';
import { Params, Router } from '@angular/router';
import { textSizeValidator } from '../core/validators/text-size.validator';
import { HttpResponse } from '@angular/common/http';
import { throwError as observableThrowError } from 'rxjs/internal/observable/throwError';
import { PersonalCodeletsService } from '../core/personal-codelets.service';
import { ErrorService } from '../core/error/error.service';


@Component({
  template: ''
})
export class SnippetFormBaseComponent implements OnInit {

  codeletFormGroup: FormGroup;
  codeSnippetsFormArray: FormArray;
  userId = null;

  // chips
  selectable = true;
  removable = true;
  addOnBlur = true;

  autocompleteTagsOptionActivated = false;

  // Enter, comma, space
  separatorKeysCodes = [ENTER, COMMA];

  commonCodeletTags = codelet_common_tags;

  autocompleteTags = [];

  tagsControl = new FormControl();

  filteredTags: Observable<any[]>;

  @Input()
  codelet: Codelet;

  @ViewChild('tagInput', {static: false})
  tagInput: ElementRef;

  constructor(
    protected formBuilder: FormBuilder,
    protected personalCodeletsService: PersonalCodeletsService,
    protected suggestedTagsStore: SuggestedTagsStore,
    protected userInfoStore: UserInfoStore,
    protected router: Router,
    protected errorService: ErrorService
  ) {
  }


  ngOnInit(): void {
    this.userInfoStore.getUserInfo$().subscribe(userInfo => {
      this.userId = userInfo.sub;
      this.suggestedTagsStore.getSuggestedCodeletTags$(this.userId).subscribe(userTags => {

        this.autocompleteTags = userTags.concat(this.commonCodeletTags.filter((item => userTags.indexOf(item) < 0))).sort();

        this.filteredTags = this.tagsControl.valueChanges.pipe(
          startWith(null),
          map((tag: string | null) => {
            return tag ? this.filter(tag) : this.autocompleteTags.slice();
          })
        );
      });
    });
  }

  addTag(event: MatChipInputEvent): void {
    const input = event.input;
    const value = event.value;

    if ((value || '').trim() && !this.autocompleteTagsOptionActivated) {
      // if ((value || '').trim()) {
      this.formArrayTags.push(this.formBuilder.control(value.trim().toLowerCase()));
    }

    // Reset the input value
    if (input) {
      input.value = '';
    }

    this.tagsControl.setValue(null);
    this.formArrayTags.markAsDirty();
  }

  removeTagByIndex(index: number): void {
    if (index >= 0) {
      this.formArrayTags.removeAt(index);
    }
    this.formArrayTags.markAsDirty();
  }

  filter(name: string) {
    return this.autocompleteTags.filter(tag => tag.toLowerCase().indexOf(name.toLowerCase()) === 0);
  }

  optionActivated($event: MatAutocompleteActivatedEvent) {
    if ($event.option) {
      this.autocompleteTagsOptionActivated = true;
    }
  }

  selectedTag(event: MatAutocompleteSelectedEvent): void {
    this.formArrayTags.push(this.formBuilder.control(event.option.viewValue));
    this.tagInput.nativeElement.value = '';
    this.tagsControl.setValue(null);
    this.autocompleteTagsOptionActivated = false;
  }

  get formArrayTags() {
    return <FormArray>this.codeletFormGroup.get('tags');
  }

  createCodeSnippet(codeSnippet: CodeSnippet): FormGroup {
    return this.formBuilder.group({
      code: [codeSnippet.code, textSizeValidator(5000, 500)],
      comment: codeSnippet.comment
    });
  }

  createInitialCodeSnippet(): FormGroup {
    return this.formBuilder.group({
      code: ['', textSizeValidator(5000, 500)],
      comment: ['', textSizeValidator(1000, 30)]
    });
  }

  createEmptyCodeSnippet(): FormGroup {
    return this.formBuilder.group({
      code: ['', textSizeValidator(5000, 500)],
      comment: ['', textSizeValidator(1000, 30)]
    });
  }

  addEmptyCodeSnippet(index: number): void {
    this.codeSnippetsFormArray.insert(index + 1, this.createEmptyCodeSnippet());
  }

  removeCodeSnippet(index: number) {
    this.codeSnippetsFormArray.removeAt(index);
  }

  createCodelet(codelet: Codelet, copyToMine: boolean, popup: any) {
    codelet.userId = this.userId;
    const now = new Date();
    codelet.lastAccessedAt = now;
    if (copyToMine) {
      delete codelet['_id'];
      codelet.createdAt = now
    }

    this.personalCodeletsService.createCodelet(this.userId, codelet)
      .subscribe(
        response => {
          const headers = response.headers;
          // get the codelet id, which lies in the "location" response header
          const lastSlashIndex = headers.get('location').lastIndexOf('/');
          const newCodeletId = headers.get('location').substring(lastSlashIndex + 1);
          codelet._id = newCodeletId;
          const queryParmas = popup ? {popup: popup} : {};
          this.navigateToCodeletDetails(codelet, queryParmas)
        },
        (error: HttpResponse<any>) => {
          this.errorService.handleError(error.body.json());
          return observableThrowError(error.body.json());
        }
      );
  }

  navigateToCodeletDetails(snippet: Codelet, queryParams: Params): void {
    const link = [`./my-snippets/${snippet._id}/details`];
    this.router.navigate(link, {
      state: {snippet: snippet},
      queryParams: queryParams
    });
  }

}
```

to hold the common functionality. And then the `UpdateSnippetFormComponent` and `CreateSnippetFormComponent` will
inherit from it.

Let's take ``CreateSnippetFormComponent`` for example

```typescript
// imports ignored for

@Component({
  selector: 'app-save-codelet-form',
  templateUrl: './create-snippet-form.component.html',
  styleUrls: ['./create-snippet-form.component.scss']
})
export class CreateSnippetFormComponent extends SnippetFormBaseComponent implements OnInit {

  codeletFormGroup: FormGroup;
  codeSnippetsFormArray: FormArray;
  userId = null;

  @Input()
  codelet$: Observable<Codelet>;

  @ViewChild('tagInput', {static: false})
  tagInput: ElementRef;

  codelet: Codelet;

  @Input()
  code; // value of "desc" query parameter if present

  @Input()
  title; // value of "title" query parameter if present

  @Input()
  sourceUrl; // value of "url" query parameter if present

  @Input()
  tagsStr; // tags received - string with comma separated values

  @Input()
  comment; // comment received via query

  @Input()
  popup; // if it's popup window

  constructor(
    protected formBuilder: FormBuilder,
    protected personalCodeletsService: PersonalCodeletsService,
    protected suggestedTagsStore: SuggestedTagsStore,
    protected userInfoStore: UserInfoStore,
    private userDataStore: UserDataStore,
    private logger: Logger,
    protected router: Router,
    private route: ActivatedRoute,
    protected errorService: ErrorService,
    private webpageInfoService: WebpageInfoService,
    private stackoverflowHelper: StackoverflowHelper,
  ) {
    super(formBuilder, personalCodeletsService, suggestedTagsStore, userInfoStore, router, errorService);
  }

  ngOnInit(): void {
    super.ngOnInit();
    this.buildInitialForm();
    this.codeSnippetsFormArray = this.codeletFormGroup.get('codeSnippets') as FormArray;

    if (this.sourceUrl) {
      const stackoverflowQuestionId = this.stackoverflowHelper.getStackoverflowQuestionIdFromUrl(this.sourceUrl);
      if (stackoverflowQuestionId) {
        this.webpageInfoService.getStackoverflowQuestionData(stackoverflowQuestionId).subscribe((webpageData: WebpageInfo) => {
            if (webpageData.tags) {
              for (let i = 0; i < webpageData.tags.length; i++) {
                this.formArrayTags.push(this.formBuilder.control(webpageData.tags[i]));
              }

              this.tagsControl.setValue(null);
              this.formArrayTags.markAsDirty();
            }
          },
          error => {
            console.error(`Problems when scraping data for stackoverflow id ${stackoverflowQuestionId}`, error);
          });
      }
    }

    this.setTagsFromQueryParameter();
  }

  private setTagsFromQueryParameter() {
    if (this.tagsStr) {
      const tags: String[] = this.tagsStr.split(',');
      for (let i = 0; i < tags.length; i++) {
        this.formArrayTags.push(this.formBuilder.control(tags[i].trim()));
      }

      this.tagsControl.setValue(null);
      this.formArrayTags.markAsDirty();
    }
  }

  buildInitialForm(): void {
    this.codeletFormGroup = this.formBuilder.group({
      title: [this.title ? this.title : '', Validators.required],
      tags: this.formBuilder.array([], [tagsValidator, Validators.required]),
      codeSnippets: new FormArray([this.createInitialCodeSnippet()]),
      sourceUrl: this.sourceUrl ? this.sourceUrl : '',
      public: false
    });

  }

  createInitialCodeSnippet(): FormGroup {
    return this.formBuilder.group({
      code: [this.code ? this.code : '', textSizeValidator(5000, 500)],
      comment: [this.comment ? this.comment : '', textSizeValidator(1000, 30)]
    });
  }

}
```


For HTML documents, such as `browserAction` popups, or tab pages see the setup section in the project's README[^1].

[^1]: <https://github.com/mozilla/webextension-polyfill#basic-setup>

## The implementation changes
The implementation uses a **background** script that will trigger the execution of another javascript,
 `launch-bookmarksdev-dialog.js` when clicked on the extension icon or right click and select **Save link to Bookmarks.dev**.
  Here I only needed to change `chrome` with `browser`, so not it looks like this:

```javascript
browser.browserAction.onClicked.addListener(launchBookmarksDevDialog);

function launchBookmarksDevDialog() {
    browser.tabs.executeScript({
        file: 'launch-bookmarksdev-dialog.js'
    });
};

browser.contextMenus.onClicked.addListener(launchBookmarksDevDialog);

browser.runtime.onInstalled.addListener(function () {
    browser.contextMenus.create({
        "id": "save-link-to-bookmarksdev",
        "title": "Save link to Bookmarks.dev",
        "contexts": ["all"]
    });
});

```

instead of `chrome.browserAction...`

## Test the extension
You can still test the extension locally by loading and reloading the sources either in Chrome or Firefox,
 but with the help `web-ext`[^2] things have gotten easier.

[^2]: <https://github.com/mozilla/web-ext#readme>

Just run the following command in the project root directory
```shell
web-ext run
```

This will start Firefox with the extension installed and reloads it when you do changes in the source code.
 For options see **web-ext command reference**.[^3]

[^3]: <https://github.com/mozilla/web-ext#readme>
https://extensionworkshop.com/documentation/develop/web-ext-command-reference/>

## Build the extension
Packaging the extension for release has also gotten easier with the help of `web-ext` utility.

If before I would use a `zip` command

```shell
zip -r bookmarks.browser.extension.zip * -x *.idea* *.git* '*resources/*' '*assets/*' "*README.md*" "*CHANGELOG.md*" '*web-ext-artifacts/*'
```

now I use the `web-ext` build command

```
web-ext build --overwrite-dest -i 'resources' 'assets' 'README.md' 'CHANGELOG.md'
```

This packages an extension into a `.zip` file, ignoring files that are commonly unwanted in packages, such as `.git` and other artifacts.
 The name of the `.zip` file is taken from the name field in the extension manifest.

You can still exclude files by yourself, as seen above with the help of the `-i`option. See the command reference[^3] for
further options.

## Conclusion

In this post you've seen a way how to migrate your Chrome Extension to be compatible with Firefox with the help
of webextension polyfill.

> If you have found this useful, please show some love and give us a star on [Github](https://github.com/BookmarksDev/bookmarks-browser-extension)

## References

