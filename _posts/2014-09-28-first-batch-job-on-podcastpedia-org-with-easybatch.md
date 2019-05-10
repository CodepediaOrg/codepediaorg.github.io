---
id: 1829
title: First batch job on Podcastpedia.org with EasyBatch
date: 2014-09-28T20:52:10+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1829
permalink: /ama/first-batch-job-on-podcastpedia-org-with-easybatch/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:54:"https://github.com/podcastpedia/podcastpedia-easybatch";s:11:"ribbon-type";i:10;}'
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
dsq_thread_id:
  - 3062743768
categories:
  - java
tags:
  - batch
  - batchprocessing
  - database
  - easybatch
  - mysql
  - podcastpedia
---
<p style="text-align: justify;">
  Remember the first batch job for Podcastpedia.org, presented in <a title="http://www.codingpedia.org/ama/spring-batch-tutorial-with-spring-boot-and-java-configuration/" href="http://www.codingpedia.org/ama/spring-batch-tutorial-with-spring-boot-and-java-configuration/" target="_blank">Spring Batch Tutorial with Spring Boot and Java Configuration</a>&#8230; There, I would read submitted podcasts from a .csv file to add them to the <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> directory (database). Well today I will present how I automated the creation of this kind of input file, with the help of Easy Batch. Why EasyBatch? Because, after seeing my initial post, I was contacted by its founder, <a title="http://www.mahmoudbenhassine.com/" href="http://www.mahmoudbenhassine.com/" target="_blank">Mahmoud Ben Hassine</a>, to have a look at Easy Batch and give it a try. I did, and I am happy about that. Read on to find out why&#8230;<!--more-->
</p>

<p style="text-align: justify;">
  <div id="toc_container" class="no_bullets">
    <p class="toc_title">
      Contents
    </p>

    <ul class="toc_list">
      <li>
        <a href="#1_Job_description">1. Job description</a>
      </li>
      <li>
        <a href="#2_Project_setup">2. Project setup</a>
      </li>
      <li>
        <a href="#3_Implementation">3. Implementation</a><ul>
          <li>
            <a href="#31_Job_launcher">3.1. Job launcher</a>
          </li>
          <li>
            <a href="#32_Connect_to_MySQL">3.2. Connect to MySQL</a>
          </li>
          <li>
            <a href="#33_Create_an_Easy_Batch_engine">3.3. Create an Easy Batch engine</a><ul>
              <li>
                <a href="#331_Mapping_the_database_records">3.3.1. Mapping the database records</a>
              </li>
              <li>
                <a href="#332_Processing_records">3.3.2. Processing records</a>
              </li>
            </ul>
          </li>

          <li>
            <a href="#34_Execution_and_reporting">3.4. Execution and reporting</a>
          </li>
        </ul>
      </li>

      <li>
        <a href="#Conclusion">Conclusion</a>
      </li>
      <li>
        <a href="#Resources">Resources</a><ul>
          <li>
            <a href="#Source_Code_GitHub">Source Code – GitHub</a>
          </li>
          <li>
            <a href="#Web">Web</a>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</p>

<h2 style="text-align: justify;">
  <span id="1_Job_description">1. Job description</span>
</h2>

<p style="text-align: justify;">
  The batch job is fairly simple: reads database entries containing the submitted podcasts from one table and generates a properly formatted .csv file
</p>

## <span id="2_Project_setup">2. Project setup</span>

Reading from a database and writing to a flat file requires the following libraries in the classpath, which also bring the transitive dependency `easybatch-core`:

<pre class="lang:default decode:true" title="EasyBatch dependencies">&lt;dependency&gt;
	&lt;groupId&gt;org.easybatch&lt;/groupId&gt;
	&lt;artifactId&gt;easybatch-flatfile&lt;/artifactId&gt;
	&lt;version&gt;${easybatch.version}&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.easybatch&lt;/groupId&gt;
	&lt;artifactId&gt;easybatch-jdbc&lt;/artifactId&gt;
	&lt;version&gt;${easybatch.version}&lt;/version&gt;
