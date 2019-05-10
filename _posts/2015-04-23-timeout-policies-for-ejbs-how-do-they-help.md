---
id: 2353
title: 'Timeout policies for EJBs : how do they help…?'
date: 2015-04-23T13:55:59+00:00
author: Abhishek Gupta
layout: post
guid: http://www.codingpedia.org/?p=2353
permalink: /abhirockzz/timeout-policies-for-ejbs-how-do-they-help/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 0
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3705792080
categories:
  - Java EE
tags:
  - ejb
  - java
  - java ee
  - timeout
---
<a href="https://jcp.org/en/jsr/detail?id=318" target="_blank">EJB 3.1</a> introduced _timeout_ related annotations as a part of its API.

  * @AccessTimeout
  * @StatefulTimeout

Let’s quickly look at what they are and why are they important<!--more-->

## @AccessTimeout

Specifies the time period after which a queued request (waiting for another thread to complete) times out.

<pre class="lang:java decode:true" title="Singleton_With_Timeout.java">@Singleton
@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER) //this is actually by default
public class Singleton_With_Timeout{
    @AccessTimeout(5000,java.util.concurrent.TimeUnit.MILLISECONDS)
    @Lock(LockType.WRITE) //default configuration
    public void find(){
        //... business logic
    }
}</pre>

<p style="text-align: justify;">
  When your session bean instances are bombarded with concurrent requests, the EJB container ensures sanity by serializing these calls i.e. blocking other threads until the current thread finishes execution. You can refine this behavior further by using this annotation.
</p>

### _Which beans can leverage this annotation ?_

This is applicable for

  * _Stateful_ (@Stateful) beans and
  * _Singleton_ beans (@Singleton) configured with container managed concurrency option (ConcurrencyManagementType.CONTAINER)

### _Why is it important ?_

<p style="text-align: justify;">
  Since the EJB container serializes concurrent requests, having this annotation ensures that the potential (waiting) threads are not kept blocked for ever and helps define a <em>concurrency policy</em>.
</p>

### _Where can I put this annotation?_

  * On a class – globally applies to a all the methods
  * On a particular method _only_
  * On a particular method to override the settings of the class level annotation

### _How to use it ?_

You can use the value and unit elements of this annotation to define its behavior

Here are a few options

  * _@AccessTimeout(0)_ – this means that your method does not support concurrent access at all and the client would end up getting a <span class="skimlinks-unlinked">java.ejb.ConcurrentAccessException</span>
  * _@AccessTimeout(-1)_ – your method will block indefinitely (I don’t think that’s good idea !)
  * _@AccessTimeout(5000)_ – method will wait for 5000 ms (5 seconds) before the next thread in queue (if any) if given a chance

_Few things to note_

  * Default value for the _unit_ element is _<span class="skimlinks-unlinked">java.util.concurrent.TimeUnit.MILLISECONDS</span>_
  * a timeout value of less than -1 is invalid

## @StatefulTimeout

Defines the threshold limit for eviction of idle stateful session beans i.e. the ones which have not received client requests for a specific interval

<pre class="lang:java decode:true " title="SFSB_With_Timeout.java">@StatefulTimeout(15000,java.util.concurrent.TimeUnit.MILLISECONDS)
public class SFSB_With_Timeout{
    public void register(){
        //....business logic
    }
}</pre>

### _Why is it important ?_

<p style="text-align: justify;">
  Imagine you have a stateful session bean handling a user registration workflow. The user is inactive for certain time interval (probably doing other stuff). How long would you want your stateful session bean active in the memory? Configuring this annotation can help prevent inactive bean instances from hogging the main memory.
</p>

### _Where can I put this annotation?_

Same rules as the @AccessTimeout annotation !

### _How to use it ?_

You can use the value and unit elements of this annotation to define its behavior

Here are a few options

  * _@StatefulTimeout(0)_ – this means that your bean instance will be removed immediately after the completion of the method which holds this annotation
  * _@StatefulTimeout(-1)_ – your method will not be sensitive to time outs (man that’s stubborn !)
  * _@StatefulTimeout(15000)_ – method will wait for 15000 ms (15 seconds) for client requests before it becomes a candidate for eviction

_Few things to note_

  * Default value for the _unit_ element is _<span class="skimlinks-unlinked">java.util.concurrent.TimeUnit.MILLISECONDS</span>_
  * a timeout value of less than -1 is invalid

Cheers !

<p class="note_normal">
  Published at Codingpedia.org with the permission of Abhishek Gupta – source <a title="https://abhirockzz.wordpress.com/2015/04/05/timeout-policies-for-ejbs-how-do-they-help/" href="https://abhirockzz.wordpress.com/2015/04/05/timeout-policies-for-ejbs-how-do-they-help/" target="_blank">Timeout policies for EJBs : how do they help…?</a> from <a title="https://abhirockzz.wordpress.com" href="https://abhirockzz.wordpress.com" target="_blank">https://abhirockzz.wordpress.com</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="http://2.gravatar.com/avatar/eb0d2c5bf9426d7718efc6f9b087efb5?s=96&d=identicon&r=G" alt="Abhishek Gupta" /> 
  
  <p id="about_author_header">
    <strong>Abhishek Gupta</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Java Platform fanatic and a Consultant in the Identity Management domain
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://abhirockzz.wordpress.com/" target="_blank"> </a> <a class="icon-googleplus" href="http://plus.google.com/103167687788180890717/posts" target="_blank"> </a> <a class="icon-twitter" href="http://twitter.com/abhi_tweeter" target="_blank"> </a> <a class="icon-github" href="https://github.com/abhirockzz" target="_blank"> </a> <a class="icon-linkedin" href="http://in.linkedin.com/pub/abhishek-gupta/27/331/866" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
