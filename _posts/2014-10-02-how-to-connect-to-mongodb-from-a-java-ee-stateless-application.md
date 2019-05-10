---
id: 1903
title: How to connect to MongoDB from a Java EE stateless application
date: 2014-10-02T17:24:40+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1903
permalink: /ama/how-to-connect-to-mongodb-from-a-java-ee-stateless-application/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3076392538
fsb_social_facebook:
  - 1
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - Java EE
tags:
  - connection pool
  - mongoDB
  - singleton
---
<p style="text-align: justify;">
  In this post I will present how to connect to MongoDB from a stateless Java EE application, to take advantage of the built-in pool of connections to the database offered by the MongoDB Java Driver. This might be the case if you develop a REST API, that executes operations against a MongoDB.<!--more-->
</p>

<h2 style="text-align: justify;">
  Get the Java MongoDb Driver
</h2>

<p style="text-align: justify;">
  To connect from Java to MongoDB, you can use the <a title="http://docs.mongodb.org/ecosystem/drivers/java/" href="http://docs.mongodb.org/ecosystem/drivers/java/" target="_blank">Java MongoDB Driver</a>.  If you are building your application with Maven, you can add the dependency to the pom.xml file:
</p>

<pre class="lang:default decode:true" title="MongoDB java driver dependency">&lt;dependency&gt;
	&lt;groupId&gt;org.mongodb&lt;/groupId&gt;
	&lt;artifactId&gt;mongo-java-driver&lt;/artifactId&gt;
	&lt;version&gt;2.12.3&lt;/version&gt;
&lt;/dependency&gt;</pre>

<p style="text-align: justify;">
  The driver provides a MongoDB client (com.mongodb.MongoClient) with internal pooling. The MongoClient class is designed to be thread safe and shared among threads. For most applications, you should have one MongoClient instace for the entire JVM. Because of that you wouldn&#8217;t want create a new MongoClient instace with each request in your Java EE stateless application.
</p>

<h2 style="text-align: justify;">
  Implement a @Singleton EJB
</h2>

<p style="text-align: justify;">
  A simple solution is to use a @Singleton EJB to hold the MongoClient:
</p>

<pre class="lang:java decode:true" title="Singleton to hold the MongoClient">package org.codingpedia.demo.mongoconnection;

import java.net.UnknownHostException;

import javax.annotation.PostConstruct;
import javax.ejb.ConcurrencyManagement;
import javax.ejb.ConcurrencyManagementType;
import javax.ejb.Lock;
import javax.ejb.LockType;
import javax.ejb.Singleton;

import com.mongodb.MongoClient;

@Singleton
@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)
public class MongoClientProvider {
		
	private MongoClient mongoClient = null;
		
	@Lock(LockType.READ)
	public MongoClient getMongoClient(){	
		return mongoClient;
	}
	
	@PostConstruct
	public void init() {
		String mongoIpAddress = "x.x.x.x";
		Integer mongoPort = 11000;
		try {
			mongoClient = new MongoClient(mongoIpAddress, mongoPort);
		} catch (UnknownHostException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}		
	}
		
}
</pre>

<strong style="font-weight: bold;">Note:</strong>

<li style="text-align: justify;">
  <code>@Singleton</code> – probably the most important line of code in this class. This annotation specifies that there will be exactly one singleton of this type of bean in the application. This bean can be invoked concurrently by multiple threads. It comes also with a <a style="color: #bc360a;" title="http://docs.oracle.com/javaee/5/api/javax/annotation/PostConstruct.html" href="http://docs.oracle.com/javaee/5/api/javax/annotation/PostConstruct.html" target="_blank"><code>@PostConstruct</code></a> annotation. This annotation is used on a method that needs to be executed after dependency injection is done to perform any initialization – in our case is to initialize the MongoClient
</li>
<li style="text-align: justify;">
  the <code>@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)</code> declares a singleton session bean’s concurrency management type. By default it is set to <code>Container,</code> I use it here only to highlight its existence. The other option <a style="color: #bc360a;" title="http://docs.oracle.com/javaee/6/api/javax/ejb/ConcurrencyManagementType.html#BEAN" href="http://docs.oracle.com/javaee/6/api/javax/ejb/ConcurrencyManagementType.html#BEAN" target="_blank"><code>ConcurrencyManagementType.BEAN</code></a> specifies that the bean developer is responsible for managing concurrent access to the bean instance.
