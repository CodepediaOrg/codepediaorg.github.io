---
id: 806
title: Spring MyBatis integration example
date: 2013-10-30T09:21:25+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=806
permalink: /ama/spring-mybatis-integration-example/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 4
dsq_thread_id:
  - 1917850980
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
fsb_social_google:
  - 7
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:49:"https://github.com/podcastpedia/podcastpedia-web/";s:11:"ribbon-type";i:10;}'
categories:
  - java
  - spring
tags:
  - framework
  - hibernate
  - iBatis
  - integration
  - MyBatis
  - persistence
---
<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Why_Mybatis">1. Why Mybatis?</a><ul>
        <li>
          <a href="#11_What_is_MyBatis">1.1. What is MyBatis?</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Spring_MyBatis_interaction">2. Spring MyBatis interaction</a><ul>
        <li>
          <a href="#21_What_is_MyBatis-Spring">2.1. What is MyBatis-Spring?</a>
        </li>
        <li>
          <a href="#22_Installation">2.2. Installation</a>
        </li>
        <li>
          <a href="#23_Spring_application_context_setup">2.3. Spring application context setup</a>
        </li>
        <li>
          <a href="#24_Injecting_Mappers">2.4. Injecting Mappers</a><ul>
            <li>
              <a href="#241_Service_Layer">2.4.1. Service Layer</a>
            </li>
            <li>
              <a href="#242_DAO_layer">2.4.2. DAO layer</a><ul>
                <li>
                  <a href="#2421_Register_mapper">2.4.2.1. Register mapper</a><ul>
                    <li>
                      <a href="#24211_With_XML_Config">2.4.2.1.1. With XML Config</a>
                    </li>
                  </ul>
                </li>
              </ul>
            </li>
          </ul>
        </li>

        <li>
          <a href="#25_MyBatis_configuration_XML">2.5. MyBatis configuration XML</a><ul>
            <li>
              <a href="#251_typeAliases">2.5.1. typeAliases</a>
            </li>
            <li>
              <a href="#252_mappers">2.5.2. mappers</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#26_Mapper_XML_Files">2.6. Mapper XML Files</a><ul>
            <li>
              <a href="#261_select">2.6.1. select</a>
            </li>
            <li>
              <a href="#262_Result_Map">2.6.2. Result Map</a><ul>
                <li>
                  <a href="#2621_id_result">2.6.2.1. id & result</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>

    <li>
      <a href="#3_Summary">3. Summary</a>
    </li>
    <li>
      <a href="#4_Resources">4. Resources</a>
    </li>
  </ul>
</div>

## <span id="1_Why_Mybatis">1. Why Mybatis?</span>

<p style="text-align: justify;">
  Short answer: simple, lightweight, open source, dynamic sql and sql control, previous iBATIS knowledge. Now let me elaborate a little bit on the subject. Back in the old days of Podcastmania.ro, see <a title="Story of Podcastpedia.org" href="http://www.codingpedia.org/ama/story-of-podcastpedia-org/" target="_blank">Story of Podcastpedia.org</a>, I used my own MVC like framwork based on servlets to develop the web application and plain old <a title="Wikipedia - JDBC" href="http://en.wikipedia.org/wiki/Java_Database_Connectivity" target="_blank">JDBC</a> to access the database. After &#8220;upgrading&#8221; to Spring MVC, I started using Spring&#8217;s <code>JdbcTemplate</code> for database access, which removed some of the boilerplate code. Later I got involved in projects where database access occured via iBATIS &#8211; Hibernate was there for a long time, but because of legacy reasons and no database normalization whatsoever, iBATIS was the optimal choice. By about the same time MyBatis had been just launched, so I read the documentation, did a pilot, liked it and switched from Spring&#8217;s <code>JdbcTemplate</code> to MyBatis. In the mean time I&#8217;ve been working on projects with Hibernate and JPA 2.0 with Hibernate used for persistence, so I&#8217;d say I have a pretty good overview on the most popular Java Persistence Frameworks. You have currently four major options:
</p>

  * JPA/Hibernate
  * myBatis (former iBatis)
  * Spring JDBC
  * JDBC

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

<!--more-->

<p style="text-align: justify;">
  Every approach has its pros and cons, but if I had to choose all over again a technology for Podcastpedia.org, I would choose MyBatis. You can find lots of resources on the web, lots of reviews, tutorials, pros and cons for using one technology over the other and so on &#8211; do your research and find out what is best persistence solution for your context. I also advise watching the following video, unfortunately the sound is pretty bad, but the content is really interesting :
</p>



### <span id="11_What_is_MyBatis">1.1. What is MyBatis?</span>

<p style="text-align: justify;">
  So now that I have made my choice, let&#8217;s see first what is MyBatis. Well according to the official website, &#8220;MyBatis is a first class persistence framework with support for custom SQL, stored procedures and advanced mappings. MyBatis eliminates almost all of the JDBC code and manual setting of parameters and retrieval of results. MyBatis can use simple XML or Annotations for configuration and map primitives, Map interfaces and Java POJOs (Plain Old Java Objects) to database records.&#8221;
</p>

Enough with talking, let&#8217;s focus now on the main topic of this post, which is to find out how Spring and MyBatis can interact.

<!--more-->

## <span id="2_Spring_MyBatis_interaction">2. Spring MyBatis interaction</span>

<p style="text-align: justify;">
  For the sake of simplicity, in this post I will present a simple example, which explains what needs to be implemented and configured to retrieve the <em>newest(recently updated) podcasts</em> from the database via MyBatis with Spring. You can experience the end result &#8220;live&#8221; by visiting the <a title="Last updated podcasts from Podcastpedia.org" href="https://github.com/Codingpedia/podcastpedia/" target="_blank">homepage of Podcastpedia.org</a>:
</p>

<div id="attachment_814" style="width: 310px" class="wp-caption alignnone">
  <a href="{{site.url}}/wp-content/uploads/2013/10/last_updated_podcasts.png"><img class="size-medium wp-image-814 " src="{{site.url}}/wp-content/uploads/2013/10/last_updated_podcasts-300x245.png" alt="last updated podcasts" width="300" height="245" srcset="{{site.url}}/wp-content/uploads/2013/10/last_updated_podcasts-300x245.png 300w, {{site.url}}/wp-content/uploads/2013/10/last_updated_podcasts-624x511.png 624w, {{site.url}}/wp-content/uploads/2013/10/last_updated_podcasts.png 1023w" sizes="(max-width: 300px) 100vw, 300px" /></a>

  <p class="wp-caption-text">
    Print screen home page Podcastpedia.org
  </p>
</div>

<p style="text-align: justify;">
  If you are familiar with iBATIS (predecessor of MyBatis), you might know that until version 3, the Spring Framework provided direct integration with iBATIS SQL Maps in terms of resource management, DAO implementation support, and transaction strategies. But by the time iBATIS became MyBatis, Spring 3 development was already over, and the Spring team did not want to release with code based on a non-released version of MyBatis, official Spring support would have to wait. Given the interest in Spring support for MyBatis, the MyBatis community decided it was time to reunite the interested contributors and add Spring integration as a community sub-project of MyBatis instead. This is how MyBatis-Spring project was born, which is also used throughout <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>
</p>

### <span id="21_What_is_MyBatis-Spring">2.1. What is MyBatis-Spring?</span>

<p style="text-align: justify;">
  MyBatis-Spring integrates MyBatis seamlessly with Spring. This library allows MyBatis to participate in Spring transactions, takes care of building MyBatis mappers and <code>SqlSession</code>s and inject them into other beans, translates MyBatis exceptions into Spring <code>DataAccessException</code>s, and finally, it lets you build your application <strong>code free</strong> of dependencies on MyBatis, Spring or MyBatis-Spring.
</p>

