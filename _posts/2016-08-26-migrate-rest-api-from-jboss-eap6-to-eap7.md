---
layout: post
title: How to migrate a Java EE REST API from JBoss EAP 6 to EAP 7
description: "In this post I will list a few points I had to do to migrate a Java REST API from JBoss EAP 6/JEE 6/JAX RS 1.0 to Jboss EAP7/JEE 7/JAX RS 2.0"
author: ama
permalink: /ama/how-to-migrate-a-java-ee-rest-api-from-jboss-eap6-to-jboss-eap7
published: true
categories: [java ee]
tags: [jboss, wildfly, rest, resteasy, java ee, keycloak]
---

In this post I will briefly describe the steps and issues encountered when migrating a Java EE REST API from JBoss EAP 6 to JBoss EAP 7. Not long time ago Red Hat  announced the general availability of their JBoss Enterprise Application Platform 7 (JBoss EAP) [^1] - this was great news because one can use Java EE 7 in the enterprise and benefit from the Red Hat's support. JBoss EAP 7 is based on Wildfly 10[^2], so the code snippets showed along the post should work there too.

[^1]: <https://www.redhat.com/en/about/press-releases/red-hat-delivers-jboss-eap-7-foundation-hybrid-cloud-applications>
[^2]: <https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/7.0/paged/introduction-to-jboss-eap/chapter-2-overview-of-jboss-eap>

<!--more-->

## Update dependencies versions

The first I've done was to migrate the library dependencies used in the project. You can find usual component version like RESTEasy, Hibernate, Hibernate-validator etc. at [JBoss Enterprise Application Platform Component Details](https://access.redhat.com/articles/112673#EAP_7). A couple of example are:

|    Library      |  JBoss EAP 6  |  JBoss EAP 7  |
| --------------  |:-------------:| -------------:|
| Hibernate Core  | 4.2.9.Final   | 5.0.9.Final   |
| RESTEasy        | 2.3.6.Final   | 3.0.16.Final  |

> Be aware to adjust the versions whenever you update to a newer minor/major release


### Update the JBoss JavaEE Specs API [^3]

[^3]: <https://github.com/jboss/jboss-javaee-specs>

From

```
<jboss-javaee-6.0.version>3.0.3.Final</jboss-javaee-6.0.version>

<dependency>
    <groupId>org.jboss.spec</groupId>
    <artifactId>jboss-javaee-6.0</artifactId>
    <version>${jboss-javaee-6.0.version}</version>
    <type>pom</type>
    <scope>provided</scope>
</dependency>
```

to

```
<jboss-javaee-7.0.version>1.0.3.Final</jboss-javaee-7.0.version>

<!-- https://mvnrepository.com/artifact/org.jboss.spec/jboss-javaee-7.0 -->
<dependency>
    <groupId>org.jboss.spec</groupId>
    <artifactId>jboss-javaee-7.0</artifactId>
    <version>${jboss-javaee-7.0.version}</version>
    <type>pom</type>
</dependency>
```

## Java EE Deployment Descriptors

