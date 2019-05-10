---
id: 1693
title: Spring Batch Tutorial with Spring Boot and Java Configuration
date: 2014-09-08T11:57:40+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1693
permalink: /ama/spring-batch-tutorial-with-spring-boot-and-java-configuration/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:50:"https://github.com/podcastpedia/podcastpedia-batch";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 10
fsb_social_google:
  - 5
fsb_social_linkedin:
  - 4
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 2998329809
categories:
  - java
  - spring
tags:
  - apache http client
  - batch
  - data source
  - database
  - email
  - spring batch
  - spring boot
  - tutorial
  - velocity
---

<p class="title" style="color: #000000; text-align: justify;">
  I&#8217;ve been working on migrating some batch jobs for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> to Spring Batch. Before, these jobs were developed in my own kind of way, and I thought it was high time to use a more &#8220;standardized&#8221; approach. Because I had never used Spring with java configuration before, I thought this were a good opportunity to learn about it, by configuring the Spring Batch jobs in java. And since I am all into trying new things with Spring, why not also throw Spring Boot into the boat&#8230;<!--more-->
</p>

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_What_I8217llbuild">1. What I&#8217;ll build</a>
    </li>
    <li>
      <a href="#2_What_you8217ll_need">2. What you&#8217;ll need</a>
    </li>
    <li>
      <a href="#3_Set_up_the_project">3. Set up the project</a><ul>
        <li>
          <a href="#31_Maven_build_file">3.1. Maven build file</a>
        </li>
        <li>
          <a href="#32_Project_directory_structure">3.2. Project directory structure</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#4_Create_a_batch_Job_configuration">4. Create a batch Job configuration</a>
    </li>
    <li>
      <a href="#5_Spring_Batchprocessing_units">5. Spring Batch processing units</a><ul>
        <li>
          <a href="#51_Readers">5.1. Readers</a><ul>
            <li>
              <a href="#511_FlatFileItemReader">5.1.1. FlatFileItemReader</a><ul>
                <li>
                  <a href="#5111_LineMapper">5.1.1.1. LineMapper</a>
                </li>
                <li>
                  <a href="#5112_FieldSetMapper">5.1.1.2. FieldSetMapper</a>
                </li>
              </ul>
            </li>

            <li>
              <a href="#52_JdbcCursorItemReader">5.2. JdbcCursorItemReader</a><ul>
                <li>
                  <a href="#521_RowMapper">5.2.1. RowMapper</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>

        <li>
          <a href="#52_Writers"> 5.2. Writers</a>
        </li>
        <li>
          <a href="#53_Processors"> 5.3. Processors</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#6_Execute_the_batch_application">6. Execute the batch application</a><ul>
        <li>
          <a href="#61_Running_the_application_on_dev_and_prod_environments">6.1. Running the application on dev and prod environments</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Summary">Summary</a>
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

<p class="title note_normal" style="text-align: justify;">
  <strong>Note:<br /> </strong>Before you begin with this tutorial I recommend you read first Spring&#8217;s <a title="http://spring.io/guides/gs/batch-processing/" href="http://spring.io/guides/gs/batch-processing/" target="_blank">Getting started &#8211; Creating a Batch Service</a>, because  the structure and the code presented here builds on that original.
</p>

<h2 class="title" style="color: #000000; text-align: justify;">
  <span id="1_What_I8217llbuild">1. What I&#8217;ll build</span>
</h2>

<p class="title" style="color: #000000; text-align: justify;">
  So, as mentioned, in this post I will present Spring Batch in the context of configuring it and developing with it some batch jobs for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>. Here&#8217;s a short description of the two jobs that are currently part of the <a title="https://github.com/podcastpedia/podcastpedia-batch" href="https://github.com/podcastpedia/podcastpedia-batch" target="_blank">Podcastpedia-batch</a> project:
</p>

  1. **addNewPodcastJob**
      1. _reads_ podcast metadata (feed url, identifier, categories etc.) from a _flat file_
      2. transforms (parses and prepares episodes to be inserted with _Http Apache Client_) the data
      3. and in the last step, _insert_ it to the Podcastpedia _database_ and _inform_ the submitter _via email_ about it
<li style="text-align: justify;">
  <strong>notifyEmailSubscribersJob</strong> &#8211; people can subscribe to their favorite podcasts on <a title="https://github.com/Codingpedia/podcastpedia/" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">Podcastpedia.org</a> via email. For those who did it is checked on a regular basis (DAILY, WEEKLY, MONTHLY) if new episodes are available, and if they are the subscribers are informed via email about those; <em>read from database</em>, expand read data via JPA, re-group it and <em>notify</em> subscriber <em>via email</em>
</li>

<p class="title note_code" style="color: #000000; text-align: justify;">
  <strong>Source code:<br /> </strong> The source code for this tutorial is available on GitHub &#8211; <a title="https://github.com/podcastpedia/podcastpedia-batch" href="https://github.com/podcastpedia/podcastpedia-batch" target="_blank">Podcastpedia-batch.</a>
</p>

<p class="title note_normal" style="color: #000000; text-align: justify;">
  <strong>Note:</strong> Before you start I also highly recommend you read the <a title="http://docs.spring.io/spring-batch/reference/html/domain.html" href="http://docs.spring.io/spring-batch/reference/html/domain.html" target="_blank">Domain Language of Batch</a>,  so that terms like &#8220;Jobs&#8221;, &#8220;Steps&#8221; or &#8220;ItemReaders&#8221; don&#8217;t sound strange to you.
</p>

<h2 class="title" style="color: #000000; text-align: justify;">
  <span id="2_What_you8217ll_need">2. What you&#8217;ll need</span>
</h2>

  * A favorite text editor or IDE
  * <a title="http://www.oracle.com/technetwork/java/javase/downloads/index.html" href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">JDK 1.7</a> or later
  * <a title="http://maven.apache.org/download.cgi" href="http://maven.apache.org/download.cgi" target="_blank">Maven 3.0+ </a>

<h2 class="title" style="color: #000000; text-align: justify;">
  <span id="3_Set_up_the_project">3. Set up the project</span>
</h2>

