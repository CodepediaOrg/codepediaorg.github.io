---
layout: post
title: How to deploy an application on Wildfly/JBoss EAP 7 via the Wildfly Maven Plugin
description: "This post is an example on how to deploy a Java EE application to a WildFly or JBoss EAP 7 via the WildFly Maven Plugin"
author: ama
permalink: /ama/how-to-deploy-an-application-on-wildfly-or-jboss-eap-7-via-the-wildfly-maven-plugin
published: true
categories: [java ee]
tags: [jboss, wildfly, maven, https, wildfly maven plugin]
---

This post shows how to deploy a Java EE application on a WildFly or JBoss EAP 7 via the WildFly Maven Plugin[^1]. You can use this plugin to deploy, undeploy
or run your application. The post has two parts:

* deploy application on local server
* deploy application on remote server via https

[^1]: <https://docs.jboss.org/wildfly/plugins/maven/latest/index.html>

<!--more-->

## Deploy on local Wildfly

Place the plugin configuration in `pom.xml` file of the module you want to deploy to JBoss. It can be either a war or ear:

```xml
<project>
    ...
    <build>
        ...
        <plugins>
            ...
            <plugin>
                <groupId>org.wildfly.plugins</groupId>
                <artifactId>wildfly-maven-plugin</artifactId>
                <version>1.1.0.Final</version>
                <configuration>
                    <hostname>localhost</hostname>
                    <port>9990</port>
                    <username>jboss-local-admin</username>
                    <password>jboss-local-admin-password</password>
                    <jboss-home>local-jboss-home</jboss-home>
                    <name>${project.build.finalName}.${project.packaging}</name>
                </configuration>
                <executions>
                    <!-- Undeploy the application on clean -->
                    <execution>
                        <id>undeploy</id>
                        <phase>clean</phase>
                        <goals>
                            <goal>undeploy</goal>
                        </goals>
                        <configuration>
                            <ignoreMissingDeployment>true</ignoreMissingDeployment>
                        </configuration>
                    </execution>

                    <!-- Deploy the application on install -->
                    <execution>
                        <id>deploy</id>
                        <phase>install</phase>
                        <goals>
                            <goal>deploy</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            ...
        </plugins>
        ...
    </build>
...
</project>
```

Note:

* **undeploy** - is bound to the `clean` phase, and it won't complain if it's not present
* **deploy** - is bound to the `install` phase
* if you don't want the deployment to happen in the installation phase, consider moving it in an upper phase  in the the maven build cycle[^2] (e.g. `deploy`) or enclose it in a profile; for the latter see the second example below

[^2]: <https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html>

## Deploy on remote Wildfly

```xml
<profiles>
    <profile>
        <id>deployRemote</id>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.wildfly.plugins</groupId>
                    <artifactId>wildfly-maven-plugin</artifactId>
                    <version>1.1.0.Final</version>
                    <configuration>
                        <hostname>remote_host</hostname>
                        <protocol>https-remoting</protocol>
                        <port>9993</port>
                        <username>jboss-admin</username>
                        <password>jboss-admin-password</password>
                        <jboss-home>remote-jboss-home</jboss-home>
                        <name>${project.build.finalName}.${project.packaging}</name>
                    </configuration>
                    <executions>
                        <!-- Undeploy the application on clean -->
                        <execution>
                            <id>undeploy</id>
                            <phase>clean</phase>
                            <goals>
                                <goal>undeploy</goal>
                            </goals>
                            <configuration>
                                <ignoreMissingDeployment>true</ignoreMissingDeployment>
                            </configuration>
                        </execution>

                        <!-- Deploy the application on install -->
                        <execution>
                            <id>deploy</id>
                            <phase>deploy</phase>
                            <goals>
                                <goal>deploy</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

Note:

* **undeploy** - is bound to the `clean` phase, and it won't complain if it's not present
* **deploy** - is bound now to the `deploy` phase
* **https** is enabled by setting the protocol to `https-remoting` and the corresponding port `9993`
* the plugin's operations are now executed only with a specified profile `deployRemote`; so you'd have to run the build like the following:

```bash
shell> mvn clean deploy -PdeployRemote
```

## References
