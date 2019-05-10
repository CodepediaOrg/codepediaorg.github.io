---
layout: post
title: How to create a multi module project with a Maven archetype generated from existing project
description: ""
author: ama
permalink: /ama/how-to-create-a-multi-module-project-with-maven-template
published: true
categories: [maven]
tags: [maven, java, javaee]
---

Did you find yourself one or several times, copying your favourite java project, renaming it, removing files, keeping
some best practice classes and so on, all that to create new projects similar to it? This is time consuming and can 
be very annoying... Well, you might wanna consider instead creating a maven archetype out of the template project
 and use to create new projects. Not only this will save you time, but it will make you look with a more 
 critical view on your "best of breed" project. In this blog post I will show how to create a maven archetype based
  on an existing project and how to generate a new project from this template.  

<!--more-->

* TOC
{:toc}  

## Template project selection
Select a "template" project your new project is most similar to. For the purpose of this tutorial we'll use the [Podcastpedia](https://github.com/Codingpedia/podcastpedia)
 multi module project:
  
```bash
git clone https://github.com/Codingpedia/podcastpedia.git
```

## Generate maven archetype

Run the [Maven archetype plugin](http://maven.apache.org/archetype/maven-archetype-plugin/index.html) on the existing project

Navigate to the root of the freshly cloned project and execute the following maven command:

```bash

$ cd podcastpedia
$ mvn archetype:create-from-project
```

We should get a **BUILD SUCCESS** informing us about the creation of the _jar_ archetype and about its location:

```bash

[INFO] Copying 2 resources
[INFO]
[INFO] --- maven-archetype-plugin:3.0.1:jar (default-jar) @ podcastpedia-archetype ---
[INFO] Building archetype jar: /Users/ama/podcastpedia/target/generated-sources/archetype/target/podcastpedia-archetype-1.2.0
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.215 s
[INFO] Finished at: 2017-12-03T07:01:26+01:00
[INFO] Final Memory: 18M/307M
[INFO] ------------------------------------------------------------------------
[INFO] Archetype project created in /Users/ama/podcastpedia/target/generated-sources/archetype
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] parent ............................................. SUCCESS [  9.351 s]
[INFO] common ............................................. SKIPPED
[INFO] core ............................................... SKIPPED
[INFO] admin .............................................. SKIPPED
[INFO] web-ui ............................................. SKIPPED
[INFO] api ................................................ SKIPPED
[INFO] sql ................................................ SKIPPED
[INFO] sql-migration ...................................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 10.620 s
[INFO] Finished at: 2017-12-03T07:01:26+01:00
[INFO] Final Memory: 24M/338M
[INFO] ------------------------------------------------------------------------
```

The plugin generates the template files we are interested in, in the _target_ directory. With a simple `tree` command we 
can better see the generated structure:

```bash
$ tree -L 6 target/
target/
└── generated-sources
    └── archetype
        ├── pom.xml
        ├── src
        │   ├── main
        │   │   └── resources
        │   │       ├── META-INF
        │   │       └── archetype-resources
        │   └── test
        │       └── resources
        │           └── projects
        └── target
            ├── classes
            │   ├── META-INF
            │   │   └── maven
            │   └── archetype-resources
            │       ├── LICENSE.txt
            │       ├── README.md
            │       ├── admin
            │       ├── api
            │       ├── common
            │       ├── core
            │       ├── pom.xml
            │       ├── sql
            │       ├── sql-migration
            │       └── web-ui
            ├── podcastpedia-archetype-1.2.0.jar
            └── test-classes
                └── projects
                    └── basic
```
 
<span class="highlight-yellow"> We can (maven) install and run the generated jar file ( _podcastpedia-archetype-1.2.0.jar_) right away
, but we might want to do some adjustments first</span>

> First, you might want to move the **generated-sources** directory to a separate folder to avoid `mvn clean`-ing it by mistake...

## Generated structure
Let's inspect for a bit the important files in the _archetype_ folder:

1. _pom.xml_ is the pom file of the archetype and contains the things needed to build the archetype jar.
   Here you might change the version of the project that will be generated.
2. the _src/_ folder holds the archetype project source. The _src/main/resources_ contains again two important sub-folders
  * _archetype-resources_ - contains the project template and what will be generated when the archetype is run
  * _META-INF/maven_ - contains the _archetype-metadata.xml file_, which stores the metadata about the archetype
 
Let's have a better look at the key files here:

### archetype-metadata.xml

This metadata, found under _src/main/resources/META-INF/maven/archetype-metadata.xml_,  file stores

1. additional properties, with corresponding default values
2. project's generated files in filesets
3. inner modules of the archetype, which enable the creation of multi-module projects using a single archetype
 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<archetype-descriptor xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0
 http://maven.apache.org/xsd/archetype-descriptor-1.0.0.xsd"
    name="podcastpedia"
    xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <fileSets>
    <fileSet filtered="true" encoding="UTF-8">
      <directory></directory>
      <includes>
        <include>LICENSE.txt</include>
      </includes>
    </fileSet>
    <fileSet encoding="UTF-8">
      <directory></directory>
      <includes>
        <include>.editorconfig</include>
        <include>.gitignore</include>
        <include>README.md</include>
      </includes>
    </fileSet>
  </fileSets>
  <modules>
    <module id="admin" dir="admin" name="admin">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.jsp</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.dtd</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.zip</include>
            <include>**/*.jpg</include>
            <include>**/*.css</include>
            <include>**/*.svg</include>
            <include>**/*.gif</include>
            <include>**/*.ttf</include>
            <include>**/*.MF</include>
            <include>**/*.js</include>
            <include>**/*.eot</include>
            <include>**/*.map</include>
            <include>**/*.woff</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/test/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="common" dir="common" name="common">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.vm</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.png</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory></directory>
          <includes>
            <include>release.properties</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory></directory>
          <includes>
            <include>README.md</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="web-ui" dir="web-ui" name="web-ui">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.txt</include>
            <include>**/*.xml</include>
            <include>**/*.vm</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.txt</include>
            <include>**/*.xml</include>
            <include>**/*.jsp</include>
            <include>**/*.html</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.jks</include>
            <include>**/*.jmx</include>
            <include>**/*.dtd</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.jpg</include>
            <include>**/*.css</include>
            <include>**/*.swf</include>
            <include>**/*.svg</include>
            <include>**/*.gif</include>
            <include>**/*.scss</include>
            <include>**/*.ttf</include>
            <include>**/*.png</include>
            <include>**/*.js</include>
            <include>**/*.eot</include>
            <include>**/*.woff</include>
            <include>**/*.ico</include>
            <include>**/*.md</include>
            <include>**/*.MF</include>
            <include>**/*.json</include>
            <include>**/*.xcf</include>
            <include>**/*.tag</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/jetty</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory></directory>
          <includes>
            <include>release.properties</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory></directory>
          <includes>
            <include>README.md</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="api" dir="api" name="api">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.MF</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="core" dir="core" name="core">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/test/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="sql" dir="sql" name="sql">
      <fileSets>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.ini</include>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>_prepare_database_for_development</directory>
          <includes>
            <include>**/*.txt</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>recommended_podcasts</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>_prepare_database_for_development</directory>
          <includes>
            <include>**/*.zip</include>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>MySQL info</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>special_tables</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>episodes_table</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>update</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>config_data_table</directory>
          <includes>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory></directory>
          <includes>
            <include>.gitattributes</include>
            <include>README.md</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    <module id="sql-migration" dir="sql-migration" name="sql-migration">
      <fileSets>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.</include>
            <include>**/*.jar</include>
            <include>**/*.sql</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/assembly</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
  </modules>
</archetype-descriptor>
```
 
<p class="note_normal">
     If you are generating from an IntelliJ project, you'd want to remove the <em>.idea/.iml</em> related fileSets entries from the _archetype-metadata.xml_
     Actually you'd kind want to remove all files and directories listed in <em>.gitignore</em>
</p>
 

The **name** attribute from header (here _podcastpedia_):

```xml

<archetype-descriptor xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0
                                          http://maven.apache.org/xsd/archetype-descriptor-1.0.0.xsd"
    name="podcastpedia"
    xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-descriptor/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
``` 
will be displayed to the user when choosing an archetype to generate a project from.

The project's generated files are stored in filesets. 
The root fileset (directly under the `archetype-descriptor` element) includes only relevant files 
(_.gitignore_, _README.md_, _.editorconfig_) that are to be found in the root/parent project:

```xml

  <fileSets>
    <fileSet filtered="true" encoding="UTF-8">
      <directory></directory>
      <includes>
        <include>LICENSE.txt</include>
      </includes>
    </fileSet>
    <fileSet encoding="UTF-8">
      <directory></directory>
      <includes>
        <include>.editorconfig</include>
        <include>.gitignore</include>
        <include>README.md</include>
      </includes>
    </fileSet>
  </fileSets>
```

Filesets can be defined from the root directory of the module or project by having an empty `<directory></directory>` element

#### Defining multiple modules in the archetype metadata

Modules are specified via `<module>` entries under `<modules>`.
 Let's consider the **admin** module: 

```xml
<modules>
    <module id="admin" dir="admin" name="admin">
      <fileSets>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.jsp</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/resources</directory>
          <includes>
            <include>**/*.dtd</include>
          </includes>
        </fileSet>
        <fileSet encoding="UTF-8">
          <directory>src/main/webapp</directory>
          <includes>
            <include>**/*.zip</include>
            <include>**/*.jpg</include>
            <include>**/*.css</include>
            <include>**/*.svg</include>
            <include>**/*.gif</include>
            <include>**/*.ttf</include>
            <include>**/*.MF</include>
            <include>**/*.js</include>
            <include>**/*.eot</include>
            <include>**/*.map</include>
            <include>**/*.woff</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" packaged="true" encoding="UTF-8">
          <directory>src/test/java</directory>
          <includes>
            <include>**/*.java</include>
          </includes>
        </fileSet>
        <fileSet filtered="true" encoding="UTF-8">
          <directory>src/test/resources</directory>
          <includes>
            <include>**/*.xml</include>
            <include>**/*.properties</include>
          </includes>
        </fileSet>
      </fileSets>
    </module>
    ..........
</modules>
```

The module entry has the following attributes:
* id - this is the name of the module that will be generated
* dir - the template directory
* name - the **artifact id* that will be put in the pom file 

You can also ues `excludes` to exclude files/files types from generation. Let's have a look at the following fileset:

```xml
<fileSet filtered="true" packaged="true" encoding="UTF-8">
  <directory>src/main/java</directory>
  <includes>
    <include>**/*.java</include>
  </includes>
  <excludes>
    <exclude>StringUtils.java</exclude>
  </excludes>
</fileSet>
```

Note:

* all the _.java_ files found under _src/main/java_ folder are included in the generated project, except for _StringUtils.java_
* filesets can be "filtered" (`filtered="true"`), which means the selected files will be used as 
[Velocity templates](http://velocity.apache.org/engine/releases/velocity-1.5/user-guide.html)
They can be non-filtered, which means the selected files will be copied without modification
* the fileset is "packaged" (`packaged="true"`), which means the selected files will be generated/copied in a directory
structure that is prepended by the package property. That is mostly likely true for _.java_ files
* 'encoding' - specifies the encoding when filtering content 

> You might one remove some file types, or instead of using wild cards (*/**) specify just one representative class/file ...

