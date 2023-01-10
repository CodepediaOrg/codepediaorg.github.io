---
layout: post
title: Configure and use environment specific values in Angular and html template
description: "Configure and use environment specific values in Angular and html template codever code snippet"
author: ama
permalink: /snippets/6389997bb160cb1fab2430fe/configure-and-use-environment-specific-values-in-angular-and-html-template
published: true
snippetId: 6389997bb160cb1fab2430fe
categories: [snippets]
tags: [typescript, angular, html-template, html, environment, configuration, codever-snippets]
---

### Configuring application environments

First define different `environment` files for the environments you might have under the project's `src/environments/` folder.
Below there is an extract from `environment.ts` (used for development) and `environment.prod.ts` used for production:

```typescript
//environment.ts
export const environment = {
  production: false,
  HOST: 'http://localhost:4200',
  API_URL: 'http://localhost:3000/api',
};

//environment.prod.ts
export const environment = {
  production: true,
  HOST: 'https://www.codever.dev',
  API_URL:  'https://www.codever.dev/api',
};
```

The `build` command (and `ng serve`) uses `environment.ts` as the build target when no environment is specified.

The main CLI configuration file, `angular.json`, contains a `fileReplacements` section in the configuration for each build target,
which lets you replace any file in the TypeScript program with a target-specific version of that file.
This is useful for including target-specific code or variables in a build that targets a specific environment, such as production or staging.

By default, no files are replaced. You can add file replacements for specific build targets. For example

```typescript
"configurations": {
  "production": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.prod.ts"
      }
    ],
    …
```

This means that when you build your production configuration with `ng build --configuration production`,
the `src/environments/environment.ts` file is replaced with the target-specific version of the file, `src/environments/environment.prod.ts`.

You can add additional configurations (e.g. pro different stages) as required.

### Using environment-specific variables in your app

Then in your code import `environment` and use the values defined in the files above, as in the following example:

```typescript
import { environment } from '../../../environments/environment'
import { localStorageKeys } from '../model/localstorage.cache-keys';

@Injectable()
export class SystemService {
  cachedQueries = {
    PUBLIC_TAGS_LIST: `${environment.API_URL}/public/bookmarks`
  }

 //...
}
```

If you want to use environment variables in your angular html template,
then just define a local variable for the imported environment and use it in the template, like in the following example:


```typescript
import { environment } from 'environments/environment';

@Component({
  selector: 'app-social-share-dialog',
  templateUrl: './social-share-dialog.component.html',
  styleUrls: ['./social-share-dialog.component.scss'],
  providers: [DatePipe]
})
export class SocialShareDialogComponent implements OnInit {

  sharableId$: Observable<any>;
  readonly environment = environment;
  //...
}
```

The **html** part, where `HOST` value is used:

```
    <div>
      <h3>Shareable</h3>
      {{ environment.HOST + '/bookmarks/shared/' + (sharableId$ | async).shareableId}}
    </div>
```

<span style="font-size: 0.9rem">
  <strong>Reference - </strong>
  <a href="https://angular.io/guide/build#using-environment-specific-variables-in-your-app" target="_blank" style="font-weight: lighter">
     https://angular.io/guide/build#using-environment-specific-variables-in-your-app
  </a>
</span>

<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="6389997bb160cb1fab2430fe" %}
