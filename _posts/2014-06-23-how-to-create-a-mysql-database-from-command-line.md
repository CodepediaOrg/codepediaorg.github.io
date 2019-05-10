---
id: 1583
title: How to create a MySQL database from command line
date: 2014-06-23T14:45:34+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1583
permalink: /ama/how-to-create-a-mysql-database-from-command-line/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:48:"https://github.com/podcastpedia/podcastpedia-sql";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 1
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2788604273
categories:
  - database
  - Podcastpedia.org
tags:
  - command line
  - mysql
  - setup
  - shell
---
<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <ul>
        <li>
          <a href="#1_Install_MySql_55_and_above">1. Install MySql 5.5 and above</a>
        </li>
        <li>
          <a href="#2_Connect_to_the_MySql_console">2. Connect to the MySql console</a>
        </li>
        <li>
          <a href="#3_Create_database_and_the_development_DB_user">3. Create database and the (development) DB user</a>
        </li>
        <li>
          <a href="#4_Import_database_from_file">4. Import database from file</a>
        </li>
        <li>
          <a href="#5_Backup_the_database_optional">5. Backup the database (optional)</a>
        </li>
      </ul></li>

      <li>
        <a href="#Resources">Resources</a><ul>
          <li>
            <a href="#Web">Web</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>

  <br /> We use currently for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org </a>a MySQL database and because we&#8217;ve recently gone <a title="PodcastpediaOrg on GitHub" href="https://github.com/Codingpedia/podcastpedia" target="_blank">open source on GitHub</a>, we&#8217;ve created a <a title="https://github.com/podcastpedia/podcastpedia-sql" href="https://github.com/Codingpedia/podcastpedia/blob/master/README.md" target="_blank">README.md to explain the setup of the database</a>. The content of that file is basically reproduced here, as &#8220;back-up&#8221;, for future reference and why not?, it might also serve others in the mean time.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> Although the steps presented here refer to the database backing <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, they should be valid for any MySQL database you might want to setup either in Linux or Windows.<!--more-->
</p>

### <span id="1_Install_MySql_55_and_above"><a class="anchor" href="https://github.com/podcastpedia/podcastpedia-sql/blob/master/README.md#install-mysql-55-and-above" name="user-content-install-mysql-55-and-above"></a>1. Install MySql 5.5 and above</span>

<ol class="task-list">
  <li>
    <a href="http://dev.mysql.com/downloads/mysql/">Download MySQL Community Server</a> version 5.5 or 5.6 for the platform of your choice.
  </li>
  <li>
    <a title="http://dev.mysql.com/doc/refman/5.6/en/installing.html" href="http://dev.mysql.com/doc/refman/5.6/en/installing.html" target="_blank">Install the MySQL Server</a> <ol>
      <li>
        <a title="http://dev.mysql.com/doc/refman/5.6/en/linux-installation.html" href="http://dev.mysql.com/doc/refman/5.6/en/linux-installation.html" target="_blank">Installing MySQL on Linux</a>
      </li>
      <li>
        <a title="http://dev.mysql.com/doc/refman/5.6/en/windows-installation.html" href="http://dev.mysql.com/doc/refman/5.6/en/windows-installation.html" target="_blank">Installing MySQL on Microsoft Windows</a>
      </li>
      <li>
        Setup MySQL configuration file <ul>
          <li style="text-align: justify;">
            For Windows place the configuration file where the MySQL server is installed &#8211; the <a href="https://github.com/podcastpedia/podcastpedia-sql/blob/master/_prepare_database_for_development/my.ini">my.ini</a> file from above is an example used on a Windows 7 machine
          </li>
          <li style="text-align: justify;">
            For linux you need to use .cnf files. You can see in this blog post &#8211;<a href="http://www.codingpedia.org/ama/optimizing-mysql-server-settings/">Optimizing MySQL server settings</a> &#8211; how the MySQL database is configured in production for <a href="https://github.com/Codingpedia/podcastpedia/">Podcastpedia.org</a>
          </li>
        </ul>
      </li>

      <li style="text-align: justify;">
        <a title="http://dev.mysql.com/doc/refman/5.6/en/setting-environment-variables.html" href="http://dev.mysql.com/doc/refman/5.6/en/setting-environment-variables.html" target="_blank">Set up the environment variables</a> &#8211; the MySQL programs are installed in <code>MySQL_installation_path/bin</code>, so you&#8217;d want this location added to the <code>PATH</code> variable to be easily invoked from shells and command lines.
      </li>
      <li>
        <strong style="line-height: 1.5;">Optional</strong><span style="line-height: 1.5;"> &#8211; install </span><a style="line-height: 1.5;" href="http://www.mysql.com/products/workbench/">MySQL Workbench</a><span style="line-height: 1.5;"> for easy DB development and administration</span>
      </li>
    </ol>
  </li>