Above we had a fixed name - **admin**, but you might considering using the root's artifact id in the name of your sub-modules. In that case
we would have had generated something like the following:

```xml
<modules>
    <module id="${rootArtifactId}-admin" dir="__rootArtifactId__-admin" name="${rootArtifactId}-admin">
        ..........
    </module>
    <module id="${rootArtifactId}-common" dir="__rootArtifactId__-common" name="${rootArtifactId}-common">
        ..........
    </module>
    <module id="${rootArtifactId}-web-ui" dir="__rootArtifactId__-web-ui" name="${rootArtifactId}--web-ui">
        ..........
    </module>                 
    ..........
</modules>
```

> After developing quite a few Java multi module projects, I now recommend using the `rootArtifactId` in the sub-modules names  

#### Define additional properties

The main properties that are used by the Velocity engine during a project's file generation are `groupId`, `artifactId`, `version` and `package`.
 It is also possible to define additional propertie that must be valued before the file generation. These additional properties can be provided with default values.
  The additional properties are defined in the _archetype-metadata.xml_ in the `requiredProperties` element:
  
```xml

<archetype-descriptor name="backend">
  <requiredProperties>
    <requiredProperty key="property-with-default">
      <defaultValue>default-value</defaultValue>
    </requiredProperty>
    <requiredProperty key="property-without-default"/>
  </requiredProperties>
...
</archetype-descriptor>
```

