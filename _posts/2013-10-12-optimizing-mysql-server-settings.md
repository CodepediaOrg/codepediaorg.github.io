---
id: 646
title: Optimizing MySQL server settings
date: 2013-10-12T09:29:24+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=646
permalink: /ama/optimizing-mysql-server-settings/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 11
dsq_thread_id:
  - 1848645690
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - optimization
tags:
  - my.cnf
  - mysql
  - optimization
  - performance
---
<p style="text-align: justify;">
  <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> uses the MySQL database, in version 5.5.32 (# mysql -V),  to store metadata (title, description, update frequency, url of the image, urls of episodes etc.) of podcasts from the directory. The database contains both <a title="MyISAM" href="http://en.wikipedia.org/wiki/MyISAM" target="_blank">MyISAM</a> (for performance and full-text indexing capabilities) and <a title="Wikipedia - InnoDB" href="http://en.wikipedia.org/wiki/InnoDB" target="_blank">InnoDB</a> tables. An upgrade to version 6 and only to InnoDB tables is considered, once it&#8217;s mature enough and GoDaddy will support it. This post presents how MySQL server is configured, and to be more specific how the <code>my.cnf</code> file looks like.<br /> <!--more-->
</p>

## The MySQL Configuration File: my.cnf

<p style="text-align: justify;">
  You can tweak MySQL server settings by undertaking a proper configuration of MySQL&#8217;s configuration file. This file, entitled <code>my.cnf</code>, stores default startup options for both the server and for clients. On Unix systems the configuration file is located at <code>/etc/my.cnf</code> (red hat), but could also be found at <code>/etc/mysql/my.cnf</code> (debian systems). There are several ways you can build this file
</p>

<li style="text-align: justify;">
  before MySQL 5.6.8, MySQL distributions include several sample option files that can be used as a basis for tuning the MySQL server. Look for files named <code class="filename">my-small.cnf</code>, <code class="filename">my-medium.cnf</code>, <code class="filename">my-large.cnf</code>, and <code class="filename">my-huge.cnf</code>, which are sample files for small, medium, large, and very large systems. On Windows, the extension is <code class="filename">.ini</code> rather than <code class="filename">.cnf</code>.
</li>
  * use a Configuration Wizard/Generator for MySQL &#8211; the <a title="Percona Configuration Wizard for MySQL" href="https://tools.percona.com/wizard" target="_blank">one from Percona</a> is pretty good
  * look through the manuals and build the file by yourself from scratch

<p style="text-align: justify;">
  After doing some research I decided to build the file from scratch, of course with some inspiration from the other two methods. This way I could also learn a few things about the database server. There are MANY MANY options that can be configured on the MySQL database, but I came to the conclusion that it&#8217;s better to configure the basic ones <strong>correctly</strong>(most properties are fine at their defaults) and focus more on proper indexing, query design and query optimization. So here it&#8217;s how the my.cnf file looks like
</p>

<pre>[mysqld]
ft_min_word_len = 3
#GENERAL
user = mysql
port = xxxx
default_storage_engine = InnoDB
socket = /var/lib/mysql/mysql.sock
pid_file = /var/lib/mysql/mysql.pid

# DATA STORAGE #
datadir = /var/lib/mysql
#INNODB
innodb_buffer_pool_size = 378M
innodb_log_file_size = 64M
innodb_file_per_table=1
innodb_flush_method = O_DIRECT
#MyISAM
key_buffer_size = 64M
#Logging
log_error = /var/lib/mysql/mysql-error.log
slow_query_log = 1
slow_query_log_file = /var/lib/mysql/mysql-slow.log
#OTHER
tmp_table_size = 32M
max_heap_table_size = 32M
query_cache_type = 0
query_cache_size = 00
max_connections = 500
thread_cache_size = 50
table_open_cache = 800
open_files_limit = 65535
[client]
socket = /var/lib/mysql/mysql.sock
port = xxxx</pre>

The explanation for a few parameters:

  * `ft_min_word_len = 3` &#8211; the minimum length of words to be indexed in the full-text search index. If you modify this value you need to rebuild the FULLTEXT indexes.
  * `user = mysql` &#8211; <a title="mysqld" href="http://dev.mysql.com/doc/refman/5.5/en/mysqld.html" target="_blank">mysqld</a> will run as the `mysql` user account on the operating system
  * `port = xxxx` &#8211; port on which MySQL listens to (default is 3306)
  * `default_storage_engine = InnoDB` &#8211; default storage engine starting with MySQL 5.5. See <a title="MySQL 5.5 InnoDB" href="http://dev.mysql.com/doc/refman/5.5/en/innodb-default-se.html" target="_blank">here </a>the benefits of using InnoDB
  * `socket = /var/lib/mysql/mysql.sock` &#8211; The mysql.sock is the socket that mysqld creates for programs to connect to.
  * `pid_file = /var/lib/mysql/mysql.pid` &#8211; A process id (pid) file is where the process id (pid) assigned by the server is recorded, when a particular instance of MySQL starts
  * `datadir = /var/lib/mysql` &#8211; location of data, popular location on many Unix variants
<li style="text-align: justify;">
  <code>innodb_buffer_pool_size = 378M</code> &#8211; the most important option for Innodb Performance. InnoDB maintains a storage area called the <a class="link" title="buffer pool" href="http://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_buffer_pool">buffer pool</a> for caching data and indexes in memory. The default value (128M) is a little bit small for my needs. Ideally, you set the size of the buffer pool to as large a value as practical, leaving enough memory for other processes on the server to run without excessive paging. The larger the buffer pool, the more <code class="literal">InnoDB</code> acts like an in-memory database, reading data from disk once and then accessing the data from memory during subsequent reads. The buffer pool even caches data changed by insert and update operations, so that disk writes can be grouped together for better performance.  See also <a title="Choosing innodb_buffer_pool_size" href="http://www.mysqlperformanceblog.com/2007/11/03/choosing-innodb_buffer_pool_size/" target="_blank">choosing innodb buffer pool size</a>
</li>
<li style="text-align: justify;">
  <code>innodb_log_file_size = 64M</code>  &#8211; The size in bytes of each <a class="link" title="log file" href="http://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_log_file">log file</a> in a <a class="link" title="log group" href="http://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_log_group">log group</a>. The combined size of log files (<code class="literal">innodb_log_file_size</code> * <a class="link" href="http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_log_files_in_group"><code class="literal">innodb_log_files_in_group</code></a>) cannot exceed a maximum value that is slightly less than 512GB. A pair of 255 GB log files, for example, would allow you to approach the limit but not exceed it. The default value is 48MB. Sensible values range from 1MB to 1/<em class="replaceable"><code>N</code></em>-th of the size of the <a class="link" title="buffer pool" href="http://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_buffer_pool">buffer pool</a>, where <em class="replaceable"><code>N</code></em> is the number of log files in the group. The larger the value, the less checkpoint flush activity is needed in the buffer pool, saving disk I/O. Larger log files also make <a class="link" title="crash recovery" href="http://dev.mysql.com/doc/refman/5.6/en/glossary.html#glos_crash_recovery">crash recovery</a> slower, although improvements to recovery performance in MySQL 5.5 and higher make the log file size less of a consideration.
</li>
<li style="text-align: justify;">
  <code>innodb_file_per_table=1</code> &#8211; If <a class="link" href="http://dev.mysql.com/doc/refman/5.0/en/innodb-parameters.html#sysvar_innodb_file_per_table"><code class="literal">innodb_file_per_table</code></a> is disabled (the default), <code class="literal">InnoDB</code> creates tables in the shared tablespace. If <a class="link" href="http://dev.mysql.com/doc/refman/5.0/en/innodb-parameters.html#sysvar_innodb_file_per_table"><code class="literal">innodb_file_per_table</code></a> is enabled (=1), <code class="literal">InnoDB</code> creates each new table using its own <code class="filename">.ibd</code> file for storing data and indexes, rather than in the shared tablespace.Tablespace also never reduces in size, the same as if you had this disabled, but you have now the possibility to rebuild the table. Follow <a title="Permanent Link to Reasons to use innodb_file_per_table" href="http://code.openark.org/blog/mysql/reasons-to-use-innodb_file_per_table" rel="bookmark">Reasons to use innodb_file_per_table</a> for more information.
</li>
  * `innodb_flush_method = O_DIRECT` &#8211; when set to O\_DIRECT the operating system&#8217;s caching wil be bypassed. This assures that there is no overhead of double buffering, since innodb\_buffer\_pool\_size was also decently set
  * #MyISAM
<li style="text-align: justify;">
  <code>key_buffer_size = 128M</code> &#8211; Index blocks for MyISAM tables are buffered and are shared by all threads. key_buffer_size is the size of the buffer used for index blocks. The key buffer is also known as the key cache. The <em>key_buffer_size</em> is probably the most useful single variable to tweak on the MyISAM engine. Be aware that MyISAM itself <strong>caches only indexes, not data</strong>. So if possible the value of this setting should cover the size of all your indexes.
</li>
  * #LOGGING
  * `log_error = /var/lib/mysql/mysql-error.log` &#8211; Log errors and startup messages to this file. See also [“The Error Log”](http://dev.mysql.com/doc/refman/5.0/en/error-log.html "5.2.1. The Error Log").
  * `slow_query_log = 1` &#8211; whether the slow query log is enabled. The value can be 0 (or `OFF`) to disable the log or 1 (or `ON`) to enable the log.
  * `slow_query_log_file = /var/lib/mysql/mysql-slow.log` &#8211; the name/destination of the slow query log file. The default value is <code class="filename">&lt;em class="replaceable">&lt;code>host_name</code></em>-slow.log</code>, but the initial value can be changed with the <code class="literal">--slow_query_log_file</code> option.
<li style="text-align: justify;">
  <code>tmp_table_size = 32M</code> &#8211; the maximum size of internal in-memory temporary tables. (The actual limit is determined as the minimum of <a href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_tmp_table_size"><code>tmp_table_size</code></a> and <a href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_max_heap_table_size"><code>max_heap_table_size</code></a>.) If an in-memory temporary table exceeds the limit, MySQL automatically converts it to an on-disk <code>MyISAM</code> table. Increase the value of <a href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_tmp_table_size"><code>tmp_table_size</code></a> (and <a href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_max_heap_table_size"><code>max_heap_table_size</code></a> if necessary) if you do many advanced <code>GROUP BY</code> queries and you have lots of memory. This variable does not apply to user-created <code>MEMORY</code> tables.You can compare the number of internal on-disk temporary tables created to the total number of internal temporary tables created by comparing the values of the <a href="http://dev.mysql.com/doc/refman/5.6/en/server-status-variables.html#statvar_Created_tmp_disk_tables"><code>Created_tmp_disk_tables</code></a> and <a href="http://dev.mysql.com/doc/refman/5.6/en/server-status-variables.html#statvar_Created_tmp_tables"><code>Created_tmp_tables</code></a> variables.
</li>
<li style="text-align: justify;">
  <code>max_heap_table_size = 32M</code> &#8211; this variable sets the maximum size to which user-created <code class="literal">MEMORY</code> tables are permitted to grow. The value of the variable is used to calculate <code class="literal">MEMORY</code> table <code class="literal">MAX_ROWS</code> values. Setting this variable has no effect on any existing <code class="literal">MEMORY</code> table, unless the table is re-created with a statement such as <a class="link" title="13.1.17. CREATE TABLE Syntax" href="http://dev.mysql.com/doc/refman/5.6/en/create-table.html"><code class="literal">CREATE TABLE</code></a> or altered with <a class="link" title="13.1.7. ALTER TABLE Syntax" href="http://dev.mysql.com/doc/refman/5.6/en/alter-table.html"><code class="literal">ALTER TABLE</code></a> or <a class="link" title="13.1.33. TRUNCATE TABLE Syntax" href="http://dev.mysql.com/doc/refman/5.6/en/truncate-table.html"><code class="literal">TRUNCATE TABLE</code></a>. A server restart also sets the maximum size of existing <code class="literal">MEMORY</code> tables to the global <a class="link" href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_max_heap_table_size"><code class="literal">max_heap_table_size</code></a> value. This variable is also used in conjunction with <a class="link" href="http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_tmp_table_size"><code class="literal">tmp_table_size</code></a> to limit the size of internal in-memory tables.
</li>
  * `query_cache_type = 0` &#8211; set the query cache type. A value of 0 or OFF means do not cache results in or retrieve results from the query cache. Note that this does not deallocate the query cache buffer. To do that, you should set [`query_cache_size`](http://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_query_cache_size) to 0.
<li style="text-align: justify;">
  <code>query_cache_size = 00</code> &#8211; the amount of memory allocated for caching query results. By default, the query cache is disabled. I prefer doing caching at the application level with EhCache &#8211; this will be explained in a future post
</li>
  * `max_connections = 377` &#8211; the maximum permitted number of simultaneous client connections. If you get a <a title="Too many connections" href="http://dev.mysql.com/doc/refman/5.6/en/too-many-connections.html" target="_blank">Too many connections</a> error when you try to connect to the [**mysqld**](http://dev.mysql.com/doc/refman/5.6/en/mysqld.html "4.3.1. mysqld — The MySQL Server") server, this means that all available connections are in use by other clients.
<li style="text-align: justify;">
  <code>thread_cache_size = 54</code> &#8211; how many threads the server should cache for reuse. When a client disconnects, the client&#8217;s threads are put in the cache if there are fewer than <a href="http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_thread_cache_size"><code>thread_cache_size</code></a> threads there. Requests for threads are satisfied by reusing threads taken from the cache if possible, and only when the cache is empty is a new thread created. This variable can be increased to improve performance if you have a lot of new connections. Normally, this does not provide a notable performance improvement if you have a good thread implementation. However, if your server sees hundreds of connections per second you should normally set <a href="http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_thread_cache_size"><code>thread_cache_size</code></a> high enough so that most new connections use cached threads. By examining the difference between the <a href="http://dev.mysql.com/doc/refman/5.5/en/server-status-variables.html#statvar_Connections"><code>Connections</code></a> (<code>SHOW GLOBAL STATUS LIKE 'Connections';</code>) and <a href="http://dev.mysql.com/doc/refman/5.5/en/server-status-variables.html#statvar_Threads_created"><code>Threads_created</code></a> (<code>SHOW GLOBAL STATUS LIKE 'Threads_created';</code>) status variables, you can see how efficient the thread cache is.
</li>
<li style="text-align: justify;">
  <code>table_open_cache = 600</code> &#8211; The number of open tables for all threads. Increasing this value increases the number of file descriptors that <a class="link" title="4.3.1. mysqld — The MySQL Server" href="http://dev.mysql.com/doc/refman/5.5/en/mysqld.html"><span class="command"><strong>mysqld</strong></span></a> requires. You can check whether you need to increase the table cache by checking the <a class="link" href="http://dev.mysql.com/doc/refman/5.5/en/server-status-variables.html#statvar_Opened_tables"><code class="literal">Opened_tables</code></a> (<code>SHOW GLOBAL STATUS LIKE 'open_tables';</code>) status variable.
</li>

To verify if the variable has been set correctly run &#8221; `show variable like 'VARIABLE_NAME'` &#8221; (e.g. `show variables like 'max_connections'`) in the MySQL console.

<p style="text-align: justify;">
  The calculation was based on the assumptions that MySQL has 756MB of RAM available, a database size around 250MB and maximum 300 users trying to access the system (the underlying machine is Cent OS 6.4 virtual private server with 2GB of RAM and 8 cores processor). The same assumptions were made trying to configure the Tomcat JDBC Connection Pool &#8211; see my post <a title="Using the Tomcat 7 JDBC Connection Pool in Production" href="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" target="_blank">Tomcat JDBC Connection Pool configuration for production and development</a> on that. Of course if resource demand grows (fingers crossed 🙂 ), I would have to adjust.
</p>

<div style="background-color: #f8e0e6; color: #580404; padding: 10px; border-style: solid; border-width: thin; margin-bottom: 15px; text-align: justify;">
  <strong>Post disclaimer</strong><br /> I am by no means a DB/MySql Expert, I come from a development background, so please take the configuration as is. You should treat your application/system optimization, as a holistic approach including OS, DB, web server, application server, web caching, 2nd level caching, etc. I posted this here to easily remember what everything means in the configuration file, why I set these values, to learn something and hopefully to get <a href="mailto:contact@codingpedia.org?Subject=Improve%20my.cnf%20configuration" target="_top">your feedback</a> if you see any room improvement.
</div>

If you really want an expert&#8217;s opinion, I highly recommend <a href="http://www.amazon.de/gp/product/1449314287/ref=as_li_tf_il?ie=UTF8&camp=1638&creative=6742&creativeASIN=1449314287&linkCode=as2&tag=codingpedia-21" target="_blank">Baron Schwartz&#8217; book &#8211; High Perfomance MySQL</a>:

<div id="mysql_book" style="margin: 0 auto;">
  <a href="http://www.amazon.de/gp/product/1449314287/ref=as_li_tf_il?ie=UTF8&camp=1638&creative=6742&creativeASIN=1449314287&linkCode=as2&tag=codingpedia-21" target="_blank"><img src="http://ws-eu.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=1449314287&Format=_SL160_&ID=AsinImage&MarketPlace=DE&ServiceVersion=20070822&WS=1&tag=codingpedia-21" alt="" border="0" /></a><img style="border: none !important; margin: 0px !important;" src="http://ir-de.amazon-adsystem.com/e/ir?t=codingpedia-21&l=as2&o=3&a=1449314287" alt="" width="1" height="1" border="0" />
</div>

<p style="text-align: justify;">
  You can find in it advanced stuff around MySQL performance like query performance optimization, indexing for high performance, optimizing schema and data types, OS and hardware optimization and so much more.
</p>

If you liked this, please show your support by following us and <a title="Podcastpedia.org how can I help" href="https://github.com/Codingpedia/podcastpedia/how_can_i_help" target="_blank">joining us</a> on <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a>
  
We promise to only share high quality podcasts and episodes.

## References

  1. <a title="MySQL reference manual" href="http://dev.mysql.com/doc/refman/5.5/en/" target="_blank">MySQL 5.5 reference manual</a>
  2. <a title="MySQL MyISAM storage engine" href="http://dev.mysql.com/doc/refman/5.6/en/myisam-storage-engine.htmlhttp://" target="_blank">MyISAM Storage Engine</a>
  3. <a title="MySQL - InnoDB Storage Engine" href="http://dev.mysql.com/doc/refman/5.5/en/innodb-storage-engine.html" target="_blank">InnoDB Storage Engine</a>
  4. <a title="MySQL Performance Blog" href="http://www.mysqlperformanceblog.com/" target="_blank">MySQL Performance Blog </a>

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
