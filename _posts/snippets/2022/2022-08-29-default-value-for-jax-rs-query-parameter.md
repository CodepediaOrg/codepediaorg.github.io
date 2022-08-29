---
layout: post
title: Default value for jax-rs query parameter
description: "Default value for jax-rs query parameter code snippet"
author: ama
permalink: /snippets/630c963fe8e6db43eab06439/default-value-for-jax-rs-query-parameter
published: true
snippetId: 630c963fe8e6db43eab06439
categories: [snippets]
tags: [java, rest, jaxrs, codever-snippets]
---

Use `DefaultValue` annotation parameter (accepts **strings**) alongside the `QueryParam` annotation

```java
@GET
@Path("/bookmarks")
@Produces(MediaType.APPLICATION_JSON)
@Operation(
    summary = "Return bookmarks from repository",
    description = "Return bookmarks from repository")
@ApiResponses({
    @ApiResponse(responseCode = "200", description = "OK"),
    @ApiResponse(responseCode = "403", description = "Forbidden")
})
@RolesAllowed(ADMIN_ROLE)
public void getAllBookmarks(
    @Parameter(description = "max number of returned bookmarks")
    @DefaultValue(Integer.MAX_VALUE + "")
    @QueryParam("maxResult") Integer maxResult) {
  bookmarksService.getBookmarks(maxResult);
}
```


<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="630c963fe8e6db43eab06439" %}
