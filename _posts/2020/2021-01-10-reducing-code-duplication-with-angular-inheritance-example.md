---
layout: post
title: Reducing code duplication with angular inheritance
description: "Real life example of using angular inheritance to reduce code duplication in create and update code snippets form components on Bookmarks.dev"
author: ama
permalink: /ama/reducing-code-duplication-with-angular-inheritance
published: true
categories: [tutorial]
tags:
    - angular
    - inheritance
    - refactoring
    - codever
---

Recently I have just added the possibility to [share your code snippets with the world](https://dev.to/bookmarks/public-code-snippets-are-now-available-3na0)
on [Bookmarks.dev](https://www.codever.land). I have noticed that the code to create and update code snippets,
 was too intertwined - trying to avoid code duplication I used initially just one component to create and update code snippets.
 Now, I just could not stand the too many conditional checks anymore, so I decided to split the functionality in two parts
- one for handling updating and copy to mine, and the second for creating new snippets.

Because there is still some common functionality in both, like handling autocompletion of tags,
 I decided to use Angular component inheritance to avoid code duplication.
 In this blog post I will just show code examples for inheritance and name the angular inheritance particularities.

{% include source-code-bookmarks.dev.html %}

<!--more-->

## Common Base Form Component

First, I have defined a snippet base form class that handles tags loading and autocompletion, create initial snippets methods
and navigation method:

```typescript
import { Component, ElementRef, Input, OnInit, ViewChild } from '@angular/core';
import { Observable } from 'rxjs';
import { FormArray, FormBuilder, FormControl, FormGroup } from '@angular/forms';
import { snippet_common_tags } from '../shared/snippet-common-tags';
import { COMMA, ENTER } from '@angular/cdk/keycodes';
import { Snippet, CodeSnippet } from '../core/model/snippet';
import { map, startWith } from 'rxjs/operators';
import { MatChipInputEvent } from '@angular/material/chips';
import { MatAutocompleteActivatedEvent, MatAutocompleteSelectedEvent } from '@angular/material/autocomplete';
import { SuggestedTagsStore } from '../core/user/suggested-tags.store';
import { UserInfoStore } from '../core/user/user-info.store';
import { Params, Router } from '@angular/router';
import { textSizeValidator } from '../core/validators/text-size.validator';
import { HttpResponse } from '@angular/common/http';
import { throwError as observableThrowError } from 'rxjs/internal/observable/throwError';
import { PersonalSnippetsService } from '../core/personal-snippets.service';
import { ErrorService } from '../core/error/error.service';


@Component({
  template: ''
})
export class SnippetFormBaseComponent implements OnInit {

  snippetFormGroup: FormGroup;
  codeSnippetsFormArray: FormArray;
  userId = null;

  // chips
  selectable = true;
  removable = true;
  addOnBlur = true;

  autocompleteTagsOptionActivated = false;

  // Enter, comma, space
  separatorKeysCodes = [ENTER, COMMA];

  commonSnippetTags = snippet_common_tags;

  autocompleteTags = [];

  tagsControl = new FormControl();

  filteredTags: Observable<any[]>;

  @Input()
  snippet: Snippet;

  @ViewChild('tagInput', {static: false})
  tagInput: ElementRef;

  constructor(
    protected formBuilder: FormBuilder,
    protected personalSnippetsService: PersonalSnippetsService,
    protected suggestedTagsStore: SuggestedTagsStore,
    protected userInfoStore: UserInfoStore,
    protected router: Router,
    protected errorService: ErrorService
  ) {
  }

  ngOnInit(): void {
    this.userInfoStore.getUserInfo$().subscribe(userInfo => {
      this.userId = userInfo.sub;
      this.suggestedTagsStore.getSuggestedSnippetTags$(this.userId).subscribe(userTags => {

        this.autocompleteTags = userTags.concat(this.commonSnippetTags.filter((item => userTags.indexOf(item) < 0))).sort();

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
    return <FormArray>this.snippetFormGroup.get('tags');
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

  createSnippet(snippet: Snippet, copyToMine: boolean, popup: any) {
    snippet.userId = this.userId;
    const now = new Date();
    snippet.lastAccessedAt = now;
    if (copyToMine) {
      delete snippet['_id'];
      snippet.createdAt = now
    }

    this.personalSnippetsService.createSnippet(this.userId, snippet)
      .subscribe(
        response => {
          const headers = response.headers;
          // get the snippet id, which lies in the "location" response header
          const lastSlashIndex = headers.get('location').lastIndexOf('/');
          const newSnippetId = headers.get('location').substring(lastSlashIndex + 1);
          snippet._id = newSnippetId;
          const queryParmas = popup ? {popup: popup} : {};
          this.navigateToSnippetDetails(snippet, queryParmas)
        },
        (error: HttpResponse<any>) => {
          this.errorService.handleError(error.body.json());
          return observableThrowError(error.body.json());
        }
      );
  }

  navigateToSnippetDetails(snippet: Snippet, queryParams: Params): void {
    const link = [`./my-snippets/${snippet._id}/details`];
    this.router.navigate(link, {
      state: {snippet: snippet},
      queryParams: queryParams
    });
  }

}
```

## Inheriting components
The `UpdateSnippetFormComponent` and `CreateSnippetFormComponent` components will
inherit from it.

Let's take have a look at one of them, for example at `CreateSnippetFormComponent`, and discuss the particularities

```typescript
// imports ignored for brevity

@Component({
  selector: 'app-save-snippet-form',
  templateUrl: './create-snippet-form.component.html',
  styleUrls: ['./create-snippet-form.component.scss']
})
export class CreateSnippetFormComponent extends SnippetFormBaseComponent implements OnInit {

  snippetFormGroup: FormGroup;
  codeSnippetsFormArray: FormArray;
  userId = null;

  @Input()
  snippet$: Observable<Snippet>;

  @ViewChild('tagInput', {static: false})
  tagInput: ElementRef;

  snippet: Snippet;

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
    protected personalSnippetsService: PersonalSnippetsService,
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
    super(formBuilder, personalSnippetsService, suggestedTagsStore, userInfoStore, router, errorService);
  }

  ngOnInit(): void {
    super.ngOnInit();
    this.buildInitialForm();
    this.codeSnippetsFormArray = this.snippetFormGroup.get('codeSnippets') as FormArray;

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
    this.snippetFormGroup = this.formBuilder.group({
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

* `extends` keyword is used to mark the inheritance - `CreateSnippetFormComponent extends SnippetFormBaseComponent`
* you need to define all the properties of the parent constructor in the child as `protected` too, and call the constructor
with the `super` keyword right at the beginning of the constructor - `super(formBuilder, personalSnippetsService, suggestedTagsStore, userInfoStore, router, errorService);`
* to trigger the functionality of the `ngOnInit` of the parent component, you need to call it with the `super` keyword
in the child component too (otherwise it will just get overwritten) - `super.ngOnInit();`

You can see the create code snippet form in action in the following gif:
![Create public snippet](https://dev-to-uploads.s3.amazonaws.com/i/m0uncs9m86vjkxixeywd.gif)

For a more detailed explanations of class inheritance in Typescript visit the [Classes section from the handbook](https://www.typescriptlang.org/docs/handbook/classes.html)

> If you have found this useful, please show some love and give us a star on [Github](https://github.com/codeverland/codever)

