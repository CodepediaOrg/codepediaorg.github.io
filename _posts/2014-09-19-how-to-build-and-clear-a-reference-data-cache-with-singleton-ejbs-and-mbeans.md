---
id: 1836
title: How to build and clear a reference data cache with singleton EJBs and MBeans
date: 2014-09-19T16:09:15+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=1836
permalink: /ama/how-to-build-and-clear-a-reference-data-cache-with-singleton-ejbs-and-mbeans/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 5
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3033422226
categories:
  - Java EE
tags:
  - cache
  - cahing
  - eclipselink
  - ejb
  - flush
  - java enterprise
  - jave enterprise edition
  - jee
  - mbean
  - mxbean
  - reference data
  - rest
  - singleton
  - web services
---
<p style="text-align: justify;">
  In one of my projects I had a requirement to load reference data from several sources in a Java EE 6 WebLogic environment, with EclipseLink as <a title="http://en.wikipedia.org/wiki/Object-relational_mapping" href="http://en.wikipedia.org/wiki/Object-relational_mapping" target="_blank">ORM</a> framework. Since I couldn&#8217;t find an annotation in the Java EE world comparable to the sweet <a title="http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/cache/annotation/Cacheable.html" href="http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/cache/annotation/Cacheable.html" target="_blank">@Cacheable</a> from Spring YET, I had to write my &#8220;own&#8221; caching solution. Although reference data barely changes over time, one extra requirement was to be able to clear the cache from exterior. So here it goes&#8230;<!--more-->
</p>

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Cache">1. Cache</a><ul>
        <li>
          <a href="#11_Interface">1.1. Interface</a>
        </li>
        <li>
          <a href="#12_Implementation">1.2. Implementation</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#2_Flushing_the_cache">2. Flushing the cache</a><ul>
        <li>
          <a href="#21_MBean">2.1. MBean</a><ul>
            <li>
              <a href="#211_Interface">2.1.1. Interface</a>
            </li>
            <li>
              <a href="#212_Implementation">2.1.2. Implementation</a>
            </li>
          </ul>
        </li>

        <li>
          <a href="#22_Rest_service_call">2.2. Rest service call</a>
        </li>
      </ul>
    </li>

    <li>
      <a href="#Resources">Resources</a><ul>
        <li>
          <a href="#Web">Web</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

## <span id="1_Cache">1. Cache</span>

This was supposed to be a read-only cache with the possibility to flush it from exterior. I wanted to have the cache as a sort of a wrapper on the service providing the actual reference data for the application &#8211; AOP style with code 🙂

### <span id="11_Interface">1.1. Interface</span>

<pre class="lang:java decode:true" title="Simple cache interface for reference data">@Local
public interface ReferenceDataCache {

	/**
	 * Returns all reference data required in the application
	 */
	ReferenceData getReferenceData();

	/**
	 * evict/flush all data from cache
	 */
	void evictAll();
}</pre>

The caching functionality defines two simple methods:

  * `getReferenceData()` &#8211; which caches the reference data gathered behind the scenes from all the different sources
  * `evictAll()` &#8211; method called to completely clear the cache

### <span id="12_Implementation">1.2. Implementation</span>

<pre class="lang:java decode:true" title="Simple reference data cache implementation with @Singleton">@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)
@Singleton
public class ReferenceDataCacheBean implements ReferenceDataCache {

	private static final String ALL_REFERENCE_DATA_KEY = "ALL_REFERENCE_DATA";

	private ConcurrentHashMap&lt;String, Object&gt; refDataCache = null;

	@EJB
	ReferenceDataService referenceDataService;

	@PostConstruct
	public void initialize(){
		this.refDataCache = new ConcurrentHashMap&lt;&gt;();
	}

	@Override
	@Lock(LockType.READ)
	public ReferenceData getReferenceData() {
		if(refDataCache.containsKey(ALL_REFERENCE_DATA_KEY)){			
			return refDataCache.get(ALL_REFERENCE_DATA_KEY);
		} else {
			ReferenceData referenceData = referenceDataService.getReferenceData();
			refDataCache.put(ALL_REFERENCE_DATA_KEY, referenceData);

			return referenceData;
		}		
	}

	@Override
	public void evictAll() {
		refDataCache.clear(); 		
	}
	..........
}</pre>

**Note:**