<p style="text-align: justify;">
  The project is built with <a title="https://maven.apache.org/" href="https://maven.apache.org/" target="_blank">Maven</a>. It uses Spring Boot, which makes it easy to create stand-alone Spring based Applications that you can &#8220;just run&#8221;.  You can learn more about the Spring Boot by visiting the <a title="http://projects.spring.io/spring-boot/" href="http://projects.spring.io/spring-boot/" target="_blank">project&#8217;s website.</a>
</p>

<h3 style="text-align: justify;">
  <span id="31_Maven_build_file">3.1. Maven build file</span>
</h3>

<p style="text-align: justify;">
  Because it uses Spring Boot it will have the <code>spring-boot-starter-parent</code> as its parent, and a couple of other spring-boot-starters that will get for us some libraries required in the project:
</p>

<pre>
  <code class="xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
    &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;

    &lt;groupId&gt;org.podcastpedia.batch&lt;/groupId&gt;
    &lt;artifactId&gt;podcastpedia-batch&lt;/artifactId&gt;
    &lt;version&gt;0.1.0&lt;/version&gt;

    &lt;properties&gt;
    	&lt;sprinb.boot.version&gt;1.1.6.RELEASE&lt;/sprinb.boot.version&gt;
    	&lt;java.version&gt;1.7&lt;/java.version&gt;
	&lt;/properties&gt;
    &lt;parent&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-parent&lt;/artifactId&gt;
        &lt;version&gt;1.1.6.RELEASE&lt;/version&gt;
    &lt;/parent&gt;

    &lt;dependencies&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
            &lt;artifactId&gt;spring-boot-starter-batch&lt;/artifactId&gt;

        &lt;/dependency&gt;  
		&lt;dependency&gt;
		  &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
		  &lt;artifactId&gt;spring-boot-starter-data-jpa&lt;/artifactId&gt;		   
		&lt;/dependency&gt;        

		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
			&lt;artifactId&gt;httpclient&lt;/artifactId&gt;
			&lt;version&gt;4.3.5&lt;/version&gt;
		&lt;/dependency&gt;		
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.httpcomponents&lt;/groupId&gt;
			&lt;artifactId&gt;httpcore&lt;/artifactId&gt;
			&lt;version&gt;4.3.2&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;!-- velocity --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.velocity&lt;/groupId&gt;
			&lt;artifactId&gt;velocity&lt;/artifactId&gt;
			&lt;version&gt;1.7&lt;/version&gt;		
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.apache.velocity&lt;/groupId&gt;
			&lt;artifactId&gt;velocity-tools&lt;/artifactId&gt;
			&lt;version&gt;2.0&lt;/version&gt;
		    &lt;exclusions&gt;
		        &lt;exclusion&gt;
					&lt;groupId&gt;org.apache.struts&lt;/groupId&gt;
					&lt;artifactId&gt;struts-core&lt;/artifactId&gt;
		        &lt;/exclusion&gt;
		    &lt;/exclusions&gt;				
		&lt;/dependency&gt;

		&lt;!-- Project rome rss, atom --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;rome&lt;/groupId&gt;
			&lt;artifactId&gt;rome&lt;/artifactId&gt;
			&lt;version&gt;1.0&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;!-- option this fetcher thing --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;rome&lt;/groupId&gt;
			&lt;artifactId&gt;rome-fetcher&lt;/artifactId&gt;
			&lt;version&gt;1.0&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.jdom&lt;/groupId&gt;
			&lt;artifactId&gt;jdom&lt;/artifactId&gt;
			&lt;version&gt;1.1&lt;/version&gt;
		&lt;/dependency&gt;		
		&lt;!-- PID 1 --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;xerces&lt;/groupId&gt;
			&lt;artifactId&gt;xercesImpl&lt;/artifactId&gt;
			&lt;version&gt;2.9.1&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;!-- MySQL JDBC connector --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;mysql&lt;/groupId&gt;
			&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
			&lt;version&gt;5.1.31&lt;/version&gt;
		&lt;/dependency&gt;
 		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
			&lt;artifactId&gt;spring-boot-starter-freemarker&lt;/artifactId&gt;   
		&lt;/dependency&gt;
 		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
			&lt;artifactId&gt;spring-boot-starter-remote-shell&lt;/artifactId&gt;
		    &lt;exclusions&gt;
		        &lt;exclusion&gt;
					&lt;groupId&gt;javax.mail&lt;/groupId&gt;
					&lt;artifactId&gt;mail&lt;/artifactId&gt;
		        &lt;/exclusion&gt;
		    &lt;/exclusions&gt;				
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;javax.mail&lt;/groupId&gt;
			&lt;artifactId&gt;mail&lt;/artifactId&gt;
			&lt;version&gt;1.4.7&lt;/version&gt;
		&lt;/dependency&gt;		
		&lt;dependency&gt;
			&lt;groupId&gt;javax.inject&lt;/groupId&gt;
			&lt;artifactId&gt;javax.inject&lt;/artifactId&gt;
			&lt;version&gt;1&lt;/version&gt;
		&lt;/dependency&gt;		
		&lt;dependency&gt;
			&lt;groupId&gt;org.twitter4j&lt;/groupId&gt;
			&lt;artifactId&gt;twitter4j-core&lt;/artifactId&gt;
			&lt;version&gt;[4.0,)&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
			&lt;artifactId&gt;spring-boot-starter-test&lt;/artifactId&gt;
		&lt;/dependency&gt;
    &lt;/dependencies&gt;

    &lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
            &lt;/plugin&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
                &lt;artifactId&gt;spring-boot-maven-plugin&lt;/artifactId&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/build&gt;
&lt;/project&gt;</code>
</pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:<br /> </strong> One big advantage of using the <code>spring-boot-starter-parent</code> as the project&#8217;s parent is that you only have to upgrade the version of the parent and it will get the &#8220;latest&#8221; libraries for you. When I started the project spring boot was in version <code>1.1.3.RELEASE</code> and by the time of finishing to write this post is already at <code>1.1.6.RELEASE</code>.
</p>

### <span id="32_Project_directory_structure">3.2. Project directory structure</span>

I structured the project in the following way:

<pre>
  <code class="bash">└── src
    └── main
        └── java
            └── org
				└── podcastpedia
					└── batch
						└── common
						└── jobs					
							└── addpodcast												
							└── notifysubscribers</code>											
</pre>

**Note:**

<li style="text-align: justify;">
  the <code>org.podcastpedia.batch.jobs</code> package contains sub-packages having specific classes to particular jobs.
</li>
<li style="text-align: justify;">
   the <code>org.podcastpedia.batch.jobs.common</code> package contains classes used by all the jobs, like for example the JPA entities that both the current jobs require.
</li>

## <span id="4_Create_a_batch_Job_configuration">4. Create a batch Job configuration</span>

I will start by presenting the Java configuration class for the first batch job:

<pre>
  <code class="java">package org.podcastpedia.batch.jobs.addpodcast;

import org.podcastpedia.batch.common.configuration.DatabaseAccessConfiguration;
import org.podcastpedia.batch.common.listeners.LogProcessListener;
import org.podcastpedia.batch.common.listeners.ProtocolListener;
import org.podcastpedia.batch.jobs.addpodcast.model.SuggestedPodcast;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.item.ItemProcessor;
import org.springframework.batch.item.ItemReader;
import org.springframework.batch.item.ItemWriter;
import org.springframework.batch.item.file.FlatFileItemReader;
import org.springframework.batch.item.file.LineMapper;
import org.springframework.batch.item.file.mapping.BeanWrapperFieldSetMapper;
import org.springframework.batch.item.file.mapping.DefaultLineMapper;
import org.springframework.batch.item.file.transform.DelimitedLineTokenizer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.core.io.ClassPathResource;

import com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException;

@Configuration
@EnableBatchProcessing
@Import({DatabaseAccessConfiguration.class, ServicesConfiguration.class})
public class AddPodcastJobConfiguration {

	@Autowired
	private JobBuilderFactory jobs;

	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	// tag::jobstep[]
	@Bean
	public Job addNewPodcastJob(){
		return jobs.get("addNewPodcastJob")
				.listener(protocolListener())
				.start(step())
				.build();
	}

	@Bean
	public Step step(){
		return stepBuilderFactory.get("step")
				.&lt;SuggestedPodcast,SuggestedPodcast&gt;chunk(1) //important to be one in this case to commit after every line read
				.reader(reader())
				.processor(processor())
				.writer(writer())
				.listener(logProcessListener())
				.faultTolerant()
				.skipLimit(10) //default is set to 0
				.skip(MySQLIntegrityConstraintViolationException.class)
				.build();
	}
	// end::jobstep[]

	// tag::readerwriterprocessor[]
	@Bean
	public ItemReader&lt;SuggestedPodcast&gt; reader(){
		FlatFileItemReader&lt;SuggestedPodcast&gt; reader = new FlatFileItemReader&lt;SuggestedPodcast&gt;();
		reader.setLinesToSkip(1);//first line is title definition
		reader.setResource(new ClassPathResource("suggested-podcasts.txt"));
		reader.setLineMapper(lineMapper());
		return reader;
	}

	@Bean
	public LineMapper&lt;SuggestedPodcast&gt; lineMapper() {
		DefaultLineMapper&lt;SuggestedPodcast&gt; lineMapper = new DefaultLineMapper&lt;SuggestedPodcast&gt;();

		DelimitedLineTokenizer lineTokenizer = new DelimitedLineTokenizer();
		lineTokenizer.setDelimiter(";");
		lineTokenizer.setStrict(false);
		lineTokenizer.setNames(new String[]{"FEED_URL", "IDENTIFIER_ON_PODCASTPEDIA", "CATEGORIES", "LANGUAGE", "MEDIA_TYPE", "UPDATE_FREQUENCY", "KEYWORDS", "FB_PAGE", "TWITTER_PAGE", "GPLUS_PAGE", "NAME_SUBMITTER", "EMAIL_SUBMITTER"});

		BeanWrapperFieldSetMapper&lt;SuggestedPodcast&gt; fieldSetMapper = new BeanWrapperFieldSetMapper&lt;SuggestedPodcast&gt;();
		fieldSetMapper.setTargetType(SuggestedPodcast.class);

		lineMapper.setLineTokenizer(lineTokenizer);
		lineMapper.setFieldSetMapper(suggestedPodcastFieldSetMapper());

		return lineMapper;
	}

	@Bean
	public SuggestedPodcastFieldSetMapper suggestedPodcastFieldSetMapper() {
		return new SuggestedPodcastFieldSetMapper();
	}

	/** configure the processor related stuff */
    @Bean
    public ItemProcessor&lt;SuggestedPodcast, SuggestedPodcast&gt; processor() {
        return new SuggestedPodcastItemProcessor();
    }

    @Bean
    public ItemWriter&lt;SuggestedPodcast&gt; writer() {
    	return new Writer();
    }
	// end::readerwriterprocessor[]

	@Bean
	public ProtocolListener protocolListener(){
		return new ProtocolListener();
	}

	@Bean
	public LogProcessListener logProcessListener(){
		return new LogProcessListener();
	}    

}</code>
</pre>

