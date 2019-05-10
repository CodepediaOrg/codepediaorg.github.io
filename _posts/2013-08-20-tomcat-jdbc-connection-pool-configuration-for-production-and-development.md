---
id: 485
title: Tomcat JDBC Connection Pool configuration for production and development
date: 2013-08-20T11:29:13+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=485
permalink: /ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 19
dsq_thread_id:
  - 1620039394
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
fsb_social_google:
  - 12
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:49:"https://github.com/podcastpedia/podcastpedia-web/";s:11:"ribbon-type";i:10;}'
categories:
  - development
  - optimization
  - tomcat
tags:
  - apache
  - dbcp
  - dbcp-common
  - development
  - jdbc configuration
  - pool
  - production
  - tomcat
---
<p style="text-align: justify;">
  As mentioned in the post <a title="Install Eclipse Kepler on Windows 7 " href=" http://www.codingpedia.org/ama/install-eclipse-kepler-64-bit-on-windows-7-64-bit" target="_blank">Install Eclipse Kepler 64 bit on Windows 7 64 bit</a>, <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> uses <a title="Apache Tomcat" href="http://tomcat.apache.org/" target="_blank">Apache Tomcat 7</a> as application server. This post presents how the Tomcat JDBC Connection Pool is configured in development and production for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>. The used database is MySql.<br />

  <!--more-->

  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#Production_environment">Production environment</a><ul>
          <li>
            <a href="#Sizing_the_conection_pool">Sizing the conection pool</a>
          </li>
          <li>
            <a href="#Validate_connections">Validate connections</a>
          </li>
          <li>
            <a href="#Connection_leaks">Connection leaks</a>
          </li>
          <li>
            <a href="#The_validationcleaner_thread">The validation/cleaner thread</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Development_environment">Development environment</a>
      </li>
      <li>
        <a href="#Watch_out_for">Watch out for</a>
      </li>
      <li>
        <a href="#Resources">Resources</a>
      </li>
    </ul>
  </div>
</p>

<p style="text-align: justify;">
  The Tomcat Connection pool is configured as a <strong>resource</strong> described in The <a title="Apache Tomcat 7 - JNDI Datasource HOW-TO" href="http://tomcat.apache.org/tomcat-7.0-doc/jndi-datasource-examples-howto.html" target="_blank">Tomcat JDBC documentation</a> with the only difference being that you have to specify the <code>factory</code> attribute and set the value to <code>org.apache.tomcat.jdbc.pool.DataSourceFactory</code>. For <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, it is configured in the <em>context.xml</em> file of the web application:<br /> <a href="{{site.url}}/wp-content/uploads/2013/08/context-file.png"><img class="alignnone size-medium wp-image-517" src="{{site.url}}/wp-content/uploads/2013/08/context-file-246x300.png" alt="context-file" width="246" height="300" srcset="{{site.url}}/wp-content/uploads/2013/08/context-file-246x300.png 246w, {{site.url}}/wp-content/uploads/2013/08/context-file.png 474w" sizes="(max-width: 246px) 100vw, 246px" /></a>
</p>

### <span id="Production_environment">Production environment</span>

<pre>
  <code class="xml">
    &lt;Resource
    	  name="jdbc/pcmDB"
    	  auth="Container"
    	  type="javax.sql.DataSource"
    	  factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
    	  initialSize="34"
    	  maxActive="377"
    	  maxIdle="233"
    	  minIdle="89"
    	  timeBetweenEvictionRunsMillis="34000"
    	  minEvictableIdleTimeMillis="55000"
    	  validationQuery="SELECT 1"
    	  validationInterval="34000"
    	  testOnBorrow="true"
    	  removeAbandoned="true"
    	  removeAbandonedTimeout="55"
    	  username="xxx"
    	  password="yyy"
    	  driverClassName="com.mysql.jdbc.Driver"
    	  url="jdbc:mysql://localhost:3306/pcmdb?allowMultiQueries=true"
     /&gt;
   </code>
 </pre>

