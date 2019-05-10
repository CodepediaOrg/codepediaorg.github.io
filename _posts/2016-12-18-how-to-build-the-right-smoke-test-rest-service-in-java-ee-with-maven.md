---
layout: post
title: How to build the right smoke test REST service in Java EE with Maven
description: "This post describes how to develop a REST service in your REST API backend that will provide the information about the current version of the implementation
plus the git sha-1 number. It can be very useful to test if the project is alive and find out which version/commit is deployed."
author: ama
permalink: /ama/how-to-build-the-right-smoke-test-rest-service-in-java-ee-with-maven
published: true
categories: [java ee, devops]
tags: [maven, java ee, rest, rest api, continuous delivery, continuous deployment, git, devops, it automation]
---

This post describes how to develop a Java EE REST service, usually part of a REST API backend, that will provide information about the current version of the implementation
plus the git sha-1 number[^1]. This can be very useful in continuous delivery[^2] scenarios or for the operations team, where they make a smoke test after deployment to "humanly see" if the API is "alive",
plus one gets which version/commit is currently deployed. The post implies you are also using Maven[^3] to build your project

[^1]: <https://gist.github.com/masak/2415865>
[^2]: <https://puppet.com/blog/continuous-delivery-vs-continuous-deployment-what-s-diff>
[^3]: <https://maven.apache.org/what-is-maven.html>

So when making a GET REST call against the service:

```java
GET http://localhost:8080/some-root-context/version
```

I would expect something like the following

```json
{
   "version": "1.2.0",
   "gitSha1": "8f0b7f7e1fc0d"
}
```

where the `version` attribute displays the version of the project from the pom.xml[^4] file and the `gitSha1` attribute contains the first, let's say 13 characters of the SHA-1 calculated from the git commit that
corresponds to this deployment.

[^4]: <https://maven.apache.org/guides/introduction/introduction-to-the-pom.html>

> Generally, eight to ten characters are more than enough to be unique within a project, but I choose thirteen just to be sure :)

You can then use the `gitSha1` value with the `git show` command to show the corresponding commit:

```bash
$ git show 8f0b7f7
commit 8f0b7f7e1fc0dcde3ccc0a9418f37a698dabb3b7
Merge: df736ab 3ebcbdb
Author: Adrian Matei <ama@codingpedia.org>
Date:   Sat Dec 3 09:30:41 2016 +0100

    Merge branch 'master' of https://github.com/Codingpedia/codingpedia.github.io

```

<!--more-->

Ok, let's see now how to build such a feature.

## Build Number Maven Plugin

So the first question is where do we get this info from? We know from a previous post of mine, [Quick way to check if the REST API is alive – GET details from Manifest file](http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/),
 that we can get the implementation details from the MANIFEST[^5] file, so it would be great if we had the git commit SHA-1 hash sum there too. It turns out we could have it, with
