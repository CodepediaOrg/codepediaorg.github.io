---
id: 2525
title: Basics of scaling Java EE applications
date: 2015-10-29T16:52:13+00:00
author: Abhishek Gupta
layout: post
guid: http://www.codingpedia.org/?p=2525
permalink: /abhirockzz/basics-of-scaling-java-ee-applications/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4271290803
fsb_social_facebook:
  - 2
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 8
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - architecture
  - java
  - Java EE
tags:
  - cache
  - cluster
  - distributed caching
  - high availability
  - java ee
  - scalability
---
<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>
    
    <ul class="toc_list">
      <li>
        <a href="#The_problem">The problem…</a>
      </li>
      <li>
        <a href="#Types_of_Scaling">Types of Scaling</a><ul>
          <li>
            <a href="#High_AvailabilityScalability">High Availability!=Scalability</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Load_Balance_your_scaled_out_cluster">Load Balance your scaled out cluster</a>
      </li>
      <li>
        <a href="#Is_my_application_stateless_or_stateful">Is my application stateless or stateful ?</a><ul>
          <li>
            <a href="#Hello_Sticky_Sessions">Hello Sticky Sessions!</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Enter_Replicated_Clustering">Enter Replicated Clustering</a>
      </li>
      <li>
        <a href="#External_store_for_stateful_components">External store for stateful components</a>
      </li>
      <li>
        <a href="#Final_frontier_Distributed_In-Memory_Cache">Final frontier: Distributed In-Memory Cache</a><ul>
          <li>
            <a href="#DistributedReplicated">Distributed!=Replicated</a>
          </li>
        </ul>
      </li>
      
      <li>
        <a href="#Further_reading_mostly_Weblogic_specific">Further reading (mostly Weblogic specific)</a>
      </li>
      <li>
        <a href="#Before_I_sign_off">Before I sign off…</a>
      </li>
    </ul>
  </div>
</p>

<p style="text-align: justify;">
  To be honest, ‘scalability’ is an exhaustive topic and generally not well understood. More often than not, its assumed to be same as High Availability. I have seen both novice programmers and ‘experienced’ architects suggest ‘<em>clustering</em>‘ as the solution for scalability and HA. There is actually nothing wrong with it, but the problem is that it is often done by googling rather than actually understanding the application itself <span class="wp-smiley wp-emoji wp-emoji-wink" title=";-)">😉</span>
</p>

<p style="text-align: justify;">
  I do not claim to be an ‘expert’, just by writing this post <span class="wp-smiley wp-emoji wp-emoji-wink" title=";-)">😉</span> It just (briefly) lays out some strategies for scaling Java EE applications in general.<!--more-->
</p>

<h2 style="text-align: justify;">
  <span id="The_problem">The problem…</span>
</h2>

<p style="text-align: justify;">
  Scalability is not a standardized component within the Java EE Platform specification. The associated techniques are mostly vendor (application server) specific and often involve using more than one products (apart from the app server itself). That’s why, architecting Java EE applications to be scalable can be a little tricky. There is no ‘cookbook’ to do the trick for you. One really needs to understand the application inside out.
</p>

<h2 style="text-align: justify;">
  <span id="Types_of_Scaling">Types of Scaling</span>
</h2>

<p style="text-align: justify;">
  I am sure it is not the first time you are reading this. Generally, scaling is classified into two broad categories – Scale Up, Scale Out
</p>

<p style="text-align: justify;">
  The first natural step towards scaling, is to scale up
</p>

<ul style="text-align: justify;">
  <li>
    <em>Scaling Up</em>: This involves adding more resources to your servers e.g. RAM, disk space, processors etc. It is useful in certain scenarios, but will turn out to be expensive after a particular point and you will discover that it’s better to resort to Scaling Out
  </li>
  <li>
    <em>Scaling Out</em>: In this process, more machines or additional server instances/nodes are added. This is also called clustering because all the servers are supposed to work together in unison (as a Group or Cluster) and should be transparent to the client.
  </li>
</ul>

<h3 style="text-align: justify;">
  <span id="High_AvailabilityScalability"><em>High Availability!=Scalability</em></span>
</h3>

<p style="text-align: justify;">
  Yes! Just because a system is Highly Available (by having multiple servers nodes to fail over to), does not mean it is scalable as well. HA just means that, if the current processing node crashes, the request would be passed on or failed over to a different node in the cluster so that it can continue from where it started – that’s pretty much it! Scalability is the ability to improve specific characteristics of the system (e.g. number of users, throughput, performance) by increasing the available resources (RAM, processor etc.) Even if the failed request is passed on to another node, you cannot guarantee that the application will behave correctly in that scenario (read on to understand why)
</p>

<p style="text-align: justify;">
  Lets look at some of the options and related discussions
</p>

<h2 style="text-align: justify;">
  <span id="Load_Balance_your_scaled_out_cluster"><em>Load Balance</em> your scaled out cluster</span>
</h2>

