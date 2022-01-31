---
layout: post
title: Set focus on input field with autocomplete in angular
description: "Example code snippet showing how to set focus on input field with autocomplete in angular material"
author: ama
permalink: /snippets/61f7a191a6871e0e9eee347b/set-focus-on-input-field-with-autocomplete-in-angular
published: true
snippetId: 61f7a191a6871e0e9eee347b
categories: [snippets]
tags: [angular, css, angular-material, autocomplete, codever-snippets]
---

**Project** [`Codever`](https://www.codever.land)

1. get access to the input field via template variable `@ViewChild('publicSearchBox') searchBoxField: ElementRef;`
2. get access to the autcomplete trigger to close the panel (we don't want that when the page loads) - `@ViewChild(MatAutocompleteTrigger) autocompleteTrigger: MatAutocompleteTrigger;``
3. call `focus()` and `closePanel()` on the two elements in one of Angular's lifecycle hooks - here in `AfterViewInit`

```typescript
@Component({
  selector: 'app-searchbar',
  templateUrl: './searchbar.component.html',
  styleUrls: ['./searchbar.component.scss']
})
export class SearchbarComponent implements OnInit, AfterViewInit {

  @ViewChild('publicSearchBox') searchBoxField: ElementRef;
  @ViewChild(MatAutocompleteTrigger) autocompleteTrigger: MatAutocompleteTrigger;

  ngAfterViewInit(): void {
    this.searchBoxField.nativeElement.focus();
    this.autocompleteTrigger.closePanel();
  }

}
```

Below you can see how the referenced template variable (`publicSearchBox`) and the angular autocomplete trigger are defined in the angular html template:

```html
  <input
    #publicSearchBox
    #autocompleteTrigger="matAutocompleteTrigger"
    matInput
    type="text"
    class="form-control"
    [formControl]="searchControl"
    placeholder="{{getPlaceholderTextForSearchbar()}}"
    [matAutocomplete]="auto"
    (focus)="focusOnSearchControl()"
    (focusout)="unFocusOnSearchControl()"
    [class.my-snippets-selection]="searchDomain === 'my-snippets'"
    [class.my-bookmarks-selection]="searchDomain === 'my-bookmarks'"
    [class.public-snippets-selection]="searchDomain === 'public-snippets'"
    (keyup)="watchForTags(publicSearchBox.value)"
    (keyup.enter)="$event.target.blur(); autocompleteTrigger.closePanel();searchBookmarksFromSearchBox(publicSearchBox.value)"
  >
```


<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="61f7a191a6871e0e9eee347b" %}