the help of the [Build Number Maven Plugin](http://www.mojohaus.org/buildnumber-maven-plugin/), which is designed to get a unique build number for each time you build your project.

[^5]: <https://docs.oracle.com/javase/tutorial/deployment/jar/manifestindex.html>

What we need to do is add this plugin to `<build><plugins>` section in your pom file and bind it to the create goal of the validate phase:

```xml
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>buildnumber-maven-plugin</artifactId>
  <version>${buildnumber-maven-plugin.version}</version>
  <executions>
      <execution>
          <phase>validate</phase>
          <goals>
              <goal>create</goal>
          </goals>
      </execution>
  </executions>
  <configuration>
      <!-- Take only the first 13 characters of the git hash code as buildNumber-->
      <shortRevisionLength>13</shortRevisionLength>
  </configuration>
</plugin>
```

The `shortRevisionLength` parameter specifies the length of the Git revision.

> For more info about maven phases and goals read the [Introduction to the Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

To get the Git revision information the plugin looks for a `<scm><plugins>` entry in the pom file, something like the following:

```xml
<scm>
    <url>https://github.com/Codingpedia/podcastpedia</url>
    <connection>scm:git:git://github.com:PodcastpediaOrg/podcastpedia.git</connection>
    <developerConnection>scm:git:git@github.com:PodcastpediaOrg/podcastpedia.git</developerConnection>
</scm>
```

Now if you execute the `mvn validate` command you should see something similar to the following:

```xml
[INFO] Storing buildNumber: 434553639c95d at timestamp: 1482094281756
[INFO] Storing buildScmBranch: develop
```


## Apache Maven WAR Plugin

We have now our 13 characters buildNumber, and next step is to put in the MANIFEST file. We can do that with the help of the [Apache Maven WAR Plugin](http://maven.apache.org/plugins/maven-war-plugin/). We
place the following configuration in our pom file:

```xml
<plugin>
    <artifactId>maven-war-plugin</artifactId>
    <version>${maven-war-plugin.version}</version>
    <configuration>
        <archive>
            <manifest>
                <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
            </manifest>
            <!-- ${buildNumber} in manifest-->
            <manifestEntries>
                <git-SHA-1>${buildNumber}</git-SHA-1>
                <Release-Version>${project.version}</Release-Version>
            </manifestEntries>
        </archive>
    </configuration>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>manifest</goal>
            </goals>
            <inherited>true</inherited>
        </execution>
    </executions>
</plugin>
```

where we specify our own `manifestEntries` (`git-SHA-1` and `Release-Version`) in addition to the `addDefaultImplementationEntries` and `addDefaultImplementationEntries`.

Notice that in order to generate the Manifest.mf file also in the file system under `webapp/META-INF`, you need to bind the manifest goal to an execution phase (e.g. package):

```xml
<executions>
    <execution>
        <phase>package</phase>
        <goals>
            <goal>manifest</goal>
        </goals>
        <inherited>true</inherited>
    </execution>
</executions>
```

When you execute now the `mvn clean package` command you should have `Release-Version` and `git-SHA-1` entries in the _MANIFEST.MF_ file, similar to the following:

```xml
Manifest-Version: 1.0
Implementation-Title: web-ui
Implementation-Version: 1.2.0
Built-By: ama
Specification-Vendor: Codingpedia Association
Specification-Title: web-ui
Implementation-Vendor-Id: org.podcastpedia
Implementation-Vendor: Codingpedia Association
Release-Version: 1.2.0
git-SHA-1: 434553639c95d
Created-By: Apache Maven 3.3.3
Build-Jdk: 1.8.0_65
Specification-Version: 1.2.0
Implementation-URL: https://github.com/Codingpedia/podcastpedia
```

## Read from the MANIFEST file

As shown in a previous post[^6], reading from the manifest file in a backend service and exposing it as a REST service in Java EE is fairly straight and easy.

[^6]: <http://www.codingpedia.org/ama/quick-way-to-check-if-the-rest-api-is-alive-get-details-from-manifest-file/>

We first build a POJO to hold the information:

```java
/**
 * Holds relevant information for the REST version service
 */
public class Version {

    String version;

    String gitSha1;

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

    public String getGitSha1() {
        return gitSha1;
    }

    public void setGitSha1(String gitSha1) {
        this.gitSha1 = gitSha1;
    }
}
```

[^7]: <https://spring.io/understanding/POJO>

then we read the information from the MANIFEST file in a backend service:

```java
import javax.ejb.Stateless;
import javax.servlet.ServletContext;
import java.io.IOException;
import java.io.InputStream;
import java.util.jar.Attributes;
import java.util.jar.Manifest;

/**
 *
 */
@Stateless
public class VersionService {

    public Version getVersion(ServletContext context) throws IOException {
        InputStream resourceAsStream = context.getResourceAsStream("/META-INF/MANIFEST.MF");
        Manifest mf = new Manifest();
        mf.read(resourceAsStream);
        Attributes atts = mf.getMainAttributes();

        Version response = new Version();
        response.setVersion(atts.getValue("Release-Version"));
        response.setGitSha1(atts.getValue("git-SHA-1"));

        return response;
    }

}

```

to finally expose it via a REST service:

```java
import javax.inject.Inject;
import javax.servlet.ServletContext;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.Context;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;
import java.io.IOException;
import java.io.InputStream;
import java.util.jar.Attributes;
import java.util.jar.Manifest;

/**
 * Created by ama on 16/12/16.
 */
@Path("version")
public class VersionResource {

    @Inject
    VersionService versionService;

    @Context
    ServletContext context;

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Response getVersion() throws IOException {
        return Response.status(Response.Status.OK)
                .entity(versionService.getVersion(context))
                .build();
    }
}
```

We this we get the result we imagined initially:

```java
GET http://localhost:8080/some-root-context/version
```

```json
{
   "version": "1.2.0",
   "gitSha1": "8f0b7f7e1fc0d"
}
```

If if you've found this useful, please share the joy and give us a star un [GitHub](https://github.com/Codingpedia/codingpedia.github.io)

## References