<p style="text-align: justify;">
  Let’s assume that you have scaled up to your maximum capacity and now you have scaled out your system by having multiple nodes forming a cluster. Now what you would do is put a Load Balancer in front of your clustered infrastructure so that you can distribute the load among your cluster members. <em>Load balancing</em> is not covered in detail since I do not have too much insight except for the basics <span class="wp-smiley wp-emoji wp-emoji-smile" title=":-)">🙂</span> But knowing this is good enough for this post
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/simple-cluster-with-lb-pptx.jpg"><img class="aligncenter size-full wp-image-1707" src="https://abhirockzz.files.wordpress.com/2015/10/simple-cluster-with-lb-pptx.jpg?w=1280&h=960" alt="simple-cluster-with-LB.pptx" width="640" height="480" /></a>
</p>

<h2 style="text-align: justify;">
  <span id="Is_my_application_stateless_or_stateful">Is my application <em>stateless </em>or <em>stateful </em>?</span>
</h2>

<p style="text-align: justify;">
  Ok so now you have scaled out – is that enough ? Scaling out is fine if your application is stateless i.e. your application logic does not depend on existing server state to process a request e.g. RESTful API back end over JAX-RS, Messaging based application exposing remote EJBs as the entry point which use JMS in the back ground etc.
</p>

<p style="text-align: justify;">
  What if you have an application which has components like HTTP session objects, Stateful EJBs, Session scoped beans (CDI, JSF) etc. ? These are specific to a client (to be more specific, the calling thread), store specific state and depend on that state being present in order to be able to execute the request e.g. a HTTP session object might store a user’s authentication state, shopping cart information etc.
</p>

<p style="text-align: justify;">
  In a scaled out or clustered application, subsequent requests might be served by any cluster in the node. <em>How will the other node handle the request without the state data which was created in the JVM of the instance to which the first request was passed to?</em>
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/stateful-or-stateless-1.jpg"><img class="aligncenter size-full wp-image-1710" src="https://abhirockzz.files.wordpress.com/2015/10/stateful-or-stateless-1.jpg?w=1280&h=960" alt="stateful-or-stateless-1" width="640" height="480" /></a>
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/stateful-or-stateless-21.jpg"><img class="aligncenter size-full wp-image-1714" src="https://abhirockzz.files.wordpress.com/2015/10/stateful-or-stateless-21.jpg?w=1280&h=960" alt="stateful-or-stateless-2" width="640" height="480" /></a>
</p>

<h3 style="text-align: justify;">
  <span id="Hello_Sticky_Sessions">Hello <em>Sticky Sessions</em>!</span>
</h3>

<p style="text-align: justify;">
  Sticky Session configuration can be done on the load balancer level to ensure that a request from a specific client/end user is always forwarded to the same instance/application server node i.e <em>server affinity</em> is maintained. Thus, we alleviate the problem of the required state not being present. But there is a catch here – <em>what if that node crashes ?</em> The state will be destroyed and the user will be forwarded to an instance where there is no existing state on which the server side request processing depends.
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/sticky-1.jpg"><img class="aligncenter size-full wp-image-1716" src="https://abhirockzz.files.wordpress.com/2015/10/sticky-1.jpg?w=1280&h=960" alt="sticky-1" width="640" height="480" /></a>
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/sticky-21.jpg"><img class="aligncenter wp-image-1719 size-full" src="https://abhirockzz.files.wordpress.com/2015/10/sticky-21.jpg?w=1280" alt="" width="640" height="480" /></a>
</p>

<h2 style="text-align: justify;">
  <span id="Enter_Replicated_Clustering">Enter <em>Replicated Clustering</em></span>
</h2>

<p style="text-align: justify;">
  In order to resolve the above problem, you can configure your application server clustering mechanism to support replication for your stateful components. By doing this you can ensure that your HTTP session data (and other stateful objects) are present on all the server instances. Thus the end user request can be forwarded to any server node now. Even if a server instance crashes or is unavailable, any other node in the cluster can handle the request. Now, your cluster is not ordinary cluster – it’s a <em>replicated cluster<br /> </em>
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/replicated-cluster1.jpg"><img class="aligncenter size-full wp-image-1723" src="https://abhirockzz.files.wordpress.com/2015/10/replicated-cluster1.jpg?w=1280&h=960" alt="replicated-cluster" width="640" height="480" /></a>
</p>

<p style="text-align: justify;">
  Cluster replication is specific to your Java EE container/app server and its best to consult its related documentation on how to go about this. Generally, most application servers support clustering of Java EE components like stateful and stateless EJBs, HTTP sessions, JMS queues etc.<br /> <em>This creates another problem though</em> – Now each node in the application server handles session data resulting in more JVM heap storage and hence more garbage collection. Also, there is an amount of processing power spent in replication as well
</p>

<h2 style="text-align: justify;">
  <span id="External_store_for_stateful_components"><em>External store</em> for stateful components</span>
</h2>

<p style="text-align: justify;">
  This can be avoided by storing session data and stateful objects in another tier. You can do so using RDBMS. Again, most application servers have inbuilt support for this.
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/rdms-as-cluster-store2.jpg"><img class="aligncenter size-full wp-image-1729" src="https://abhirockzz.files.wordpress.com/2015/10/rdms-as-cluster-store2.jpg?w=1280&h=960" alt="rdms-as-cluster-store" width="640" height="480" /></a>
</p>

