---
id: 2084
title: How to build and clear a reference data cache with singleton EJBs, Ehcache and MBeans
date: 2014-11-07T16:15:42+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=2084
permalink: /ama/how-to-build-and-clear-a-reference-data-cache-with-singleton-ejbs-ehcache-and-mbeans/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 2
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3201795005
categories:
  - java
  - Java EE
tags:
  - cache
  - caching
  - ehcache
  - ejb
  - java enterprise
  - singleton
---
<p style="text-align: justify;">
  In this post I will present how to build a simple reference data cache in Java EE, using singleton EJBs and <a title="http://ehcache.org/" href="http://ehcache.org/" target="_blank">Ehcache</a>. The cache will reset itself after a given period of time, and can be cleared &#8220;manually&#8221; by calling a REST endpoint or a MBean method. This post actually builds on a previous post <a title="http://www.codingpedia.org/ama/how-to-build-and-clear-a-reference-data-cache-with-singleton-ejbs-and-mbeans/" href="http://www.codingpedia.org/ama/how-to-build-and-clear-a-reference-data-cache-with-singleton-ejbs-and-mbeans/" target="_blank">How to build and clear a reference data cache with singleton EJBs and MBeans</a>; the only difference is that instead of the storing of the data in a <code>ConcurrentHashMap&lt;String, Object&gt;</code> I will be using an Ehcache cache, and the cache is able to renew itself by Ehcache means.<!--more-->
</p>

## <span id="1_Cache">1. Cache</span>

This was supposed to be a read-only cache with the possibility to flush it from exterior. I wanted to have the cache as a sort of a wrapper on the service providing the actual reference data for the application – AOP style with code <img class="wp-smiley" src="http://www.codingpedia.org/wp-includes/images/smilies/icon_smile.gif" alt=":)" />

### <span id="11_Interface">1.1. Interface</span>

<pre class="lang:java decode:true " title="Simple Interface for Reference Data">@Local
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

  * `getReferenceData()` – which caches the reference data gathered behind the scenes from all the different sources
  * `evictAll()` – method called to completely clear the cache

### 1.2. Implementation

<pre class="lang:java decode:true" title="Simple reference data cache implementation with Ehcache">@ConcurrencyManagement(ConcurrencyManagementType.CONTAINER)
@Singleton
public class ReferenceDataCacheBean implements ReferenceDataCache {

	private static final String ALL_REFERENCE_DATA_KEY = "ALL_REFERENCE_DATA";

	private static final int CACHE_MINUTES_TO_LIVE = 100;

	private CacheManager cacheManager;

	private Cache refDataEHCache = null; 	

	@EJB
	ReferenceDataLogic referenceDataService;

	@PostConstruct
	public void initialize(){		

		cacheManager = CacheManager.getInstance();
		CacheConfiguration cacheConfiguration = new CacheConfiguration("referenceDataCache", 1000);
		cacheConfiguration.setTimeToLiveSeconds(CACHE_MINUTES_TO_LIVE * 60);

		refDataEHCache = new Cache(cacheConfiguration );
		cacheManager.addCache(refDataEHCache);
	}

	@Override
	@Lock(LockType.READ)
	public ReferenceData getReferenceData() {
		Element element = refDataEHCache.get(ALL_REFERENCE_DATA_KEY);

		if(element != null){
			return (ReferenceData) element.getObjectValue();
		} else {
			ReferenceData referenceData = referenceDataLogic.getReferenceData();

			refDataEHCache.putIfAbsent(new Element(ALL_REFERENCE_DATA_KEY, referenceData));

			return referenceData;
		}		
	}

	@Override
	public void evictAll() {
		cacheManager.clearAll();
	}
	...........
}

</pre>

<strong style="font-weight: bold;">Note:</strong>

  * `@Singleton` – probably the most important line of code in this class. This annotation specifies that there will be exactly one singleton of this type of bean in the application. This bean can be invoked concurrently by multiple threads.

Let&#8217;s break now the code into the different parts:

#### 1.2.1. Cache initialization

<p class="title" style="color: #000000; text-align: justify;">
  The <code>@PostConstruct</code> annotation is used on a method that needs to be executed after dependency injection is done, to perform any initialization – in our case is to create and initialize the (eh)cache.
</p>

<pre class="lang:java decode:true" title="Ehcache initialization">@PostConstruct
	public void initialize(){		

		cacheManager = CacheManager.create();

		CacheConfiguration cacheConfiguration = new CacheConfiguration("referenceDataCache", 1000);
		cacheConfiguration.setTimeToLiveSeconds(CACHE_MINUTES_TO_LIVE * 60);

		refDataEHCache = new Cache(cacheConfiguration );
		cacheManager.addCache(refDataEHCache);
	}</pre>