<div class="section">
  <h3>
    <span id="22_Installation">2.2. Installation</span>
  </h3>

  <p>
    To use the MyBatis-Spring module, you just need to include the <code> mybatis-spring-x.x.x.jar </code> file and its dependencies in the <a title="Wikipedia classpath java" href="http://en.wikipedia.org/wiki/Classpath_%28Java%29" target="_blank">classpath</a>.
  </p>

  <p>
    If you are using Maven just add the following dependency to your pom.xml:
  </p>

  <pre class="lang:default decode:true" title="MyBatis maven dependencies">&lt;!-- MyBatis integration --&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.mybatis&lt;/groupId&gt;
	&lt;artifactId&gt;mybatis-spring&lt;/artifactId&gt;
	&lt;version&gt;1.2.1&lt;/version&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
	&lt;groupId&gt;org.mybatis&lt;/groupId&gt;
	&lt;artifactId&gt;mybatis&lt;/artifactId&gt;
	&lt;version&gt;3.2.3&lt;/version&gt;
&lt;/dependency&gt;</pre>

  <h3>
    <span id="23_Spring_application_context_setup">2.3. Spring application context setup</span>
  </h3>

  <p>
    To use MyBatis with Spring you need at least two things defined in the Spring application context: an <code>SqlSessionFactory</code> and at least one mapper interface.
  </p>

  <p style="text-align: justify;">
    In MyBatis-Spring, an <code>SqlSessionFactoryBean</code> is used to create an <code>SqlSessionFactory</code>. Every MyBatis application centers around an instance of <code>SqlSessionFactory</code>. You use the <code>SqlSessionFactory</code> to create an <code>SqlSession</code>. Once you have a session, you use it to execute your mapped statements, commit or rollback connections and finally, when it is no longer needed, you close the session. With MyBatis-Spring you don&#8217;t need to use <code>SqlSessionFactory</code> directly because your beans can be injected with a thread safe <code>SqlSession</code> that automatically commits, rollbacks and closes the session based on Spring&#8217;s transaction configuration.
  </p>

  <p>
    To configure the factory bean, put the following in the Spring XML configuration file:
  </p>

  <pre class="lang:default decode:true" title="MyBatis application context configuration">&lt;bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"&gt;
	&lt;property name="dataSource" ref="dataSource" /&gt;
	&lt;property name="configLocation" value="classpath:config/mybatisV3-config.xml"/&gt;
&lt;/bean&gt;</pre>

  <p>
    Notice that the <code>SqlSessionFactory</code> requires a <code>DataSource</code>. This can be any <code>DataSource</code> and should be configured just like any other Spring database connection. For <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> the DataSource is configured via <a title="Wikipedia - JNDI" href="http://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface" target="_blank">JNDI</a>:
  </p>

  <pre class="lang:default decode:true" title="DataSource configuration in the application context">&lt;!-- ========================= DATASOURCE DEFINITION via JNDI ========================= --&gt;
&lt;!-- When resourceRef is true, the value of jndiName will be prepended with
server’s JNDI directory. Consequently, the actual name used will be
java:comp/env/jdbc/pcmDB.  --&gt;
&lt;bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean" scope="singleton"&gt;
	&lt;property name="jndiName" value="java:comp/env/jdbc/pcmDB" /&gt;
	&lt;property name="resourceRef" value="true" /&gt;