</ol>

### <span id="2_Connect_to_the_MySql_console"><a class="anchor" href="https://github.com/podcastpedia/podcastpedia-sql/blob/master/README.md#connect-to-the-mysql-console" name="user-content-connect-to-the-mysql-console"></a>2. Connect to the MySql console</span>

When installing the MySQL server, you were asked to set up a &#8220;root&#8221; user. Now use it to connect to the MySQL console:

<pre class="lang:sh decode:true" title="Command to connect to MySQL shell">shell&gt; mysql --host=localhost --user=root -p</pre>

You will be asker for your root&#8217;s password

### <span id="3_Create_database_and_the_development_DB_user"><a class="anchor" href="https://github.com/podcastpedia/podcastpedia-sql/blob/master/README.md#create-database-and-development-db-user" name="user-content-create-database-and-development-db-user"></a>3. Create database and the (development) DB user</span>

Once you are connected to the MySQL command line do following the steps:

<pre class="lang:mysql decode:true" title="Create DB and DB user commands">-- delete the pcmDB database if existent
mysql&gt; DROP DATABASE IF EXISTS pcmDB;

--create the pcmDB database
mysql&gt; CREATE DATABASE pcmDB;

-- connect to the created database
mysal&gt; use pcmDB;

-- drop 'pcm' user
mysql&gt; DROP USER 'pcm'@'localhost';

-- create the development user 'pcm' identified by the password 'pcm_pw'
mysql&gt; CREATE USER 'pcm'@'localhost' IDENTIFIED BY 'pcm_pw';

-- verify user has been created
mysql&gt; select host, user, password from mysql.user;
+-----------+-------------+-------------------------------------------+
| host      | user        | password                                  |
+-----------+-------------+-------------------------------------------+
| localhost | root        | *C2BD8E7A5247DF69A9A8CB29C8C6E8FC83D3681F |
| 127.0.0.1 | root        | *C2BD8E7A5247DF69A9A8CB29C8C6E8FC83D3681F |
| ::1       | root        | *C2BD8E7A5247DF69A9A8CB29C8C6E8FC83D3681F |
| %         | pcm_user    | *32D8ED777E1B90734ED5A6AFCD0E354230826743 |
| %         | rest_demo   | *3B8DD81985A42FD9B56133326F3B25A2985A3F75 |
*** | localhost | pcm         | *68DC5C435B9AAA7280CA4C89391C28EFEEC0E946 |***
| localhost | pdp_user    | *F776A21503EFA57908FEF30C914DFB9A9FC78EF3 |
+-----------+-------------+-------------------------------------------+

-- check user privileges
mysql&gt; SELECT host, user, select_priv, insert_priv, update_priv, delete_priv, create_priv, alter_priv, password FROM mysql.user WHERE user='pcm';

-- should have no(N) privileges
+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+-
| host      | user | select_priv | insert_priv | update_priv | delete_priv | create_priv | alter_priv |
+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+-
| localhost | pcm  | N           | N           | N           | N           | N           | N          |
+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+-

-- grant full privileges to the user, for easy development
mysql&gt; GRANT ALL PRIVILEGES ON *.* TO 'pcm'@'localhost';

-- verify privileges were set (Y)
mysql&gt; SELECT host, user, select_priv, insert_priv, update_priv, delete_priv, create_priv, alter_priv, password FROM mysql.user WHERE user='pcm';

+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+
| host      | user | select_priv | insert_priv | update_priv | delete_priv | create_priv | alter_priv |
+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+
| localhost | pcm  | Y           | Y           | Y           | Y           | Y           | Y          |
+-----------+------+-------------+-------------+-------------+-------------+-------------+------------+

-- exit the mysql command line
exit;</pre>