</li>
<li style="text-align: justify;">
  the <code>@Lock(LockType.READ)</code> specifies the concurrency lock type for singleton beans with container-managed concurrency. When set to <code>LockType.READ</code>, it enforces the method to permit full concurrent access to it (assuming no write locks are held). This permits several threads to access the same MongoClient instance and take advantage of the internal pool of connections to the database. This is VERY IMPORTANT, because the other more conservative option <code>@Lock(LockType.WRITE)</code>, is the DEFAULT and enforces exclusive access to the bean instance. This should make the method slower in a highly concurrent environment…
</li>

## Use the @Singleton EJB

Now that you have the MongoClient &#8220;persisted&#8221; in the application, you can inject the MongoClientProvider to access the MongoDB (to get the collection names for example):

<pre class="lang:java decode:true" title="Access MongoClient from other beans example">package org.codingpedia.demo.mongoconnection;

import java.util.Set;

import javax.ejb.EJB;
import javax.ejb.Stateless;

import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBObject;
import com.mongodb.MongoClient;
import com.mongodb.util.JSON;

@Stateless
public class TestMongoClientProvider {
	
	@EJB
	MongoClientProvider mongoClientProvider;
	
	public Set&lt;String&gt; getCollectionNames(){
		
		MongoClient mongoClient = mongoClientProvider.getMongoClient();
		
		DB db = mongoClient.getDB("myMongoDB");		
		Set&lt;String&gt; colls = db.getCollectionNames();
		
		for (String s : colls) {
		    System.out.println(s);
		}		
		
		return colls;
	}
	
}
</pre>

<p style="text-align: justify;">
  <strong>Note:</strong> The db object will be a connection to a MongoDB server for the specified database. With it, you can do further operations.  I encourage you to read the <a title="http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-java-driver/" href="http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-java-driver/" target="_blank">Getting Started with Java Driver</a> for more on that&#8230;
</p>

## Be aware

One aspect to bear mind:

<p style="padding-left: 30px;">
  <em>&#8220;For every request to the DB (find, insert, etc) the Java thread will obtain a connection from the pool, execute the operation, and release the connection. This means the connection (socket) used may be different each time.</em>
</p>

<p style="padding-left: 30px;">
  <em>Additionally in the case of a replica set with slaveOk option turned on, the read operations will be distributed evenly across all slaves. This means that within the same thread, a write followed by a read may be sent to different servers (master then slave). In turn the read operation may not see the data just written since replication is asynchronous. If you want to ensure complete consistency in a “session” (maybe an http request), you would want the driver to use the same socket, which you can achieve by using a “consistent request”. Call requestStart() before your operations and requestDone() to release the connection back to the pool:</em>
</p>

<pre class="lang:java decode:true " title="Ensuring complete consistency in a " session="" maybe="" an="" http="" request="" style="padding-left: 30px;"><em>DB db...;
db.requestStart();
try {
   db.requestEnsureConnection();

   code....
} finally {
   db.requestDone();
}</em></pre>

<p style="padding-left: 30px;">
  <em><tt class="docutils literal" style="color: #000000;"><span class="pre">DB</span></tt><span style="color: #494747;"> and </span><tt class="docutils literal" style="color: #000000;"><span class="pre">DBCollection</span></tt><span style="color: #494747;"> are completely thread safe. In fact, they are cached so you get the same instance no matter what.</span>&#8221; [3]</em>
</p>

## Resources

  1. <a title="http://docs.mongodb.org/ecosystem/drivers/java/" href="http://docs.mongodb.org/ecosystem/drivers/java/" target="_blank">Java MongoDB Driver</a>
  2. <a title="http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-java-driver/" href="http://docs.mongodb.org/ecosystem/tutorial/getting-started-with-java-driver/" target="_blank">Getting Started with Java Driver</a>
  3. <a title="http://docs.mongodb.org/ecosystem/drivers/java-concurrency/" href="http://docs.mongodb.org/ecosystem/drivers/java-concurrency/" target="_blank">Java Driver Concurrency</a>
  4. <a title="https://github.com/mongodb/mongo-java-driver/tree/master/src/examples/example" href="https://github.com/mongodb/mongo-java-driver/tree/master/src/examples/example" target="_blank">GitHub &#8211; mongodb / mongo-java-driver examples</a>