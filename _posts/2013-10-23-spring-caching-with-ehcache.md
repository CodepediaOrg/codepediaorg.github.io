---
id: 769
title: Spring caching with Ehcache
date: 2013-10-23T09:14:18+00:00
author: Adrian Matei
layout: post
guid: http://www.codingpedia.org/?p=769
permalink: /ama/spring-caching-with-ehcache/
fsb_show_social:
  - 0
fsb_social_facebook:
  - 5
dsq_thread_id:
  - 1889949003
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
fsb_social_google:
  - 13
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:49:"https://github.com/podcastpedia/podcastpedia-web/";s:11:"ribbon-type";i:10;}'
dsq_needs_sync:
  - 1
categories:
  - optimization
  - spring
tags:
  - cache
  - caching
  - ehcache
  - optimization
  - performance
  - spring
---

## <span id="1_Why_use_Caching">1. Why use Caching?</span>

<p style="text-align: justify;">
  Are you aware of the Pareto principle, also known as the 80-20 rule, which states that, for many events, roughly 80% of the effects come from 20% of the causes? Well, this principle also holds true for <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a>, where most of the traffic is driven by some of the podcasts, and only some of the search criteria are used the most. So why not <a title="Wikipedia - cache" href="http://en.wikipedia.org/wiki/Cache_%28computing%29" target="_blank">cache</a> them?
</p>

<p style="text-align: justify;">
  For application caching Podcastpedia uses <a title="Ehcache.org" href="http://ehcache.org/" target="_blank">Ehcache</a>, which is an open source, standards-based cache for boosting performance, offloading your database, and simplifying scalability. It&#8217;s the most widely-used Java-based cache because it&#8217;s robust, proven, and full-featured.
</p>

This post presents how Ehcache is integrated with Spring, which is the main technology used to develop Podcastpedia.org

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

<!--more-->

<div id="toc_container" class="no_bullets">
  <p class="toc_title">
    Contents
  </p>

  <ul class="toc_list">
    <li>
      <a href="#1_Why_use_Caching">1. Why use Caching?</a>
    </li>
    <li>
      <a href="#2_Spring_Cache_abstraction">2. Spring Cache abstraction</a><ul>
        <li>
          <a href="#21_Maven_dependency">2.1. Maven dependency</a>
        </li>
        <li>
          <a href="#22_Configuring_the_cache_storage">2.2. Configuring the cache storage</a>
        </li>
        <li>
          <a href="#23_Declarative_annotation-based_cache">2.3. Declarative annotation-based cache</a>
        </li>
        <li>
          <a href="#24_Enabling_Caching_annotations">2.4. Enabling Caching annotations</a><ul>
            <li>
              <a href="#241_Cacheable">2.4.1. @Cacheable</a><ul>
                <li>
                  <a href="#2411_Default_Key_Generation">2.4.1.1. Default Key Generation</a><ul>
                    <li>
                      <a href="#24111_Example_one_8211_non-primitive_input_parameter">2.4.1.1.1. Example one &#8211; non-primitive input parameter</a>
                    </li>
                    <li>
                      <a href="#24112_Example_two_8211_primitive_input_parameter">2.4.1.1.2. Example two &#8211; primitive input parameter</a>
                    </li>
                  </ul>
                </li>

                <li>
                  <a href="#2412_Custom_Key_Generation_Declaration">2.4.1.2. Custom Key Generation Declaration</a><ul>
                    <li>
                      <a href="#24121_Example_one_8211_use_method_name_as_key">2.4.1.2.1. Example one &#8211; use method name as key</a>
                    </li>
                    <li>
                      <a href="#24122_Example_two_8211_use_method_name_as_key_and_no_input_parameters">2.4.1.2.2. Example two &#8211; use method name as key and no input parameters</a>
                    </li>
                    <li>
                      <a href="#24123_Example_three_8211_key_generated_with_SpEL">2.4.1.2.3. Example three &#8211; key generated with SpEL</a>
                    </li>
                  </ul>
                </li>
              </ul>
            </li>

            <li>
              <a href="#242_CacheEvict">2.4.2. @CacheEvict</a><ul>
                <li>
                  <a href="#2421_Example_one_8211_cache_eviction_based_on_key">2.4.2.1. Example one &#8211; cache eviction based on key</a>
                </li>
                <li>
                  <a href="#2422_Example_two_8211_multiple_cache_evictionflush">2.4.2.2. Example two &#8211; multiple cache eviction/flush</a>
                </li>
              </ul>
            </li>
          </ul>
        </li>

        <li>
          <a href="#25_Ehcachexml">2.5. Ehcache.xml</a>
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