The next thing to modify were the Java EE deployment descriptors. Check out the [Java EE: XML Schemas for Java EE Deployment Descriptors](http://www.oracle.com/webfolder/technetwork/jsc/xml/ns/javaee/index.html) document from Oracle to see the new descriptor versions and namespaces. All Java EE 7 and newer Deployment Descriptor Schemas share now the namespace __http://xmlns.jcp.org/xml/ns/javaee/__

Down below are listed the ones I needed in my project:

### web.xml

From

```
<web-app
        xmlns="http://java.sun.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
        version="3.0">
.....
</web-app>
```

to

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
```

### beans.xml

From

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/beans_1_0.xsd">
</beans>
```

to

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
       version="1.1" bean-discovery-mode="all">
</beans>
```

### persistence.xml
TODO add here and test

## REST API modifications

> [The documentation for RESTEasy is for the version 3.0.16.Final](http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html) as this is the version used for JBoss EAP 7.0.0 for which the migration took place.


### REST API

TODO - modify the version and hypermedia stuff...

### RESTEasy Client API

The backend of the api some calls are made to a Keycloak[^4] Admin REST API[^5]. Although the current RESTEasy implementation comes with JAX-RS 2.0 support[^6], I preferred using the `RestEasyClientBuilder` implementation in combination with the Resteasy Proxy Framework[^7], as so I've used it in JBoss EAP 6 and I still find it cool to use JAX-RS annotations to invoke on a remote HTTP resource. The way it works is that you write a Java interface and use JAX-RS annotations on methods and the interface. Check out the documentation[^7] and the code snippets posted below to see what I mean

[^4]: <http://www.keycloak.org>
[^5]: <http://www.keycloak.org/docs/rest-api/index.html>
[^6]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#RESTEasy_Client_Framework>
[^7]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#d4e2149>

#### REST client producer

From

```java
import org.jboss.resteasy.client.ProxyFactory;
import javax.enterprise.inject.Produces;

public class KeycloakApiClientProducer {

    private static final String KEYCLOAK_AUTH_ADMIN_REALMS_PODCASTPEDIA_BASE_URL = "http://localhost:8180/auth/admin/realms/podcastpedia";

    @Produces
    public KeycloakApiClient getKeycloakApiClient(){
        return ProxyFactory.create(KeycloakApiClient.class, KEYCLOAK_AUTH_ADMIN_REALMS_ONEPORTAL_BASE_URL);
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

    private static final String KEYCLOAK_AUTH_ADMIN_REALMS_PODCASTPEDIA_BASE_URL = "http://localhost:8180/auth/admin/realms/podcastpedia";

    @Produces
    public KeycloakApiClient getKeycloakApiClient(){
        ResteasyClientBuilder resteasyClientBuilder = new ResteasyClientBuilder().connectionPoolSize(20);
        ResteasyClient client = resteasyClientBuilder.build();
        ResteasyWebTarget target = client.target(KEYCLOAK_AUTH_ADMIN_REALMS_ONEPORTAL_BASE_URL);

        return target.proxy(KeycloakApiClient .class);

    }
}

```

> A producer method acts as a source of objects to be injected[^8] in CDI

[^8]: <http://stackoverflow.com/questions/16534728/please-explain-the-produces-annotation-in-cdi>

#### REST Client itself

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
 * Currently supported operations: reset password and update user details
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

```
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
 * Currently supported operations: reset password and update user details
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


#### REST Client Usage

```java
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

to

```java
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

### Filters and Interceptors

#### Logging interceptor

Used to log incoming request paths and responses in INFO log level for traceability. Uses now standard JAX RS 2.0 filters - `javax.ws.rs.container.ContainerResponseFilter`

*From*

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

*to*

```java
mport org.codehaus.jackson.map.ObjectMapper;
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

#### CORS interceptor
TODO
http://stackoverflow.com/questions/29388937/problems-resteasy-3-09-corsfilter


#### GZIP filter
TODO

## Jackson

Besides the Jettision JAXB adapter for JSON, Resteasy also support integration with the Jackson project. Many users (I am one of them) find the output from Jackson much much nicer than the Badger format or Mapped format provided by Jettison. Jackson lives at http://jackson.codehaus.org. It allows you to easily marshal Java objects to and from JSON. It has a Java Bean based model as well as JAXB like APIs. Resteasy integrates with the JavaBean model.

While Jackson does come with its own JAX-RS integration. Resteasy expanded it a little. To include it within your project, just add this maven dependency to your build. Resteasy supports both Jackson 1.9.x and Jackson 2.2.x. Read further on how to use each.[^].

[^]: <http://docs.jboss.org/resteasy/docs/3.0.16.Final/userguide/html_single/index.html#json>

Because the Keycloak version I am using, 1.7.0.Final, but also the newest one, 2.1.0.Final still use Jackson Version 1.9.x I had to convince the JBoss Server EAP 7 that this is the version I want.
Some of the Jackson classes like for example `ObjectMapper` are used throughout the code, for example to show a pretty format in Jackson of an `AppException`:

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

, I had to import the RestEasy Jackson Provider maven dependency as provided:

```xml
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-jackson-provider</artifactId>
        <version>3.0.16.Final</version>
        <scope>provided</scope>
    </dependency>
```
 

To get thinks working I had to also create a `jboss-deployment-structure.xml` file withing the *WEB-INF* directory and tell JBoss to exclude the `resteasy-jackson2-provider` and 
to import the `resteasy-jackson-provider` :

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

### Keycloak

The Keycloak core uses also Jackson, and to make sure not to make a conflicting version of it exclude the libraries from the dependency and it will use the one provided by the JBoss container:

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


## References