#### <span id="Sizing_the_conection_pool">Sizing the conection pool</span>

  * `initialSize = 34` &#8211; the initial number of connections that are created when the pool is started
  * `maxActive = 377` &#8211; the maximum number of active connections that can be allocated from this pool at the same time. This attribute is used to limit the number of connections a pool can have open so that capacity planning can be done on the database side &#8211; in <em>my.cnf</em> MySql configuration file <code>max_connections = 610 (maxActive+maxIdle)</code>
  * `maxIdle = 233` &#8211; the maximum number of idle connections that should be kept in the pool at all times. Idle connections are checked periodically (if enabled) and connections that have been idle for longer than <code>minEvictableIdleTimeMillis</code> will be released
  * `minIdle= 89` &#8211; the minimum number of established connections that should be kept in the pool at all times. The connection pool can shrink below this number if validation queries fail.
  * `timeBetweenEvictionRunsMillis = 34000` &#8211; the number of milliseconds to sleep between runs of the idle connection validation/cleaner thread. This value should not be set under 1 second. It dictates how often we check for idle, abandoned connections, and how often we validate idle connections.
  * `minEvictableIdleTimeMillis = 55000` &#8211; the minimum amount of time an object may sit idle in the pool before it is eligible for eviction.

#### <span id="Validate_connections">Validate connections</span>

<p style="text-align: justify;">
  At first I avoided to configure connection validation, as I thought it would have an impact on performance. But several problems forced me to activate that. With the following configuration, connections are validated, but no more than every 34 seconds:
</p>

  * `testOnBorrow = true` &#8211; by setting this, the objects will be validated before being borrowed from the pool. If the object fails to validate, it will be dropped from the pool, and we will attempt to borrow another.<em> NOTE &#8211; for a true value to have any effect, the validationQuery parameter must be set to a non-null string.</em>
  * `validationInterval = 34000` &#8211; used to avoid excess validation, only run validation at most at this frequency &#8211; time in milliseconds. If a connection is due for validation, but has been validated previously within this interval, it will not be validated again. The larger the value, the better the performance, but you increase the chance of a stale connection being presented to your application.
  * `validationQuery= "SELECT 1"` &#8211; MySql SQL query used to validate connections from the pool before returning them to the caller

#### <span id="Connection_leaks">Connection leaks</span>

There are several configuration settings to help detect connection leaks:

  * `removeAbandoned = true` &#8211; Flag to remove abandoned connections if they exceed the <code>removeAbandonedTimeout</code>. A connection is considered abandoned and eligible for removal if it has been in use longer than the <code>removeAbandonedTimeout</code>. This way db connections can be recovered from applications that fail to close a connection.
  * `removeAbandonedTimeout = 54` &#8211; timeout in seconds before an abandoned(in use) connection can be removed. The value should be set to the longest running query your applications might have.
  * `validationQuery= "SELECT 1"` &#8211; MySql SQL query used to validate connections from the pool before returning them to the caller

#### <span id="The_validationcleaner_thread">The validation/cleaner thread</span>

<p style="text-align: justify;">
  <code>timeBetweenEvictionRunsMillis &gt; 0 AND removeAbandoned=true AND removeAbandonedTimeout &gt; 0</code> means the <strong>pool sweeper</strong> is enabled. The pool sweeper is the background thread that can test idle connections and resize the pool while the pool is active. The sweeper is also responsible for the detection of connection leaks. In this case the number of idle connections can grow beyond <code>maxIdle</code>, but can shrink down to <code>minIdle</code> if the connection has been idle for longer than <code>minEvictableIdleTimeMilis</code>.
</p>

### <span id="Development_environment">Development environment</span>

<pre>
  <code class="xml">
    &lt;Resource
    	 name="jdbc/pcmDB"
    	 auth="Container"
    	 type="javax.sql.DataSource"
    	 factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
    	 initialSize="5"
    	 maxActive="55"
    	 maxIdle="21"
    	 minIdle="13"
    	 timeBetweenEvictionRunsMillis="34000"
    	 minEvictableIdleTimeMillis="55000"
    	 validationQuery="SELECT 1"
    	 validationInterval="34"
    	 testOnBorrow="true"
    	 removeAbandoned="true"
    	 removeAbandonedTimeout="233"
    	 username="xxx"
    	 password="yyy"
    	 driverClassName="com.mysql.jdbc.Driver"
    	 url="jdbc:mysql://localhost:3307/pcmDB?allowMultiQueries=true"
    /&gt;
  </code>