Here we define two additional properties: `property-with-default` and `property-without-default`.

## Generate project from archetype

### Install the archetype

Now that we tweaked a bit the generation file, the first thing we need to do, is to install the archetype

```bash

$ cd target/generated-sources/archetype/
$ mvn install


[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building podcastpedia-archetype 1.2.0
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ podcastpedia-archetype ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 590 resources
[INFO] 
[INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ podcastpedia-archetype ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 2 resources
[INFO] 
[INFO] --- maven-archetype-plugin:3.0.1:jar (default-jar) @ podcastpedia-archetype ---
[INFO] Building archetype jar: /Users/ama/podcastpedia/target/generated-sources/archetype/target/podcastpedia-archetype-1.2.0
[INFO] 
[INFO] --- maven-archetype-plugin:3.0.1:integration-test (default-integration-test) @ podcastpedia-archetype ---
[INFO] Processing Archetype IT project: basic
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: podcastpedia-archetype:1.2.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: archetype.it
[INFO] Parameter: artifactId, Value: basic
[INFO] Parameter: version, Value: 0.1-SNAPSHOT
[INFO] Parameter: package, Value: it.pkg
[INFO] Parameter: packageInPathFormat, Value: it/pkg
[INFO] Parameter: version, Value: 0.1-SNAPSHOT
[INFO] Parameter: package, Value: it.pkg
[INFO] Parameter: groupId, Value: archetype.it
adrians-mbp:archetype ama$ mvn install
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building podcastpedia-archetype 1.2.0
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ podcastpedia-archetype ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 590 resources
[INFO] 
[INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ podcastpedia-archetype ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 2 resources
[INFO] 
[INFO] --- maven-archetype-plugin:3.0.1:jar (default-jar) @ podcastpedia-archetype ---
[INFO] Building archetype jar: /Users/ama/podcastpedia/target/generated-sources/archetype/target/podcastpedia-archetype-1.2.0
[INFO] 
[INFO] --- maven-archetype-plugin:3.0.1:integration-test (default-integration-test) @ podcastpedia-archetype ---
[INFO] Processing Archetype IT project: basic
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: podcastpedia-archetype:1.2.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: archetype.it
[INFO] Parameter: artifactId, Value: basic
[INFO] Parameter: version, Value: 0.1-SNAPSHOT
[INFO] Parameter: package, Value: it.pkg
[INFO] Parameter: packageInPathFormat, Value: it/pkg
[INFO] Parameter: version, Value: 0.1-SNAPSHOT
[INFO] Parameter: package, Value: it.pkg
[INFO] Parameter: groupId, Value: archetype.it
[INFO] Parameter: artifactId, Value: basic
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/admin/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/common/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/web-ui/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/api/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/core/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/sql/pom.xml
[INFO] Parent element not overwritten in /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic/sql-migration/pom.xml
[INFO] Project created from Archetype in dir: /Users/ama/podcastpedia/target/generated-sources/archetype/target/test-classes/projects/basic/project/basic
[INFO] 
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ podcastpedia-archetype ---
[INFO] Installing /Users/ama/podcastpedia/target/generated-sources/archetype/target/podcastpedia-archetype-1.2.0.jar to /Users/ama/.m2/repository/org/podcastpedia/podcastpedia-archetype/1.2.0/podcastpedia-archetype-1.2.0.jar
[INFO] Installing /Users/ama/podcastpedia/target/generated-sources/archetype/pom.xml to /Users/ama/.m2/repository/org/podcastpedia/podcastpedia-archetype/1.2.0/podcastpedia-archetype-1.2.0.pom
[INFO] 
[INFO] --- maven-archetype-plugin:3.0.1:update-local-catalog (default-update-local-catalog) @ podcastpedia-archetype ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.781 s
[INFO] Finished at: 2017-12-03T12:39:50+01:00
[INFO] Final Memory: 13M/309M
[INFO] ------------------------------------------------------------------------
```