Instead of executing all these commands manually, you can alternatively put them in a `.sql` script file, like **_[prepare\_database\_for_import.sql](https://github.com/podcastpedia/podcastpedia-sql/blob/master/_prepare_database_for_development/prepare_database_for_import.sql)_, ** and run the following command in the OS shell:

<pre class="lang:sh decode:true" title="Execute SQL script file from MySQL shell">shell&gt; mysql &lt; "PATH_TO_FILE\prepare_database_for_import.sql"</pre>

Now that the database and user are put in place, you can start creating your tables and fill them with data.

### <span id="4_Import_database_from_file"><a class="anchor" href="https://github.com/podcastpedia/podcastpedia-sql/blob/master/README.md#import-database-from-file" name="user-content-import-database-from-file"></a>4. Import database from file</span>

<p style="text-align: justify;">
  Tables and data required for Podcastpedia.org will be imported via a single <em>.sql</em> file, which you can download <a href="https://github.com/Codingpedia/podcastpedia/blob/master/sql/_prepare_database_for_development/podcastpedia-2014-07-17-dev-db.sql">podcastpedia-2014-06-17-dev-db.sql</a> After download you can import the database data into the pcmDB database by issuing the following command on the command line:
</p>

<pre class="lang:sh decode:true" title="Import DB From file example">shell&gt; mysql -p -u pcm pcmDB &lt; "PATH_TO_FILE\podcastpedia-2014-06-17-dev-db.sql"
-- e.g. mysql -p -u pcm pcmDB &lt; "C:\tmp\podcastpedia-2014-06-17-dev-db.sql"</pre>

<p style="text-align: justify;">
  These should take a moment depending on your PC&#8217;s performance, and once is ready you can check that everything was OK by connecting to the <em>mysql command line</em> and issuing SQL commands like &#8220;show tables&#8221; or &#8220;select from a table&#8221;:
</p>

<pre class="lang:mysql decode:true" title="Verify successful DB-import">-- connect to the database with the development user
shell&gt; mysql --host=localhost --user=pcm --password=pcm_pw

-- use the podcastpedia database
mysql&gt; USE pcmDB;

-- show tables imported
mysql&gt; SHOW TABLES;

-- select data from a table, e.g. "categories"
mysql&gt; SELECT * from categories;
+-------------+-----------------------+--------------------+
| CATEGORY_ID | NAME                  | DESCRIPTION        |
+-------------+-----------------------+--------------------+
|          21 | science_technology    | science            |
|          22 | education             | education          |
|          24 | arts_culture          | Arts & culture     |
|          25 | health_medicine       | Health             |
|          27 | music                 | Music              |
|          28 | religion_spirituality | Religion           |
|          29 | tv_film               | science            |
|          31 | sport                 | Sport              |
|          33 | economy               | Economy            |
|          35 | hobby_freetime        | Hobby & free time  |
|          37 | family_children       | Family & children  |
|          38 | travel_transport      | Travel & Transport |
|          39 | people_society        | People             |
|          41 | internet_computer     | Internet           |
|          42 | news_politics         | News               |
|          43 | radio                 | Radio              |
|          44 | money_business        | Money              |
|          45 | entertainment         | Entertainment      |
|          46 | food_drink            | Food and drink     |
|          47 | nature_environment    | Nature             |
|          48 | general               | General            |
|          49 | history               | History            |
+-------------+-----------------------+--------------------+</pre>

### <span id="5_Backup_the_database_optional">5. Backup the database (optional)</span>

If you ever want to backup up the database you can use the _mysqldump_ program, by issuing a command similar to the following on the command line: ``

<pre class="lang:sh decode:true" title="Backup MySQL database command">shell&gt; mysqldump pcmdb -u pcm -p -h 127.0.0.1 --single-transaction &gt; c:/tmp/pcmdb-backup-2014.06.22.sql</pre>

The pcmDB database will be than saved into the single file _pcmdb-backup-2014.06.22.sql,_ which you can later import as mentioned on the previous step.

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="Resources">Resources</span>

### <span id="Web">Web</span>

  1.  <a title="MySQL documentation" href="http://dev.mysql.com/doc/" target="_blank">MySQL documentation</a>
      1. <a title="http://dev.mysql.com/doc/refman/5.6/en/connecting.html" href="http://dev.mysql.com/doc/refman/5.6/en/connecting.html" target="_blank">Connecting to the MySQL Server</a>
      2. <a title="http://dev.mysql.com/doc/refman/5.5/en/adding-users.html" href="http://dev.mysql.com/doc/refman/5.5/en/adding-users.html" target="_blank">Adding User Accounts</a>
      3. <a title="http://dev.mysql.com/doc/refman/5.6/en/drop-database.html" href="http://dev.mysql.com/doc/refman/5.6/en/drop-database.html" target="_blank">DROP DATABASE Syntax</a>
  2. <a title="https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql" href="https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql" target="_blank">How To Create a New User and Grant Permissions in MySQL</a>

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

&nbsp;