&lt;/dependency&gt;</pre>

The current version is 2.2.0.

## <span id="3_Implementation">3. Implementation</span>

### <span id="31_Job_launcher">3.1. Job launcher</span>

For simplicity I chose to launch the job from a main method:

<pre class="lang:java mark:31-38 decode:true " title="Job Implementation with Easy Batch">package org.podcastpedia.batch.jobs.generatefilefromsuggestions;

import java.io.File;
import java.io.FileWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

import org.easybatch.core.api.EasyBatchReport;
import org.easybatch.core.impl.EasyBatchEngine;
import org.easybatch.core.impl.EasyBatchEngineBuilder;
import org.easybatch.jdbc.JdbcRecordReader;

public class JobLauncher {

	private static final String OUTPUT_FILE_HEADER = "FEED_URL; IDENTIFIER_ON_PODCASTPEDIA; CATEGORIES; LANGUAGE; MEDIA_TYPE; UPDATE_FREQUENCY; KEYWORDS; FB_PAGE; TWITTER_PAGE; GPLUS_PAGE; NAME_SUBMITTER; EMAIL_SUBMITTER";

	public static void main(String[] args) throws Exception {

		 //connect to MySql Database
		Class.forName("com.mysql.jdbc.Driver").newInstance();
		Connection connection = DriverManager.getConnection(System.getProperty("db.url"), System.getProperty("db.user"), System.getProperty("db.pwd"));

		FileWriter fileWriter = new FileWriter(getOutputFilePath());
		fileWriter.write(OUTPUT_FILE_HEADER + "\n");

		// Build an easy batch engine
		EasyBatchEngine easyBatchEngine = new EasyBatchEngineBuilder()
		.registerRecordReader(new JdbcRecordReader(connection, "SELECT * FROM ui_suggested_podcasts WHERE insertion_date &gt;= STR_TO_DATE(\'" + args[0] + "\', \'%Y-%m-%d %H:%i\')" ))
		.registerRecordMapper(new CustomMapper())
		.registerRecordProcessor(new Processor(fileWriter))
		.build();

		// Run easy batch engine
		EasyBatchReport easyBatchReport = easyBatchEngine.call();

		//close file writer
		fileWriter.close();
		System.out.println(easyBatchReport);
	}

	private static String getOutputFilePath() throws Exception {

		//create if not existent a "weeknum" directory in the given "output.directory.base" directory
		Date now = new Date();
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(now);
		int weeknum = calendar.get(Calendar.WEEK_OF_YEAR);
		String targetDirPath = System.getProperty("output.directory.base") + String.valueOf(weeknum);		
		File targetDirectory = new File(targetDirPath);
		if(!targetDirectory.exists()){
			boolean created = targetDirectory.mkdir();
			if(!created){
				throw new Exception("Target directory could not be created");
			}
		}

		//build the file name based on current time to be placed in the "weeknum" directory  
		DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH.mm");
		String outputFileName = "suggestedPodcasts " + dateFormat.format(now) + ".csv";

		String filePath = targetDirPath + "/" + outputFileName;		
		return filePath;
	}

}</pre>

Let&#8217;s have a look at the different components from the Launcher class:

### <span id="32_Connect_to_MySQL">3.2. Connect to MySQL</span>

<pre class="lang:java decode:true" title="Build connection to MySQL database">Class.forName("com.mysql.jdbc.Driver").newInstance();
Connection connection = DriverManager.getConnection(System.getProperty("db.url"), System.getProperty("db.user"), System.getProperty("db.pwd"));
</pre>

When using the JDBC outside of an application server, the `DriverManager` class manages the establishment of connections. You have to:

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;Specify to the DriverManager which JDBC drivers to try to make Connections with. The easiest way to do this is to use Class.forName() on the class that implements the java.sql.Driver interface. With MySQL Connector/J, the name of this class is com.mysql.jdbc.Driver. With this method, you could use an external configuration file to supply the driver class name and driver parameters to use when connecting to a database.&#8221; [2]</em>
</p>