&lt;/bean&gt;</pre>

  <p style="text-align: justify;">
    See my post <a title="Tomcat JDBC Connection Pool configuration for production and development" href="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" target="_blank">Tomcat JDBC Connection Pool configuration for production and development</a>, to find out how the database resource is configured for production and development/testing environments.
  </p>

  <p>
    The <code>location</code> parameter specifies the location of the MyBatis config file. You will see in a coming section, <a href="#mybatis_config_file">MyBatis configuration XML,</a> how this file looks like.
  </p>

  <h3>
    <span id="24_Injecting_Mappers">2.4. Injecting Mappers</span>
  </h3>

  <p style="text-align: justify;">
    Rather than code data access objects (DAOs) manually using <code>SqlSessionDaoSupport</code> or <code>SqlSessionTemplate</code>, Mybatis-Spring can create a thread safe mapper that you can inject directly into other bean. I like to call mappers also DAOs.
  </p>

  <h4>
    <span id="241_Service_Layer">2.4.1. Service Layer</span>
  </h4>

  <p style="text-align: justify;">
    I will start at the Service Layer, which lies before the DAO layer. The service layer implementation class &#8211; <code>StartPageServiceImpl</code>, which loads the newest podcasts in the model to be displayed on the homepage, will be injected the <code>PodcastDao</code> mapper:
  </p>

  <pre class="lang:default decode:true">&lt;!-- =================== Service beans configuration ================== --&gt;
&lt;bean id="startPageService" class="org.podcastpedia.service.impl.StartPageServiceImpl"&gt;
	&lt;property name="podcastDao" ref="podcastDao"/&gt;
&lt;/bean&gt;</pre>

  <p>
    Once the mapper is injected it can then be used in the business logic:
  </p>

  <pre class="lang:java mark:5,7-9,13 decode:true" title="Service Layer snippet">public class StartPageServiceImpl implements StartPageService {

   private static final Integer NUMBER_OF_PODCASTS_IN_CHART = 5;
   private static Logger LOG = Logger.getLogger(StartPageServiceImpl.class);
   private PodcastDao podcastDao;

   public void setPodcastDao(PodcastDao podcastDao) {
	   this.podcastDao = podcastDao;
   }

   @Cacheable(value="newestAndRecommendedPodcasts", key="#root.method.name")
   public  List&lt;Podcast&gt; getNewestPodcasts() {
	   List&lt;Podcast&gt; newestPodcasts = podcastDao.getNewestPodcasts(NUMBER_OF_PODCASTS_IN_CHART);
	   for(Podcast p : newestPodcasts) {
		   Episode lastEpisode = episodeDao.getLastEpisodeForPodcast(p.getPodcastId());
		   p.setLastEpisode(lastEpisode);
	   }

	   return newestPodcasts;
   }
  ..............
}</pre>

  <p style="text-align: justify;">
    Notice that there are no <code>SqlSession</code> or MyBatis references at the service layer. Nor is there any need to create, open or close the session, MyBatis-Spring will take care of that. We&#8217;ll see in the next section how I implemented and configured the <code>PodcastDao</code>.
  </p>

  <p style="padding-left: 30px;">
    <em><strong>Note:</strong> If you want to find out how the <code>@Cacheable</code> annotation is configured, visit my post <a title="Codingpedia.org - Spring caching with Ehcache" href="http://www.codingpedia.org/ama/spring-caching-with-ehcache/" target="_blank">Spring caching with Ehcache</a></em>.
  </p>

  <h4>
    <span id="242_DAO_layer">2.4.2. DAO layer</span>
  </h4>

  <h5>
    <span id="2421_Register_mapper">2.4.2.1. Register mapper</span>
  </h5>

  <p style="text-align: justify;">
    You can register the mapper either using a classical XML configuration or the new 3.0+ Java Config (a.k.a. @Configuration). I prefer the classical way with XML configuration, as I like to hold the configuration as separate as possible from the code:
  </p>

  <h6>
    <span id="24211_With_XML_Config">2.4.2.1.1. With XML Config</span>
  </h6>

  <p>
    The mapper/dao is registered to Spring by including a <code>MapperFactoryBean</code> in the application context XML config:
  </p>

  <pre class="lang:default decode:true " title="MyBatis mapper/dao configuration">&lt;!-- =============== MyBATIS beans configuration ================== --&gt;
&lt;bean id="podcastDao" class="org.mybatis.spring.mapper.MapperFactoryBean"&gt;
   &lt;property name="sqlSessionFactory" ref="sqlSessionFactory"/&gt;
   &lt;property name="mapperInterface" value="org.podcastpedia.dao.PodcastDao" /&gt;
