---
layout: post
title: Pass input as text in a POST request body in Jax-RS
description: "Pass input as text in a POST request body in Jax-RS code snippet"
author: ama
permalink: /snippets/606c2030c6a18456bf09aa8f/pass-input-as-text-in-a-post-request-body-in-jax-rs
published: true
snippetId: 606c2030c6a18456bf09aa8f
categories: [snippets]
tags: [java, rest, jaxrs, resteasy, codever-snippets]
---

Use `MediaType.TEXT_PLAIN` in the `@Consumes` annotation and then you have access to the text content as string

```java
    @POST
    @Path("organisation")
    @Consumes(MediaType.TEXT_PLAIN)
    @ApiOperation(value = "Create bookmark from text")
    @ApiResponses({
            @ApiResponse(code = 201, message = "Bookmark successfully created.", response = Bookmark.class),
            @ApiResponse(code = 403, message = "Forbidden")
    })
    public Response createBookmark(@ApiParam("Bookmark") String boookmark, @Context UriInfo uriInfo) throws JAXBException {
        Bookmark created = bookmarkService.createBookmarkFromString(bookmark);
        UriBuilder builder = uriInfo.getAbsolutePathBuilder();
        builder.path(created.getUuid().toString());
        return Response.created(builder.build()).build();
    }
```


<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="606c2030c6a18456bf09aa8f" %}
