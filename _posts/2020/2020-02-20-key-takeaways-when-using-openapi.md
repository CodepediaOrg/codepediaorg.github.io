---
layout: post
title: Key takeaways when using Open API Specification 3 to document an ExpressJS API
description: "Presents key points you should consider when using Open API to document an ExpressJS REST API"
author: ama
permalink: /ama/key-takeaways-when-using-oas-3-to-document-an-expressjs-rest-api
published: true
categories: [article]
tags:
    - openapi
    - expressjs
    - swagger
    - swagger-editor
    - swagger-ui
    - rest
    - api
    - documentation
    - nodejs
    - codever
---


Recently I've taken the time to update the API documentation for [bookmarks.dev](https://github.com/codeverland/codever).
I used the Swagger 2.0 (aka OAS 2) before and I decided to update to OpenAPI Specification (OAS) 3. In this post I will
highlight the main points about the process and documenting the API with OAS 3. Some points might still apply to the former OAS 2 (fka Swagger)
documentation, but they are worth mentioning since I hadn't payed enough attention before and I find them useful.

> You can find the OAS 3 specification  for **bookmarks.dev** [on Github](https://github.com/codeverland/codever/blob/master/backend/docs/openapi/openapi.yaml)
and the result is available at [bookmarks.dev/api/docs/](https://www.codever.land/api/docs/)

Here are the key takeaways.
<!--more-->

## 1. Read  the [A Guide to What’s New in OpenAPI 3.0](https://swagger.io/blog/news/whats-new-in-openapi-3-0/) article
In this article, they share some of the major updates in the latest version of OAS, and breakdown what you need to know when transitioning to OAS 3.0.
It's based on this webinar [OpenAPI 3.0, And What it Means for the Future of Swagger](https://www.youtube.com/watch?v=wBDSR0x3GZo) - 1 hour long

## 2. Use the OpenAPI/Swagger 2.0 to OpenAPI 3.0 Converter WebService

to convert your Swagger specification to OpenAPI 3.0.

It is available online at [https://converter.swagger.io/](https://converter.swagger.io/) and as a [docker image](https://hub.docker.com/r/swaggerapi/swagger-converter):

```
docker pull swaggerapi/swagger-converter:v1.0.2
docker run -it -p 8080:8080 --name swagger-converter swaggerapi/swagger-converter:v1.0.2
```

## 3. Use [Swagger-Editor](https://editor.swagger.io/) to immediate validate your specification and preview it in real time
Swagger Editor lets you edit Swagger API specifications in YAML inside your browser and to preview documentations in real time.

 You can use it online, as an [npm](https://www.npmjs.com/package/swagger-editor) distribution or as a [docker image](https://hub.docker.com/r/swaggerapi/swagger-editor/).
 For more details check the [Readme](https://github.com/swagger-api/swagger-editor#readme) of the project.

## 4. Use [Swagger UI](https://github.com/swagger-api/swagger-ui) to present your documentation
Swagger UI is a collection of HTML, Javascript, and CSS assets that dynamically generate beautiful documentation from a Swagger-compliant API.

I use it indirectly with the help of [Swagger UI Express](https://www.npmjs.com/package/swagger-ui-express). That way
you can access the Swagger UI documentation as a route in the API for example, in my case at [bookmarks.dev/api/docs/](https://www.codever.land/api/docs/)

The code part needed in `app.js`:

```javascript
const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const swaggerDocument = YAML.load('./docs/openapi/openapi.yaml');

app.use('/api/docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

> It's helpful to include the open specification file (here `openapi.yaml`) in the nodemon watch (e.g. `nodemon --inspect ./bin/www --watch src --watch docs/openapi/openapi.yaml`),
 so that you can reload the UI without having to manually restart the ExpressJS server

### 4.1 Use swagger-jsdoc for a code-first approach
Another point worth mentioning here is that you could use [swagger-jsdoc](https://github.com/Surnet/swagger-jsdoc),
 to integrate [Swagger](https://swagger.io) using [`JSDoc`](https://jsdoc.app/) comments in your code. The `swagger-jsdoc` project
  assumes that you want document your existing/living/working code in a way to "give life" to it, generating a specification
   which can then be fed into other Swagger tools, and not the vice-versa.

> For now I manage documentation centrally in one [openapi.yaml](https://github.com/codeverland/codever/blob/master/backend/docs/openapi/openapi.yaml) file,
 but I might consider it at a later time

## 5. Use [tags](https://swagger.io/docs/specification/grouping-operations-with-tags/) to group the operations
You can assign a list of tags to each API operation. Thus **Swagger UI** and **Swagger Editor** will display the operations by
tags which comes very handy. To control the sorting in Swagger UI you need to add them also at the root level as global
tags. There you can also add a description  and link to external documentation for them.

Here are the tags I use for the API for
```yaml

tags:
  - name: root
    description: Used to mark the root endpoint
  - name: version
    description: Access to the project's version and gitSha1
  - name: public-bookmarks
    description: Access to public bookmarks
  - name: personal-bookmarks
    description: Operations performed on personal bookmarks
  - name: user-data
    description: Operations performed on user data
  - name: helper
    description: Helper endpoints/operations
```

## 6. Use the [servers](https://swagger.io/docs/specification/api-host-and-base-path/) array to specify one or more base URLs for your API.
In OpenAPI 3.0, you use the servers array to specify one or more base URLs for your API. servers replaces the `host`, `basePath`
 and `schemes` keywords used in OpenAPI 2.0. Each server has an `url` and an optional Markdown-formatted `description`.

```yaml
servers:
  - url: http://localhost:3000/api
    description: Local server for development
  - url: https://www.codever.land/api
    description: Main (production) server
```


## 7. Use [components](https://swagger.io/docs/specification/components/) in order to define and reuse resources
Often, multiple API operations have some common parameters or return the same response structure.
 To avoid code duplication, you can place the common definitions in the global `components` section and reference them using `$ref`.

For example for a list of bookmarks response that occurs with several operations I defined a `BookmarkListResponse` under
the global `responses` section

```yaml
components:
  responses:
    BookmarkListResponse:
      description: List of bookmarks
      content:
        application/json:
          schema:
            type: array
            items:
              $ref: '#/components/schemas/Bookmark'
```

and I reference it in the different operations (e.g. `get-public-bookmarks`):

```yaml
  /public/bookmarks:
    get:
      summary: Returns list of public bookmarks filtered with query parameters.
      tags:
        - public-bookmarks
      description: |
        * if `q` param is present they are filtered by the query text contained in it. (takes precedence over `location`)
        * if `location` param is present returns a list with one **public** bookmark with that URL
        * else **defaults** to the latest added 100 public bookmarks

        > The list is empty if not public bookmarks returned for filter
      parameters:
        - $ref: "#/components/parameters/searchTextQueryParam"
        - $ref: "#/components/parameters/limitQueryParam"
        - $ref: "#/components/parameters/locationQueryParam"
      responses:
        200:
          description: OK
          $ref: '#/components/responses/BookmarkListResponse'
```


Notice above also the `locationQueryParam`. It's a `location` query parameter defined in the `components > parameters`
section and then referenced in multiple places in the API specification (one of them above):

```yaml
componentes:
  parameters:
    locationQueryParam:
      name: location
      in: query
      description: location of the bookmark, usually an URL
      required: false
      schema:
        type: string
```
## 8. Add [examples](https://swagger.io/docs/specification/adding-examples/) to make it clearer
You can add examples to parameters, properties and objects to make OpenAPI specification of your web service clearer.
 Examples can be read by tools and libraries that process your API in some way.
 For example, an API mocking tool can use sample values to generate mock requests. You can specify examples for objects,
  individual properties and operation parameters. To specify an example, you use the `example` or `examples` keys.

For example the **search text** used to filter bookmarks can have complex values, and what better ways to explain it than with
some examples:

```yaml
components:
  parameters:
    searchTextQueryParam:
      name: q
      in: query
      description: |
        search query (terms are separated by space). There are special filters available:
          * `lang:iso_language_code` - e.g. `lang:en` for English, `lang:es` for Spanish and `lang:de` for German bookmarks
          * `site:site_URL` - e.g. `site:codepedia.org` bookmarks only from website [www.codepedia.org](https://www.codepedia.org)
          * `userId:UUID-user` - to be used only when querying **public bookmarks** submitted by the user with  `userId`
          * `private:only` - makes sense **only** when used for querying **personal bookmarks**
      schema:
        type: string
      examples:       # Multiple examples
        german:
          value: 'lang:de'
          summary: Will look only for bookmarks in German
        site:
          value: 'site:codepedia.org'
          summary: Wille look only for bookmarks with the domain **codepedia.org**
        complex:
          value: 'exception handling [java] site:codepedia.org'
          summary: Wille look only for bookmarks with terms "exception" and "handling" tagged with "java" and the domain **codepedia.org**
        complex-private-only:
          value: 'exception handling [java] site:wiki.my-corporation.com private:only'
          summary: Same as above but only within **private** bookmarks
```

or show what a bookmark input for creation might look like, for different scenarios (normal article, youtube video or StackOverflow question):

```yaml
paths:
  /personal/users/{userId}/bookmarks:
    post:
      description: Create new bookmark for user
      operationId: create-bookmark
      tags:
        - personal-bookmarks
      parameters:
        - $ref: '#/components/parameters/userIdPathParam'
      requestBody:
        description: Bookmark json data
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Bookmark'
            examples:
              blog-article:
                value:
                  name: Cleaner code in an Express REST API with unified error handling – CodepediaOrg
                  location: https://www.codepedia.org/ama/cleaner-code-in-expressjs-rest-api-with-custom-error-handling
                  language: en
                  tags: [nodejs, error-handling, expressjs]
                  publishedOn: 2019-12-02,
                  sourceCodeURL: https://github.com/codeverland/codever
                  description: Shows how you can make your backend ExpressJS REST API cleaner by using custom error handling middleware. Code snippets of before and after refactoring are presented to make the point
                  descriptionHtml: <p>Shows how you can make your backend ExpressJS REST API cleaner by using custom error handling middleware. Code snippets of before and after refactoring are presented to make the point</p>
                  userId: 4c617f2b-2bad-498b-a9c6-4e9a8c303798
                  public: true,
                  lastAccessedAt: 2020-02-15T20:42:19.297Z
                  likeCount: 0
              stackoverflow-question:
                value:
                  name: Create GUID / UUID in JavaScript?
                  location: https://stackoverflow.com/questions/105034/create-guid-uuid-in-javascript
                  language: en
                  tags: [javascript, guid, uuid]
                  publishedOn: 2008-09-19
                  sourceCodeURL:
                  description: UUIDs (Universally Unique IDentifier), also known as GUIDs (Globally Unique IDentifier), according to [RFC 4122](https://www.ietf.org/rfc/rfc4122.txt), are identifiers with a certain uniqueness guarantee.\n\nThe best way to generate them, is to follow implementation instructions in the said RFC, use one of the many community vetted open source implementations.\n\nA popular Open Source tool for working with UUIDs in JavaScript is [node-uuid](https://github.com/kelektiv/node-uuid)\n\nNote that just randomly generating the identifiers byte by byte, or character by character, will not give you the same guarantees as a conforming implementation. Also, very important, systems working with compliant UUIDs may choose not to accept randomly generated ones, and many open source validators will actually check for a valid structure.\n\nAn UUID must have this format:\n```\nxxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx\n```\nWhere the M and N positions may only have certain values. At this time, the only valid values for M are 1, 2, 3, 4 and 5, so randomly generating that position would make most results unacceptable.
                  descriptionHtml: <p>UUIDs (Universally Unique IDentifier), also known as GUIDs (Globally Unique IDentifier), according to <a href=\"https://www.ietf.org/rfc/rfc4122.txt\">RFC 4122</a>, are identifiers with a certain uniqueness guarantee.</p>\n<p>The best way to generate them, is to follow implementation instructions in the said RFC, use one of the many community vetted open source implementations.</p>\n<p>A popular Open Source tool for working with UUIDs in JavaScript is <a href=\"https://github.com/kelektiv/node-uuid\">node-uuid</a></p>\n<p>Note that just randomly generating the identifiers byte by byte, or character by character, will not give you the same guarantees as a conforming implementation. Also, very important, systems working with compliant UUIDs may choose not to accept randomly generated ones, and many open source validators will actually check for a valid structure.</p>\n<p>An UUID must have this format:</p>\n<pre><code>xxxxxxxx-xxxx-Mxxx-Nxxx-xxxxxxxxxxxx\n</code></pre>\n<p>Where the M and N positions may only have certain values. At this time, the only valid values for M are 1, 2, 3, 4 and 5, so randomly generating that position would make most results unacceptable.</p>
                  userId: 4c617f2b-2bad-498b-a9c6-4e9a8c303798
                  public: true
                  lastAccessedAt: 2020-02-15T20:59:45.447Z
                  likeCount: 0
                  stackoverflowQuestionId: 105034
              youtube-video:
                value:
                  name: Jake Archibald In The Loop - JSConf.Asia - 35min
                  location: https://www.youtube.com/watch?v=cCOL7MC4Pl0
                  language: en
                  tags: [javascript, event-loop, browser]
                  publishedOn: 2018-02-09
                  sourceCodeURL:
                  description: Have you ever had a bug where things were happening in the wrong order, or particular style changes were being ignored? Ever fixed that bug by wrapping a section of code in a setTimeout? Ever found that fix to be unreliable, and played around with the timeout number until it kinda almost always worked? \nThis talk looks at the browser's event loop, the thing that orchestrates the main thread of the browser, which includes JavaScript, events, and rendering. We'll look at the difference between tasks, microtasks, requestAnimationFrame, requestIdleCallback, and where events land. \nHopefully you'll never have to use setTimeout hacks again!\"
                  descriptionHtml: <p>Have you ever had a bug where things were happening in the wrong order, or particular style changes were being ignored? Ever fixed that bug by wrapping a section of code in a setTimeout? Ever found that fix to be unreliable, and played around with the timeout number until it kinda almost always worked? \nThis talk looks at the browser's event loop, the thing that orchestrates the main thread of the browser, which includes JavaScript, events, and rendering. We'll look at the difference between tasks, microtasks, requestAnimationFrame, requestIdleCallback, and where events land. \nHopefully you'll never have to use setTimeout hacks again!\"</p>
                  userId: 4c617f2b-2bad-498b-a9c6-4e9a8c303798
                  public: true
                  lastAccessedAt: 2020-02-15T21:12:12.670Z
                  likeCount: 0
                  youtubeVideoId: cCOL7MC4Pl0
```

## 9. Use [enums](https://swagger.io/docs/specification/data-models/enums/) to define constants

Open API [does not support the `const` keyword of JSON Schema yet](https://swagger.io/docs/specification/data-models/keywords/)
 but you can use `enum` to define one value. For example I know that the validation errors have a HTTP Status of 400 (Bad request)
  so I can model this the following way:

```yaml
components:
  schemas:
    ValidationErrorModel:
      allOf:
        - $ref: '#/components/schemas/BasicErrorModel'
        - type: object
          required:
            - validationErrors
            - httpStatus
          properties:
            httpStatus:
              type: integer
              enum: [400]
            validationErrors:
              type: array
              items:
                type: string
```

Of course you can use enums to specify values for a request parameter or a model property, as they were intended. For example
see the  `orderBy` possible values when filtering personal bookmarks

```yaml
paths:
  /personal/users/{userId}/bookmarks:
    get:
      parameters:
        - $ref: "#/components/parameters/userIdPathParam"
        - $ref: "#/components/parameters/searchTextQueryParam"
        - $ref: "#/components/parameters/limitQueryParam"
        - $ref: "#/components/parameters/locationQueryParam"
        - name: orderBy
        in: query
          description: |
          It is considered in the abscence of `q` or `location` parameters
          Possible values:
            * MOST_LIKES - personal bookmarks most liked by the community
            * LAST_CREATED - personal bookmarks last added
            * MOST_USED - personal bookmarks the user (owner) clicked the most
          schema:
            type: string
            enum: [MOST_LIKES, LAST_CREATED, MOST_USED]
```
## 10. Bookmark the resources you might recall later

I have bookmarked quite a few Swagger/OpenAPI resources and tools along the way and made them public at
[my [openapi] public resources on bookmarks.dev](https://www.codever.land/?q=%5Bopenapi%5D%20user:33d22b0e-9474-46b3-9da4-b1fb5d273abc&sd=public&tab=search-results)


## Conclusion

These were ten takeaways when starting with OpenAPI Specification 3. I hope you find them useful and if you have others worth mentioning and trying
please leave a comment below.