<p style="text-align: justify;">
  Once the MySQL driver has been registered, you can obtain a Connection to the database by calling the <code>DriverManager.getConnection()</code> method with given MySQL database URL.
</p>

**Note** &#8211; make sure you also have the MySQL JDBC connector in your classpath:

<pre class="lang:default decode:true" title="MySQL JDBC connector maven dependency" style="padding-left: 30px;">&lt;!-- MySQL JDBC connector --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;mysql&lt;/groupId&gt;
	&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
	&lt;version&gt;5.1.31&lt;/version&gt;
&lt;/dependency&gt;</pre>

### <span id="33_Create_an_Easy_Batch_engine">3.3. Create an Easy Batch engine</span>

<p style="text-align: justify;">
  <span style="color: #333333;">Creating an Easy Batch engine is straightforward and can be done through the </span><code>EasyBatchEngineBuilder</code> API as follows
</p>

<pre class="lang:java decode:true" title="Build an easy batch engine">// Build an easy batch engine
EasyBatchEngine easyBatchEngine = new EasyBatchEngineBuilder()
.registerRecordReader(new JdbcRecordReader(connection, "SELECT * FROM ui_suggested_podcasts WHERE insertion_date &gt;= STR_TO_DATE(\'" + args[0] + "\', \'%Y-%m-%d %H:%i\')" ))
.registerRecordMapper(new CustomMapper())
.registerRecordProcessor(new Processor(fileWriter))
.build();</pre>

This is actually the whole batch configuration for the job. Short and clear:

  * register a record reader, in our case a `JdbcRecordReader`, for which you need to specify the connection created earlier and SQL string to execute against it
  * register a custom record mapper
  * register a processor

<p class="alert_normal">
  <strong>Note:</strong> You don&#8217;t need to iterate over the Jdbc ResultSet, Easy Batch will do it for you.
</p>

#### <span id="331_Mapping_the_database_records">3.3.1. Mapping the database records</span>

To map the database object to the domain object I defined a `CustomMapper`:

<pre class="lang:java decode:true" title="Custom record mapper">public class CustomMapper implements RecordMapper&lt;SuggestedPodcast&gt;{

	@SuppressWarnings("rawtypes")
	@Override
	public SuggestedPodcast mapRecord(Record record) throws Exception {
        JdbcRecord jdbcRecord = (JdbcRecord) record;
        ResultSet resultSet = jdbcRecord.getRawContent();

        SuggestedPodcast response = new SuggestedPodcast();
        response.setMetadataLine(resultSet.getString("metadata_line"));

		return response;
	}

}</pre>

<p style="text-align: justify;">
  For that I had to implement the <code>RecordMapper</code> interface with its single <code>mapRecord()</code> method.
</p>

#### <span id="332_Processing_records">3.3.2. Processing records</span>

<p style="text-align: justify;">
  Easy Batch lets you define your batch processing business logic through the <code>RecordProcessor</code> interface. This is where you define what to do for each record. The processor is registered in the line:
</p>

<pre class="lang:java decode:true " title="register processor">.registerRecordProcessor(new Processor(fileWriter))</pre>

<p style="text-align: justify;">
  My custom processor  extends the <code>AbstractRecordProcessor</code>, which is a abstract record processor implementation to extend by clients that do not need to implement <code>RecordProcessor.getEasyBatchResult()</code>:
</p>

<pre class="lang:java decode:true" title="Processor implementation">public class Processor extends AbstractRecordProcessor&lt;SuggestedPodcast&gt;{

	 private FileWriter fileWriter;

	 public Processor(FileWriter fileWriter) {
		 this.fileWriter = fileWriter;
	 }

	@Override
	public void processRecord(SuggestedPodcast record) throws Exception {
		 fileWriter.write(record.getMetadataLine() + "\n");
		 fileWriter.flush();		
	}

}</pre>