<li style="text-align: justify;">
  <code>@Singleton</code> &#8211; probably the most important line of code in this class. This annotation specifies that there will be exactly one singleton of this type of bean in the application. This bean can be invoked concurrently by multiple threads. It comes also with a <a title="http://docs.oracle.com/javaee/5/api/javax/annotation/PostConstruct.html" href="http://docs.oracle.com/javaee/5/api/javax/annotation/PostConstruct.html" target="_blank"><code>@PostConstruct</code></a> annotation. This annotation is used on a method that needs to be executed after dependency injection is done to perform any initialization &#8211; in our case is to initialize the &#8220;cache&#8221;(hash map)
</li>
<li style="text-align: justify;">
  the <code>@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)</code> declares a singleton session bean&#8217;s concurrency management type. By default it is set to <code>Container</code>. I use it here only to highlight its existence. The other option <a title="http://docs.oracle.com/javaee/6/api/javax/ejb/ConcurrencyManagementType.html#BEAN" href="http://docs.oracle.com/javaee/6/api/javax/ejb/ConcurrencyManagementType.html#BEAN" target="_blank"><code>ConcurrencyManagementType.BEAN</code></a> specifies that the bean developer is responsible for managing concurrent access to the bean instance.
</li>
<li style="text-align: justify;">
  the actual &#8220;cache&#8221; is a <a title="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ConcurrentHashMap.html" href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ConcurrentHashMap.html" target="_blank"><code>ConcurrentHashMap</code></a> which has <code>String</code> based keys and stores <code>Object</code>s. This is being held in memory due to the singleton nature of the bean
</li>
<li style="text-align: justify;">
  the injected <code>ReferenceDataService</code> is a <code>@Stateless</code> <code>@EJB</code> that, behind the scenes,  gathers the reference data from the different sources
</li>
<li style="text-align: justify;">
  the getReferenceData() method implementation is very simple &#8211; it checks whether the <code>ConcurrentHashMap</code> has an entry with the String key specified as constant &#8220;<code>ALL_REFERENCE_DATA</code>&#8220;. If so this will be retrieved from memory, otherwise will be loaded by the service bean
</li>
<li style="text-align: justify;">
  the <code>@Lock(LockType.READ)</code> specifies the concurrency lock type for singleton beans with container-managed concurrency. When set to <code>LockType.READ</code>, it enforces the method to permit full concurrent access to it (assuming no write locks are held). This is exactly what I wanted, as I only need to do read operations. The other more conservative option <code>@Lock(LockType.WRITE)</code>, which is the DEFAULT by the way, enforces exclusive access to the bean instance. This should make the method slower in a highly concurrent environment&#8230;
</li>
<li style="text-align: justify;">
  the <code>evictAll()</code> method, just removes all the elements from the hash map
</li>

<h2 style="text-align: justify;">
  <span id="2_Flushing_the_cache">2. Flushing the cache</span>
</h2>

<p style="text-align: justify;">
  The second part of this post will deal with the possibilities of clearing the cache. Since the cache implementation is an enterprise java bean, we can call it either from an MBean or, why not, from a web service.
</p>

<h3 style="text-align: justify;">
  <span id="21_MBean">2.1. MBean</span>
</h3>

<p style="text-align: justify;">
  If you are new to Java Management Extensions (JMX) , <em>which is a Java technology that supplies tools for managing and monitoring applications, system objects, devices (e.g. printers) and service oriented networks. Those resources are represented by objects called MBeans (for Managed Bean)</em>, I highly recommend you start with this tutorial <a title="http://docs.oracle.com/javase/tutorial/jmx/" href="http://docs.oracle.com/javase/tutorial/jmx/" target="_blank">Trail: Java Management Extensions (JMX)</a>
</p>

#### <span id="211_Interface">2.1.1. Interface</span>

The method exposed will only allow the reset of the cache via JMX:

<pre class="lang:java decode:true" title="CacheRest MBean">@MXBean
public interface CacheResetMXBean {
	void resetReferenceDataCache();
}</pre>

<p style="padding-left: 30px; text-align: justify;">
  <em>&#8220;<span style="color: #000000;">An </span>MXBean<span style="color: #000000;"> is a type of MBean that references only a predefined set of data types. In this way, you can be sure that your MBean will be usable by any client, including remote clients, without any requirement that the client have access to model-specific classes representing the types of your MBeans. MXBeans provide a convenient way to bundle related values together, without requiring clients to be specially configured to handle the bundles.</span>&#8221; [4]</em>
</p>

#### <span id="212_Implementation">2.1.2. Implementation</span>

<pre class="lang:java decode:true" title="MBean implementation of CacheReset">@Singleton
@Startup
public class CacheReset implements CacheResetMXBean {

	private MBeanServer platformMBeanServer;
    private ObjectName objectName = null;

