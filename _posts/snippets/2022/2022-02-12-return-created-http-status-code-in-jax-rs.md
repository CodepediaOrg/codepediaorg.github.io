---
layout: post
title: Return CREATED HTTP Status Code in Jax-Rs
description: "Return CREATED HTTP Status Code in Jax-Rs code snippet"
author: ama
permalink: /snippets/62069c03e102f148ceb9f5c3/return-created-http-status-code-in-jax-rs
published: true
snippetId: 62069c03e102f148ceb9f5c3
categories: [snippets]
tags: [java, jaxrs, rest, javaee, codever-snippets]
---

Use `created(URI location)`  of the `javax.ws.rs.core.Response` class. Usually when you return the 201 HTTP Status Code,
you should return a `location` header with the location where the new REST resource is available.

```java
import javax.ejb.Stateless;
import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.*;

@Path("messages")
@Stateless
@Tag(name = "Message")
public class MessageRestResource {

  @Inject private MessageService messageService;

  @POST
  @Consumes(MediaType.TEXT_PLAIN)
  @Operation(summary = "Create message")
  @ApiResponses({
    @ApiResponse(
        responseCode = "201",
        description = "Message successfully created."),
    @ApiResponse(responseCode = "403", description = "Forbidden")
  })
  public Response createMessage(
      @Parameter(description = "Message") String message, @Context UriInfo uriInfo)
      throws JAXBException {
    Message created = messageService.createMessage(message);
    UriBuilder builder = uriInfo.getAbsolutePathBuilder();
    builder.path(created.getUuid().toString());
    return Response.created(builder.build()).build();
  }
```

In the snippet the `UriBuilder` method  `builder.path(created.getUuid().toString())`
appends the identifier of the newly created message (`uuid`) to the absolute path of the request,
which was obtained from the `uriInfo`

<hr/>

 {% include snippet-post-recommendation-ending.html snippetId="62069c03e102f148ceb9f5c3" %}