<p style="text-align: justify;">
  <strong>Note:</strong> Only one method can be annotated with this annotation.
</p>

<p style="text-align: justify;">
  All usages of Ehcache start with the creation of a <code>CacheManager</code>, which is a container for <code>Ehcache</code>s that maintain all aspects of their lifecycle. I use the <code>CacheManager.create()</code> method, which is a factory method to create a singleton CacheManager with default config, or return it if it exists:
</p>

<pre class="lang:java decode:true ">cacheManager = CacheManager.create();</pre>

<p style="text-align: justify;">
  I&#8217;ve built then a  <code>CacheConfiguration</code> object by providing the name of the cache (&#8220;referenceDataCache&#8221;) and number of  the maximum number of elements in memory (<code>maxEntriesLocalHeap</code>), before they are evicted (0 == no limit), and finally I set the default amount of time to live for an element from its creation date:
</p>

<pre class="lang:java decode:true ">CacheConfiguration cacheConfiguration = new CacheConfiguration("referenceDataCache", 1000);
cacheConfiguration.setTimeToLiveSeconds(CACHE_MINUTES_TO_LIVE * 60);</pre>

<p style="text-align: justify;">
  Now, with the help of the CacheConfiguration object I <strong>programmatically</strong> create my reference data cache and add to the CacheManager. Note that Caches are not usable until they have been added to a CacheManager:
</p>

<pre class="lang:java decode:true ">refDataEHCache = new Cache(cacheConfiguration );
cacheManager.addCache(refDataEHCache);</pre>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong>  You can also create the caches in a declarative way: when the CacheManager is created, it creates caches found in the configuration. You can create CacheManager by specifying the path of a configuration file, from a configuration in the classpath, from a configuration in an InputStream or by havind the default ehcache.xml file in your classpath. Take a look at <a title="http://ehcache.org/documentation/2.8/code-samples" href="http://ehcache.org/documentation/2.8/code-samples" target="_blank">Ehcache code samples</a> for more information.
</p>

#### 1.2.2. Get data from cache

<pre class="lang:java decode:true ">@Override
@Lock(LockType.READ)
public ReferenceData getReferenceData() {
	Element element = refDataEHCache.get(ALL_REFERENCE_DATA_KEY);

	if(element != null){
		return (ReferenceData) element.getObjectValue();
	} else {
		ReferenceData referenceData = referenceDataLogic.getReferenceData();

		refDataEHCache.put(new Element(ALL_REFERENCE_DATA_KEY, referenceData));

		return referenceData;
	}		
}</pre>

<p style="text-align: justify;">
  First I try to get the element from the cache based on its key, and if it&#8217;s present in the cache (<code>==null</code>), then if will be received from the service class and placed in cache for future requests.
</p>

<p style="text-align: justify;">
  <strong>Note: </strong><br /> The <code>@Lock(LockType.READ)</code> specifies the concurrency lock type for singleton beans with container-managed concurrency. When set to <code>LockType.READ</code>, it enforces the method to permit full concurrent access to it (assuming no write locks are held). This is exactly what I wanted, as I only need to do read operations. The other more conservative option <code>@Lock(LockType.WRITE)</code>, which is the DEFAULT by the way, enforces exclusive access to the bean instance. This should make the method slower in a highly concurrent environment…
</p>

<h4 style="text-align: justify;">
  1.2.3. Clear the cache
</h4>

<pre class="lang:java decode:true" title="Clear cache">@Override
public void evictAll() {
	cacheManager.clearAll();
}</pre>

<p style="text-align: justify;">
  The <code>clearAll()</code> method of the CacheManager clears the contents of all caches in the CacheManager, but without removing any caches. I just used it here for simplicity and because I only have one cache I need to refresh.
</p>

<p class="note_normal" style="text-align: justify;">
  <strong>Note:</strong> If you have several caches, that is several cache-names, and want to clear only one you need to use the <code>CacheManager.clearAllStartingWith(String prefix)</code>, which clears the contents of all caches in the CacheManager with a name starting with the prefix, but without removing them.
</p>

## <span id="2_Flushing_the_cache">2. How to trigger flusing the cache</span>

<p style="text-align: justify;">
  The second part of this post will deal with the possibilities of clearing the cache. Since the cache implementation is an enterprise java bean, we can call it either from an MBean or, why not, from a web service.
</p>

### <span id="21_MBean">2.1. MBean</span>

