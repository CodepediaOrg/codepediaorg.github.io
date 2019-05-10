---
id: 2070
title: SpringMVC4 + Spring Data JPA + SpringSecurity configuration using JavaConfig
date: 2014-11-05T14:16:59+00:00
author: Siva Reddy
layout: post
guid: http://www.codingpedia.org/?p=2070
permalink: /sivalabs/springmvc4-spring-data-jpa-springsecurity-configuration-using-javaconfig/
fsb_show_social:
  - 0
dsq_thread_id:
  - 3175711418
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:105:"https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 3
fsb_social_google:
  - 1
fsb_social_linkedin:
  - 8
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - hibernate
  - java
  - spring
tags:
  - javaconfig
  - spring data
  - spring mvc
  - spring security
---
<span style="color: #000000;">In this article we will see how to configure and integrate SpringMVC4, Spring Data JPA with Hibernate and SpringSecurity using JavaConfig.</span>

## 1. Pom.xml

<span style="color: #000000;">First let&#8217;s configure all the necessary dependencies in <strong>pom.xml</strong></span><!--more-->

<pre class="lang:xhtml decode:true" title="pom.xml ">&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
	&lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
	&lt;groupId&gt;com.sivalabs&lt;/groupId&gt;
	&lt;artifactId&gt;spring-javaconfig&lt;/artifactId&gt;
	&lt;version&gt;1.0&lt;/version&gt;
	&lt;packaging&gt;war&lt;/packaging&gt;
	&lt;name&gt;SpringApp JavaConfig Demo&lt;/name&gt;
	
	&lt;properties&gt;
		&lt;java.version&gt;1.7&lt;/java.version&gt;
		&lt;junit.version&gt;4.11&lt;/junit.version&gt;
		&lt;slf4j.version&gt;1.7.5&lt;/slf4j.version&gt;
		&lt;logback.version&gt;1.0.13&lt;/logback.version&gt;
		&lt;spring.version&gt;4.0.0.RELEASE&lt;/spring.version&gt;
		&lt;spring-data-jpa.version&gt;1.4.1.RELEASE&lt;/spring-data-jpa.version&gt;
		&lt;spring-security.version&gt;3.2.0.RELEASE&lt;/spring-security.version&gt;
		&lt;hibernate.version&gt;4.2.6.Final&lt;/hibernate.version&gt;
		&lt;aspectj.version&gt;1.7.2&lt;/aspectj.version&gt;
		&lt;mysql.version&gt;5.1.26&lt;/mysql.version&gt;
		&lt;jackson-json.version&gt;2.3.1&lt;/jackson-json.version&gt;
		&lt;commons-dbcp.version&gt;1.2.2&lt;/commons-dbcp.version&gt;
		&lt;commons-lang3.version&gt;3.1&lt;/commons-lang3.version&gt;
	&lt;/properties&gt;
	
	&lt;build&gt;
		&lt;finalName&gt;${project.artifactId}&lt;/finalName&gt;
		&lt;plugins&gt;
			&lt;plugin&gt;
				&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
				&lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
				&lt;version&gt;3.1&lt;/version&gt;
				&lt;configuration&gt;
					&lt;source&gt;${java.version}&lt;/source&gt;
					&lt;target&gt;${java.version}&lt;/target&gt;
				&lt;/configuration&gt;
			&lt;/plugin&gt;
		&lt;/plugins&gt;
	&lt;/build&gt;

	&lt;dependencies&gt;
	&lt;!-- Logging dependencies --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.slf4j&lt;/groupId&gt;
			&lt;artifactId&gt;jcl-over-slf4j&lt;/artifactId&gt;
			&lt;version&gt;${slf4j.version}&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.slf4j&lt;/groupId&gt;
			&lt;artifactId&gt;slf4j-api&lt;/artifactId&gt;
			&lt;version&gt;${slf4j.version}&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.slf4j&lt;/groupId&gt;
			&lt;artifactId&gt;slf4j-log4j12&lt;/artifactId&gt;
			&lt;version&gt;${slf4j.version}&lt;/version&gt;
		&lt;/dependency&gt;
		
		&lt;dependency&gt;
			&lt;groupId&gt;ch.qos.logback&lt;/groupId&gt;
			&lt;artifactId&gt;logback-classic&lt;/artifactId&gt;
			&lt;version&gt;${logback.version}&lt;/version&gt;
		&lt;/dependency&gt;		

		&lt;!-- Spring dependencies --&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-context-support&lt;/artifactId&gt;
			&lt;exclusions&gt;
				&lt;exclusion&gt;
					&lt;groupId&gt;commons-logging&lt;/groupId&gt;
					&lt;artifactId&gt;commons-logging&lt;/artifactId&gt;
				&lt;/exclusion&gt;
			&lt;/exclusions&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-webmvc&lt;/artifactId&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework&lt;/groupId&gt;
			&lt;artifactId&gt;spring-test&lt;/artifactId&gt;
		&lt;/dependency&gt;
		&lt;!-- Spring Data JPA dependencies --&gt;
		
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.data&lt;/groupId&gt;
			&lt;artifactId&gt;spring-data-jpa&lt;/artifactId&gt;
			&lt;version&gt;${spring-data-jpa.version}&lt;/version&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;org.hibernate&lt;/groupId&gt;
			&lt;artifactId&gt;hibernate-entitymanager&lt;/artifactId&gt;
			&lt;version&gt;${hibernate.version}&lt;/version&gt;
		&lt;/dependency&gt;
		
		&lt;!-- SpringSecurity dependencies --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
			&lt;artifactId&gt;spring-security-core&lt;/artifactId&gt;
			&lt;version&gt;${spring-security.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
			&lt;artifactId&gt;spring-security-web&lt;/artifactId&gt;
			&lt;version&gt;${spring-security.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
			&lt;artifactId&gt;spring-security-config&lt;/artifactId&gt;
			&lt;version&gt;${spring-security.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.security&lt;/groupId&gt;
			&lt;artifactId&gt;spring-security-taglibs&lt;/artifactId&gt;
			&lt;version&gt;${spring-security.version}&lt;/version&gt;
		&lt;/dependency&gt;	
		
		&lt;dependency&gt;
			&lt;groupId&gt;org.aspectj&lt;/groupId&gt;
			&lt;artifactId&gt;aspectjweaver&lt;/artifactId&gt;
			&lt;version&gt;${aspectj.version}&lt;/version&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.aspectj&lt;/groupId&gt;
			&lt;artifactId&gt;aspectjrt&lt;/artifactId&gt;
			&lt;version&gt;${aspectj.version}&lt;/version&gt;
		&lt;/dependency&gt;	

		&lt;!-- Testing dependencies --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;junit&lt;/groupId&gt;
			&lt;artifactId&gt;junit&lt;/artifactId&gt;
			&lt;version&gt;${junit.version}&lt;/version&gt;
			&lt;scope&gt;test&lt;/scope&gt;
		&lt;/dependency&gt;		

		&lt;!-- DB dependencies --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;mysql&lt;/groupId&gt;
			&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;
			&lt;version&gt;${mysql.version}&lt;/version&gt;
		&lt;/dependency&gt;
				
		&lt;dependency&gt;
			&lt;groupId&gt;commons-dbcp&lt;/groupId&gt;
			&lt;artifactId&gt;commons-dbcp&lt;/artifactId&gt;
			&lt;version&gt;${commons-dbcp.version}&lt;/version&gt;
		&lt;/dependency&gt;
				
		&lt;dependency&gt;
			&lt;groupId&gt;com.fasterxml.jackson.core&lt;/groupId&gt;
			&lt;artifactId&gt;jackson-databind&lt;/artifactId&gt;
			&lt;version&gt;${jackson-json.version}&lt;/version&gt;
		&lt;/dependency&gt;
		
		&lt;dependency&gt;
		    &lt;groupId&gt;javax.mail&lt;/groupId&gt;
		    &lt;artifactId&gt;mail&lt;/artifactId&gt;
		    &lt;version&gt;1.4.3&lt;/version&gt;
	    &lt;/dependency&gt;
	    
		&lt;!-- Web dependencies --&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;javax.servlet&lt;/groupId&gt;
			&lt;artifactId&gt;javax.servlet-api&lt;/artifactId&gt;
			&lt;version&gt;3.0.1&lt;/version&gt;
			&lt;scope&gt;provided&lt;/scope&gt;
		&lt;/dependency&gt;

		&lt;dependency&gt;
			&lt;groupId&gt;taglibs&lt;/groupId&gt;
			&lt;artifactId&gt;standard&lt;/artifactId&gt;
			&lt;version&gt;1.1.2&lt;/version&gt;
			&lt;scope&gt;compile&lt;/scope&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;jstl&lt;/groupId&gt;
			&lt;artifactId&gt;jstl&lt;/artifactId&gt;
			&lt;version&gt;1.2&lt;/version&gt;
			&lt;scope&gt;compile&lt;/scope&gt;
		&lt;/dependency&gt;
	&lt;/dependencies&gt;

	&lt;dependencyManagement&gt;
		&lt;dependencies&gt;
			&lt;dependency&gt;
				&lt;groupId&gt;org.springframework&lt;/groupId&gt;
				&lt;artifactId&gt;spring-framework-bom&lt;/artifactId&gt;
				&lt;version&gt;${spring.version}&lt;/version&gt;
				&lt;type&gt;pom&lt;/type&gt;
				&lt;scope&gt;import&lt;/scope&gt;
			&lt;/dependency&gt;		
		&lt;/dependencies&gt;
	&lt;/dependencyManagement&gt;
	
&lt;/project&gt;</pre>

## 2. Application properties

<span style="color: #000000;">Configure database connection properties and email settings in <strong>application.properties</strong></span>

<pre class="lang:default decode:true" title="application.properties">################### DataSource Configuration ##########################

jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=admin

init-db=false

################### Hibernate Configuration ##########################

hibernate.dialect=org.hibernate.dialect.MySQLDialect
hibernate.show_sql=true
hibernate.hbm2ddl.auto=update

################### JavaMail Configuration ##########################
smtp.host=smtp.gmail.com
smtp.port=465
smtp.protocol=smtps
smtp.username=sivaprasadreddy.k@gmail.com
smtp.password=
support.email=sivaprasadreddy.k@gmail.com</pre>

## 3. Service layer configuration

Configure common Service Layer beans such as `PropertySourcesPlaceholderConfigurer` and `JavaMailSender` etc in `com.sivalabs.springapp.config.AppConfig`

<pre class="lang:java decode:true" title="AppConfig.java">package com.sivalabs.springapp.config;

import java.util.Properties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.CacheManager;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.cache.concurrent.ConcurrentMapCacheManager;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
import org.springframework.core.env.Environment;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.scheduling.annotation.EnableScheduling;

@Configuration
@ComponentScan(basePackages={"com.sivalabs.springapp"},
		excludeFilters=@ComponentScan.Filter(type=FilterType.REGEX, pattern={"com.sivalabs.springapp.web.*"}))
@PropertySource(value = { "classpath:application.properties" })
@EnableScheduling
@EnableAspectJAutoProxy
@EnableCaching
public class AppConfig 
{
	@Autowired
	private Environment env;

	@Bean
	public static PropertySourcesPlaceholderConfigurer placeHolderConfigurer()
	{
		return new PropertySourcesPlaceholderConfigurer();
	}
	
	@Bean
	public JavaMailSenderImpl javaMailSenderImpl() {
		JavaMailSenderImpl mailSenderImpl = new JavaMailSenderImpl();
		mailSenderImpl.setHost(env.getProperty("smtp.host"));
		mailSenderImpl.setPort(env.getProperty("smtp.port", Integer.class));
		mailSenderImpl.setProtocol(env.getProperty("smtp.protocol"));
		mailSenderImpl.setUsername(env.getProperty("smtp.username"));
		mailSenderImpl.setPassword(env.getProperty("smtp.password"));

		Properties javaMailProps = new Properties();
		javaMailProps.put("mail.smtp.auth", true);
		javaMailProps.put("mail.smtp.starttls.enable", true);

		mailSenderImpl.setJavaMailProperties(javaMailProps);

		return mailSenderImpl;
	}
		
	@Bean
	public CacheManager cacheManager()
	{
		return new ConcurrentMapCacheManager();
	}
}</pre>

<span style="color: #000000;">Observe that we have excluded the package <code>com.sivalabs.springapp.web.*</code> from component scanning using new REGEX excludeFilter type. If we don&#8217;t exclude web related packages and try to run JUnit test for service layer beans we will encounter the following Exception: </span>

<p style="text-align: justify; padding-left: 30px;">
  <em><span style="color: #000000;">java.lang.IllegalArgumentException: A ServletContext is required to configure default servlet handling</span></em>
</p>

 <span style="color: #000000;">Also note that we have enabled Caching using <code>@EnableCaching</code>, so we should declare <code>CacheManager</code> bean.</span>

## 4. Persistence layer configuration

Configure Persistence Layer beans in `com.sivalabs.springapp.config.PersistenceConfig.java` as follows:

<pre class="lang:java decode:true" title="PersistenceConfig.java">package com.sivalabs.springapp.config;

import java.util.Properties;
import javax.persistence.EntityManagerFactory;
import javax.sql.DataSource;
import org.apache.commons.dbcp.BasicDataSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.env.Environment;
import org.springframework.core.io.ClassPathResource;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.instrument.classloading.InstrumentationLoadTimeWeaver;
import org.springframework.jdbc.datasource.init.DataSourceInitializer;
import org.springframework.jdbc.datasource.init.ResourceDatabasePopulator;
import org.springframework.orm.hibernate4.HibernateExceptionTranslator;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(basePackages="com.sivalabs.springapp.repositories")
public class PersistenceConfig 
{
	@Autowired
	private Environment env;

	@Value("${init-db:false}")
	private String initDatabase;
	
	@Bean
	public PlatformTransactionManager transactionManager()
	{
		EntityManagerFactory factory = entityManagerFactory().getObject();
		return new JpaTransactionManager(factory);
	}

	@Bean
	public LocalContainerEntityManagerFactoryBean entityManagerFactory()
	{
		LocalContainerEntityManagerFactoryBean factory = new LocalContainerEntityManagerFactoryBean();

		HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
		vendorAdapter.setGenerateDdl(Boolean.TRUE);
		vendorAdapter.setShowSql(Boolean.TRUE);

		factory.setDataSource(dataSource());
		factory.setJpaVendorAdapter(vendorAdapter);
		factory.setPackagesToScan("com.sivalabs.springapp.entities");

		Properties jpaProperties = new Properties();
		jpaProperties.put("hibernate.hbm2ddl.auto", env.getProperty("hibernate.hbm2ddl.auto"));
		factory.setJpaProperties(jpaProperties);

		factory.afterPropertiesSet();
		factory.setLoadTimeWeaver(new InstrumentationLoadTimeWeaver());
		return factory;
	}

	@Bean
	public HibernateExceptionTranslator hibernateExceptionTranslator()
	{
		return new HibernateExceptionTranslator();
	}
	
	@Bean
	public DataSource dataSource()
	{
		BasicDataSource dataSource = new BasicDataSource();
		dataSource.setDriverClassName(env.getProperty("jdbc.driverClassName"));
		dataSource.setUrl(env.getProperty("jdbc.url"));
		dataSource.setUsername(env.getProperty("jdbc.username"));
		dataSource.setPassword(env.getProperty("jdbc.password"));
		return dataSource;
	}
	
	@Bean
	public DataSourceInitializer dataSourceInitializer(DataSource dataSource) 
	{
		DataSourceInitializer dataSourceInitializer = new DataSourceInitializer();
		dataSourceInitializer.setDataSource(dataSource);
		ResourceDatabasePopulator databasePopulator = new ResourceDatabasePopulator();
		databasePopulator.addScript(new ClassPathResource("db.sql"));
		dataSourceInitializer.setDatabasePopulator(databasePopulator);
		dataSourceInitializer.setEnabled(Boolean.parseBoolean(initDatabase));
		return dataSourceInitializer;
	}	
}</pre>

<p style="text-align: justify;">
  Here we have configured DataSource and JPA <code>EntityManagerFactory</code> bean using Hibernate implementation. Also we have configured <code>DataSourceInitializer</code> bean to initialize and populate our tables with seed data. We can enable/disable executing this db.sql script by changing <code>init-db</code> property value in <code>application.properties</code>. And finally we have enabled Spring Data JPA repositories scanning using <code>@EnableJpaRepositories</code> to scan <code>com.sivalabs.springapp.repositories</code> package for JPA repository interfaces.
</p>

<h2 style="text-align: justify;">
  <span style="color: #000000;">5. Web related beans configuration</span>
</h2>

### 5.1. WebMvcConfig

<p style="text-align: justify;">
  <span style="color: #000000;"> Now let us configure Web related beans in <code>com.sivalabs.springapp.web.config.WebMvcConfig.java</code></span>
</p>

<pre class="lang:java decode:true" title="WebMvcConfig.java">package com.sivalabs.springapp.web.config;

import java.util.Properties;
import org.springframework.context.MessageSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.support.ReloadableResourceBundleMessageSource;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
import org.springframework.web.servlet.handler.SimpleMappingExceptionResolver;
import org.springframework.web.servlet.view.InternalResourceViewResolver;


@Configuration
@ComponentScan(basePackages = { "com.sivalabs.springapp.web"}) 
@EnableWebMvc
public class WebMvcConfig extends WebMvcConfigurerAdapter
{
	@Override
	public void addViewControllers(ViewControllerRegistry registry)
	{
		super.addViewControllers(registry);
		registry.addViewController("login/form").setViewName("login");		
		registry.addViewController("welcome").setViewName("welcome");
		registry.addViewController("admin").setViewName("admin");
	}

	@Bean
	public ViewResolver resolver()
	{
		InternalResourceViewResolver url = new InternalResourceViewResolver();
		url.setPrefix("/WEB-INF/jsp/");
		url.setSuffix(".jsp");
		return url;
	}

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry)
	{
		registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
	}

	@Override
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer)
	{
		configurer.enable();
	}

	@Bean(name = "messageSource")
	public MessageSource configureMessageSource()
	{
		ReloadableResourceBundleMessageSource messageSource = new ReloadableResourceBundleMessageSource();
		messageSource.setBasename("classpath:messages");
		messageSource.setCacheSeconds(5);
		messageSource.setDefaultEncoding("UTF-8");
		return messageSource;
	}

	@Bean
	public SimpleMappingExceptionResolver simpleMappingExceptionResolver()
	{
		SimpleMappingExceptionResolver b = new SimpleMappingExceptionResolver();
		Properties mappings = new Properties();
		mappings.put("org.springframework.dao.DataAccessException", "error");
		b.setExceptionMappings(mappings);
		return b;
	}
}</pre>

### 5.2. DispatcherService

<span style="color: #000000;">Configure <code>DispatcherService</code> using <code>AbstractAnnotationConfigDispatcherServletInitializer</code> convinient class. </span>

<pre class="lang:java decode:true" title="SpringWebAppInitializer.java">package com.sivalabs.springapp.web.config;

import javax.servlet.Filter;
import org.springframework.orm.jpa.support.OpenEntityManagerInViewFilter;
import org.springframework.web.filter.DelegatingFilterProxy;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;
import com.sivalabs.springapp.config.AppConfig;

public class SpringWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer
{

	@Override
	protected Class&lt;?&gt;[] getRootConfigClasses()
	{
		return new Class&lt;?&gt;[] { AppConfig.class};
	}

	@Override
	protected Class&lt;?&gt;[] getServletConfigClasses()
	{
		return new Class&lt;?&gt;[] { WebMvcConfig.class };
	}

	@Override
	protected String[] getServletMappings()
	{
		return new String[] { "/" };
	}

	@Override
    protected Filter[] getServletFilters() {
       return new Filter[]{ 
							new OpenEntityManagerInViewFilter()
						  };
    }

}</pre>

<p style="text-align: justify;">
  Here few things to note are we configured <code>AppConfig.class</code> as <code>RootConfig</code> classes and <code>WebMvcConfig.class</code> as ServletConfigClasses which is similar to how we configure in web.xml using ContextLoaderListener and DispatcherServlet&#8217;s contextConfigLocation . Also we have registered <code>OpenEntityManagerInViewFilter</code> to enable lazy loading of JPA entity graphs in view rendering phase.
</p>

## 7. Spring security

<span style="color: #000000;">Let us configure SpringSecurity. </span>

<p style="text-align: justify;">
  <span style="color: #000000;">First let us create a <code>SecurityUser</code> class which extends our application specific <code>User</code> class and implements <code>org.springframework.security.core.userdetails.UserDetails</code></span>
</p>

<pre class="lang:java decode:true" title="SecurityUser.java">package com.sivalabs.springapp.web.config;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Set;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import com.sivalabs.springapp.entities.Role;
import com.sivalabs.springapp.entities.User;

public class SecurityUser extends User implements UserDetails
{

	private static final long serialVersionUID = 1L;
	public SecurityUser(User user) {
		if(user != null)
		{
			this.setId(user.getId());
			this.setName(user.getName());
			this.setEmail(user.getEmail());
			this.setPassword(user.getPassword());
			this.setDob(user.getDob());
			this.setRoles(user.getRoles());
		}		
	}
	
	@Override
	public Collection&lt;? extends GrantedAuthority&gt; getAuthorities() {
		
		Collection&lt;GrantedAuthority&gt; authorities = new ArrayList&lt;&gt;();
		Set&lt;Role&gt; userRoles = this.getRoles();
		
		if(userRoles != null)
		{
			for (Role role : userRoles) {
				SimpleGrantedAuthority authority = new SimpleGrantedAuthority(role.getRoleName());
				authorities.add(authority);
			}
		}
		return authorities;
	}

	@Override
	public String getPassword() {
		return super.getPassword();
	}

	@Override
	public String getUsername() {
		return super.getEmail();
	}

	@Override
	public boolean isAccountNonExpired() {
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		return true;
	}

	@Override
	public boolean isEnabled() {
		return true;
	}	
}</pre>

We will implement a custom `UserDetailsService` and use Spring Data JPA repositories to load User details.

<pre class="lang:java decode:true" title="CustomUserDetailsService.java">package com.sivalabs.springapp.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;
import com.sivalabs.springapp.entities.User;
import com.sivalabs.springapp.services.UserService;
import com.sivalabs.springapp.web.config.SecurityUser;

@Component
public class CustomUserDetailsService implements UserDetailsService
{
	@Autowired
	private UserService userService;
	
	@Override
	public UserDetails loadUserByUsername(String userName)
			throws UsernameNotFoundException {
		User user = userService.findUserByEmail(userName);
		if(user == null){
			throw new UsernameNotFoundException("UserName "+userName+" not found");
		}
		return new SecurityUser(user);
	}
}</pre>

Now create `com.sivalabs.springapp.config.SecurityConfig.java` which contains `SpringSecurity` related bean definitions.

<pre class="lang:java decode:true " title="SecurityConfig.java">package com.sivalabs.springapp.config;

import javax.sql.DataSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
//import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;


@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter
{
	@Autowired
	private DataSource dataSource;

	@Autowired
	private CustomUserDetailsService customUserDetailsService;

	@Override
    protected void configure(AuthenticationManagerBuilder registry) throws Exception {
		/*
        registry
        .inMemoryAuthentication()
        .withUser("siva")
          .password("siva")
          .roles("USER")
          .and()
        .withUser("admin")
          .password("admin")
          .roles("ADMIN","USER");
        */
        
        //registry.jdbcAuthentication().dataSource(dataSource);
		registry.userDetailsService(customUserDetailsService);
    }


	  @Override
	  public void configure(WebSecurity web) throws Exception {
	    web
	      .ignoring()
	         .antMatchers("/resources/**");
	  }

	  @Override
	  protected void configure(HttpSecurity http) throws Exception {
	    http
	    .csrf().disable()
	    .authorizeRequests()
	        .antMatchers("/login","/login/form**","/register","/logout").permitAll()
	        .antMatchers("/admin","/admin/**").hasRole("ADMIN")
	        .anyRequest().authenticated()
	        .and()
	    .formLogin()
	        .loginPage("/login/form")
	        .loginProcessingUrl("/login")
	        .failureUrl("/login/form?error")
	        .permitAll();
	  }
}</pre>

Update `SpringWebAppInitializer` which we created eariler to add `SecurityConfig` configuration class.

<pre class="lang:java decode:true" title="SpringWebAppInitializerWithSec.java">public class SpringWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer
{
	@Override
	protected Class&lt;?&gt;[] getRootConfigClasses()
	{
		return new Class&lt;?&gt;[] { AppConfig.class};
		//As we have SecurityConfig.java in same package as AppConfig.java and enabled ComponentScan to scan "com.sivalabs.springapp.config" we don't need to explicitely configure it.
		//otherwise we should add SecurityConfig.class to getRootConfigClasses()
		//return new Class&lt;?&gt;[] { AppConfig.class, SecurityConfig.class};
	}
	...
	...
	@Override
    protected Filter[] getServletFilters() {
       return new Filter[]{ 
    		   new DelegatingFilterProxy("springSecurityFilterChain"),
    		   new OpenEntityManagerInViewFilter()};
    } 

}</pre>

As per our SpringSecurity custom Form Login configuration, we will use the following login form in login.jsp.

<pre class="lang:xhtml decode:true " title="login.jsp">&lt;!DOCTYPE html&gt;
&lt;%@taglib uri="http://www.springframework.org/tags"  prefix="spring"%&gt;
&lt;%@taglib uri="http://www.springframework.org/tags/form" prefix="form" %&gt;
&lt;%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %&gt;
&lt;%@ taglib uri="http://java.sun.com/jstl/core_rt" prefix="c" %&gt;
&lt;c:url var="rootURL" value="/"/&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Login&lt;/title&gt;
&lt;link href="${rootURL}resources/bootstrap/css/bootstrap.css" media="screen" rel="stylesheet" type="text/css" /&gt;
&lt;script type="text/javascript" src="${rootURL}resources/jquery/jquery-1.10.2.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="${rootURL}resources/bootstrap/js/bootstrap.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="${rootURL}resources/js/app.js"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
		&lt;div class="col-md-6 col-md-offset-2"&gt;	
		&lt;c:if test="${param.error != null}"&gt;
             &lt;div class="alert alert-danger"&gt;
                 Invalid UserName and Password.
             &lt;/div&gt;
         &lt;/c:if&gt;
         &lt;c:if test="${param.logout != null}"&gt;
             &lt;div class="alert alert-success"&gt;
                 You have been logged out.
             &lt;/div&gt;
         &lt;/c:if&gt;	
         &lt;/div&gt;  
            
     &lt;div class="row"&gt;
		&lt;div class="col-md-6 col-md-offset-2"&gt;	
			&lt;h2&gt;User Login Form&lt;/h2&gt;
			&lt;form:form id="loginForm" method="post" action="${rootURL}login" modelAttribute="user" 
		class="form-horizontal" role="form" cssStyle="width: 800px; margin: 0 auto;"&gt;
		  &lt;div class="form-group"&gt;
		    &lt;label for="username" class="col-sm-2 control-label"&gt;UserName*&lt;/label&gt;
		    &lt;div class="col-sm-4"&gt;
		      &lt;input type="text" id="username" name="username" class="form-control" placeholder="UserName" /&gt;
		    &lt;/div&gt;
		  &lt;/div&gt;
		  &lt;div class="form-group"&gt;
		    &lt;label for="password" class="col-sm-2 control-label"&gt;Password*&lt;/label&gt;
		    &lt;div class="col-sm-4"&gt;
		      &lt;input type="password" id="password" name="password" class="form-control" placeholder="Password" /&gt;
		    &lt;/div&gt;
		  &lt;/div&gt;
		  &lt;div class="form-group"&gt;
		    &lt;div class="col-sm-offset-2 col-sm-4"&gt;
		      &lt;input type="submit" class="btn btn-primary" value="Login"&gt;
		    &lt;/div&gt;
		  &lt;/div&gt;
		  
		&lt;/form:form&gt;
	&lt;/div&gt;
&lt;/div&gt;	
&lt;/body&gt;
&lt;/html&gt;</pre>

<span style="color: #000000;">Once we successfully login we can obtain the authenticated use details using </span><b style="color: #000000;"></b><span style="color: #000000;">and secure parts of the view using </span><b style="color: #000000;"></b><span style="color: #000000;">as follows:</span>

<pre class="lang:xhtml decode:true " title="welcome.jsp">&lt;h3&gt;Email: &lt;sec:authentication property="name"/&gt;&lt;/h3&gt;
&lt;h3&gt;
	&lt;sec:authorize access="hasRole('ROLE_ADMIN')"&gt;
		&lt;a href="admin"&gt;Administration&lt;/a&gt;
	&lt;/sec:authorize&gt;
&lt;/h3&gt;
&lt;p&gt;	&lt;a href="logout"&gt;Logout&lt;/a&gt;&lt;/p&gt;
&lt;/body&gt;</pre>

You can find the source code at github <a title="https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo" href="https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo" target="_blank">https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo</a>

<p class="note_alert">
  There are few issues while running the same application on JBoss AS 7.1. I have made few changes to run on JBossAS7.1 and published code at<a title=" https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo-jboss7" href="//github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo-jboss7" target="_blank"> https://github.com/sivaprasadreddy/sivalabs-blog-samples-code/tree/master/springmvc-datajpa-security-demo-jboss7</a>
</p>

<p class="note_normal">
  Published at Codingpedia.org with permission of Siva Reddy<span style="font-style: italic; color: #3a4145;"> &#8211; source <a title="http://www.sivalabs.in/2014/03/springmvc4-spring-data-jpa.html" href="http://www.sivalabs.in/2014/03/springmvc4-spring-data-jpa.html" target="_blank">SpringMVC4 + Spring Data JPA + SpringSecurity configuration using JavaConfig </a></span><span style="font-style: italic; color: #3a4145;"> from </span><a style="font-style: italic; color: #bc360a;" title="http://www.sivalabs.in/" href="http://www.sivalabs.in/" target="_blank">http://www.sivalabs.in/</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/sivalabs.png" alt="Siva Reddy" /> 
  
  <p id="about_author_header">
    <strong>Siva Reddy</strong>
  </p>
  
  <div id="author_details" style="text-align: justify;">
    Passionate java developer, open source enthusiast, blogger. I like to cover Java, Struts, Spring, Hibernate, Ajax Tutorials, How-To's and Best Practices.
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://www.sivalabs.in/" target="_blank"> </a> <a class="icon-googleplus" href="https://plus.google.com/+SivaPrasadReddy" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/sivalabs" target="_blank"> </a> <a class="icon-github" href="https://github.com/sivaprasadreddy/" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>

&nbsp;
