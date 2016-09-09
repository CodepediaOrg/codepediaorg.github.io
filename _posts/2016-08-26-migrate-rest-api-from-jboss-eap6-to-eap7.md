---
layout: post
title: How to migrate a Java EE REST API from JBoss EAP 6 to EAP 7
description: "In this post I will present a few points I had to do to migrate a Java REST API from JBoss EAP 6/JEE 6/JAX RS 1.0 to Jboss EAP7/JEE 7/JAX RS 2.0"
author: ama
permalink: /ama/how-to-migrate-a-java-ee-rest-api-from-jboss-eap6-to-jboss-eap7
published: true
categories: [java ee]
tags: [jboss, wildfly, rest, resteasy, java ee, keycloak, cors]
---

In this post I will briefly describe the steps and issues encountered when migrating a Java EE REST API from JBoss EAP 6 to JBoss EAP 7 - this implies migrating from a Java EE 6/JAX RS 1.0 implementation to a Java EE 7/JAX RS.2.0 implementation. The trigger was the announcement from Red Hat regarding the general availability of their JBoss Enterprise Application Platform 7 (JBoss EAP) [^1]. JBoss EAP 7 is based on Wildfly 10[^2], so the code snippets showed along the post should work on Wildfly 10 too.

[^1]: <https://www.redhat.com/en/about/press-releases/red-hat-delivers-jboss-eap-7-foundation-hybrid-cloud-applications>
[^2]: <https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/7.0/paged/introduction-to-jboss-eap/chapter-2-overview-of-jboss-eap>

<!--more-->

* TOC
{:toc}

## Update dependencies versions

The first thing I've done was to migrate the library dependencies used in the project. You can find usual component versions, like RESTEasy, Hibernate, Hibernate-validator etc. at [JBoss Enterprise Application Platform Component Details](https://access.redhat.com/articles/112673#EAP_7). A couple of example are:

|    Library      |  JBoss EAP 6  |  JBoss EAP 7  |
| --------------  |:-------------:| -------------:|
| Hibernate Core  | 4.2.9.Final   | 5.0.9.Final   |
| RESTEasy        | 2.3.6.Final   | 3.0.16.Final  |

> Be aware to adjust the versions whenever you update to a newer minor/major release

### Update the JBoss JavaEE Specs API [^3]

[^3]: <https://github.com/jboss/jboss-javaee-specs>

**From**

```xml
<dependency>
    <groupId>org.jboss.spec</groupId>
    <artifactId>jboss-javaee-6.0</artifactId>
    <version>3.0.3.Final</version>
    <type>pom</type>
    <scope>provided</scope>
</dependency>
```

**to**

```xml
<!-- https://mvnrepository.com/artifact/org.jboss.spec/jboss-javaee-7.0 -->
<dependency>
    <groupId>org.jboss.spec</groupId>
    <artifactId>jboss-javaee-7.0</artifactId>
    <version>1.0.3.Final</version>
    <type>pom</type>
</dependency>
```

## Java EE Deployment Descriptors