The `@EnableBatchProcessing` annotation adds many critical beans that support jobs and saves us configuration work. For example you will also be able to `@Autowired` some useful stuff into your context:

  * a `JobRepository` (bean name &#8220;jobRepository&#8221;)
  * a `JobLauncher` (bean name &#8220;jobLauncher&#8221;)
  * a `JobRegistry` (bean name &#8220;jobRegistry&#8221;)
  * a `PlatformTransactionManager` (bean name &#8220;transactionManager&#8221;)
  * a `JobBuilderFactory` (bean name &#8220;jobBuilders&#8221;) as a convenience to prevent you from having to inject the job repository into every job, as in the examples above
  * a `StepBuilderFactory` (bean name &#8220;stepBuilders&#8221;) as a convenience to prevent you from having to inject the job repository and transaction manager into every step

The first part focuses on the actual job configuration:

<pre>
  <code class="java">@Bean
public Job addNewPodcastJob(){
	return jobs.get("addNewPodcastJob")
			.listener(protocolListener())
			.start(step())
			.build();
}

@Bean
public Step step(){
	return stepBuilderFactory.get("step")
			.&lt;SuggestedPodcast,SuggestedPodcast&gt;chunk(1) //important to be one in this case to commit after every line read
			.reader(reader())
			.processor(processor())
			.writer(writer())
			.listener(logProcessListener())
			.faultTolerant()
			.skipLimit(10) //default is set to 0
			.skip(MySQLIntegrityConstraintViolationException.class)
			.build();
}</code>
</pre>

<p style="text-align: justify;">
  The first method defines a job and the second one defines a single step. As you&#8217;ve read in <a title="http://docs.spring.io/spring-batch/reference/html/domain.html" href="http://docs.spring.io/spring-batch/reference/html/domain.html" target="_blank">The Domain Language of Batch</a>,  jobs are built from steps, where each step can involve a reader, a processor, and a writer.
</p>

<p style="text-align: justify;">
  In the step definition, you define how much data to write at a time (in our case 1 record at a time). Next you specify the reader, processor and writer.
</p>

## <span id="5_Spring_Batchprocessing_units">5. Spring Batch processing units</span>

<p style="text-align: justify;">
  Most of the batch processing can be described as reading data, doing some transformation on it and then writing the result out. This mirrors somehow the <a title="http://en.wikipedia.org/wiki/Extract,_transform,_load" href="http://en.wikipedia.org/wiki/Extract,_transform,_load" target="_blank">Extract, Transform, Load (ETL)</a> process, in case you know more about that. Spring Batch provides three key interfaces to help perform bulk reading and writing: <span style="color: #333333;"> </span><code class="classname" style="color: #333333;">ItemReader</code><span style="color: #333333;">, </span><code class="classname" style="color: #333333;">ItemProcessor</code><span style="color: #333333;"> and </span><code class="classname" style="color: #333333;">ItemWriter</code><span style="color: #333333;">.</span>
</p>

### <span id="51_Readers">5.1. Readers</span>

ItemReader is an abstraction providing the mean to retrieve data from many different types of input: _flat files_, _xml files_, _database_, _jms_ etc., one item at a time. _See the <a title="http://docs.spring.io/spring-batch/trunk/reference/html/listOfReadersAndWriters.html" href="http://docs.spring.io/spring-batch/trunk/reference/html/listOfReadersAndWriters.html" target="_blank">Appendix A. List of ItemReaders and ItemWriters</a>_ for a complete list of available item readers.

In the Podcastpedia batch jobs I use the following specialized ItemReaders:

#### <span id="511_FlatFileItemReader">5.1.1. FlatFileItemReader</span>

<p style="text-align: justify;">
  which, as the name implies, <span style="color: #333333;">reads lines of data from a flat file that typically describe records with fields of data defined by fixed positions in the file or delimited by some special character (e.g. Comma)</span>. This type of <code>ItemReader</code> is being used in the first batch job, <em>addNewPodcastJob</em>. The input file used is named <em>suggested-podcasts.in</em>, resides in the classpath (<em>src/main/resources</em>) and looks something like the following:
</p>

<pre class="lang:default decode:true" title="Input file for FlatFileItemReader">FEED_URL; IDENTIFIER_ON_PODCASTPEDIA; CATEGORIES; LANGUAGE; MEDIA_TYPE; UPDATE_FREQUENCY; KEYWORDS; FB_PAGE; TWITTER_PAGE; GPLUS_PAGE; NAME_SUBMITTER; EMAIL_SUBMITTER
http://www.5minutebiographies.com/feed/; 5minutebiographies; people_society, history; en; Audio; WEEKLY; biography, biographies, short biography, short biographies, 5 minute biographies, five minute biographies, 5 minute biography, five minute biography; https://www.facebook.com/5minutebiographies; https://twitter.com/5MinuteBios; ; Adrian Matei; adrianmatei@gmail.com
http://notanotherpodcast.libsyn.com/rss; NotAnotherPodcast; entertainment; en; Audio; WEEKLY; Comedy, Sports, Cinema, Movies, Pop Culture, Food, Games; https://www.facebook.com/notanotherpodcastusa; https://twitter.com/NAPodcastUSA; https://plus.google.com/u/0/103089891373760354121/posts; Adrian Matei; adrianmatei@gmail.com</pre>

<p style="text-align: justify;">
  As you can see the first line defines the names of the &#8220;columns&#8221;, and the following lines contain the actual data (delimited by &#8220;;&#8221;), that needs translating to domain objects relevant in the context.
</p>

Let&#8217;s see now how to configure the `FlatFileItemReader`:

<pre>
  <code class="java">@Bean
public ItemReader&lt;SuggestedPodcast&gt; reader(){
	FlatFileItemReader&lt;SuggestedPodcast&gt; reader = new FlatFileItemReader&lt;SuggestedPodcast&gt;();
	reader.setLinesToSkip(1);//first line is title definition
	reader.setResource(new ClassPathResource("suggested-podcasts.in"));
	reader.setLineMapper(lineMapper());
	return reader;
}</code>
</pre>

<p style="text-align: justify;">
  You can specify, among other things, the input resource, the number of lines to skip, and a line mapper.
</p>

<h5 style="text-align: justify;">
  <span id="5111_LineMapper">5.1.1.1. LineMapper</span>
</h5>

<p style="text-align: justify;">
  The <code>LineMapper</code> is an interface for mapping lines (strings) to domain objects, typically used to map lines read from a file to domain objects on a per line basis.  For the Podcastpedia job I used the <code>DefaultLineMapper</code>, which is two-phase implementation consisting of tokenization of the line into a <code>FieldSet</code> followed by mapping to item:
</p>

<pre>
  <code class="java">@Bean
public LineMapper&lt;SuggestedPodcast&gt; lineMapper() {
	DefaultLineMapper&lt;SuggestedPodcast&gt; lineMapper = new DefaultLineMapper&lt;SuggestedPodcast&gt;();

	DelimitedLineTokenizer lineTokenizer = new DelimitedLineTokenizer();
	lineTokenizer.setDelimiter(";");
	lineTokenizer.setStrict(false);
	lineTokenizer.setNames(new String[]{"FEED_URL", "IDENTIFIER_ON_PODCASTPEDIA", "CATEGORIES", "LANGUAGE", "MEDIA_TYPE", "UPDATE_FREQUENCY", "KEYWORDS", "FB_PAGE", "TWITTER_PAGE", "GPLUS_PAGE", "NAME_SUBMITTER", "EMAIL_SUBMITTER"});

	BeanWrapperFieldSetMapper&lt;SuggestedPodcast&gt; fieldSetMapper = new BeanWrapperFieldSetMapper&lt;SuggestedPodcast&gt;();
	fieldSetMapper.setTargetType(SuggestedPodcast.class);

	lineMapper.setLineTokenizer(lineTokenizer);
	lineMapper.setFieldSetMapper(suggestedPodcastFieldSetMapper());

	return lineMapper;
}</code>
</pre>

  * the `DelimitedLineTokenizer`  splits the input String via the &#8220;;&#8221; delimiter.
  * if you set the `strict` flag to `false` then lines with less tokens will be tolerated and padded with empty columns, and lines with more tokens will simply be truncated.
  * the columns names from the first line are set `lineTokenizer.setNames(...);`
  * and the `fieldMapper` is set (line 14)

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong><br /> The <code>FieldSet</code> is an &#8220;i<span style="color: #474747;">nterface used by flat file input sources to encapsulate concerns of converting an array of Strings to Java native types. A bit like the role played by </span><code style="color: #474747;">ResultSet</code><span style="color: #474747;"> in JDBC, clients will know the name or position of strongly typed fields that they want to extract.</span>&#8220;
</p>

##### <span id="5112_FieldSetMapper">5.1.1.2. FieldSetMapper</span>

The `FieldSetMapper` is an interface that is used to map data obtained from a `FieldSet` into an object. Here&#8217;s my implementation which maps the fieldSet to the `SuggestedPodcast` domain object that will be further passed to the processor:

<pre>
  <code class="java">public class SuggestedPodcastFieldSetMapper implements FieldSetMapper&lt;SuggestedPodcast&gt; {

	@Override
	public SuggestedPodcast mapFieldSet(FieldSet fieldSet) throws BindException {

		SuggestedPodcast suggestedPodcast = new SuggestedPodcast();

		suggestedPodcast.setCategories(fieldSet.readString("CATEGORIES"));
		suggestedPodcast.setEmail(fieldSet.readString("EMAIL_SUBMITTER"));
		suggestedPodcast.setName(fieldSet.readString("NAME_SUBMITTER"));
		suggestedPodcast.setTags(fieldSet.readString("KEYWORDS"));

		//some of the attributes we can map directly into the Podcast entity that we'll insert later into the database
		Podcast podcast = new Podcast();
		podcast.setUrl(fieldSet.readString("FEED_URL"));
		podcast.setIdentifier(fieldSet.readString("IDENTIFIER_ON_PODCASTPEDIA"));
		podcast.setLanguageCode(LanguageCode.valueOf(fieldSet.readString("LANGUAGE")));
		podcast.setMediaType(MediaType.valueOf(fieldSet.readString("MEDIA_TYPE")));
		podcast.setUpdateFrequency(UpdateFrequency.valueOf(fieldSet.readString("UPDATE_FREQUENCY")));
		podcast.setFbPage(fieldSet.readString("FB_PAGE"));
		podcast.setTwitterPage(fieldSet.readString("TWITTER_PAGE"));
		podcast.setGplusPage(fieldSet.readString("GPLUS_PAGE"));

		suggestedPodcast.setPodcast(podcast);

		return suggestedPodcast;
	}

}</code>
</pre>

#### <span id="52_JdbcCursorItemReader">5.2. JdbcCursorItemReader</span>

<p style="text-align: justify;">
  In the second job, <em>notifyEmailSubscribersJob</em>, in the reader, I only read email subscribers from a single database table, but further in the processor a more detailed read(via JPA) is executed to retrieve all the new episodes of the podcasts the user subscribed to. This is a common pattern employed in the batch world. Follow this <a title="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" href="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" target="_blank">link</a> for more Common Batch Patterns.
</p>

<p style="text-align: justify;">
  For the initial read, I chose the <code>JdbcCursorItemReader</code>, which is a simple reader implementation that opens a JDBC cursor and continually retrieves the next row in the <code>ResultSet</code>:
</p>

<pre>
  <code class="java">@Bean
public ItemReader&lt;User&gt; notifySubscribersReader(){

	JdbcCursorItemReader&lt;User&gt; reader = new JdbcCursorItemReader&lt;User&gt;();
	String sql = "select * from users where is_email_subscriber is not null";

	reader.setSql(sql);
	reader.setDataSource(dataSource);
	reader.setRowMapper(rowMapper());		

	return reader;
}</code>
</pre>

Note I had to set the `sql`, the `datasource` to read from and a `RowMapper`.

##### <span id="521_RowMapper">5.2.1. RowMapper</span>

<p style="text-align: justify;">
  The <code>RowMapper</code> is an interface used by <code>JdbcTemplate</code> for mapping rows of a Result&#8217;set on a per-row basis. My implementation of this interface, , performs the actual work of mapping each row to a result object, but I don&#8217;t need to worry about exception handling:
</p>

<pre>
  <code class="java">public class UserRowMapper implements RowMapper&lt;User&gt; {

	@Override
	public User mapRow(ResultSet rs, int rowNum) throws SQLException {
		User user = new User();
		user.setEmail(rs.getString("email"));

		return user;
	}

}</code>
</pre>

### <span id="52_Writers"> 5.2. Writers</span>

<code class="classname">ItemWriter</code> is an abstraction that represents the output of a <code class="classname">Step</code>, one batch or chunk of items at a time. Generally, an item writer has no knowledge of the input it will receive next, only the item that was passed in its current invocation.

The writers for the two jobs presented are quite simple. They just use external services to send email notifications and post tweets on <a title="https://twitter.com/podcastpedia" href="https://twitter.com/podcastpedia" target="_blank">Podcastpedia&#8217;s account</a>. Here is the implementation of the `ItemWriter` for the first job &#8211; **addNewPodcast**:

<pre>
  <code class="java">package org.podcastpedia.batch.jobs.addpodcast;

import java.util.Date;
import java.util.List;

import javax.inject.Inject;
import javax.persistence.EntityManager;

import org.podcastpedia.batch.common.entities.Podcast;
import org.podcastpedia.batch.jobs.addpodcast.model.SuggestedPodcast;
import org.podcastpedia.batch.jobs.addpodcast.service.EmailNotificationService;
import org.podcastpedia.batch.jobs.addpodcast.service.SocialMediaService;
import org.springframework.batch.item.ItemWriter;
import org.springframework.beans.factory.annotation.Autowired;

public class Writer implements ItemWriter&lt;SuggestedPodcast&gt;{

	@Autowired
	private EntityManager entityManager;

	@Inject
	private EmailNotificationService emailNotificationService;

	@Inject
	private SocialMediaService socialMediaService;

	@Override
	public void write(List&lt;? extends SuggestedPodcast&gt; items) throws Exception {

		if(items.get(0) != null){
			SuggestedPodcast suggestedPodcast = items.get(0);

			//first insert the data in the database
			Podcast podcast = suggestedPodcast.getPodcast();

			podcast.setInsertionDate(new Date());
			entityManager.persist(podcast);
			entityManager.flush();

			//notify submitter about the insertion and post a twitt about it
			String url = buildUrlOnPodcastpedia(podcast);

			emailNotificationService.sendPodcastAdditionConfirmation(
					suggestedPodcast.getName(), suggestedPodcast.getEmail(),
					url);
			if(podcast.getTwitterPage() != null){
				socialMediaService.postOnTwitterAboutNewPodcast(podcast,
				url);				
			}					
		}

	}

	private String buildUrlOnPodcastpedia(Podcast podcast) {
		StringBuffer urlOnPodcastpedia = new StringBuffer(
				"https://github.com/Codingpedia/podcastpedia");
		if (podcast.getIdentifier() != null) {
			urlOnPodcastpedia.append("/" + podcast.getIdentifier());
		} else {
			urlOnPodcastpedia.append("/podcasts/");
			urlOnPodcastpedia.append(String.valueOf(podcast.getPodcastId()));
			urlOnPodcastpedia.append("/" + podcast.getTitleInUrl());
		}		
		String url = urlOnPodcastpedia.toString();
		return url;
	}

}</code>
</pre>

<p style="text-align: justify;">
  As you can see there&#8217;s nothing special here, except that the <code>write</code> method has to be overriden and this is where the injected external services <code>EmailNotificationService</code> and <code>SocialMediaService</code> are used to inform via email the podcast submitter about the addition to the podcast directory, and if a Twitter page was submitted a tweet will be posted on the <a title="Podcastpedia on Twitter" href="https://twitter.com/podcastpedia" target="_blank">Podcastpedia&#8217;s wall</a>. You can find detailed explanation on how to send email via Velocity and how to post on Twitter from Java in the following posts:
</p>

  * <a title="http://www.codingpedia.org/ama/how-to-compose-html-emails-in-java-with-spring-and-velocity/" href="http://www.codingpedia.org/ama/how-to-compose-html-emails-in-java-with-spring-and-velocity/" target="_blank">How to compose html emails in Java with Spring and Velocity</a>
  * <a title="http://www.codingpedia.org/ama/how-to-post-to-twittter-from-java-with-twitter4j-in-10-minutes/" href="http://www.codingpedia.org/ama/how-to-post-to-twittter-from-java-with-twitter4j-in-10-minutes/" target="_blank">How to post to Twittter from Java with Twitter4J in 10 minutes</a>

### <span id="53_Processors"> 5.3. Processors</span>

<p style="text-align: justify;">
  <code class="classname">ItemProcessor</code> is an abstraction that represents the business processing of an item. While the <code class="classname">ItemReader</code> reads one item, and the <code class="classname">ItemWriter</code> writes them, the <code class="classname">ItemProcessor</code> provides access to transform or apply other business processing. When using your own <code>Processors</code> you have to implement the <code>ItemProcessor&lt;I,O&gt;</code> interface, with its only method <code>O process(I item) throws Exception</code>, returning a potentially modified or a new item for continued processing. If the returned result is null, it is assumed that processing of the item should not continue.
</p>

While the processor of the first job requires a little bit of more logic, because I have to set the `etag` and `last-modified` header attributes, the feed attributes, episodes, categories and keywords of the podcast:

<pre>
  <code class="java">public class SuggestedPodcastItemProcessor implements ItemProcessor&lt;SuggestedPodcast, SuggestedPodcast&gt; {

	private static final int TIMEOUT = 10;

	@Autowired
	ReadDao readDao;

	@Autowired
	PodcastAndEpisodeAttributesService podcastAndEpisodeAttributesService;

	@Autowired
	private PoolingHttpClientConnectionManager poolingHttpClientConnectionManager;  

	@Autowired
	private SyndFeedService syndFeedService;

	/**
	 * Method used to build the categories, tags and episodes of the podcast
	 */
	@Override
	public SuggestedPodcast process(SuggestedPodcast item) throws Exception {

		if(isPodcastAlreadyInTheDirectory(item.getPodcast().getUrl())) {
			return null;
		}

		String[] categories = item.getCategories().trim().split("\\s*,\\s*");		

		item.getPodcast().setAvailability(org.apache.http.HttpStatus.SC_OK);

		//set etag and last modified attributes for the podcast
		setHeaderFieldAttributes(item.getPodcast());

		//set the other attributes of the podcast from the feed
		podcastAndEpisodeAttributesService.setPodcastFeedAttributes(item.getPodcast());

		//set the categories
		List&lt;Category&gt; categoriesByNames = readDao.findCategoriesByNames(categories);
		item.getPodcast().setCategories(categoriesByNames);

		//set the tags
		setTagsForPodcast(item);

		//build the episodes
		setEpisodesForPodcast(item.getPodcast());

		return item;
	}
	......
}</code>
</pre>

<p style="text-align: justify;">
  the processor from the second job uses the <a title="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" href="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" target="_blank">&#8216;Driving Query&#8217; approach</a>, where I expand the data retrieved from the Reader with another &#8220;JPA-read&#8221; and I group the items on podcasts with episodes so that it looks nice in the emails that I am sending out to subscribers:
</p>

<pre>
  <code class="java">@Scope("step")
public class NotifySubscribersItemProcessor implements ItemProcessor&lt;User, User&gt; {

	@Autowired
	EntityManager em;

	@Value("#{jobParameters[updateFrequency]}")
	String updateFrequency;

	@Override
	public User process(User item) throws Exception {

		String sqlInnerJoinEpisodes = "select e from User u JOIN u.podcasts p JOIN p.episodes e WHERE u.email=?1 AND p.updateFrequency=?2 AND"
				+ " e.isNew IS NOT NULL  AND e.availability=200 ORDER BY e.podcast.podcastId ASC, e.publicationDate ASC";
		TypedQuery&lt;Episode&gt; queryInnerJoinepisodes = em.createQuery(sqlInnerJoinEpisodes, Episode.class);
		queryInnerJoinepisodes.setParameter(1, item.getEmail());
		queryInnerJoinepisodes.setParameter(2, UpdateFrequency.valueOf(updateFrequency));		

		List&lt;Episode&gt; newEpisodes = queryInnerJoinepisodes.getResultList();

		return regroupPodcastsWithEpisodes(item, newEpisodes);

	}
	.......
}</code>
</pre>

<p class="note_normal">
  <strong>Note:</strong><br /> If you&#8217;d like to find out more how to use the Apache Http Client, to get the <code>etag</code> and <code>last-modified</code> headers, you can have a look at my post &#8211; <a title="http://www.codingpedia.org/ama/how-to-use-the-new-apache-http-client-to-make-a-head-request/" href="http://www.codingpedia.org/ama/how-to-use-the-new-apache-http-client-to-make-a-head-request/" target="_blank">How to use the new Apache Http Client to make a HEAD request</a>
</p>

## <span id="6_Execute_the_batch_application">6. Execute the batch application</span>

<p style="text-align: justify;">
  Batch processing can be embedded in web applications and WAR files, but I chose in the beginning the simpler approach that creates a standalone application, that can be started by the Java <code>main()</code> method:
</p>

<pre>
  <code class="java">package org.podcastpedia.batch;
//imports ...;

@ComponentScan
@EnableAutoConfiguration
public class Application {

    private static final String NEW_EPISODES_NOTIFICATION_JOB = "newEpisodesNotificationJob";
	private static final String ADD_NEW_PODCAST_JOB = "addNewPodcastJob";

	public static void main(String[] args) throws BeansException, JobExecutionAlreadyRunningException, JobRestartException, JobInstanceAlreadyCompleteException, JobParametersInvalidException, InterruptedException {

    	Log log = LogFactory.getLog(Application.class);

        SpringApplication app = new SpringApplication(Application.class);
        app.setWebEnvironment(false);
        ConfigurableApplicationContext ctx= app.run(args);
        JobLauncher jobLauncher = ctx.getBean(JobLauncher.class);

        if(ADD_NEW_PODCAST_JOB.equals(args[0])){
        	//addNewPodcastJob
        	Job addNewPodcastJob = ctx.getBean(ADD_NEW_PODCAST_JOB, Job.class);
        	JobParameters jobParameters = new JobParametersBuilder()
    		.addDate("date", new Date())
    		.toJobParameters();  

        	JobExecution jobExecution = jobLauncher.run(addNewPodcastJob, jobParameters);

        	BatchStatus batchStatus = jobExecution.getStatus();
        	while(batchStatus.isRunning()){
        		log.info("*********** Still running.... **************");
        		Thread.sleep(1000);
        	}
        	log.info(String.format("*********** Exit status: %s", jobExecution.getExitStatus().getExitCode()));
        	JobInstance jobInstance = jobExecution.getJobInstance();
        	log.info(String.format("********* Name of the job %s", jobInstance.getJobName()));

        	log.info(String.format("*********** job instance Id: %d", jobInstance.getId()));

        	System.exit(0);

        } else if(NEW_EPISODES_NOTIFICATION_JOB.equals(args[0])){
        	JobParameters jobParameters = new JobParametersBuilder()
    		.addDate("date", new Date())
    		.addString("updateFrequency", args[1])
    		.toJobParameters();  

        	jobLauncher.run(ctx.getBean(NEW_EPISODES_NOTIFICATION_JOB,  Job.class), jobParameters);   
        } else {
        	throw new IllegalArgumentException("Please provide a valid Job name as first application parameter");
        }

        System.exit(0);
    }

}</code>
</pre>

The best explanation for  `SpringApplication`-, `@ComponentScan`&#8211; and `@EnableAutoConfiguration`-magic you get from the source &#8211; Getting Started &#8211; Creating a Batch Service:

<div class="paragraph" style="color: #34302d;">
  <p style="font-weight: 400; color: #34302d; text-align: justify; padding-left: 30px;">
    &#8220;The <code style="color: #305cb5;">main()</code> method defers to the <a style="color: #5fa134;" href="http://docs.spring.io/spring-boot/docs/1.1.5.RELEASE/api/org/springframework/boot/SpringApplication.html"><code style="color: #305cb5;">SpringApplication</code></a> helper class, providing <code style="color: #305cb5;">Application.class</code> as an argument to its <code style="color: #305cb5;">run()</code> method. This tells Spring to read the annotation metadata from <code style="color: #305cb5;">Application</code> and to manage it as a component in the <a style="color: #5fa134;" href="http://spring.io/understanding/application-context">Spring application context</a>.
  </p>
</div>

<div class="paragraph" style="color: #34302d; padding-left: 30px;">
  <p style="font-weight: 400; color: #34302d; text-align: justify;">
    <span style="color: #34302d;">The <code style="color: #305cb5;">@ComponentScan</code> annotation tells Spring to search recursively through the <code>org.podcastpedia.batch</code></span><span style="color: #305cb5; font-family: monospace, serif; font-size: small;"><span style="line-height: 19.09090805053711px;"> </span></span><span style="color: #34302d;">package and its children for classes marked directly or indirectly with Spring’s </span><code style="color: #305cb5;">&lt;a style="color: #5fa134;" href="http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/stereotype/Component.html">@Component&lt;/a> </code><span style="color: #34302d;">annotation. This directive ensures that Spring finds and registers </span><code style="color: #305cb5;">BatchConfiguration</code><span style="color: #34302d;">, because it is marked with </span><code style="color: #305cb5;">@Configuration</code><span style="color: #34302d;">, which in turn is a kind of </span><code style="color: #305cb5;">@Component </code><span style="color: #34302d;">annotation.</span>
  </p>
</div>

<div class="paragraph" style="color: #34302d;">
  <p style="font-weight: 400; color: #34302d; text-align: justify; padding-left: 30px;">
    The <a style="color: #5fa134;" href="http://docs.spring.io/spring-boot/docs/1.1.5.RELEASE/api/org/springframework/boot/autoconfigure/EnableAutoConfiguration.html"><code style="color: #305cb5;">@EnableAutoConfiguration</code></a> annotation switches on reasonable default behaviors based on the content of your classpath. For example, it looks for any class that implements the <code style="color: #305cb5;">CommandLineRunner</code> interface and invokes its <code style="color: #305cb5;">run()</code> method.&#8221;
  </p>
</div>

<p style="text-align: justify;">
  Execution construction steps:
</p>

<li style="text-align: justify;">
  the <a title="http://docs.spring.io/spring-batch/trunk/apidocs/org/springframework/batch/core/launch/JobLauncher.html" href="http://docs.spring.io/spring-batch/trunk/apidocs/org/springframework/batch/core/launch/JobLauncher.html" target="_blank"><code>JobLauncher</code></a>, which is a simple interface for controlling jobs,  is retrieved from the ApplicationContext. Remember this is automatically made available via the <code>@EnableBatchProcessing</code> annotation.
</li>
  * now based on the first parameter of the application (`args[0]`), I will retrieve the corresponding <a title="http://docs.spring.io/spring-batch/trunk/apidocs/org/springframework/batch/core/Job.html" href="http://docs.spring.io/spring-batch/trunk/apidocs/org/springframework/batch/core/Job.html" target="_blank"><code>Job</code></a> from the `ApplicationContext`
  * then the <a title="http://docs.spring.io/spring-batch/apidocs/org/springframework/batch/core/JobParameters.html" href="http://docs.spring.io/spring-batch/apidocs/org/springframework/batch/core/JobParameters.html" target="_blank"><code>JobParameters</code></a> are prepared, where I use the current date &#8211; `.addDate("date", new Date())`, so that the job executions are always unique.
  * once everything is in place, the job can be executed: `JobExecution jobExecution = jobLauncher.run(addNewPodcastJob, jobParameters);`
  * you can use the returned `jobExecution` to gain access to `BatchStatus`, exit code, or job name and id.

<p class="note_normal">
  <strong style="color: #000000;">Note:</strong><span style="color: #000000;"> I highly recommend you read and understand the <a title="http://docs.spring.io/spring-batch/reference/html/metaDataSchema.html" href="http://docs.spring.io/spring-batch/reference/html/metaDataSchema.html" target="_blank">Meta-Data Schema for Spring Batch</a>. It will also help you better understand the Spring Batch Domain objects.</span>
</p>

### <span id="61_Running_the_application_on_dev_and_prod_environments">6.1. Running the application on dev and prod environments</span> {.note_normal}

<p class="note_normal" style="text-align: justify;">
  To be able to run the Spring Batch / Spring Boot application on different environments I make use of the Spring Profiles capability. By default the application runs with development data (database). But if I want the job to use the production database I have to do the following:
</p>

  * provide the following environment argument  `-Dspring.profiles.active=prod`
  * have the production database properties configured in the `application-prod.properties` file in the classpath, right besides the default `application.properties` file

## <span id="Summary"><span style="color: #000000;">Summary</span></span>

<p style="text-align: justify;">
  <span style="color: #000000;">In this tutorial we&#8217;ve learned how to configure a Spring Batch project with Spring Boot and Java configuration, how to use some of the most common readers in batch processing, how to configure some simple jobs, and how to start Spring Batch jobs from a main method.<br /> </span>
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:<br /> </strong>As I mentioned, I am fairly new to Spring Batch, and especially to Spring Boot and Spring Configuration with Java, so if you see any potential for improvement (code, job design etc.) please make a pull request or leave a comment below. Thanks a lot.
</p>

<h2 class="title" style="color: #000000;">
  <span id="Resources">Resources</span>
</h2>

### <span id="Source_Code_GitHub"><span id="Source_Code_8211_GitHub">Source Code – GitHub</span></span>

  * <a title="https://github.com/podcastpedia/podcastpedia-batch" href="https://github.com/podcastpedia/podcastpedia-batch" target="_blank">podcastpedia-batch </a>&#8211; project presented in this tutorial. Please make a pull request for any improvement proposals

### <span id="Web">Web</span>

  1. <a title="http://projects.spring.io/spring-batch/" href="http://projects.spring.io/spring-batch/" target="_blank">Spring Batch project</a>
      1. <a title="http://docs.spring.io/spring-batch/reference/html/" href="http://docs.spring.io/spring-batch/reference/html/" target="_blank">Spring Batch &#8211; Reference Documentation</a>
      2. <a title="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" href="http://docs.spring.io/spring-batch/trunk/reference/html/patterns.html" target="_blank">Common Batch Patterns</a>
      3. <a title="http://spring.io/guides/gs/batch-processing/" href="http://spring.io/guides/gs/batch-processing/" target="_blank">Creating a Batch Service</a>
  2. <a title="http://docs.spring.io/spring-batch/apidocs/" href="http://docs.spring.io/spring-batch/apidocs/" target="_blank">Spring Batch Api Docs</a>
  3. <a title="http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/" href="http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/" target="_blank">Spring Boot Reference Guide</a>
  4. <a title="https://blog.codecentric.de/en/2013/06/spring-batch-2-2-javaconfig-part-1-a-comparison-to-xml/" href="https://blog.codecentric.de/en/2013/06/spring-batch-2-2-javaconfig-part-1-a-comparison-to-xml/" target="_blank">CodeCentric Spring Batch Series</a> by <a title="https://twitter.com/TobiasFlohre" href="https://twitter.com/TobiasFlohre" target="_blank">Tobias Flohre</a>

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