	@EJB
	ReferenceDataCache referenceDataCache;

    @PostConstruct
    public void registerInJMX() {
        try {
        	objectName = new ObjectName("org.codingpedia.simplecacheexample:type=CacheReset");
            platformMBeanServer = ManagementFactory.getPlatformMBeanServer();

            //unregister the mbean before registerting again
            Set&lt;ObjectName&gt; existing = platformMBeanServer.queryNames(objectName, null);
            if(existing.size() &gt; 0){
            	platformMBeanServer.unregisterMBean(objectName);
            }

            platformMBeanServer.registerMBean(this, objectName);
        } catch (Exception e) {
            throw new IllegalStateException("Problem during registration of Monitoring into JMX:" + e);
        }
    }

	@Override
	public void resetReferenceDataCache() {
		referenceDataCache.evictAll();

	}

}</pre>

** Note: **

  * as mentioned the implementation only calls the `evictAll()` method of the injected singleton bean described in the previous section
  * the bean is also defined as `@Singleton`
  * the `@Startup` annotation causes the bean to be instantiated by the container when the application starts &#8211; **eager initialization**
  * I use again the `@PostConstruct` functionality. Here **this** bean is registered in JMX, checking before if the `ObjectName` is used to remove it if so&#8230;

<h3 style="text-align: justify;">
  <span id="22_Rest_service_call">2.2. Rest service call</span>
</h3>

I&#8217;ve also built in the possibility to clear the cache by calling a REST resource. This happends when you execute a HTTP POST on the (rest-context)/reference-data/flush-cache:

<pre class="lang:java decode:true " title="Rest calls on Reference data cache">@Path("/reference-data")
public class ReferenceDataResource {

	@EJB
	ReferenceDataCache referenceDataCache;

        @POST
	@Path("flush-cache")
	public Response flushReferenceDataCache() {
		referenceDataCache.evictAll();

		return Response.status(Status.OK).entity("Cache successfully flushed").build();
	}

	@GET
	@Produces({ MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML })
	public Response getReferenceData(@QueryParam("version") String version) {
		ReferenceData referenceData = referenceDataCache.getReferenceData();				

		if(version!=null && version.equals(referenceData.getVersion())){
			return Response.status(Status.NOT_MODIFIED).entity("Reference data was not modified").build();				
		} else {
			return Response.status(Status.OK)
					.entity(referenceData).build();				
		}
	}
}</pre>

<p style="text-align: justify;">
  Notice the existence of the version query parameter in the <code>@GET</code> <code>getReferenceData(...)</code> method.  This represents a hash on the reference data and if it hasn&#8217;t modified the client will receive a <em>304 Not Modified HTTP Status</em>. This is a nice way to spare some bandwidth, especially if you have mobile clients. See my post <a title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>, for a detailed discussion around REST services design and implementation.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> In a clustered environment, you need to call resetCache(&#8230;) on each JVM where the application is deployed, when the reference data changes.
</p>

<p style="text-align: justify;">
  Well, that&#8217;s it. In this post we&#8217;ve learned how to build a simple cache with Java EE annotations :). Of course you can easily extend the cache functionality to offer more granular access/clearing to cached objects. Don&#8217;t forget to use <code>LockType.WRITE</code> for the clear methods in this case&#8230;
</p>

<h2 class="title" style="color: #000000;">
  <span id="Resources"><span id="Resources">Resources</span></span>
</h2>

### <span id="Web">Web</span>

  1. <a title="http://www.adam-bien.com/roller/abien/entry/singleton_the_perfect_cache_facade" href="http://www.adam-bien.com/roller/abien/entry/singleton_the_perfect_cache_facade" target="_blank">@Singleton &#8211; the perfect cache facade</a> by Adam Bien
  2. <a title="http://www.adam-bien.com/roller/abien/entry/singleton_the_simplest_possible_jmx" href="http://www.adam-bien.com/roller/abien/entry/singleton_the_simplest_possible_jmx" target="_blank">@Singleton &#8211; the simplest possible JMX MXBean </a>by Adam Bien
  3. <a title="http://tomee.apache.org/singleton-beans.html" href="http://tomee.apache.org/singleton-beans.html" target="_blank">Tomee &#8211; Singleton Beans</a>
  4. <a title="http://docs.oracle.com/javase/tutorial/jmx/" href="http://docs.oracle.com/javase/tutorial/jmx/" target="_blank">Trail: Java Management Extensions (JMX)</a>
  5. <a title="http://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html" href="http://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html" target="_blank">Spring Cache Abstraction</a>

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
