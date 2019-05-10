---
id: 1930
title: The anatomy of connection pooling
date: 2014-10-07T20:37:52+00:00
author: Vlad Mihalcea
layout: post
guid: http://www.codingpedia.org/?p=1930
permalink: /vladmihalcea/the-anatomy-of-connection-pooling/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:79:"https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/db-facts";s:11:"ribbon-type";i:10;}'
dsq_thread_id:
  - 3093036818
fsb_social_facebook:
  - 2
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - architecture
  - database
tags:
  - connection pooling
  - performance tuning
  - rdbms
---
<h2 style="font-weight: inherit; color: #444444;">
  Introduction
</h2>

<p style="text-align: justify;">
  All projects I’ve been working on have used database <strong style="font-style: inherit;">connection pooling</strong> and that’s for very good reasons. Sometimes we might forget why we are employing one design pattern or a particular technology, so it’s worth stepping back and reason on it. Every technology or technological decision has both upsides and downsides, and if you can’t see any drawback you need to wonder what you are missing.<!--more-->
</p>

<h2 style="font-weight: inherit; color: #444444;">
  The database connection life-cycle
</h2>

<p style="color: #444444;">
  Every database read or write operation requires a connection. So let’s see how database connection flow looks like:
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/04/connectionlifecycle.gif"><img id="i-2138" class="size-full wp-image" src="http://vladmihalcea.files.wordpress.com/2014/04/connectionlifecycle.gif?w=785" alt="Image" /></a>
</p>

<p style="color: #444444;">
  The flow goes like this:
</p>

<ol style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    The application data layer ask the DataSource for a database connection
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    The DataSource will use the database Driver to open a database connection
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    A database connection is created and a TCP socket is opened
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    The application reads/writes to the database
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    The connection is no longer required so it is closed
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    The socket is closed
  </li>
</ol>

You can easily deduce that opening/closing connections is quite an expensive operation. PostgreSQL uses a <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://www.postgresql.org/docs/9.3/static/connect-estab.html">separate OS process</a> for every client connection, so a high rate of opening/closing connections is going to put a strain on your database management system.

<p style="color: #444444;">
  The most obvious reasons for reusing a database connection would be:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit; text-align: justify;">
    reducing the application and database management system OS I/O overhead for creating/destroying a TCP connection
  </li>
  <li style="font-weight: inherit; font-style: inherit; text-align: justify;">
    reducing JVM object garbage
  </li>
</ul>

<h2 style="font-weight: inherit; color: #444444;">
  Pooling vs No Pooling
</h2>

Let’s compare how a <strong style="font-style: inherit;">no pooling</strong> solution compares to <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://brettwooldridge.github.io/HikariCP/">HikariCP</a> which is probably the fastest <strong style="font-style: inherit;">connection pooling</strong> framework available.

The test will open and close 1000 connections.

<pre class="lang:java decode:true ">private static final Logger LOGGER = LoggerFactory.getLogger(DataSourceConnectionTest.class);
 
private static final int MAX_ITERATIONS = 1000;
 
private Slf4jReporter logReporter;
 
private Timer timer;
 
protected abstract DataSource getDataSource();
 
@Before
public void init() {
    MetricRegistry metricRegistry = new MetricRegistry();
    this.logReporter = Slf4jReporter
            .forRegistry(metricRegistry)
            .outputTo(LOGGER)
            .build();
    timer = metricRegistry.timer("connection");
}
 
@Test
public void testOpenCloseConnections() throws SQLException {
    for (int i = 0; i &lt; MAX_ITERATIONS; i++) {
        Timer.Context context = timer.time();
        getDataSource().getConnection().close();
        context.stop();
    }
    logReporter.report();
}</pre>

The chart displays the time spent during opening and closing connections so lower is better.

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/04/nopoolingvsconnectionpooling1.png"><img class="alignnone size-medium wp-image-2166" src="http://vladmihalcea.files.wordpress.com/2014/04/nopoolingvsconnectionpooling1.png?w=450&h=255" alt="NoPoolingVsConnectionPooling" width="300" height="170" /></a>
</p>

<p style="color: #444444; text-align: justify;">
  The <strong style="font-style: inherit;">connection pooling </strong>is <em style="font-weight: inherit;">600 time</em>s faster than the <strong style="font-style: inherit;">no pooling</strong> alternative. Our enterprise system consists of tens of applications and just one batch processor system could issue more than 2 million database connections per hour, so a 2 orders of magnitude optimization is worth considering.
</p>

