---
id: 2046
title: MongoDB Incremental Migration Scripts
date: 2014-10-26T16:15:14+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=2046
permalink: /vladmihalcea/mongodb-incremental-migration-scripts/
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:84:"https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/mongodb-facts";s:11:"ribbon-type";i:10;}'
fsb_show_social:
  - 0
dsq_thread_id:
  - 3157235811
fsb_social_facebook:
  - 2
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - database
tags:
  - agile
  - flyway
  - flywayDB
  - incremental migration scripts
  - migration
  - Mongeez
  - mongoDB
---
## Introduction

<p style="text-align: justify;">
  An incremental software development process requires an incremental database migration strategy.
</p>

<p style="text-align: justify;">
  I remember working on an enterprise application where the <a href="http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch03.html#configuration-misc-properties"><em>hibernate.hbm2ddl.auto</em></a> was the default data migration tool.
</p>

<p style="text-align: justify;">
  Updating the production environment required intensive preparation and the migration scripts were only created on-the-spot. An unforeseen error could have led production data corruption.<!--more-->
</p>

## Incremental updates to the rescue

<p style="text-align: justify;">
  The incremental database update is a technical feature that needs to be addressed in the very first application development iterations.
</p>

<p style="text-align: justify;">
  We used to develop our own custom data migration implementations and spending time on writing/supporting frameworks is always working against your current project budget.
</p>

<p style="text-align: justify;">
  A project must be packed with both application code and all associated database schema/data updates scripts. Using incremental migration scripts allows us to automate the deployment process and to take advantage of <a href="http://en.wikipedia.org/wiki/Continuous_delivery">continuous delivery</a>.
</p>

<p style="text-align: justify;">
  Nowadays you don’t have to implement data migration tools, <a href="http://flywaydb.org/">Flyway</a> does a better job than all our previous custom frameworks. All database schema and data changes have to be recorded in incremental update scripts following a <a href="http://flywaydb.org/documentation/migration/">well-defined naming convention</a>.
</p>

<p style="text-align: justify;">
  A <a href="http://en.wikipedia.org/wiki/Relational_database_management_system">RDBMS</a> migration plan addresses both schema and data changes. It’s always good to separate schema and data changes. Integration tests might only use the schema migration scripts in conjunction with test-time related data .
</p>

<p style="text-align: justify;">
  Flyway supports all major relation database systems but for NoSQL (e.g. MongoDB) your need to look somewhere else.
</p>

## Mongeez

<p style="text-align: justify;">
  <a href="https://github.com/secondmarket/mongeez">Mongeez</a> is an open-source project aiming to automate MongoDB data migration. MongoDB is schema-less, so migration scripts are only targeting data updates only.
</p>

### Integrating mongeez

First you have to define a mongeez configuration file:

#### mongeez.xml

<pre class="lang:default decode:true ">&lt;changeFiles&gt;
    &lt;file path="v1_1__initial_data.js"/&gt;
    &lt;file path="v1_2__update_products.js"/&gt;
&lt;/changeFiles&gt;</pre>

Then you add the actual migrate scripts:

#### v1\_1\_\_initial\_data.js

<pre class="lang:js decode:true ">//mongeez formatted javascript
//changeset system:v1_1
db.product.insert({
    "_id": 1,
    "name" : "TV",
    "price" : 199.99,
    "currency" : 'USD',
    "quantity" : 5,
    "version" : 1
});
db.product.insert({
    "_id": 2,
    "name" : "Radio",
    "price" : 29.99,
    "currency" : 'USD',
    "quantity" : 3,
    "version" : 1
});</pre>

#### v1\_2\_\_update\_products.js

And you need to add the MongeezRunner too:

<pre class="lang:default decode:true ">&lt;bean id="mongeez" class="org.mongeez.MongeezRunner" depends-on="mongo"&gt;
    &lt;property name="mongo" ref="mongo"/&gt;
    &lt;property name="executeEnabled" value="true"/&gt;
    &lt;property name="dbName" value="${mongo.dbname}"/&gt;
    &lt;property name="file" value="classpath:mongodb/migration/mongeez.xml"/&gt;
&lt;/bean&gt;</pre>

### Running mongeez

When the application first starts, the incremental scripts will be analyzed and only run if necessary:

<pre class="lang:sh decode:true ">INFO  [main]: o.m.r.FilesetXMLReader - Num of changefiles 2
INFO  [main]: o.m.ChangeSetExecutor - ChangeSet v1_1 has been executed
INFO  [main]: o.m.ChangeSetExecutor - ChangeSet v1_2 has been executed</pre>

Mongeez uses a separate MongoDB collection to record previously run scripts:

<pre class="lang:js decode:true ">db.mongeez.find().pretty();
{
        "_id" : ObjectId("543b69eeaac7e436b2ce142d"),
        "type" : "configuration",
        "supportResourcePath" : true
}
{
        "_id" : ObjectId("543b69efaac7e436b2ce142e"),
        "type" : "changeSetExecution",
        "file" : "v1_1__initial_data.js",
        "changeId" : "v1_1",
        "author" : "system",
        "resourcePath" : "mongodb/migration/v1_1__initial_data.js",
        "date" : "2014-10-13T08:58:07+03:00"
}
{
        "_id" : ObjectId("543b69efaac7e436b2ce142f"),
        "type" : "changeSetExecution",
        "file" : "v1_2__update_products.js",
        "changeId" : "v1_2",
        "author" : "system",
        "resourcePath" : "mongodb/migration/v1_2__update_products.js",
        "date" : "2014-10-13T08:58:07+03:00"
}</pre>

## Conclusion

<p style="text-align: justify;">
  To automate the deployment process you need to create self-sufficient packs, containing both bytecode and all associated configuration (xml files, resource bundles and data migration scripts). Before starting writing your own custom framework, you should always investigate for available open-source alternatives.
</p>

Code available on [GitHub](https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/mongodb-facts).

**If you have enjoyed reading my article and you’re looking forward to getting instant email notifications of my latest posts, you just need to [follow my blog](http://vladmihalcea.com/2014/10/17/mongodb-incremental-migration-scripts/follow-me/).**

<p class="note_normal">
  Published at Codingpedia.org with permission of <a title="http://www.codingpedia.org/author/vladmihalcea" href="http://www.codingpedia.org/author/vladmihalcea" target="_blank">Vlad Mihalcea</a> &#8211; source <a title="http://vladmihalcea.com/2014/10/17/mongodb-incremental-migration-scripts/" href="http://vladmihalcea.com/2014/10/17/mongodb-incremental-migration-scripts/" target="_blank">MongoDB Incremental Migration Scripts</a> from <a title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://lh5.googleusercontent.com/-TE09duPdvbA/U1pkmDy2uSI/AAAAAAAACUM/0AVivijfro4/w896-h897-no/VladMihalcea.jpg" alt="Vlad Mihalcea" /> 
  
  <p id="about_author_header">
    <strong>Vlad Mihalcea</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Software architect passionate about software integration, high scalability and concurrency challenges
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://vladmihalcea.com/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/102351970868518518557/posts" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/vlad_mihalcea" target="_blank"> </a> <a class="icon-github" href="https://github.com/vladmihalcea" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/pub/vlad-mihalcea/20/a59/580" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