The next thing to modify, were the Java EE deployment descriptors. Check out the [Java EE: XML Schemas for Java EE Deployment Descriptors](http://www.oracle.com/webfolder/technetwork/jsc/xml/ns/javaee/index.html) document from Oracle to see the new descriptor versions and namespaces. All Java EE 7 and newer Deployment Descriptor Schemas share now the namespace __http://xmlns.jcp.org/xml/ns/javaee/__

Down below are listed the ones I used in my project:

### web.xml

**From**

```xml
<web-app
        xmlns="http://java.sun.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
        version="3.0">
.....
</web-app>
```

**to**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
```

### beans.xml

**From**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/beans_1_0.xsd">
</beans>
```

**To**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
       version="1.1" bean-discovery-mode="all">
</beans>
```

### persistence.xml

**From**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence
        xmlns="http://java.sun.com/xml/ns/persistence"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
        version="2.0">
...........
</persistence>

```

**To**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
             version="2.1">
...........
</persistence>

```

## Meet JAX-RS 2.0

> The RESTEasy documentation version referenced throughout this post is [3.0.16.Final](http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html), as this is the version used for JBoss EAP 7.0.0, for which the migration took place at the time of the writing.
For other/newer versions check the [RESTEasy Documentation](http://resteasy.jboss.org/docs.html), where you can find  examples, HTML, PDF, Javadocs for all RESTEasy versions.

### Server API

On the server side the things have remained much or less the same. One has to admit they were very good to begin with.
The way to go with with RESTEasy is to define an `javax.ws.rs.core.Application` class that is annotated with the `@ApplicationPath` annotation.If you return an empty set of classes and singletons (or nothing), your WAR will be scanned for JAX-RS annotation resource and provider classes[^4]:

```java
import javax.ws.rs.ApplicationPath;
import javax.ws.rs.core.Application;

@ApplicationPath("/root-path")
public class MyApplication extends Application
{
}     
```

[^4]: <http://docs.jboss.org/resteasy/docs/3.0.19.Final/userguide/html_single/index.html#d4e42>

### Client API

JAX-RS 1.0 was more or less a server side API. To write client calls one would most likely go to Apache's HTTP Client[^5]. Now JAX-RS 2.0  introduces a new API to make requests to REST web services. I needed such a REST client to make api calls to a Keycloak[^6] Admin REST API[^7]. Although the current RESTEasy implementation comes with JAX-RS 2.0 support[^8], I preferred to use  the `RestEasyClientBuilder` implementation in combination with the Resteasy Proxy Framework[^7], because I've used it like that in JBoss EAP 6 and I still find it cool to use JAX-RS annotations on the client side too. The way it works is that you write a Java interface and use JAX-RS annotations on methods of the interface. Check out the code snippets posted below and the documentation[^9] to see what I mean

[^5]: <https://hc.apache.org/httpcomponents-client-ga/>
[^6]: <http://www.keycloak.org>
[^7]: <http://www.keycloak.org/docs/rest-api/index.html>
[^8]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#RESTEasy_Client_Framework>
[^9]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#d4e2149>

#### REST Client interface

From

```java
import org.jboss.resteasy.client.ClientResponse;
import org.keycloak.representations.idm.CredentialRepresentation;
import org.keycloak.representations.idm.RoleRepresentation;
import org.keycloak.representations.idm.UserRepresentation;

import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import java.util.List;

/**
 * Client to access the Keycloak admin REST API
 */
public interface KeycloakApiClient {

    @POST
    @Path("users")
    @Consumes(MediaType.APPLICATION_JSON)
    ClientResponse<String> createUser(UserRepresentation userRepresentation,
                                      @HeaderParam("Authorization") String authorization);

    @GET
    @Path("users")
    @Produces(MediaType.APPLICATION_JSON)
    ClientResponse<List<UserRepresentation>> getUserByEmail(@QueryParam("email") String email,
                                      @HeaderParam("Authorization") String authorization);
    ...........
}
```

to

```java
import org.jboss.resteasy.client.ClientResponse;
import org.keycloak.representations.idm.CredentialRepresentation;
import org.keycloak.representations.idm.RoleRepresentation;
import org.keycloak.representations.idm.UserRepresentation;

import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;
import java.util.List;

/**
 * Client to access the Keycloak admin REST API
 */
public interface KeycloakApiClient {

    @POST
    @Path("users")
    @Consumes(MediaType.APPLICATION_JSON)
    Response createUser(UserRepresentation userRepresentation,
                        @HeaderParam("Authorization") String authorization);

    @GET
    @Path("users")
    @Produces(MediaType.APPLICATION_JSON)
    List<UserRepresentation>  getUserByEmail(@QueryParam("email") String email,
                                      @HeaderParam("Authorization") String authorization);

    ...............................
}
```

#### REST client producer

From

```java
import org.jboss.resteasy.client.ProxyFactory;
import javax.enterprise.inject.Produces;

public class KeycloakApiClientProducer {

    private static final String KEYCLOAK_AUTH_ADMIN_REALMS_TEST_BASE_URL = "http://localhost:8180/auth/admin/realms/podcastpedia";

    @Produces
    public KeycloakApiClient getKeycloakApiClient(){
        return ProxyFactory.create(KeycloakApiClient.class, KEYCLOAK_AUTH_ADMIN_REALMS_TEST_BASE_URL);
    }
}
```

to

```java
import org.jboss.resteasy.client.jaxrs.ResteasyClient;
import org.jboss.resteasy.client.jaxrs.ResteasyClientBuilder;
import org.jboss.resteasy.client.jaxrs.ResteasyWebTarget;

import javax.enterprise.inject.Produces;

public class KeycloakApiClientProducer {

    private static final String KEYCLOAK_AUTH_ADMIN_REALMS_TEST_BASE_URL = "http://localhost:8180/auth/admin/realms/podcastpedia";

    @Produces
    public KeycloakApiClient getKeycloakApiClient(){
        ResteasyClientBuilder resteasyClientBuilder = new ResteasyClientBuilder().connectionPoolSize(20);
        ResteasyClient client = resteasyClientBuilder.build();
        ResteasyWebTarget target = client.target(KEYCLOAK_AUTH_ADMIN_REALMS_TEST_BASE_URL);

        return target.proxy(KeycloakApiClient .class);
    }
}
```

> A producer method acts as a source of objects to be injected[^10] in CDI

[^10]: <http://stackoverflow.com/questions/16534728/please-explain-the-produces-annotation-in-cdi>

#### REST Client Usage

**From**

```
@Inject
KeycloakClient keycloakClient;

String keycloakUserId;
ClientResponse<String> userDetailsUpdateResponse = null;
try{
    userDetailsUpdateResponse = keycloakApiClient.createUser(userRepresentation, bearerToken);
    if(userDetailsUpdateResponse.getResponseStatus() != Response.Status.CREATED) {
        ErrorMessage errorMessage = new ErrorMessage.Builder()
                .httpStatus(userDetailsUpdateResponse.getResponseStatus().getStatusCode())
                .message("Error when trying to create user in Keycloak")
                .build();

        throw new AppException("Error when trying to create user in Keycloak", errorMessage);
    } else {
        //get keycloakUserId of the user
        String location = userDetailsUpdateResponse.getHeaders().get("Location").get(0);
        String[] split = location.split("/");
        keycloakUserId = split[split.length - 1];
    }
} finally {
    if(userDetailsUpdateResponse != null) {
        userDetailsUpdateResponse.releaseConnection();
    }
}

ClientResponse<UserRepresentation> userDetailsRepresentation  = null;
String ldapId = null;
try{
    userDetailsRepresentation = keycloakApiClient.getUserDetails(keycloakUserId, bearerToken);
    UserRepresentation userDetails = userDetailsRepresentation.getEntity();

    if(userDetailsRepresentation.getResponseStatus() != Response.Status.OK) {
        ErrorMessage errorMessage = new ErrorMessage.Builder()
                .httpStatus(userDetailsRepresentation.getResponseStatus().getStatusCode())
                .message("Error when trying to get Keycloak user details")
                .build();

        throw new AppException("Error when trying to get Keycloak user details", errorMessage);
    }

    ldapId = userDetails.getAttributesAsListValues().get("LDAP_ID").get(0);
} finally {
    if(userDetailsRepresentation !=null) {
        userDetailsRepresentation.releaseConnection();
    }
}
```

**To**

```java
@Inject
KeycloakClient keycloakClient;
.......
String keycloakUserId;
Response userDetailsUpdateResponse = null;
try{
    userDetailsUpdateResponse = keycloakApiClient.createUser(userRepresentation, realmAdminBearerToken);
    if(userDetailsUpdateResponse.getStatus() != Response.Status.CREATED.getStatusCode()) {
        ErrorMessage errorMessage = new ErrorMessage.Builder()
                .httpStatus(userDetailsUpdateResponse.getStatus())
                .message("Error when trying to create user in Keycloak")
                .build();

        throw new AppException("Error when trying to create user in Keycloak", errorMessage);
    } else {
        //get keycloakUserId of the user
        String location = (String) userDetailsUpdateResponse.getHeaders().get("Location").get(0);

        String[] split = location.split("/");
        keycloakUserId = split[split.length - 1];
    }
} finally {
    userDetailsUpdateResponse.close();
}

UserRepresentation userDetails = keycloakApiClient.getUserDetails(keycloakUserId, realmAdminBearerToken);
```

> Note in the examples above that Resteasy will release the connection under covers. On the other hand, if the result of an invocation is an instance of **Response**, then **Response.close()** method must be used to released the connection - the method <code class="java"> Response createUser(UserRepresentation userRepresentation, @HeaderParam("Authorization") String authorization)</code> and its usage in the **To** snippet above is a good example

### Filters and Interceptors

#### Logging interceptor

The logging interceptor is used to trace incoming request paths and responses at INFO level and now is implemented with standard JAX RS 2.0 filters - `javax.ws.rs.container.ContainerResponseFilter`

**From**

```java
import org.jboss.resteasy.annotations.interception.ServerInterceptor;
import org.jboss.resteasy.core.ResourceMethod;
import org.jboss.resteasy.core.ServerResponse;
import org.jboss.resteasy.spi.Failure;
import org.jboss.resteasy.spi.HttpRequest;
import org.jboss.resteasy.spi.interception.PreProcessInterceptor;
import org.slf4j.Logger;

import javax.inject.Inject;
import javax.servlet.http.HttpServletRequest;
import javax.ws.rs.WebApplicationException;
import javax.ws.rs.core.Context;
import javax.ws.rs.ext.Provider;

/**
 * Basic logging interceptor
 */
@Provider
@ServerInterceptor
public class LoggingInterceptor implements PreProcessInterceptor {

    @Inject
    private Logger logger;

    @Context
    HttpServletRequest servletRequest;

    public ServerResponse preProcess(HttpRequest request,
                                     ResourceMethod resourceMethod) throws Failure,
            WebApplicationException {

        String methodName = resourceMethod.getMethod().getName();

        String httpMethod = resourceMethod.getHttpMethods().iterator().next();

        logger.info("Receiving " + httpMethod + " request from: " + servletRequest.getRemoteAddr());
        logger.info("Attempt to invoke method \"" + methodName + "\"");

        return null;
    }
}
```

**To**

```java
import org.codehaus.jackson.map.ObjectMapper;
import org.slf4j.Logger;

import javax.inject.Inject;
import javax.ws.rs.container.ContainerRequestContext;
import javax.ws.rs.container.ContainerResponseContext;
import javax.ws.rs.core.Context;
import javax.ws.rs.core.UriInfo;
import javax.ws.rs.ext.Provider;
import java.io.IOException;

/**
 * Basic logging interceptor for all API calls. Logs in INFO log level the HTTP Method, path and the response entity
 *
 */
@Provider
public class LoggingInterceptor implements javax.ws.rs.container.ContainerResponseFilter {

    @Inject
    private Logger logger;

    @Context
    private UriInfo uriInfo;

    @Override
    public void filter(ContainerRequestContext containerRequestContext, ContainerResponseContext containerResponseContext) throws IOException {
        String httpMethod = containerRequestContext.getRequest().getMethod();

        int httpResponseStatus = containerResponseContext.getStatus();

        logger.info(httpMethod.toUpperCase() + " " + uriInfo.getPath() + " " + httpResponseStatus);
        ObjectMapper mapper = new ObjectMapper();
        logger.info("Response entity: \n"
                + mapper.writerWithDefaultPrettyPrinter().writeValueAsString(containerResponseContext.getEntity())
                + "\n");
    }
}
```

> All interceptors and filters registered with the `@Provider` annotation are globally enabled for all resources.

#### CORS interceptor

Resteasy has a built-in `ContainerRequestFilter` that can be used to handle CORS preflight and actual requests -  the `org.jboss.resteasy.plugins.interceptors.CorsFilter`[^11]. In order to use it, you must allocate this and register it as a singleton provider from your `Application` class. See below an example:

[^11]:<http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#d4e1327>

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

> If you want to implement a CORS filter yourself and not make you dependent of the RESTEasy framework you can just inspire yourself from the [RESTEasy implementation](https://github.com/resteasy/Resteasy/blob/4e5acbb3f61263a300f0316d952233a404f9b702/resteasy-jaxrs/src/main/java/org/jboss/resteasy/plugins/interceptors/CorsFilter.java), which, as said, handles CORS[^12] requests both preflight and simple CORS requests.

[^12]:<https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS>

## Jackson

Besides the Jettison JAXB adapter for JSON, Resteasy also support integration with the Jackson project. Many users (I am one of them) find the output from Jackson much much nicer than the Badger format or Mapped format provided by Jettison. Jackson allows you to easily marshal Java objects to and from JSON. It has a Java Bean based model as well as JAXB like APIs. Resteasy integrates with the JavaBean model.

While Jackson does come with its own JAX-RS integration. Resteasy expanded it a little.To include it within your project, just add this maven dependency to your build. Resteasy supports both Jackson 1.9.x and Jackson 2.2.x. Read further on how to use each.[^13].

[^13]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#json>

Because of the Keycloak version I am using, 1.7.0.Final still use Jackson Version 1.9.x (apparently the newest one, 2.1.0.Final, also uses the same version), I had to convince the JBoss Server EAP 7 that this is the version I want. To do this I had to import the RestEasy Jackson Provider maven dependency and mark it as _provided_:

```xml
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-jackson-provider</artifactId>
        <version>3.0.16.Final</version>
        <scope>provided</scope>
    </dependency>
```

and also create a `jboss-deployment-structure.xml` file within the *WEB-INF* directory and tell JBoss to exclude the `resteasy-jackson2-provider` and import the `resteasy-jackson-provider` :

```xml
<jboss-deployment-structure>
    <deployment>
        <exclusions>
            <module name="org.jboss.resteasy.resteasy-jackson2-provider"/>
        </exclusions>
        <dependencies>
            <module name="org.jboss.resteasy.resteasy-jackson-provider" services="import"/>
        </dependencies>
    </deployment>
</jboss-deployment-structure>
```

Some of the Jackson classes can now be used throughout the application, like for example the `ObjectMapper`, to pretty print in JSON a `AppException`:

```java
    ObjectMapper mapper = new ObjectMapper();
    try {
        logger.error("Application Exception: \n "
                + mapper.writerWithDefaultPrettyPrinter().writeValueAsString(appException.getErrorMessage())
                + "\n");
    } catch (IOException e1) {
        logger.error("Error when pretty printing the error message", e1);
    }
```

### Keycloak

As mentioned, the Keycloak core uses also Jackson, and to avoid a potential conflicting  or undebuggable errors, I force Keycloak to use the Jackson packages that come with the RESTEasy provider via maven exclusions:

```xml
    <dependency>
        <groupId>org.keycloak</groupId>
        <artifactId>keycloak-core</artifactId>
        <version>${keycloak-version}</version>
        <scope>provided</scope>
        <exclusions>
            <exclusion>
                <groupId>org.codehaus.jackson</groupId>
                <artifactId>jackson-core-asl</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.codehaus.jackson</groupId>
                <artifactId>jackson-mapper-asl</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
```

## Other good REST(easy) related resources

* [RESTEasy Documentation examples, HTML, PDF, Javadocs](http://resteasy.jboss.org/docs.html) - for all versions...
* [Java EE 7 and JAX-RS 2.0](http://www.oracle.com/technetwork/articles/java/jaxrs20-1929352.html) by Adam Bien
* [What's new in JAX-RS 2.0](https://www.infoq.com/news/2013/06/Whats-New-in-JAX-RS-2.0) at InfoQ
* [Java EE 7 Deployment Descriptors](https://antoniogoncalves.org/2013/06/04/java-ee-7-deployment-descriptors/)

## References
