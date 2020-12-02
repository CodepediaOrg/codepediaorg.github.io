---
layout: post
title: Complete example of how to upload images to Amazon S3 from Angular via Express
description: "Presents a productive example of how to upload images to Amazon S3 in Angular and ExpressJS/NodeJS. Source code is also available."
author: ama
permalink: /ama/complete-example-of-how-to-upload-images-to-amazon-s3-bucket-from-angular-via-express
published: true
categories: [tutorial]
tags:
    - angular
    - node.js
    - expressjs
    - amazon-s3
    - file
---

In this blog post I present a complete example of how to upload an image to Amazon S3 bucket all the way from frontend
implemented in Angular to the backend implemented with NodeJS/ExpressJS. This is based on a real use case running in production
at the [www.bookmarks.dev](https://www.bookmarks.dev) : once you register for an account you have the possibility to change your profile picture
with something more personal at [user settings](https://www.bookmarks.dev/settings).

{% include source-code-bookmarks.dev.html %}

So let's jump into the tutorial
<!--more-->

## Frontend (Angular)
We will start with the front end part.

### Angular Template

And namely with the html template part in angular:

```html
<div *ngIf="(userData$ | async) as userDataVar; else loading" class="navigation-space-buffer">
  <h2>Profile image <i class="fas fa-portrait"></i></h2>
  <div class="profile-image">
    <img *ngIf="selectedFileSrc || userDataVar.profile.imageUrl else robot"
         [src]="selectedFileSrc || userDataVar.profile.imageUrl"
         height="200"
         alt="User profile image">
    <br/>
    <br/>
    <ng-template #robot>
      <div class="generic-profile-image"><i class="fas fa-robot"></i></div>
    </ng-template>
    <div class="custom-file">
      <input #imageInput
             type="file"
             accept="image/*"
             class="form-control"
             id="picture"
             (change)="changeImage(imageInput)">
      <label class="custom-file-label" style="cursor: pointer" for="picture">{{uploadImageLabel}}</label>
    </div>
  </div>

  <div [hidden]="!imageFileIsTooBig"
       class="alert alert-danger mt-2">
    The image file selected is too big (max 1MB)
  </div>
  <div *ngIf="profileImageChangedStatus === 'ok'" class="alert alert-success mt-2"> Image Profile updated successfully!</div>
  <div *ngIf="profileImageChangedStatus === 'fail'" class="alert alert-danger mt-2"> Image Profile failed to update!</div>

  <hr/>
</div>
```
The `async` forces the page rendering to wait before displaying the image and the image upload input.

The `src` attribute of the `img` element is set to the `selectedFileSrc` which points to the new changed image which is immediately displayed,
 or to the existing profile `userDataVar.profile.imageUrl` (order is important). Otherwise, a robot font character is shown:

```html
    <img *ngIf="selectedFileSrc || userDataVar.profile.imageUrl else robot"
         [src]="selectedFileSrc || userDataVar.profile.imageUrl"
         height="200"
         alt="User profile image">
    <br/>
```

The **most important section** here is the **input of type `file`**:
```html
    <div class="custom-file" >
      <input #imageInput
             type="file"
             accept="image/*"
             class="form-control"
             id="picture"
             (change)="changeImage(imageInput)">
      <label class="custom-file-label" style="cursor: pointer" for="picture">{{uploadImageLabel}}</label>
    </div>
```
This is wrapped in a [Bootstrap `custom-file` input](https://getbootstrap.com/docs/4.1/components/input-group/#custom-file-input) paragraph.

**Note**:
  * type [`file`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file) of the `input` element,
   which let the user choose the file from the device storage.
  * tell the browser to accept only images (`accept="image/*"`)- [you should also validate](https://stackoverflow.com/questions/3828554/how-to-allow-input-type-file-to-accept-only-image-files#answer-6225815)
   the uploaded file type on the server also (see the backend section)
  * when the native [`change` event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event) of the `input`
   is fired, we call the `changeImage(imageInput)` with the `input` element itself; this was defined before
   the template reference variable `#imageInput`, which enables us to make this call

The label of the `input` element displays the file's name and size, once the file is uploaded in the browser:
```html
      <label class="custom-file-label" style="cursor: pointer" for="picture">{{uploadImageLabel}}</label>
```

If the user uploads a picture that is bigger than 1MB the user gets an instant feedback via:
```html
  <div [hidden]="!imageFileIsTooBig"
       class="alert alert-danger mt-2">
    The image file selected is too big (max 1MB)
  </div>
```

Finally, the user is informed via the `profileImageUploadStatus` variable whether the profile image change was successful or not:
```html
  <div *ngIf="profileImageChangedStatus === 'ok'" class="alert alert-success mt-2"> Image Profile updated successfully!</div>
  <div *ngIf="profileImageChangedStatus === 'fail'" class="alert alert-danger mt-2"> Image Profile failed to update!</div>
```

### Angular Component
Let's now see what the Angular component looks like:

```typescript
@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.scss']
})
export class UserProfileComponent implements OnInit {

  userProfileForm: FormGroup;

  profileImageChangedStatus = 'init';
  uploadImageLabel = 'Choose file (max size 1MB)';
  imageFileIsTooBig = false;
  selectedFileSrc: string;

  // ...... other methods and imports skipped for brevity

  changeImage(imageInput: HTMLInputElement) {
    const file: File = imageInput.files[0];
    this.uploadImageLabel = `${file.name} (${(file.size * 0.000001).toFixed(2)} MB)`;
    if (file.size > 1048576) {
      this.imageFileIsTooBig = true;
    } else {
      this.imageFileIsTooBig = false;
      const reader = new FileReader();

      reader.addEventListener('load', (event: any) => {
        this.selectedFileSrc = event.target.result;
        this.userDataService.uploadProfileImage(this.userData.userId, file).subscribe(
          (response) => {
            this.userData.profile.imageUrl = response.url;
            this.userDataStore.updateUserData$(this.userData).subscribe(
              () => {
                this.profileImageChangedStatus = 'ok';
              },
              () => {
                this.profileImageChangedStatus = 'fail';
              });
          },
          () => {
            this.profileImageChangedStatus = 'fail';
          });
      });

      if (file) {
        reader.readAsDataURL(file);
      }
    }
  }
}
```

**Note**:
  * we define a couple of helper variables in the beginning
    * `profileImageChangedStatus` - use to inform the user if the picture was changed successfully or not
    * `uploadImageLabel` - the input's label text; changes to the file name and size once one is uploaded
    * `selectedFileSrc` - used to display the picture more rapidly, even before it is loaded in the amazon S3 bucket, to
    give the user a "taste" of what it looks like
    * `imageFileIsTooBig` - flag to notify the user if the image file is too big
  * the image file to be uploaded is extracted from the `files` attribute of the [`HTMLInputElement`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement)
  * we define a [`FileReader`](https://developer.mozilla.org/en-US/docs/Web/API/FileReader) to asynchronously read
  the content of the image file [`reader.readAsDataURL(file)`](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsDataURL)
  * when the file has been successfully read, i.e. the [`load` event](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/load_event)
   is triggered, we upload the image file to the backend via `UserDataService` - `this.userDataService.uploadProfileImage(this.userData.userId, file)`;
   see below the implementation of this method
  * by subscribing to the method mentioned before we give feedback to the user if the change was successful or not


###  The Service Method

```typescript
@Injectable()
export class UserDataService {

  private usersApiBaseUrl = '';  // URL to web api

  constructor(private httpClient: HttpClient) {
    this.usersApiBaseUrl = environment.API_URL + '/personal/users';
  }

  uploadProfileImage(userId: String, image: File): Observable<any> {
    const formData = new FormData();
    formData.append('image', image);

    return this.httpClient.post(`${this.usersApiBaseUrl}/${userId}/profile-picture`, formData);
  }
  // imports and other methods are absent for brevity
}
```

In the service method you use the [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData) interface
 to construct basically a form data to send to backend. This is in the same format if the encoding type would be `multipart/form-data`.

 This requires a key value pair, where `image` is the key of the `formData`  and the second paramater is the file itself.
  You will see in the following section how to handle it in the backend.

## Backend (ExpressJS)
Let's see how you can handle the upload in the backend.

```javascript
const ImageValidationHelper = require('./image-validation.helper');

const aws = require('aws-sdk');
aws.config.update({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: process.env.AWS_REGION
});

const s3 = new aws.S3();
const multer = require("multer");
const multerS3 = require('multer-s3');
const path = require('path')

const upload = multer({
  limits: {
    fileSize: 1048576 // 1MB
  },
  fileFilter: ImageValidationHelper.imageFilter,
  storage: multerS3({
    s3: s3,
    bucket: 'bookmarks.dev',
    acl: 'public-read',
    cacheControl: 'max-age=31536000',
    contentType: multerS3.AUTO_CONTENT_TYPE,
    metadata: function (req, file, cb) {
      cb(null, {fieldName: file.fieldname});
    },
    key: function (req, file, cb) {
      const key = `user-profile-images/${process.env.NODE_ENV}/${req.params.userId}_${Date.now().toString()}${path.extname(file.originalname)}`
      cb(null, key);
    }
  })
});

// other methods and imports are exclude for brevity

/* save profile picture */
usersRouter.post('/:userId/profile-picture', keycloak.protect(),
  upload.single("image" /* name attribute of <file> element in your form */),
  async (request, response) => {
    userIdTokenValidator.validateUserId(request);

    return response.status(HttpStatus.OK).send({
      url: request.file.location
    });
  });

```

### AWS-SDK Setup
To upload an image to S3 bucket you need to install the `aws-sdk` package - `npm install aws-sdk` - and configure it:
```javascript
const aws = require('aws-sdk');
aws.config.update({
  accessKeyId: process.env.AWS_ACCESS_KEY_ID,
  secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
  region: process.env.AWS_REGION
});
```
The credentials and region come from environment variables, but there are other options explained in the AWS SDK documentation for [getting your credentials](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-your-credentials.html)
>- just make sure you [create a dedicated IAM account](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)
> to access the S3 bucket and use its credentials instead of the ones for root user

### Multer Setup
In an ExpressJS backend, [Multer](https://github.com/expressjs/multer) as Node.js middleware is the way to handle `multipart/form-data` for uploading
the image - `npm install --save multer`

```javascript
const s3 = new aws.S3();
const multer = require("multer");
const multerS3 = require('multer-s3');
const path = require('path')

const upload = multer({
  limits: {
    fileSize: 1048576 // 1MB
  },
  fileFilter: ImageValidationHelper.imageFilter,
  storage: multerS3({
    s3: s3,
    bucket: 'bookmarks.dev',
    acl: 'public-read',
    cacheControl: 'max-age=31536000',
    contentType: multerS3.AUTO_CONTENT_TYPE,
    metadata: function (req, file, cb) {
      cb(null, {fieldName: file.fieldname});
    },
    key: function (req, file, cb) {
      const key = `user-profile-images/${process.env.NODE_ENV}/${req.params.userId}_${Date.now().toString()}${path.extname(file.originalname)}`
      cb(null, key);
    }
  })
});
```
First we verify the size to be less than 1MB:
```javascript
  limits: {
    fileSize: 1048576 // 1MB
  }
```

Multer will through a standard Express error if this is the case, that we can catch in our [final error handler](https://www.codepedia.org/ama/cleaner-code-in-expressjs-rest-api-with-custom-error-handling):
```javascript
app.use(function (error, req, res, next) {
  if ( res.headersSent ) {
    return next(error)
  } else if ( error.code === 'LIMIT_FILE_SIZE') { // Multer error - see https://github.com/expressjs/multer/blob/master/lib/multer-error.js && https://github.com/expressjs/multer#error-handling
    return res
      .status(HttpStatus.BAD_REQUEST)
      .json({
        httpStatus: HttpStatus.BAD_REQUEST,
        message: error.code + ' ' + error.message,
        stack: app.get('env') === 'development' ? error.stack : {}
      });
  } else {
    res.status(error.status || HttpStatus.INTERNAL_SERVER_ERROR);
    res.send({
      message: error.message,
      stack: app.get('env') === 'development' ? error.stack : {}
    });
  }
});
```


Secondly, we check that the uploaded file is an image via `fileFilter: ImageValidationHelper.imageFilter` option. The implementation
of the filter:
```javascript
const ValidationError = require('../../error/validation.error');

const imageFilter = function(req, file, cb) {
  // Accept images only
  if (!file.originalname.match(/\.(jpg|JPG|jpeg|JPEG|png|PNG|gif|GIF)$/)) {
    req.fileValidationError = 'Only image files are allowed!';
    return cb(new ValidationError('Method accespts only images [jpg|JPG|jpeg|JPEG|png|PNG|gif|GIF]', ['The file uploaded is not an image']), false);
  }
  cb(null, true);
};
exports.imageFilter = imageFilter;
```
checks the file extension, which needs to be one in the list above.

### Multer storage engine - MulterS3

```javascript
  storage: multerS3({
    s3: s3,
    bucket: 'bookmarks.dev',
    acl: 'public-read',
    cacheControl: 'max-age=31536000',
    contentType: multerS3.AUTO_CONTENT_TYPE,
    metadata: function (req, file, cb) {
      cb(null, {fieldName: file.fieldname});
    },
    key: function (req, file, cb) {
      const key = `user-profile-images/${process.env.NODE_ENV}/${req.params.userId}_${Date.now().toString()}${path.extname(file.originalname)}`
      cb(null, key);
    }
```
We use [multerS3](https://github.com/badunk/multer-s3#readme) as a [Multer Storage Engine](https://github.com/expressjs/multer/blob/master/StorageEngine.md) to upload
the file to S3. We provide it with the following options:
  * `bucket: 'bookmarks.dev'` - the bucket to store the file in
  * `acl: 'public-read'` - owner gets `FULL_CONTROL`, all other users gets `READ` access (in this case the user profile picture is public)
  * `cacheControl: 'max-age=31536000'` - the `max-age` for caching is set to the maxim recommended of **one year**
  * `metadata` contains the `metadata` object to be sent to S3; here it sets fieldName to `image`, the value that comes from `formData`
  from frontend
  * `key` is the name of the file in the bucket - here it is environment specific, user specific (`userId`) plus current date and original file name

### Upload the file to Amazon S3 Bucket
Finally, we upload the file to the bucket by calling the `single` method of the Multer middleware:

```javascript
/* save profile picture */
usersRouter.post('/:userId/profile-picture', keycloak.protect(),
  upload.single("image" /* name attribute of <file> element in your form */),
  async (request, response) => {
    userIdTokenValidator.validateUserId(request);

    return response.status(HttpStatus.OK).send({
      url: request.file.location
    });
  });
```

If the upload is successful, we send a `200 OK` status back and in response we put the public url of the image from the bucket.
You can get it from `request.file.location` attribute.

## Conclusion
You should know by now how to upload a picture to Amazon S3 bucket with Angular and ExpressJS. You have also learned
how to validate the upload both in frontend and backend.

{% include action-to-star-bookmarksdev-on-github.html %}