## <span id="2_Spring_Cache_abstraction">2. Spring Cache abstraction</span>

<p style="text-align: justify;">
  Since version 3.1, Spring Framework provides support for transparently adding caching into an existing Spring application. Similar to the <a title="12. Transaction Management" href="http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/transaction.html">transaction</a> support, the caching abstraction allows consistent use of various caching solutions with minimal impact on the code.
</p>

### <span id="21_Maven_dependency">2.1. Maven dependency</span>

<p style="text-align: justify;">
  First of all, to be able to use Ehache make sure you have the corresponding .jar in your application&#8217;s classpath. To build Podcastpedia I use maven, so I added the following dependency to the <code>pom.xml</code> file:
</p>

<pre>
  <code class="xml">
    &lt;dependency&gt;
    	&lt;groupId&gt;net.sf.ehcache&lt;/groupId&gt;
    	&lt;artifactId&gt;ehcache&lt;/artifactId&gt;
    	&lt;version&gt;2.7.4&lt;/version&gt;
    &lt;/dependency&gt;
  </code>
</pre>

### <span id="22_Configuring_the_cache_storage">2.2. Configuring the cache storage</span>

<p style="text-align: justify;">
  Out of the box, the cache abstraction provides integration with two storages &#8211; one on top of the JDK <code>ConcurrentMap</code> and one for <code>ehcache</code> library. As mentioned in the beginning of the post, for Podcastpedia I use the latter. To configure it, I need to simply declare an appropriate <code>CacheManager</code> in the Spring application context.
</p>

The EhCache implementation is located under `org.springframework.cache.ehcache` package:

<pre>
  <code class="xml">
    &lt;!-- *******************************
    	 ***** CACHE CONFIGURATION *****
    	 ******************************* --&gt;
    &lt;bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager"&gt;
    	&lt;property name="cacheManager" ref="ehcache"/&gt;
    &lt;/bean&gt;
    &lt;bean id="ehcache" class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean"&gt;
    	&lt;property name="configLocation" value="classpath:config/ehcache.xml"/&gt;
    	&lt;property name="shared" value="true"/&gt;
    &lt;/bean&gt;
  </code>
</pre>

<p style="text-align: justify;">
  All usages of Ehcache start with the creation of a <code>CacheManager</code>, which is an entity that controls and manages the <code>Cache</code>s and can be used to retrieve this for storage. The entire ehcache-specific configuration is read from the resource <code>ehcache.xml</code> (<code>configLocation</code> property) &#8211; you&#8217;ll see in a dedicated section bellow how this file looks like. When set to <code>true</code>, the <code>shared</code> property specifies that the EhCache CacheManager should be shared as a singleton at the VM level.
</p>

### <span id="23_Declarative_annotation-based_cache">2.3. Declarative annotation-based cache</span>

<p style="text-align: justify;">
  Spring provides two Java annotations for the caching declaration: <code>@Cacheable</code> and <code>@CacheEvict</code>, which allow methods to trigger cache population or cache eviction. Before I show you some use examples of the two annotation, you have to make sure that caching annotations are enabled:
</p>

### <span id="24_Enabling_Caching_annotations">2.4. Enabling Caching annotations</span>

<p style="text-align: justify;">
  It is important to note that even though declaring the cache annotations does not automatically triggers their actions &#8211; like many things in Spring, the feature has to be declaratively enabled (which means if you ever suspect caching is to blame, you can disable it by removing only one configuration line rather then all the annotations in your code).
</p>

For XML configuration use the `cache:annotation-driven` element in the application context:

<pre>
  <code class="xml">
    &lt;cache:annotation-driven key-generator="enhancedDefaultKeyGenerator"/&gt;
    &lt;bean id ="enhancedDefaultKeyGenerator" class="org.podcastpedia.cache.interceptor.EnhancedDefaultKeyGenerator"/&gt;
  </code>