The &#8220;logic&#8221; is very simple, it just writes new lines at the end of the file.

### <span id="34_Execution_and_reporting">3.4. Execution and reporting</span>

<p style="text-align: justify;">
  Easy Batch engine records several metrics during record processing and provides a complete report at the end of execution. This report is an instance of the <code>EasyBatchReport</code> class and contains the following information:
</p>

  *     The batch start and end times
  *     The batch duration
  *     The data source name
  *     The total records number
  *     The number of filtered, ignored and rejected records
  *     The number of records processed with errors
  *     The number of records successfully processed
  *     The record processing time average
  *     And the computation result if any

You obtain an Easy Batch report when running the Easy Batch engine:

<pre class="lang:java decode:true " title="Easy Batch engine execution and report generation">// Run easy batch engine
EasyBatchReport easyBatchReport = easyBatchEngine.call();</pre>

Check out the <a title="http://www.easybatch.org/documentation/userGuide.html" href="http://www.easybatch.org/documentation/userGuide.html" target="_blank">Easy Batch user guide</a> for other report formatting options.

## <span id="Conclusion">Conclusion</span>

<p style="text-align: justify;">
  For this easy job I had to implement, Easy Batch proved to be a simple, yet powerful batch framework, with good samples and documentation. Before starting my next batch job I will definetely have a look at Easy Batch first, before considering the &#8220;mightier&#8221; Spring Batch framework. But, to quote the author from Easy Batch, from <a title="http://mahmoudbenhassine.wordpress.com/2014/03/03/spring-batch-vs-easy-batch-a-hello-world-comparison/" href="http://mahmoudbenhassine.wordpress.com/2014/03/03/spring-batch-vs-easy-batch-a-hello-world-comparison/" target="_blank">Spring Batch vs Easy Batch: a Hello World comparison</a>
</p>

<p style="text-align: justify; padding-left: 30px;">
  <em>&#8220;Choose the right tool for the right job! If your application requires advanced features like retry on failure, remoting or flows, then go for Spring Batch (or an implementation of JSR352). If you don’t need all this advanced stuff; then Easy Batch can be very handy to simplify your batch application development. &#8220;</em>
</p>

<h2 class="title" style="color: #000000;">
  <span id="Resources"><span id="Resources">Resources</span></span>
</h2>

### <span id="Source_Code_GitHub"><span id="Source_Code_GitHub">Source Code – GitHub</span></span>

  * <a title="https://github.com/podcastpedia/podcastpedia-easybatch" href="https://github.com/podcastpedia/podcastpedia-easybatch" target="_blank">podcastpedia-easybatch </a>&#8211; project presented in this tutorial. Please make a pull request for any improvement proposals

### <span id="Web"><span id="Web">Web</span></span>

  1. <a title="http://www.easybatch.org/" href="http://www.easybatch.org/" target="_blank">EasyBatch.org</a>
      1. <a title="http://www.easybatch.org/tutorials/index.html" href="http://www.easybatch.org/tutorials/index.html" target="_blank">Tutorials</a>
      2. <a title="http://www.easybatch.org/documentation/userGuide.html" href="http://www.easybatch.org/documentation/userGuide.html" target="_blank">User guide</a>
      3. <a title="http://www.easybatch.org/tutorials/customers.html" href="http://www.easybatch.org/tutorials/customers.html" target="_blank">Customers ETL tutorial</a>
  2. <a title="http://dev.mysql.com/doc/connector-j/en/connector-j-usagenotes-connect-drivermanager.html" href="http://dev.mysql.com/doc/connector-j/en/connector-j-usagenotes-connect-drivermanager.html" target="_blank">Connecting to MySQL Using the JDBC DriverManager Interface</a>

<p style="text-align: justify;">
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
</p>