<table style="color: #444444;">
  <tr style="font-weight: inherit; font-style: inherit;">
    <th style="font-style: inherit;">
      TYPE
    </th>
    
    <th style="font-style: inherit;">
      NO POOLING TIME (MILLISECONDS)
    </th>
    
    <th style="font-style: inherit;">
      CONNECTION POOLING TIME (MILLISECONDS)
    </th>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      min
    </td>
    
    <td style="font-style: inherit;">
      74.551414
    </td>
    
    <td style="font-style: inherit;">
      0.002633
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      max
    </td>
    
    <td style="font-style: inherit;">
      146.69324
    </td>
    
    <td style="font-style: inherit;">
      125.528047
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      mean
    </td>
    
    <td style="font-style: inherit;">
      78.216549
    </td>
    
    <td style="font-style: inherit;">
      0.128900
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      stddev
    </td>
    
    <td style="font-style: inherit;">
      5.9438335
    </td>
    
    <td style="font-style: inherit;">
      3.969438
    </td>
  </tr>
  
  <tr style="font-weight: inherit; font-style: inherit;">
    <td style="font-style: inherit;">
      median
    </td>
    
    <td style="font-style: inherit;">
      76.150440
    </td>
    
    <td style="font-style: inherit;">
      0.003218
    </td>
  </tr>
</table>

<h2 style="font-weight: inherit; color: #444444;">
  Why is pooling so much faster?
</h2>

<p style="color: #444444; text-align: justify;">
  To understand why the pooling solution performed so well, we need to analyse the pooling connection management flow:
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/04/poolingconnectionlifecycle.gif"><img class="alignnone size-medium wp-image-2171" src="http://vladmihalcea.files.wordpress.com/2014/04/poolingconnectionlifecycle.gif?w=450&h=281" alt="PoolingConnectionLifeCycle" width="300" height="186" /></a>
</p>

<p style="color: #444444; text-align: justify;">
  Whenever a connection is requested, the pooling data source will use the available connections pool to acquire a new connection. The pool will only create new connections when there are no available ones left and the pool hasn’t yet reached its maximum size. The pooling connection <em style="font-weight: inherit;">close()</em> method is going to return the connection to the pool, instead of actually closing it.
</p>

<p style="color: #444444;">
  <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.files.wordpress.com/2014/04/connectionacquirerequeststates.gif"><img class="alignnone size-medium wp-image-2174" src="http://vladmihalcea.files.wordpress.com/2014/04/connectionacquirerequeststates.gif?w=450&h=363" alt="ConnectionAcquireRequestStates" width="300" height="241" /></a>
</p>

<h2 style="font-weight: inherit; color: #444444;">
  Faster and safer
</h2>

<p style="color: #444444; text-align: justify;">
  The connection pool acts as a bounded buffer for the incoming connection requests. If there is a traffic spike the connection pool will level it instead of saturating all available database resources.
</p>

<p style="color: #444444; text-align: justify;">
  The waiting step and the timeout mechanism are safety hooks, preventing excessive database server load. If one application gets way too much database traffic, the connection pool is going to mitigate it therefore preventing it from taking down the database server (hence affecting the whole enterprise system).
</p>

<h2 style="font-weight: inherit; color: #444444;">
  With great power comes great responsibility
</h2>

<p style="color: #444444; text-align: justify;">
  All these benefits come at a price, materialized in the extra complexity of the pool configuration (especially in large enterprise systems). So this is no silver-bullet and you need to pay attention to many pool settings such as:
</p>

<ul style="color: #444444;">
  <li style="font-weight: inherit; font-style: inherit;">
    minimum size
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    maximum size
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    max idle time
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    acquire timeout
  </li>
  <li style="font-weight: inherit; font-style: inherit;">
    timeout retry attempts
  </li>
</ul>

<p style="color: #444444; text-align: justify;">
  My <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="http://vladmihalcea.com/2014/04/17/the-anatomy-of-connection-pooling/2014/04/25/flexy-pool-reactive-connection-pooling/">next article</a> will dig into enterprise <strong style="font-style: inherit;">connection pooling</strong> challenges and how <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://github.com/vladmihalcea/flexy-pool">FlexyPool</a> can assist you finding the right pool sizes.
</p>

<p style="color: #444444;">
  Code available on <a style="font-weight: inherit; font-style: inherit; color: #01a0db;" href="https://github.com/vladmihalcea/vladmihalcea.wordpress.com/tree/master/db-facts">GitHub</a>.
</p>

<p class="note_normal" style="color: #444444; text-align: justify;">
  Published at Codingpedia.org with the permission of Vlad Mihalcea &#8211; source <a title="http://vladmihalcea.com/2014/04/17/the-anatomy-of-connection-pooling/" href="http://vladmihalcea.com/2014/04/17/the-anatomy-of-connection-pooling/" target="_blank">The anatomy of connection pooling</a> from <a title="http://vladmihalcea.com/" href="http://vladmihalcea.com/" target="_blank">http://vladmihalcea.com/</a>
</p>

<p style="color: #444444;">
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
</p>

<p style="color: #444444;">