</pre>

Normally you would just have  `<cache:annotation-driven>` inserted into your application context, but if the `org.springframework.cache.interceptor.DefaultKeyGenerator` will not suffice you have to implement a custom one. This was my case and here is how my `Default<strong>Enhanced</strong>KeyGenerator` looks like:

<pre>
  <code class="java">
    package org.podcastpedia.cache.interceptor;

    import java.lang.reflect.Method;
    import java.util.HashSet;

    import org.springframework.cache.interceptor.KeyGenerator;

    /**
     * Default key generator. Returns {@value #NO_PARAM_KEY} if no
     * parameters are provided, the parameter itself (if primitive type) if only one is given or
     * a hash code computed from all given parameters' hash code values.
     * Uses the constant value {@value #NULL_PARAM_KEY} for any
     * {@code null} parameters given.
     *
     * @author Costin Leau
     * @author Chris Beams
     * @since 3.1
     */
    public class EnhancedDefaultKeyGenerator implements KeyGenerator {

    	public static final int NO_PARAM_KEY = 0;
    	public static final int NULL_PARAM_KEY = 53;

    	private static final HashSet&lt;Class&lt;?&gt;&gt; WRAPPER_TYPES = getWrapperTypes();

    	public Object generate(Object target, Method method, Object... params) {
    		if (params.length == 1 && isWrapperType(params[0].getClass())) {
    				return (params[0] == null ? NULL_PARAM_KEY : params[0]);
    		}
    		if (params.length == 0) {
    			return NO_PARAM_KEY;
    		}
    		int hashCode = 17;
    		for (Object object : params) {
    			hashCode = 31 * hashCode + (object == null ? NULL_PARAM_KEY : object.hashCode());
    		}
    		return Integer.valueOf(hashCode);
    	}

        public static boolean isWrapperType(Class&lt;?&gt; clazz)
        {
            return WRAPPER_TYPES.contains(clazz);
        }

        private static HashSet&lt;Class&lt;?&gt;&gt; getWrapperTypes()
        {
            HashSet&lt;Class&lt;?&gt;&gt; ret = new HashSet&lt;Class&lt;?&gt;&gt;();
            ret.add(Boolean.class);
            ret.add(Character.class);
            ret.add(Byte.class);
            ret.add(Short.class);
            ret.add(Integer.class);
            ret.add(Long.class);
            ret.add(Float.class);
            ret.add(Double.class);
            ret.add(Void.class);
            return ret;
        }
    }
  </code>
</pre>

<p style="text-align: justify;">
  The only difference to the original implementation is at the <code>line 27</code>, where the <code>isWrapperType(params[0].getClass())</code> condition is added. This way, when the cached method has only one parameter, the returned result will be the parameter itself ONLY IF it is a primitive type, for other objects the hashcode will be computed and returned. In the next section, where the <code>@Cacheable</code> annotation is presented, I will mention where such a custom implementation was required.
</p>

#### <span id="241_Cacheable">2.4.1. @Cacheable</span>

<p style="text-align: justify;">
  @Cacheable is used in front of methods that are cacheable &#8211; that is, methods for whom the result is stored into the cache so on subsequent invocations (with the same arguments), the value in the cache is returned without having to actually execute the method.
</p>

Now I am going to present some use examples of the `@Cacheable` annotation.

##### <span id="2411_Default_Key_Generation">2.4.1.1. Default Key Generation</span>

<p class="listitem" style="text-align: justify;">
  Since caches are essentially key-value stores, each invocation of a cached method needs to be translated into a suitable key for cache access. If you specify only the <code>value</code> of the cache in the <code>@Cacheable</code> annotation in front of the method, then the <code>default key generation</code> or the one you specified as default will kick in.
</p>

As specified above, I used a custom `KeyGenerator,` which functions after the following algorithm:

<div>
  <ul type="disc">
    <li>
      If no params are given, return 0.
    </li>
    <li>
      If only one param is given and is primitive, return that instance.
    </li>
    <li>
      Else return a key computed from the hashes of all parameters.
    </li>
  </ul>

  <h6>
    <span id="24111_Example_one_8211_non-primitive_input_parameter">2.4.1.1.1. Example one &#8211; non-primitive input parameter</span>
  </h6>
