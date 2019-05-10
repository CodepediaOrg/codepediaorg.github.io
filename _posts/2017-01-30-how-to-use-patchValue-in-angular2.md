---
layout: post
title: Patching my way through FormGroups and FormControls in Angular reactive forms
description: "This post shows how to dynamically fill in data in a reactive form field, based on other field's data"
author: ama
permalink: /ama/patching-my-way-through-formgroups-and-formcontrols-in-angular-reactive-forms
published: true
categories: [angular]
tags: [codingmarks, coding bookmarks, angular, forms, reactive forms]
---

Here goes my first Angular post - yeeey. Well, it's not super exciting, but practical and specific... Topic - generic: present how to update a field's value in a reactive form field,
once a value in another field is given. Topic - concrete: there is a **personal** bookmarks section on [https://www.codingmarks.org](https://www.codingmarks.org); when you add a new bookmark and you fill in the **location** field
with an URL, the **title** field is being automatically filled in by scraping the page for its title. Of course later you have the possibility to change it.

In the end I present the struggle that led me to the solution.

{% include source-code-codingmarks.html %}

<!--more-->

## The component template

The HTML template is pretty standard. I use reactive forms[^1] to get the data:

[^1]: <https://angular.io/docs/ts/latest/cookbook/form-validation.html#!#reactivel>

```html

<div class="container">
  <div class="col-md-8 col-md-offset-2">
    <form [formGroup]="bookmarkForm" novalidate (ngSubmit)="saveBookmark(bookmarkForm.value, bookmarkForm.valid)">
      <div class="form-group">
        <label for="location">Location*</label>
        <input type="text" class="form-control" id="location"
               required
               name="location"
               formControlName="location"
               placeholder="Usually an URL">
        <div [hidden]="bookmarkForm.controls.location.valid || (bookmarkForm.controls.location.pristine && !submitted)"
             class="alert alert-danger">
          Location is required
        </div>
      </div>
      <div class="form-group">
        <label for="name">Title*</label>
        <input type="text" class="form-control" id="name"
               required
               formControlName="name"
               placeholder="Name of the bookmark to recognize later">
        <div [hidden]="bookmarkForm.controls.name.valid || (bookmarkForm.controls.name.pristine && !submitted)"
             class="alert alert-danger">
          Name is required
        </div>
      </div>
      <div class="form-group">
        <label for="tags">Tags* - comma separated</label>
        <input type="text" class="form-control" id="tags"
               formControlName="tagsLine"
               pattern="^[-\w\s]+(?:,[-\w\s]*)*$"
               placeholder='keywoards - comma separated (e.g. "jvm, regex")'>
        <div [hidden]="bookmarkForm.controls.tagsLine.valid || (bookmarkForm.controls.tagsLine.pristine && !submitted)"
             class="alert alert-danger">
          Please provide the tags separated with comma
        </div>
      </div>
      <div class="form-group">
        <label for="description">Notes...</label>
        <textarea class="form-control"
                  id="description"
                  formControlName="description"
                  placeholder="Make some notes to help you later by searching...">
        </textarea>
      </div>
      <div class="form-check">
        <label class="form-check-label">
          <input class="form-check-input" type="checkbox" value="true" formControlName="shared">
          <strong> Make this bookmark public so others can benefit from it </strong>
        </label>
      </div>
      <button type="submit" class="btn btn-default" [disabled]="!bookmarkForm.valid">Submit</button>
    </form>
  </div>
</div>
```

## The component class

```

export class UserBookmarkFormComponent implements OnInit {

  model = new Bookmark('', '', '', [], '');
  bookmarkForm: FormGroup;
  userId = null;

  constructor(
    private userBookmarkStore: UserBookmarkStore,
    private router: Router,
    private formBuilder: FormBuilder,
    private keycloakService: KeycloakService,
    private bookmarkService: BookmarkService
  ){
    const keycloak = keycloakService.getKeycloak();
    if(keycloak) {
      this.userId = keycloak.subject;
    }
  }

  ngOnInit(): void {

    this.bookmarkForm = this.formBuilder.group({
      name: ['', Validators.required],
      location: ['', Validators.required],
      tagsLine:['', Validators.required],
      description:'',
      shared: false
    });

    this.bookmarkForm.valueChanges
      .debounceTime(600)
      .distinctUntilChanged()
      .subscribe(formData => {
        if(formData.location && !formData.name){
          console.log('location changed', formData);
          this.bookmarkService.getBookmarkTitle(formData.location).subscribe(response => {
            if(response){
              this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
            }
          });
        }
      });
  }

  saveBookmark(model: Bookmark, isValid: boolean) {
    model.tags = model.tagsLine.split(",");
    var newBookmark = new Bookmark(model.name, model.location, model.category,model.tagsLine.split(","), model.description);

    newBookmark.userId = this.userId;
    newBookmark.shared = model.shared;

    let obs = this.userBookmarkStore.addBookmark(this.userId, newBookmark);

    obs.subscribe(
      res => {
        this.router.navigate(['/personal']);
      });
  }
}
```

In the component class I declare a form builder[^2] with a couple of validators:

[^2]: <https://angular.io/docs/ts/latest/api/forms/index/FormBuilder-class.html>


```javascript

this.bookmarkForm = this.formBuilder.group({
  name: ['', Validators.required],
  location: ['', Validators.required],
  tagsLine:['', Validators.required],
  description:'',
  shared: false
});
```

to then listen for valuesChanges on the on the location field/FormControl and update the name/title field/FormControl when that's the case (too long, cut it):

```javascript

this.bookmarkForm.controls['location'].valueChanges
  .debounceTime(800)
  .distinctUntilChanged()
  .subscribe(location => {
    console.log('Location: ', location);
    this.bookmarkService.getBookmarkTitle(location).subscribe(response => {
      if(response){
        this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
      }
    });
  });
```

## The struggle

Very interesting is how I came up with this solution. Initially I patched the whole form/group of elements[^3]:

[^3]: <https://angular.io/docs/ts/latest/api/forms/index/FormGroup-class.html>

```javascript

 // ************ WRONG **************
 bookmarkForm: FormGroup;
 ........
 this.bookmarkForm.valueChanges
    .debounceTime(800)
    .distinctUntilChanged()
    .subscribe(formData => {
      if(formData.location){
        console.log('location changed', formData);
        this.bookmarkService.getBookmarkTitle(formData.location).subscribe(response => {
          if(response){
            this.bookmarkForm.patchValue({name:response.title});
          }
        });
      }
    });
```

because this is the proper way to partially update the reactive form, right? Wrong, at least in my case, because the `valueChanges` started to fire continually. Setting the `emitEvent` flag on `false` at this level, did not help
 either, as perhaps suggested in the documentation [^4]:

```javascript

 // ************ BETTER, but still WRONG **************
 this.bookmarkForm.valueChanges
    .debounceTime(800)
    .distinctUntilChanged()
    .subscribe(formData => {
      if(formData.location){
        console.log('location changed', formData);
        this.bookmarkService.getBookmarkTitle(formData.location).subscribe(response => {
          if(response){
            this.bookmarkForm.patchValue({name:response.title}, {emitEvent : false});
          }
        });
      }
    });
```

[^4]: <https://angular.io/docs/ts/latest/api/forms/index/FormGroup-class.html#!#patchValue-anchor>

Looking through the source code, you can see that the `emitEvent` flag does not apply at the `FormGroup` Level:

```javascript

/**
 *  Patches the value of the {@link FormGroup}. It accepts an object with control
 *  names as keys, and will do its best to match the values to the correct controls
 *  in the group.
 *
 *  It accepts both super-sets and sub-sets of the group without throwing an error.
 *
 *  ### Example
 *
 *  ```
 *  const form = new FormGroup({
 *     first: new FormControl(),
 *     last: new FormControl()
 *  });
 *  console.log(form.value);   // {first: null, last: null}
 *
 *  form.patchValue({first: 'Nancy'});
 *  console.log(form.value);   // {first: 'Nancy', last: null}
 *
 *  ```
 */
patchValue(value: {
    [key: string]: any;
}, {onlySelf}?: {
    onlySelf?: boolean;
}): void;
```

but rather at the `FormControl` level:

```javascript

/**
 * Patches the value of a control.
 *
 * This function is functionally the same as {@link FormControl.setValue} at this level.
 * It exists for symmetry with {@link FormGroup.patchValue} on `FormGroups` and `FormArrays`,
 * where it does behave differently.
 */
patchValue(value: any, options?: {
    onlySelf?: boolean;
    emitEvent?: boolean;
    emitModelToViewChange?: boolean;
    emitViewToModelChange?: boolean;
}): void;
```

where it acts similar to the `setValue` function. Knowing now this, I came up with a working solution:

```javascript

// ************ WORKS, but not OPTIMAL **************
this.bookmarkForm.valueChanges
  .debounceTime(800)
  .distinctUntilChanged()
  .subscribe(formData => {
    if(formData.location && !formData.name){ //have data in location field and name/title field is still empty
      console.log('location changed', formData);
      this.bookmarkService.getBookmarkTitle(formData.location).subscribe(response => {
        console.log('Response: ', response);
        if(response){
          this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
        }
      });
    }
  });
```

to patch the whole `FormGroup` and build in some `if()` logic to fulfill my needs. After sleeping it through, the right and more elegant solution came to mind, which I'll post againg bellow :

```javascript

// ************ OPTIMAL so far **************
this.bookmarkForm.controls['location'].valueChanges
  .debounceTime(800)
  .distinctUntilChanged()
  .subscribe(location => {
    console.log('Location: ', location);
    this.bookmarkService.getBookmarkTitle(location).subscribe(response => {
      if(response){
        this.bookmarkForm.controls['name'].patchValue(response.title, {emitEvent : false});
      }
    });
  });
```

If you found this useful, please star it, share it and improve it:

{% include source-code-codingmarks.html %}

## References