<p style="text-align: justify;">
  If you notice, we have moved the storage from an in-memory tier to a persistent tier – at the end of the day, you might end up facing scalability issues because of the Database. I am not saying this will happen for sure, but depending upon your application, your DB might get overloaded and latency might creep in e.g. in case of a fail over scenario, think about recreating the entire user session state from the DB for use within another cluster instance – this can take time and affect end user experience during peak loads
</p>

<h2 style="text-align: justify;">
  <span id="Final_frontier_Distributed_In-Memory_Cache">Final frontier: <em>Distributed In-Memory Cache</em></span>
</h2>

<p style="text-align: justify;">
  It is the final frontier – at least in my opinion, since it moves us back to the in-memory approach. Yo can’t get better than that! Products like <em>Oracle Coherence, Hazelcast</em> or any other distributed caching/in-memory grid product can be used to offload the stateful state storage and replication/distribution – this is nothing but a <em>Caching Tier</em>. The good part is that most of these products support HTTP session storage as a default feature
</p>

<p style="text-align: justify;">
  <a href="https://abhirockzz.files.wordpress.com/2015/10/distributed-cache-cluster-store1.jpg"><img class="aligncenter size-full wp-image-1735" src="https://abhirockzz.files.wordpress.com/2015/10/distributed-cache-cluster-store1.jpg?w=1280&h=960" alt="distributed-cache-cluster-store" width="640" height="480" /></a>
</p>

<p style="text-align: justify;">
  This kind of architectural setup means that application server restarts do not effect existing user sessions – it’s always nice to patch your systems without downtime and end user outage (not as easy as it sounds but definitely and option!). In general, the idea is that the app tier and web session caching tier can work and scale independently and not interfere each other.
</p>

<h3 style="text-align: justify;">
  <span id="DistributedReplicated"><em>Distributed!=Replicated</em></span>
</h3>

<p style="text-align: justify;">
  There a a huge difference b/w these words and it’s vital to understand the difference in terms of your caching tier. Both have their pros and cons
</p>

<ul style="text-align: justify;">
  <li>
    <em>Distributed</em>: Members of the cache share data i.e. the data set is partitioned among cache cluster nodes (using a product specific algorithm)
  </li>
  <li>
    <em>Replicated</em>: All cache nodes have ALL the data i.e. each cache server contains a copy of the entire data set.
  </li>
</ul>

<h2 style="text-align: justify;">
  <span id="Further_reading_mostly_Weblogic_specific">Further reading (mostly Weblogic specific)</span>
</h2>

<ul style="text-align: justify;">
  <li>
    <a href="https://docs.oracle.com/cd/E24329_01/web.1211/e24425/config.htm#CLUST162" target="_blank">Clustering configuration</a>
  </li>
  <li>
    <a href="http://docs.oracle.com/middleware/1213/wls/WBAPP/sessions.htm#WBAPP309" target="_blank">RDBMS configuration for session persistence</a>
  </li>
  <li>
    Distributed Web Session replication – <a href="http://docs.oracle.com/middleware/1213/coherence/administer-http-sessions/cweb_wls.htm#COHCW255" target="_blank">Oracle Coherence</a>, <a href="http://docs.hazelcast.org/docs/3.5/manual/html/websessionreplication.html" target="_blank">Hazelcast</a>
  </li>
  <li>
    <a href="http://highscalability.com/" target="_blank">High Scalability</a> – great resource!
  </li>
</ul>

<h2 style="text-align: justify;">
  <span id="Before_I_sign_off">Before I sign off…</span>
</h2>

<ul style="text-align: justify;">
  <li>
    High/Extreme Scalability might not be a requirement for every Java EE application out there. But it will be definitely useful to factor that into your design if you are planning on building internet/public facing applications
  </li>
  <li>
    Scalable design is a must for applications which want to leverage the Cloud Platforms (mostly PaaS) like automated elasticity (economically viable!) and HA
  </li>
  <li>
    Its not too hard to figure out that stateful applications are often more challenging to scale. Complete ‘statelessness’ might not be possible, but one should strive towards that
  </li>
</ul>

<p style="text-align: justify;">
  Feel free to share tips and techniques which you have used to scale your Java EE apps.
</p>

<p style="text-align: justify;">
  Cheers!
</p>

<p class="note_normal" style="text-align: justify;">
  Published at Codingpedia.org with the permission of Abhishek Gupta – source <a title="https://abhirockzz.wordpress.com/2015/04/05/timeout-policies-for-ejbs-how-do-they-help/" href="https://abhirockzz.wordpress.com/2015/10/19/basics-of-scaling-java-ee-applications/" target="_blank">Basics of scaling Java EE applications</a> from <a title="https://abhirockzz.wordpress.com" href="https://abhirockzz.wordpress.com/" target="_blank">https://abhirockzz.wordpress.com</a>
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