<p style="text-align: justify;">
  If you are new to Java Management Extensions (JMX) , <em>which is a Java technology that supplies tools for managing and monitoring applications, system objects, devices (e.g. printers) and service oriented networks. Those resources are represented by objects called MBeans (for Managed Bean)</em>, I highly recommend you start with this tutorial <a style="color: #bc360a;" title="http://docs.oracle.com/javase/tutorial/jmx/" href="http://docs.oracle.com/javase/tutorial/jmx/" target="_blank">Trail: Java Management Extensions (JMX)</a>
</p>

#### <span id="211_Interface">2.1.1. Interface</span>

The method exposed will only allow the reset of the cache via JMX:

<pre class="lang:java decode:true ">@MXBean
public interface CacheResetMXBean {
    void resetReferenceDataCache();    
}</pre>

<p style="text-align: justify; padding-left: 30px;">
   <em>“<span style="color: #000000;">An </span>MXBean<span style="color: #000000;"> is a type of MBean that references only a predefined set of data types. In this way, you can be sure that your MBean will be usable by any client, including remote clients, without any requirement that the client have access to model-specific classes representing the types of your MBeans. MXBeans provide a convenient way to bundle related values together, without requiring clients to be specially configured to handle the bundles.</span>” [5]</em>
</p>

####   2.1.2. Implementation

<pre class="lang:java decode:true " title="CacheReset MxBean implementation">@Singleton
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

  * as mentioned the implementation only calls the `evictAll()` method of the injected singleton bean described in the previous section
  * the bean is also defined as `@Singleton`
  * the `@Startup` annotation causes the bean to be instantiated by the container when the application starts – **eager initialization**
  * I use again the `@PostConstruct` functionality. Here **this** bean is registered in JMX, checking before if the `ObjectName` is used to remove it if so…

### <span id="22_Rest_service_call">2.2. Rest service call</span>

I’ve also built in the possibility to clear the cache by calling a REST resource. This happends when you execute a HTTP POST on the (rest-context)/reference-data/flush-cache:

<pre class="lang:java decode:true " title="Trigger cache refresh via REST resource">@Path("/reference-data")
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
  Notice the existence of the version query parameter in the <code>@GET</code> <code>getReferenceData(...)</code> method.  This represents a hash on the reference data and if it hasn’t modified the client will receive a <em>304 Not Modified HTTP Status</em>. This is a nice way to spare some bandwidth, especially if you have mobile clients. See my post <a style="color: #bc360a;" title="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" href="http://www.codingpedia.org/ama/tutorial-rest-api-design-and-implementation-in-java-with-jersey-and-spring/" target="_blank">Tutorial – REST API design and implementation in Java with Jersey and Spring</a>, for a detailed discussion around REST services design and implementation.
</p>

<p class="note_normal" style="font-style: italic;">
  <strong>Note:</strong> In a clustered environment, you need to call resetCache(…) on each JVM where the application is deployed, when the reference data changes.
</p>

<p style="text-align: justify;">
  Well, that’s it. In this post we’ve learned how to build a simple reference data cache in Java EE with the help of Ehcache. Of course you can easily extend the cache functionality to offer more granular access/clearing to cached objects. Don’t forget to use <code>LockType.WRITE</code> for the clear methods in this case…
</p>

<h2 class="title" style="color: #000000;">
  <span id="Resources">Resources</span>
</h2>

### <span id="Web">Web</span>

  1. <a style="color: #bc360a;" title="http://www.adam-bien.com/roller/abien/entry/singleton_the_perfect_cache_facade" href="http://www.adam-bien.com/roller/abien/entry/singleton_the_perfect_cache_facade" target="_blank">@Singleton – the perfect cache facade</a> by Adam Bien
  2. <a style="color: #bc360a;" title="http://www.adam-bien.com/roller/abien/entry/singleton_the_simplest_possible_jmx" href="http://www.adam-bien.com/roller/abien/entry/singleton_the_simplest_possible_jmx" target="_blank">@Singleton – the simplest possible JMX MXBean </a>by Adam Bien
  3. <a style="color: #bc360a;" title="http://tomee.apache.org/singleton-beans.html" href="http://tomee.apache.org/singleton-beans.html" target="_blank">Tomee – Singleton Beans</a>
  4. <a title="http://ehcache.org/" href="http://ehcache.org/" target="_blank">ehcache.org</a>
      1. <a title="http://ehcache.org/documentation/2.8/code-samples" href="http://ehcache.org/documentation/2.8/code-samples" target="_blank">Code samples</a>
  5. <a style="color: #bc360a;" title="http://docs.oracle.com/javase/tutorial/jmx/" href="http://docs.oracle.com/javase/tutorial/jmx/" target="_blank">Trail: Java Management Extensions (JMX)</a>
  6. <a style="color: #bc360a;" title="http://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html" href="http://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html" target="_blank">Spring Cache Abstraction</a>

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