&lt;/bean&gt;</pre>

  <p style="text-align: justify;">
    The <code>Mapper BeanFactory</code> can be set up with a <code>SqlSessionFactory</code>, as here, or a pre-configured <code>SqlSessionTemplate</code>. You saw how the <code>SqlSessionFactory</code> was configured in the section Installation/Spring application context setup. You can also download the complete Spring configuration file for the DAO layer.
  </p>

  <p>
    The second parameter &#8211; <code>mapperInterface</code> &#8211; sets the mapper interface of the MyBatis mapper. Note that the mapper class specified <b>must</b> be an interface, NOT an actual implementation class:
  </p>

  <pre class="lang:java decode:true" title="PodcastDao interface code snippet">package org.podcastpedia.dao;

import java.util.Date;
import java.util.List;
import java.util.Map;

import org.podcastpedia.domain.Comment;
import org.podcastpedia.domain.Podcast;
import org.podcastpedia.domain.Tag;

/**
 * Interface for database access
 *
 * @author ama
 *
 */
public interface PodcastDao {

	  /**
	   * Returns the newest podcasts (ORDER BY last_updated DESC)
	   *
	   * @param numberOfPodcasts (number of podcasts to be returned)
	   * @return
	   */
	  public List&lt;Podcast&gt; getNewestPodcasts(Integer numberOfPodcasts);

      ....................
}</pre>

  <p style="text-align: justify;">
    If the <code>PodcastDao</code> had a corresponding MyBatis XML mapper file in the same classpath location as the mapper interface, it would have been parsed automatically by the <code>MapperFactoryBean</code>. But because they are different places in the classpath locations, the parameter had to be specified.
  </p>

  <p>
    Let&#8217;s see now how the sql statement is configured and implemented in MyBatis.
  </p>

  <h3>
    <span id="25_MyBatis_configuration_XML">2.5. MyBatis configuration XML</span>
  </h3>

  <p style="text-align: justify;">
    As mentioned in the beginning of the post the <code>SqlSessionFactoryBean</code> has the <code>configLocation</code> parameter that defines where the MyBatis configuration resides. Here is an extract from the configuration file that is relevant for the example presented here:
  </p>

  <pre class="lang:default decode:true" title="MyBatis configuration file snippet">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;
	&lt;typeAliases&gt;
		&lt;typeAlias type="org.podcastpedia.domain.Podcast" alias="Podcast"/&gt;
         .......
	&lt;/typeAliases&gt;
    &lt;mappers&gt;
        &lt;mapper resource="maps/PodcastMapper.xml" /&gt;
		........
    &lt;/mappers&gt;