</div>

<pre>
  <code class="java">
    @Cacheable(value="searchResults")
    public SearchResult getResultsForSearchCriteria(SearchData searchData) throws UnsupportedEncodingException {
    .........
    }
  </code>
</pre>

<p style="text-align: justify;">
  In the example above the method <code>getResultsForSearchCriteria</code> will be cached in the cache <code>"searchResults"</code> (see <code>ehcache.xml</code> to find out how the cache is configured), and the key of the cached will be computed from the hash of the <code>SearchData</code> input parameter. I had to override the <code>equals</code> and <code>hashCode</code> methods for this class:
</p>

<pre>
  <code class="java">
    public class SearchData implements Serializable {

    	private static final long serialVersionUID = 4682314801277970962L;

    	/** query text */
    	private String queryText;

    	/** query text in natural mode */
    	private String queryTextInNaturalMode;

    	/** any of these words */
    	private String anyOfTheseWords;

    	/** all of these words **/
    	private String allTheseWords;

    	/** exact pharse **/
    	private String exactPhrase;

    	/** none of these words */
    	private String noneOfTheseWords;

    	/** Language code the podcasts should be in */
    	private LanguageCode languageCode;

    	/** search mode type - at the moment either natural or boolean */
    	private String searchMode;

    	/** number of results per page */
    	private Integer numberResultsPerPage;
    	private Integer currentPage;
    	private Integer firstItemOnPage; //=(currentPage - 1)*numberResultsPerPage
    	private boolean isOrderByPopularity;

    	/** where to look for the given search criteria - at the moment podcast and episodes is availabel */
    	private String searchTarget;

    	/** List of selected categories id to be looked for */
    	private List&lt;Integer&gt; categId;

    	/** look for videos or audio files, or both (identified by "all") */
    	private MediaType mediaType;

    	/** contains the target where to search in - podcasts or episodes **/
    	private String termsSearchTarget;

    	/** order by criteria **/
    	private OrderByOption orderBy;

    	/**flag to mark that the model attribute is for feed generation */
    	private boolean isForFeed;

    	/** id of the tag that is being looked for */
    	private Integer tagId;

    	/** placeholder for the query string to be passed to the next request */
    	private StringBuffer queryString;

    	...............

    	@Override
    	public int hashCode() {
    		final int prime = 31;
    		int result = 1;
    		result = prime * result
    				+ ((allTheseWords == null) ? 0 : allTheseWords.hashCode());
    		result = prime * result
    				+ ((anyOfTheseWords == null) ? 0 : anyOfTheseWords.hashCode());
    		result = prime * result + ((categId == null) ? 0 : categId.hashCode());
    		result = prime * result
    				+ ((currentPage == null) ? 0 : currentPage.hashCode());
    		result = prime * result
    				+ ((exactPhrase == null) ? 0 : exactPhrase.hashCode());
    		result = prime * result
    				+ ((firstItemOnPage == null) ? 0 : firstItemOnPage.hashCode());
    		result = prime * result + (isForFeed ? 1231 : 1237);
    		result = prime * result + (isOrderByPopularity ? 1231 : 1237);
    		result = prime * result
    				+ ((languageCode == null) ? 0 : languageCode.hashCode());
    		result = prime * result
    				+ ((mediaType == null) ? 0 : mediaType.hashCode());
    		result = prime
    				* result
    				+ ((noneOfTheseWords == null) ? 0 : noneOfTheseWords.hashCode());
    		result = prime
    				* result
    				+ ((numberResultsPerPage == null) ? 0 : numberResultsPerPage
    						.hashCode());
    		result = prime * result + ((orderBy == null) ? 0 : orderBy.hashCode());
    		result = prime * result
    				+ ((queryText == null) ? 0 : queryText.hashCode());
    		result = prime
    				* result
    				+ ((queryTextInNaturalMode == null) ? 0
    						: queryTextInNaturalMode.hashCode());
    		result = prime * result + (int) (searchId ^ (searchId &gt;&gt;&gt; 32));
    		result = prime * result
    				+ ((searchMode == null) ? 0 : searchMode.hashCode());
    		result = prime * result
    				+ ((searchTarget == null) ? 0 : searchTarget.hashCode());
    		result = prime * result + ((tagId == null) ? 0 : tagId.hashCode());
    		result = prime
    				* result
    				+ ((termsSearchTarget == null) ? 0 : termsSearchTarget
    						.hashCode());
    		return result;
    	}

    	@Override
    	public boolean equals(Object obj) {
    		if (this == obj)
    			return true;
    		if (obj == null)
    			return false;
    		if (getClass() != obj.getClass())
    			return false;
    		SearchData other = (SearchData) obj;
    		return this.hashCode() == other.hashCode();
    	}
    	...............
    }
  </code>