Now the archetype is present in the local maven repository.

> Use `mvn deploy` command if you want to upload the maven archetype to a Nexus server for example...

### Generate new project base on archetype 

Finally move to the place, where you want to create the new project and use the newly created archetype.

```bash
$ mkdir /tmp/archetype
$ cd /tmp/archetype
$ mvn archetype:generate -DarchetypeCatalog=local
```

A list of archetypes will be shown from the local archetype catalog (`-DarchetypeCatalog=local`).
 Since I only have one at the moment, there is not that difficult to choose from :).
  Answer the questions for setting the `groupId`, `artifactId` and other required properties:

```bash
[INFO] Generating project in Interactive mode
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
Choose archetype:
1: local -> org.podcastpedia:podcastpedia-archetype (Parent project for podcastpedia.org website, and the web admin application)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): : 1
Define value for property 'groupId': org.codingpedia
Define value for property 'artifactId': codingpedia-example
Define value for property 'version' 1.0-SNAPSHOT: : 1.0.0-SNAPSHOT
Define value for property 'package' org.codingpedia: : org.codingpedia.example
Confirm properties configuration:
groupId: org.codingpedia
artifactId: codingpedia-example
version: 1.0.0-SNAPSHOT
package: org.codingpedia.example
 Y: : y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: podcastpedia-archetype:1.2.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: org.codingpedia
[INFO] Parameter: artifactId, Value: codingpedia-example
[INFO] Parameter: version, Value: 1.0.0-SNAPSHOT
[INFO] Parameter: package, Value: org.codingpedia.example
[INFO] Parameter: packageInPathFormat, Value: org/codingpedia/example
[INFO] Parameter: package, Value: org.codingpedia.example
[INFO] Parameter: version, Value: 1.0.0-SNAPSHOT
[INFO] Parameter: groupId, Value: org.codingpedia
[INFO] Parameter: artifactId, Value: codingpedia-example
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/admin/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/common/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/web-ui/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/api/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/core/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/sql/pom.xml
[INFO] Parent element not overwritten in /private/tmp/archetype/codingpedia-example/sql-migration/pom.xml
[INFO] Project created from Archetype in dir: /private/tmp/archetype/codingpedia-example
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:11 min
[INFO] Finished at: 2017-12-03T12:47:44+01:00
[INFO] Final Memory: 16M/285M
[INFO] ------------------------------------------------------------------------
```