&lt;/configuration&gt;</pre>

  <h4>
    <span id="251_typeAliases">2.5.1. typeAliases</span>
  </h4>

  <p style="text-align: justify;">
    A type alias is simply a shorter name for a Java type. It&#8217;s only relevant to the XML configuration and simply exists to reduce redundant typing of fully qualified classnames which include package names. As you&#8217;ll see in the following section, when referencing a org.podcastpedia.domain.Podcast object, I will just use &#8220;Podcast&#8221;.
  </p>

  <h4>
    <span id="252_mappers">2.5.2. mappers</span>
  </h4>

  <p style="text-align: justify;">
    Configuration elements which tell MyBatis where to find the mappers. Java doesn’t really provide any good means of auto-discovery in this regard, so the best way to do it is to simply tell MyBatis where to find the mapping files. You can use class path relative resource references, or literal, fully qualified url references (including <code>file:///</code> URLs):
  </p>

  <div id="attachment_821" style="width: 264px" class="wp-caption alignnone">
    <a href="{{site.url}}/wp-content/uploads/2013/10/MyBatis-files.png"><img class="size-medium wp-image-821" src="{{site.url}}/wp-content/uploads/2013/10/MyBatis-files-254x300.png" alt="MyBatis config files " width="254" height="300" srcset="{{site.url}}/wp-content/uploads/2013/10/MyBatis-files-254x300.png 254w, {{site.url}}/wp-content/uploads/2013/10/MyBatis-files.png 482w" sizes="(max-width: 254px) 100vw, 254px" /></a>

    <p class="wp-caption-text">
      MyBatis configuration file and mappers location
    </p>
  </div>

  <p style="padding-left: 30px;">
    <em><strong>Note:</strong> There are many other parameters you can set up in the configuration file for MyBatis. See the <a title="MyBatis configuration" href="http://mybatis.github.io/mybatis-3/configuration.html" target="_blank">Configuration</a></em> documentation page for more details.
  </p>

  <h3>
    <span id="26_Mapper_XML_Files">2.6. Mapper XML Files</span>
  </h3>

  <p style="text-align: justify;">
    The true power of MyBatis is in the Mapped Statements. This is where the magic happens. For all of their power, the Mapper XML files are relatively simple. Certainly if you were to compare them to the equivalent JDBC code, you would immediately see a savings of 95% of the code. MyBatis was built to focus on the SQL, and does its best to stay out of your way.
  </p>

  <p>
    Here I will present a snippet from the <code>PodcastMapper.xml</code> that shows how the <code>PodcastDao</code> interface method is mapped:
  </p>

  <pre class="lang:default mark:5,21 decode:true" title="MyBatis map snippet">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="org.podcastpedia.dao.PodcastDao"&gt;
	&lt;!--    result maps     --&gt;
	&lt;resultMap id="podcastsMap" type="Podcast" &gt;
		&lt;id column="podcast_id" property="podcastId"  /&gt;
		&lt;result column="url" property="url" /&gt;
		&lt;result column="rating" property="rating" /&gt;
		&lt;result column="numberRatings" property="number_ratings" /&gt;
		&lt;result column="number_visitors" property="numberOfVisitors" /&gt;
		&lt;result column="DESCRIPTION" property="description" /&gt;
		&lt;result column="PODCAST_IMAGE_URL" property="urlOfImageToDisplay" /&gt;
		&lt;result column="TITLE" property="title" /&gt;
		&lt;result column="last_episode_url" property="lastEpisodeMediaUrl" /&gt;
		&lt;result column="title_in_url" property="titleInUrl" /&gt;
		&lt;result column="publication_date" property="publicationDate"/&gt;
	&lt;/resultMap&gt;

	&lt;select id="getNewestPodcasts" resultMap="podcastsMap" parameterType="Integer"&gt;
		SELECT
			PODCAST_ID,
			URL,
			NUMBER_VISITORS,
			DESCRIPTION,
			PODCAST_IMAGE_URL,
			TITLE,
			last_episode_url,
			title_in_url,
			publication_date
		FROM
			podcasts
		WHERE
			availability=200
		ORDER BY publication_date DESC
			limit 0, #{value};
	&lt;/select&gt;
	.....
&lt;/mapper&gt;</pre>
</div>

Notice the mapper&#8217;s namespace value at line 5 &#8211; `org.podcastpedia.dao.PodcastDao` and the id of the `<select>` element at line 21. If you combine, it results `org.podcastpedia.dao.PodcastDao.getNewestPodcasts` which is exactly the method of the interface defined at the DAO layer.

#### <span id="261_select">2.6.1. select</span>

<p style="text-align: justify;">
  The <code>select</code> statement is one of the most popular elements that you&#8217;ll use in MyBatis. Putting data in a database isn&#8217;t terribly valuable until you get it back out, so most applications query far more than they modify the data. For every insert, update or delete, there is probably many selects. This is one of the founding principles of MyBatis, and is the reason so much focus and effort was placed on querying and result mapping.
</p>

<p style="text-align: justify;">
  The statement exposed here is called <code>getNewestPodcasts</code> (same as corresponding method in the <code>PodcastDao</code> interface), takes a parameter of type <code>Integer</code> (same as input of the corresponding method in <code>PodcastDao</code> interface) and has a <code>resultMap</code> parameter of value <code>"podcastsMap"</code>, which is a named reference to an external resultMap.
</p>

#### <span id="262_Result_Map">2.6.2. Result Map</span>

<p style="text-align: justify;">
  The <code>resultMap</code> element is the most important and powerful element in MyBatis. It&#8217;s what allows you to do away with 90% of the code that JDBC requires to retrieve data from <code>ResultSet</code>s, and in some cases allows you to do things that JDBC does not even support. In fact, to write the equivalent code for something like a join mapping for a complex statement could probably span thousands of lines of code. The design of the <code>ResultMap</code>s is such that simple statements don&#8217;t require explicit result mappings at all, and more complex statements require no more than is absolutely necessary to describe the relationships.
</p>

<p style="text-align: justify;">
  Let&#8217;s talk a little about the structure of the resultMap. The top element has an <code>id</code>(=&#8221;podcastMap&#8221;) which uniquely identifies the <code>select</code> in the mapper (namespace), and is referenced in the <strong>select</strong>, and a <code>type</code>(=&#8221;Podcast&#8221;), which is a fully qualified Java class name, or a type alias (this is the case here, <code>"Podcast"</code> being the type alias defined in the MyBatis configuration file)
</p>

<h5 style="text-align: justify;">
  <span id="2621_id_result">2.6.2.1. id & result</span>
</h5>

<pre class="lang:default decode:true" title="id & result of resultMap">&lt;id column="podcast_id" property="podcastId"  /&gt;
&lt;result column="url" property="url" /&gt;</pre>

<p style="text-align: justify;">
  These are the most basic of result mappings. Both <code>id</code>, and <code>column</code> map a single column value to a single property or field of a simple data type (String, int, double, Date, etc.).
</p>

<p style="text-align: justify;">
  The only difference between the two is that <code>id</code> will flag the result as an identifier property to be used when comparing object instances. This helps to improve general performance, but especially performance of caching and nested result mapping (i.e. join mapping).
</p>

<p style="text-align: justify;">
  What happens in this map is that some of the columns of the PODCASTS table are mapped to the properties of the Podcast bean:
</p>

  * the `column` attribute represents the column name from the database, or the aliased column label &#8211; this is the same string that would normally be passed to `resultSet.getString(columnName)`.
<li style="text-align: justify;">
  the <code>property</code> attribute represents the field or property to map the column result to. If a matching JavaBeans property exists for the given name, then that will be used. Otherwise, MyBatis will look for a field of the given name. In both cases you can use complex property navigation using the usual dot notation. For example, you can map to something simple like: <code>username</code>, or to something more complicated like: <code>address.street.number</code>.
</li>

<p style="text-align: justify;">
  Believe it or not, that&#8217;s it. It might seem like a lot just do a simple SELECT, but once you learn the basics it becomes really simple and you get some powerful stuff, some of which I will present in a future post. And also is important to mention that the learning curve is pretty small.
</p>

<h2 style="text-align: justify;">
  <span id="3_Summary">3. Summary</span>
</h2>

Well, that&#8217;s it. You&#8217;ve learned about MyBatis, how it integrates with Spring via MyBatis-Spring, how to configure MyBatis in Spring&#8217;s application context and how to use it in a Spring application.


<p class="note_normal">
<img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>


## <span id="4_Resources">4. Resources</span>

  1. <a title="MyBatis" href="http://mybatis.github.io/mybatis-3/" target="_blank">MyBatis</a>
  2. <a title="MyBatis-Spring" href="http://mybatis.github.io/spring/index.html" target="_blank">MyBatis-Spring</a>
  3. <a title="Data access with JDBC" href="http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/jdbc.html" target="_blank">Spring Data Access with JDBC </a>
  4. <a title="MyBatis blog" href="http://blog.mybatis.org/" target="_blank">MyBatis-Blog</a>

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