</pre>

###### <span id="24112_Example_two_8211_primitive_input_parameter">2.4.1.1.2. Example two &#8211; primitive input parameter</span>

<pre>
  <code class="java">
    @Cacheable(value="podcasts")
    public Podcast getPodcastById(int podcastId) throws BusinessException{
    .........
    }
  </code>
</pre>

In this example the result (a podcast) will be placed in the `"podcasts"`-cache, and because the input is a primitive type, the cache key will be exactly the `podcastId`.

<p style="padding-left: 30px; text-align: justify;">
  <em><strong>Note:</strong> Make sure you don&#8217;t have overllaping keys in your cache. Image I had a method <code>Integer getNumberOfEpisodes(int podcastId)</code> cached and set the cache value to the same <code>"podcasts"</code>. If I would invoke the method, with the let&#8217;s say podcastId=1, instead of returning the number of episodes as I would have expected it could return a podcast, and get runtime exception after that.</em>
</p>

##### <span id="2412_Custom_Key_Generation_Declaration">2.4.1.2. Custom Key Generation Declaration</span> {.title}

<p style="text-align: justify;">
  What if the target methods to be cached have various signatures that cannot be simply mapped on top of the cache structure, or what if it doesn&#8217;t make sense to use all parameters to generate the hash key, or what if I want to cache a method with no parameters and not having 0 as the cache key?
</p>

<p style="text-align: justify;">
  For such cases, the <code class="literal">@Cacheable</code> annotation allows the user to specify how the key is generated through its <code class="literal">key</code> attribute. The developer can also use <a class="link" title="8. Spring Expression Language (SpEL)" href="http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/expressions.html">SpEL</a> to pick the arguments of interest (or their nested properties), perform operations or even invoke arbitrary methods without having to write any code or implement any interface.
</p>

###### <span id="24121_Example_one_8211_use_method_name_as_key">2.4.1.2.1. Example one &#8211; use method name as key</span>

<pre>
  <code class="java">
    @Cacheable(value="randomAndTopRatedPodcasts", key="#root.method.name")
    public List&lt;Podcast&gt; getRandomPodcasts(Integer numberOfPodcasts) {
    	List&lt;Podcast&gt; randomPodcasts = podcastDao.getRandomPodcasts(numberOfPodcasts);
    	for(Podcast p : randomPodcasts){
    		p.setLastEpisode(episodeDao.getLastEpisodeForPodcast(p.getPodcastId()));
    	}
    	return randomPodcasts;
    }
  </code>
</pre>

<p style="text-align: justify;">
  In this example the result is persisted in the <code>"randomAndTopRatedPodcasts"</code>-cache. The key of the cache is specified via <code>key="#root.method.name"</code> and set to the <code>name of the method</code> being invoked, instead of being assigned the input <code>numberOfPodcasts</code>.
</p>

<p style="padding-left: 30px;">
  <em><strong>Note</strong>: make sure not to have two methods with the same name in the same cache and use the methods&#8217; name as cache-key.</em>
</p>

###### <span id="24122_Example_two_8211_use_method_name_as_key_and_no_input_parameters">2.4.1.2.2. Example two &#8211; use method name as key and no input parameters</span>

<pre>
  <code class="java">
    @Cacheable(value="referenceData", key="#root.method.name")
    public List&lt;Category&gt; getCategoriesOrderedByNoOfPodcasts() {
    	return categoryDao.getCategoriesOrderedByNoOfPodcasts();
    }
  </code>
