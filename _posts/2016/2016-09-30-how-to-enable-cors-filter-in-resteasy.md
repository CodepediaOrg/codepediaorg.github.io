---
layout: post
title: How to enable the CORS Filter in RESTEasy
description: "In this post I will shortly present how to enable the provided RESTEasy CORS Filter in a JAX-RS REST API backend"
author: ama
permalink: /ama/how-to-enable-cors-filter-in-resteasy
published: true
categories: [article]
tags: [jboss, wildfly, rest, resteasy, javaee, cors]
---

In this post I will shortly present how to enable the provided RESTEasy CORS Filter in a REST API backend application. Well, it is officially documented, but for me was not that
straightforward to use it and I want to share how I accomplished this. If you are new to CORS and want to learn more about it I suggest you read the document from Mozilla Developer Network - [HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

<!--more-->

Starting with version _3.0.7.Final_ the RESTEasy framework has a `ContainerRequestFilter` that can be used to handle CORS preflight and actual requests - `org.jboss.resteasy.plugins.interceptors.CorsFilter`.
In the documentation it says that "you must allocate this and register it as a singleton provider from your Application class. See the javadoc or its various settings." [^1]
Here is the code snippet provided with that:

```java
CorsFilter filter = new CorsFilter();
filter.getAllowedOrigins().add("http://localhost");
```

[^1]: <https://docs.jboss.org/resteasy/docs/3.0.19.Final/userguide/html_single/index.html#d4e1343>

Well, to make it work for me I had to register it to the `classes` component of the JAX-RS `Application` bootstrap class:

```java
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;
import java.util.HashSet;
import java.util.Set;

@ApplicationPath("")
public class TestApplication extends Application {

    private Set<Object> singletons = new HashSet<Object>();
    private HashSet<Class<?>> classes = new HashSet<Class<?>>();

    public TestApplication()
    {
        CorsFilter corsFilter = new CorsFilter();
        corsFilter.getAllowedOrigins().add("*");
        corsFilter.setAllowedMethods("OPTIONS, GET, POST, DELETE, PUT, PATCH");
        singletons.add(corsFilter);

        classes.add(ApiResource.class);
        classes.add(UsersResource.class);
    }

    @Override
    public Set<Object> getSingletons() {
        return singletons;
    }

    @Override
    public HashSet<Class<?>> getClasses(){
      return classes;
    }
}
```

In the example above I enabled the HTTP Methods `OPTIONS, GET, POST, DELETE, PUT, PATCH` and calls from all origins, specified by __*__ - `corsFilter.getAllowedOrigins().add("*");`

If you don't want to make you dependent of the RESTEasy framework and you can implement one yourself. A good starting point the [RESTEasy Filter itself](https://github.com/resteasy/Resteasy/blob/4e5acbb3f61263a300f0316d952233a404f9b702/resteasy-jaxrs/src/main/java/org/jboss/resteasy/plugins/interceptors/CorsFilter.java), which, as said, handles CORS[^12] requests both preflight and simple CORS requests.
I am just to copy the source code here

```java
package org.jboss.resteasy.plugins.interceptors;

import org.jboss.resteasy.resteasy_jaxrs.i18n.Messages;
import org.jboss.resteasy.spi.CorsHeaders;

import javax.ws.rs.ForbiddenException;
import javax.ws.rs.container.ContainerRequestContext;
import javax.ws.rs.container.ContainerRequestFilter;
import javax.ws.rs.container.ContainerResponseContext;
import javax.ws.rs.container.ContainerResponseFilter;
import javax.ws.rs.container.PreMatching;
import javax.ws.rs.core.Response;

import java.io.IOException;
import java.util.HashSet;
import java.util.Set;

/**
 * Handles CORS requests both preflight and simple CORS requests.
 * You must bind this as a singleton and set up allowedOrigins and other settings to use.
 *
 * @author <a href="mailto:bill@burkecentral.com">Bill Burke</a>
 * @version $Revision: 1 $
 */
@PreMatching
public class CorsFilter implements ContainerRequestFilter, ContainerResponseFilter
{
   protected boolean allowCredentials = true;
   protected String allowedMethods;
   protected String allowedHeaders;
   protected String exposedHeaders;
   protected int corsMaxAge = -1;
   protected Set<String> allowedOrigins = new HashSet<String>();

   /**
    * Put "*" if you want to accept all origins
    *
    * @return
    */
   public Set<String> getAllowedOrigins()
   {
      return allowedOrigins;
   }

   /**
    * Defaults to true
    *
    * @return
    */
   public boolean isAllowCredentials()
   {
      return allowCredentials;
   }

   public void setAllowCredentials(boolean allowCredentials)
   {
      this.allowCredentials = allowCredentials;
   }

   /**
    * Will allow all by default
    *
    * @return
    */
   public String getAllowedMethods()
   {
      return allowedMethods;
   }

   /**
    * Will allow all by default
    * comma delimited string for Access-Control-Allow-Methods
    *
    * @param allowedMethods
    */
   public void setAllowedMethods(String allowedMethods)
   {
      this.allowedMethods = allowedMethods;
   }

   public String getAllowedHeaders()
   {
      return allowedHeaders;
   }

   /**
    * Will allow all by default
    * comma delimited string for Access-Control-Allow-Headers
    *
    * @param allowedHeaders
    */
   public void setAllowedHeaders(String allowedHeaders)
   {
      this.allowedHeaders = allowedHeaders;
   }

   public int getCorsMaxAge()
   {
      return corsMaxAge;
   }

   public void setCorsMaxAge(int corsMaxAge)
   {
      this.corsMaxAge = corsMaxAge;
   }

   public String getExposedHeaders()
   {
      return exposedHeaders;
   }

   /**
    * comma delimited list
    *
    * @param exposedHeaders
    */
   public void setExposedHeaders(String exposedHeaders)
   {
      this.exposedHeaders = exposedHeaders;
   }

   @Override
   public void filter(ContainerRequestContext requestContext) throws IOException
   {
      String origin = requestContext.getHeaderString(CorsHeaders.ORIGIN);
      if (origin == null)
      {
         return;
      }
      if (requestContext.getMethod().equalsIgnoreCase("OPTIONS"))
      {
         preflight(origin, requestContext);
      }
      else
      {
         checkOrigin(requestContext, origin);
      }
   }

   @Override
   public void filter(ContainerRequestContext requestContext, ContainerResponseContext responseContext) throws IOException
   {
      String origin = requestContext.getHeaderString(CorsHeaders.ORIGIN);
      if (origin == null || requestContext.getMethod().equalsIgnoreCase("OPTIONS") || requestContext.getProperty("cors.failure") != null)
      {
         // don't do anything if origin is null, its an OPTIONS request, or cors.failure is set
         return;
      }
      responseContext.getHeaders().putSingle(CorsHeaders.ACCESS_CONTROL_ALLOW_ORIGIN, origin);
      if (allowCredentials) responseContext.getHeaders().putSingle(CorsHeaders.ACCESS_CONTROL_ALLOW_CREDENTIALS, "true");

      if (exposedHeaders != null) {
         responseContext.getHeaders().putSingle(CorsHeaders.ACCESS_CONTROL_EXPOSE_HEADERS, exposedHeaders);
      }
   }


   protected void preflight(String origin, ContainerRequestContext requestContext) throws IOException
   {
      checkOrigin(requestContext, origin);

      Response.ResponseBuilder builder = Response.ok();
      builder.header(CorsHeaders.ACCESS_CONTROL_ALLOW_ORIGIN, origin);
      if (allowCredentials) builder.header(CorsHeaders.ACCESS_CONTROL_ALLOW_CREDENTIALS, "true");
      String requestMethods = requestContext.getHeaderString(CorsHeaders.ACCESS_CONTROL_REQUEST_METHOD);
      if (requestMethods != null)
      {
         if (allowedMethods != null)
         {
            requestMethods = this.allowedMethods;
         }
         builder.header(CorsHeaders.ACCESS_CONTROL_ALLOW_METHODS, requestMethods);
      }
      String allowHeaders = requestContext.getHeaderString(CorsHeaders.ACCESS_CONTROL_REQUEST_HEADERS);
      if (allowHeaders != null)
      {
         if (allowedHeaders != null)
         {
            allowHeaders = this.allowedHeaders;
         }
         builder.header(CorsHeaders.ACCESS_CONTROL_ALLOW_HEADERS, allowHeaders);
      }
      if (corsMaxAge > -1)
      {
         builder.header(CorsHeaders.ACCESS_CONTROL_MAX_AGE, corsMaxAge);
      }
      requestContext.abortWith(builder.build());

   }

   protected void checkOrigin(ContainerRequestContext requestContext, String origin)
   {
      if (!allowedOrigins.contains("*") && !allowedOrigins.contains(origin))
      {
         requestContext.setProperty("cors.failure", true);
         throw new ForbiddenException(Messages.MESSAGES.originNotAllowed(origin));
      }
   }
}
```

> About CORS Filter enabling and how I migrated a Java EE REST API from JAX-RS 1.0 to JAX-RS 2.0 with RESTEasy I have written in my article [How to migrate a Java EE REST API from JBoss EAP 6 to EAP 7](https://www.codepedia.org/ama/how-to-migrate-a-java-ee-rest-api-from-jboss-eap6-to-jboss-eap7)

> Other REST related posts can be found at [https://www.codepedia.org/tags/#rest](https://www.codepedia.org/tags/#rest)


## References