</pre>

<p style="text-align: justify;">
   The development environment configuration is just a copy of the configuration used in production, with smaller values for attributes to size the pool, and bigger values for attributes to determine leaked connection, so that I can be in debug mode longer.
</p>

### <span id="Watch_out_for">Watch out for</span>

One of the exceptions I have got was:

<pre>
  <code class="bash">
    After mysql server was restarted or connection was lost I got this kind of errors:
    org.springframework.dao.DataAccessResourceFailureException:
    ### Error querying database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: No operations allowed after connection closed.Connection was implicitly closed by the driver.
    ### The error may exist in maps/PodcastMapper.xml
    ### The error may involve org.podcastpedia.dao.PodcastDao.getTopRatedPodcasts
    ### The error occurred while executing a query
    ### SQL: SELECT      p.podcast_id,      p.url,   (select sum(rating) / count(rating) from ratings    where podcast_id = p.podcast_id and episode_id=-1) as podcast_rating,   (select count(rating)  from ratings      where podcast_id = p.podcast_id and episode_id=-1) as podcast_number_ratings,      p.number_visitors,      p.description,      p.short_description,      p.podcast_image_url,      p.title,      p.last_episode_url,      p.title_in_url,      p.publication_date       FROM      podcasts p     WHERE      p.availability=200     ORDER BY podcast_rating DESC      limit 0, ?;
    ### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: No operations allowed after connection closed.Connection was implicitly closed by the driver.
    ; SQL []; No operations allowed after connection closed.Connection was implicitly closed by the driver.;
     nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: No operations allowed after connection closed.Connection was implicitly closed by the driver.
   </code>
 </pre>

<p style="text-align: justify;">
   This was solved by introducing the validation attributes mentioned above.
</p>

<p style="text-align: justify;">
  Well that&#8217;s it&#8230; Thanks again to the open source community for developing Tomcat, and a special thank you to <a title="Apache Tomcat" href="http://www.tomcatexpert.com/users/fhanik" target="_blank">Filip Hanik</a> for explaining the JDBC- pool configuration so clearly.
</p>

<p style="text-align: justify;">
  If you notice any room for improvement, please <a href="mailto:contact@codingepdia.org?Subject=Apache%20optimization" target="_top">contact us</a> or leave a message.
</p>

### <span id="Resources">Resources</span>

  * <a title="Apache Tomcat" href="http://tomcat.apache.org/" target="_blank">Apache Tomcat</a>
  * <a title="The Tomcat JDBC Connection Pool" href="http://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html" target="_blank">The Tomcat JDBC Connection Pool</a>
  * <a title="Apache Tomcat 7 - JNDI Datasource HOW-TO" href="http://tomcat.apache.org/tomcat-7.0-doc/jndi-datasource-examples-howto.html" target="_blank">Apache Tomcat 7 &#8211; JNDI Datasource HOW-TO</a>
  * <a title="Using the Tomcat 7 JDBC Connection Pool in Production" href="http://www.tomcatexpert.com/blog/2012/01/24/using-tomcat-7-jdbc-connection-pool-production" target="_blank">Using the Tomcat 7 JDBC Connection Pool in Production</a>
  * <a title="Configuring jdbc-pool for high-concurrency" href="http://www.tomcatexpert.com/blog/2010/04/01/configuring-jdbc-pool-high-concurrency" target="_blank">Configuring jdbc-pool for high-concurrency</a>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/amacoder.png" alt="Podcastpedia image" />

  <p id="about_author_header">
    <strong>Adrian Matei</strong>
  </p>

  <div id="author_details" style="text-align: justify;">
    Creator of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> and <a title="Codingpedia, sharing coding knowledge" href="http://www.codingpedia.org" target="_blank">Codingpedia.org</a>, computer science engineer, husband, father, curious and passionate about science, computers, software, education, economics, social equity, philosophy - but these are just outside labels and not that important, deep inside we are all just consciousness, right?
  </div>

  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