</pre>

<p style="text-align: justify;">
  The method has no arguments. Result is cached in the <code>"referenceData"</code> pinned cache &#8211; we&#8217;ll see what means when I present the ehcache configuration file. The key of the cache is set to the <code>name of the method</code> being invoked.
</p>

###### <span id="24123_Example_three_8211_key_generated_with_SpEL">2.4.1.2.3. Example three &#8211; key generated with SpEL</span>

<pre>
  <code class="java">
    @Cacheable(value="podcasts", key="T(java.lang.String).valueOf(#podcastId).concat('-').concat(#episodeId)")
    public EpisodeWrapper getEpisodeDetails(Integer podcastId, Integer episodeId)  throws BusinessException {
    	..........
    }
  </code>
</pre>

<p style="text-align: justify;">
  If I had let the default cache key generator calculate the cache key based on <code>podcastId</code> and <code>episodeId</code> it would have definetely come to collisions and present the wrong result to the user, so to cache episodes in <code>"podcasts"</code> I built a key made out of strings <code>"podcastId-episodeId"</code> (e.g. &#8220;1-10&#8221;, &#8220;1109-3&#8221;, &#8220;5-31&#8221;), which asures their uniqueness in the cache.
</p>

<pre>
  <code class="java">
    @Cacheable(value="referenceData", key="#root.method.name")
    public List&lt;Category&gt; getCategoriesOrderedByNoOfPodcasts() {
    	return categoryDao.getCategoriesOrderedByNoOfPodcasts();
    }
  </code>
</pre>

<p style="text-align: justify;">
  <em><strong>Note:</strong> I use the <code>@Cacheable</code> annotation at the <strong>service layer</strong> rather than at the <strong><a title="Wikipedia - data access object" href="http://en.wikipedia.org/wiki/Data_access_object" target="_blank">DAO</a> layer</strong>, because there are many cases (e.g. <code>getPodcastDetails()</code>) when a service method invokes several DAO methods, and if in cache I avoid these calls completely. </em>
</p>

#### <span id="242_CacheEvict">2.4.2. @CacheEvict</span>

<p style="text-align: justify;">
  The cache abstraction allows not just population of a cache store but also eviction. This process is useful for removing stale or unused data from the cache. Opposed to <code class="literal">@Cacheable</code>, annotation <code class="literal">@CacheEvict</code> demarcates methods that perform cache <span class="emphasis"><em>eviction</em></span>, that is methods that act as triggers for removing data from the cache. Just like its sibling, <code class="literal">@CacheEvict</code> requires one to specify one (or multiple) caches that are affected by the action, allows a key or a condition to be specified but in addition, features an extra parameter <code class="literal">allEntries</code> which indicates whether a cache-wide eviction needs to be performed rather then just an entry one (based on the key).
</p>

I am going to give again some examples of using this annotation and explain why I configured it like that.

##### <span id="2421_Example_one_8211_cache_eviction_based_on_key">2.4.2.1. Example one &#8211; cache eviction based on key</span>

<pre>
  <code class="java">
    @Transactional
    @CacheEvict(value="podcasts", key="#rating.podcastId")
    public ItemRatingResponse addRatingForPodcast(Rating rating, Integer currentNumberOfRatings, Float currentRating) {
    	return addRatingForItem(rating, currentNumberOfRatings, currentRating);
    }
  </code>
</pre>

<p style="text-align: justify;">
  In this first example when a visitor rates a podcast, it gets evicted from the <code>"podcasts"</code> cache. The cache key is the podcast&#8217;s id which is a property of the <code>Rating</code> class. When requested again the podcast will be presented to the visitor with the new rating and loaded in the cache.
</p>

##### <span id="2422_Example_two_8211_multiple_cache_evictionflush">2.4.2.2. Example two &#8211; multiple cache eviction/flush</span>

<pre>
  <code class="java">
    @Caching(evict = {
    		@CacheEvict(value="referenceData", allEntries=true),
    		@CacheEvict(value="podcasts", allEntries=true),
    		@CacheEvict(value="searchResults", allEntries=true),
    		@CacheEvict(value="newestAndRecommendedPodcasts", allEntries=true),
    		@CacheEvict(value="randomAndTopRatedPodcasts", allEntries=true)
    	})
    public void flushAllCaches() {
    	LOG.warn("All caches have been completely flushed");
    }
  </code>
</pre>

<p style="text-align: justify;">
  As the name implies, when this method is invoked all the defined caches are flushed. To allow the use of multiple nested <code>@CacheEvict</code> on the same method, I had to use an enclosing annotion &#8211; <code>@Caching</code>.
</p>

### <span id="25_Ehcachexml">2.5. Ehcache.xml</span>

As mentioned in the section _Configure the cache storage_, the caches configuration is done declaratively in the `ehcache.xml` file:

<pre>
  <code class="xml">
    &lt;ehcache
    	xsi:noNamespaceSchemaLocation="ehcache.xsd"
    	updateCheck="true"
    	monitoring="autodetect"
    	dynamicConfig="true"
    	maxBytesLocalHeap="150M"
    	&gt;
    	&lt;diskStore path="java.io.tmpdir"/&gt;

    	&lt;cache name="searchResults"
    	      maxBytesLocalHeap="100M"
    	      eternal="false"
    	      timeToIdleSeconds="300"
    	      overflowToDisk="true"
    	      maxElementsOnDisk="1000"
    	      memoryStoreEvictionPolicy="LRU"/&gt;

    	&lt;cache name="podcasts"
    	      maxBytesLocalHeap="40M"
    	      eternal="false"
    	      timeToIdleSeconds="300"
    	      overflowToDisk="true"
    	      maxEntriesLocalDisk="1000"
    	      diskPersistent="false"
    	      diskExpiryThreadIntervalSeconds="120"
    	      memoryStoreEvictionPolicy="LRU"/&gt;

    	&lt;cache name="referenceData"
    	      maxBytesLocalHeap="5M"
    	      eternal="true"
    	      memoryStoreEvictionPolicy="LRU"&gt;
    	      &lt;pinning store="localMemory"/&gt;
    	 &lt;/cache&gt;

    	&lt;cache name="newestAndRecommendedPodcasts"
                  maxBytesLocalHeap="3M"
    	      eternal="true"
    	      memoryStoreEvictionPolicy="LRU"&gt;
    	      &lt;pinning store="localMemory"/&gt;
    	&lt;/cache&gt;

    	&lt;cache name="randomAndTopRatedPodcasts"
                  maxBytesLocalHeap="1M"
    	      timeToLiveSeconds="300"
    	      memoryStoreEvictionPolicy="LRU"&gt;
    	 &lt;/cache&gt;

    &lt;/ehcache&gt;
  </code>
</pre>

Most of the properties set in the caches are self explaining by name, but here are some extra details:

  * `maxBytesLocalHeap` &#8211; defines how many bytes the cache may use from the VM&#8217;s heap. If a CacheManager    maxBytesLocalHeap has been defined, this Cache&#8217;s specified amount will be   subtracted from the CacheManager. Other caches will share the remainder. This attribute&#8217;s values are given as <number>k|K|m|M|g|G for  kilobytes (k|K), megabytes (m|M), or gigabytes (g|G). For example, maxBytesLocalHeap=&#8221;2g&#8221; allots 2 gigabytes of heap memory. If you specify a maxBytesLocalHeap, you can&#8217;t use the maxEntriesLocalHeap attribute. maxEntriesLocalHeap can&#8217;t be used if a CacheManager maxBytesLocalHeap is set.<br /> <em><strong>Note</strong>: Set at the highest level this property defines the memory allocated for all the defined caches. You have to divide it afterwards to the individual caches.</em>
  * `eternal` &#8211; sets whether elements are eternal. If eternal,  timeouts are ignored and the  element is never expired.
  * `timeToIdleSeconds`</code>` &#8211; sets the time to idle for an element before it expires. i.e. The maximum amount of time between accesses before an element expires. Is only used if the element is not eternal. Optional attribute. A value of 0 means that an Element can idle for infinity. The default value is 0.
  * `timeToLiveSeconds` &#8211; sets the time to live for an element before it expires. i.e. The maximum time between creation time and when an element expires. Is only used if the element is not eternal. Optional attribute. A value of 0 means that and Element can live for infinity. The default value is 0.
  * `memoryStoreEvictionPolicy` &#8211; policy would be enforced upon reaching the maxEntriesLocalHeap limit. Default policy is `Least Recently Used` (specified as `LRU`).

<p style="padding-left: 30px; text-align: justify;">
  <em><strong>Notes:<br /> </strong>If you want take some load of your database you could also you the <code>localTempSwap</code> persistance strategy, and in that case you can use <code>maxEntriesLocalDisk</code> or <code>maxBytesLocalDisk</code> at either the Cache or CacheManager level to control the size of the disk tier. As the database of <a title="Podcastpedia.org, knowledge to go" href="https://github.com/Codingpedia/podcastpedia" target="_blank">Podcastpedia.org</a> is not overloaded at the moment, that strategy wouldn&#8217;t make any sense.</em>
</p>

<p style="padding-left: 30px; text-align: justify;">
  <em>Two of the configured caches, <code>"referenceData"</code> and <code>"newestAndRecommendedPodcasts"</code> are pinned in the local memory (<code>&lt;pinning store="localMemory"/&gt;</code>), that means the data will remain in the cache at all times. To unpin the data from the cache you have to clear the cache.</em>
</p>

<h2 style="text-align: justify;">
  <span id="3_Summary">3. Summary</span>
</h2>

<p style="text-align: justify;">
  Well, this concludes the application caching strategy for Podcastpedia. You&#8217;ve learned what libraries are required for caching with Ehcache, how to configure the cache storage, how Spring supports caching via annotations.
</p>

<p style="text-align: justify;">
  But as I mentioned before, application/website optimization should be a holistic approach, with application caching being just a part of that &#8211; it might also want to
</p>

  * <a title="Optimizing MySQL server settings" href="http://www.codingpedia.org/ama/optimizing-mysql-server-settings/" target="_blank">optimize your MySQL server settings</a>,
  * <a title="How To: Enable compression and leverage browser caching with Apache Server" href="http://www.codingpedia.org/ama/how-to-enable-compression-and-leverage-browser-caching-with-apache-server/" target="_blank">enable compression and leverage browser caching with Apache Server</a>

    or
  *  <a title="How To: Enable compression and leverage browser caching with Apache Server" href="http://www.codingpedia.org/ama/tomcat-jdbc-connection-pool-configuration-for-production-and-development/" target="_blank">to tweak your Tomcat&#8217;s JDBC connection pool configuration </a>

<p class="note_normal">
  <img style="float: left; width: 35px; height: 29px; margin-right: 10px;" src="{{site.url}}/wp-content/uploads/2015/06/Octocat-smaller.png" alt="Octocat" /> Source code for this post is available on <a href="https://github.com/Codingpedia/podcastpedia">Github</a> - <b>podcastpedia.org</b> is an open source project.
</p>

## <span id="4_Resources">4. Resources</span>

  1. <a title="Ehcache.org" href="http://ehcache.org/" target="_blank">Ehcache.org</a>
  2. <a title="Cache abstraction in Spring" href="http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/html/cache.html" target="_blank">Spring Documentation &#8211; Cache Abstraction</a>
  3. <a title="Ehcache configuration" href="http://ehcache.org/documentation/user-guide/configuration" target="_blank">Ehcache configuration</a>
  4. <a title="Pinning, Expiration, and Eviction" href="http://ehcache.org/documentation/configuration/data-lifehttp://" target="_blank">Ehcache.org &#8211; Pinning of Caches and Elements in Memory</a>
  5. <a title="Persistence and Restartability" href="http://ehcache.org/documentation/configuration/fast-restart" target="_blank">Ehcache.org &#8211; Persistence and restartability</a>
  6. <a title="ehcache.xml" href="http://ehcache.org/ehcache.xml" target="_blank">ehcache.xml</a>

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
       <a class="icon-twitter" href="https://twitter.com/codingpedia" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/codingpedia" target="_blank"> </a> <a class="icon-linkedin" href="https://www.linkedin.com/company/codingpediaorg" target="_blank"> </a> <a class="icon-github" href="https://github.com/adrianmatei-me" target="_blank"> </a>
    </div>

    <div class="clear">
    </div>
  </div>
</div>