### The New Project Structure

The new project's structure should look in this case similar to the following:

```bash
$ tree -L 5 /tmp/archetype/
/tmp/archetype/
└── codingpedia-example
    ├── LICENSE.txt
    ├── README.md
    ├── admin
    │   ├── pom.xml
    │   └── src
    │       ├── main
    │       │   ├── java
    │       │   ├── resources
    │       │   └── webapp
    │       └── test
    │           ├── java
    │           └── resources
    ├── api
    │   ├── pom.xml
    │   └── src
    │       ├── main
    │       │   ├── java
    │       │   ├── resources
    │       │   └── webapp
    │       └── test
    │           └── resources
    ├── common
    │   ├── README.md
    │   ├── pom.xml
    │   ├── release.properties
    │   └── src
    │       ├── main
    │       │   ├── java
    │       │   └── resources
    │       └── test
    │           └── resources
    ├── core
    │   ├── pom.xml
    │   └── src
    │       ├── main
    │       │   ├── java
    │       │   └── resources
    │       └── test
    │           ├── java
    │           └── resources
    ├── pom.xml
    ├── sql
    │   ├── MySQL\ info
    │   ├── README.md
    │   ├── _prepare_database_for_development
    │   │   ├── codingpedia-example-2014-06-17-dev-db.zip
    │   │   └── help_guide.txt
    │   ├── config_data_table
    │   ├── episodes_table
    │   ├── pom.xml
    │   ├── recommended_podcasts
    │   ├── special_tables
    │   ├── src
    │   │   └── main
    │   │       └── resources
    │   └── update
    ├── sql-migration
    │   ├── pom.xml
    │   └── src
    │       ├── assembly
    │       │   └── distribution.xml
    │       └── main
    │           └── resources
    └── web-ui
        ├── README.md
        ├── pom.xml
        ├── release.properties
        └── src
            ├── jetty
            │   ├── jetty.xml
            │   ├── jetty7.xml
            │   ├── realm.properties
            │   └── webdefault.xml
            ├── main
            │   ├── java
            │   ├── resources
            │   └── webapp
            └── test
                └── resources

58 directories, 22 files
```

The sub-directories listed contain files/file-types specified via `includes`, and miss `excluded` file/files-types
 
## References

* [Maven Archetype](http://maven.apache.org/archetype/index.html)
  * [Archetype creation](http://maven.apache.org/archetype/maven-archetype-plugin/advanced-usage.html)
  * [Create an archetype from a multi-module project](http://maven.apache.org/archetype/maven-archetype-plugin/examples/create-multi-module-project.html)
  * [How is metadata about an archetype stored?](http://maven.apache.org/archetype/maven-archetype-plugin/specification/archetype-metadata.html)
  * [ArchetypeDescriptor](http://maven.apache.org/archetype/archetype-models/archetype-descriptor/archetype-descriptor.html)
