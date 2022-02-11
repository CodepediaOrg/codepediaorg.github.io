---
layout: post
title: Return an OK HTTP Response in Jax-Rs
description: "Return an OK HTTP Response in Jax-Rs code snippet"
author: ama
permalink: /snippets/620672f4e102f148ceb9f3ba/return-an-ok-http-response-in-jax-rs
published: true
snippetId: 620672f4e102f148ceb9f3ba
categories: [snippets]
tags: [java, jaxrs, http, http-status-codes, rest, codever-snippets]
---

Use the `ok()` method of the `javax.ws.rs.core.Reponse` class to create a `ReponseBuilder` with a status of 200 (OK),
or the `ok(Object entity)` to return **OK** with data

```java
import javax.ejb.Stateless;
import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;

@Path("comparison")
@Stateless
@Tag(name = "Comparison")
public class ComparisonRestResource {

  @Inject private ComparisonService comparisonService;

  @HEAD
  @Operation(
      summary = "Ping HEAD",
      description = "Check availability of the resource. ")
  @ApiResponses(@ApiResponse(responseCode = "200", description = "Service is reachable via HTTP"))
  public Response head() {
    return Response.ok().build();
  }

  @GET
  @Produces(MediaType.TEXT_PLAIN)
  @Operation(
      summary = "Ping GET",
      description = "Check availability of the example resource. ")
  @ApiResponses(@ApiResponse(responseCode = "200", description = "Service is reachable via HTTP"))
  public Response ping() {
    return Response.ok("pong").build();
  }
}
```

**Note** that the `ok()` methods shown before are just shortcuts for
```
return Response
        .status(Response.Status.OK)
        .build()
```

and

```
return Response
        .status(Response.Status.OK)
        .entity("pong")
        .build()
```
respectively.



<hr/>


 {% include snippet-post-recommendation-ending.html snippetId="620672f4e102f148ceb9f3ba" %}
