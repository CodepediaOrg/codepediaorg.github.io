---
id: 2171
title: 'Embedded Jetty and Apache CXF: secure REST services with Spring Security'
date: 2015-01-01T16:38:28+00:00
author: Andriy Redko
layout: post
guid: http://www.codingpedia.org/?p=2171
permalink: /aredko/embedded-jetty-and-apache-cxf-secure-rest-services-with-spring-security/
fsb_show_social:
  - 0
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:50:"https://github.com/reta/jax-rs-2.0-spring-security";s:11:"ribbon-type";i:10;}'
fsb_social_facebook:
  - 0
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
dsq_thread_id:
  - 3380031842
categories:
  - java
  - spring
tags:
  - apache cxf
  - authentication
  - jetty
  - rest
  - rest api
  - spring security
---
<p style="text-align: justify;">
  Recently I run into very interesting problem which I thought would take me just a couple of minutes to solve: protecting <a href="http://cxf.apache.org/">Apache CXF</a> (current release <b>3.0.1</b>)/ <a href="https://jax-rs-spec.java.net/">JAX-RS</a> <a href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a> services with <a href="http://projects.spring.io/spring-security/">Spring Security</a> (current stable version <b>3.2.5</b>) in the application running inside embedded <a href="http://www.eclipse.org/jetty/">Jetty</a> container (current release <b>9.2</b>). At the end, it turns out to be very easy, once you understand how things work together and known subtle intrinsic details. This blog post will try to reveal that.<!--more-->
</p>

<p style="text-align: justify;">
  Our example application is going to expose a simple <a href="https://jax-rs-spec.java.net/">JAX-RS</a> / <a href="http://en.wikipedia.org/wiki/Representational_state_transfer">REST</a> service to manage people. However, we do not want everyone to be allowed to do that so the <a href="http://en.wikipedia.org/wiki/Basic_access_authentication">HTTP basic authentication</a> will be required in order to access our endpoint, deployed at <b>http://localhost:8080/api/rest/people</b>. Let us take a look on the <b>PeopleRestService</b> class:
</p>

<pre class="lang:java decode:true ">package com.example.rs;

import javax.json.Json;
import javax.json.JsonArray;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

@Path( "/people" ) 
public class PeopleRestService {
    @Produces( { "application/json" } )
    @GET
    public JsonArray getPeople() {
        return Json.createArrayBuilder()
            .add( Json.createObjectBuilder()
                .add( "firstName", "Tom" )
                .add( "lastName", "Tommyknocker" )
                .add( "email", "a@b.com" ) )
            .build();
    }
}
</pre>

As you can see in the snippet above, nothing is pointing out to the fact that this [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) service is secured, just couple of familiar [JAX-RS](https://jax-rs-spec.java.net/) annotations.

Now, let us declare the desired security configuration following excellent [Spring Security documentation](http://docs.spring.io/spring-security/site/docs/3.2.5.RELEASE/reference/htmlsingle/). There are many ways to configure [Spring Security](http://projects.spring.io/spring-security/) but we are going to show off two of them: using in-memory authentication and using user details service, both built on top of **WebSecurityConfigurerAdapter**. Let us start with in-memory authentication as it is the simplest one:

<pre class="lang:java decode:true ">package com.example.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity( securedEnabled = true )
public class InMemorySecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .withUser( "user" ).password( "password" ).roles( "USER" ).and()
            .withUser( "admin" ).password( "password" ).roles( "USER", "ADMIN" );
    }

    @Override
    protected void configure( HttpSecurity http ) throws Exception {
        http.httpBasic().and()
            .sessionManagement().sessionCreationPolicy( SessionCreationPolicy.STATELESS ).and()
            .authorizeRequests().antMatchers("/**").hasRole( "USER" );
    }
}
</pre>

In the snippet above there two users defined: **user** with the role **USER** and **admin** with the roles **USER**, **ADMIN**. We also protecting all URLs (**/****) by setting authorization policy to allow access only users with role **USER**. Being just a part of the application configuration, let us plug it into the **AppConfig** class using **@Import** annotation.

<pre class="lang:java decode:true">package com.example.config;

import java.util.Arrays;

import javax.ws.rs.ext.RuntimeDelegate;

import org.apache.cxf.bus.spring.SpringBus;
import org.apache.cxf.endpoint.Server;
import org.apache.cxf.jaxrs.JAXRSServerFactoryBean;
import org.apache.cxf.jaxrs.provider.jsrjsonp.JsrJsonpProvider;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.DependsOn;
import org.springframework.context.annotation.Import;

import com.example.rs.JaxRsApiApplication;
import com.example.rs.PeopleRestService;

@Configuration
@Import( InMemorySecurityConfig.class )
public class AppConfig { 
    @Bean( destroyMethod = "shutdown" )
    public SpringBus cxf() {
        return new SpringBus();
    }
 
    @Bean @DependsOn ( "cxf" )
    public Server jaxRsServer() {
        JAXRSServerFactoryBean factory = RuntimeDelegate.getInstance().createEndpoint( jaxRsApiApplication(), JAXRSServerFactoryBean.class );
        factory.setServiceBeans( Arrays.&lt; Object &gt;asList( peopleRestService() ) );
        factory.setAddress( factory.getAddress() );
        factory.setProviders( Arrays.&lt; Object &gt;asList( new JsrJsonpProvider() ) );
        return factory.create();
    }
 
    @Bean 
    public JaxRsApiApplication jaxRsApiApplication() {
        return new JaxRsApiApplication();
    }
 
    @Bean 
    public PeopleRestService peopleRestService() {
        return new PeopleRestService();
    }  
}
</pre>

<p style="text-align: justify;">
  At this point we have all the pieces except the most interesting one: the code which runs embedded <a href="http://www.eclipse.org/jetty/">Jetty</a> instance and creates proper servlet mappings, listeners, passing down the configuration we have created.
</p>

<pre class="lang:java decode:true">package com.example;

import java.util.EnumSet;

import javax.servlet.DispatcherType;

import org.apache.cxf.transport.servlet.CXFServlet;
import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.servlet.FilterHolder;
import org.eclipse.jetty.servlet.ServletContextHandler;
import org.eclipse.jetty.servlet.ServletHolder;
import org.springframework.web.context.ContextLoaderListener;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;
import org.springframework.web.filter.DelegatingFilterProxy;

import com.example.config.AppConfig;

public class Starter {
    public static void main( final String[] args ) throws Exception {
        Server server = new Server( 8080 );
          
        // Register and map the dispatcher servlet
        final ServletHolder servletHolder = new ServletHolder( new CXFServlet() );
        final ServletContextHandler context = new ServletContextHandler();   
        context.setContextPath( "/" );
        context.addServlet( servletHolder, "/rest/*" );  
        context.addEventListener( new ContextLoaderListener() );
   
        context.setInitParameter( "contextClass", AnnotationConfigWebApplicationContext.class.getName() );
        context.setInitParameter( "contextConfigLocation", AppConfig.class.getName() );
   
        // Add Spring Security Filter by the name
        context.addFilter(
            new FilterHolder( new DelegatingFilterProxy( "springSecurityFilterChain" ) ), 
                "/*", EnumSet.allOf( DispatcherType.class )
        );
         
        server.setHandler( context );
        server.start();
        server.join(); 
    }
}
</pre>

<p style="text-align: justify;">
  Most of the code does not require any explanation except the the filter part. This is what I meant by subtle intrinsic detail: the <b>DelegatingFilterProxy</b> should be configured with the filter name which must be exactly <b>springSecurityFilterChain</b>, as <a href="http://projects.spring.io/spring-security/">Spring Security</a> names it. With that, the security rules we have configured are going to apply to any <a href="https://jax-rs-spec.java.net/">JAX-RS</a> service call (the security filter is executed before the <a href="http://cxf.apache.org/">Apache CXF</a> servlet), requiring the full authentication. Let us quickly check that by building and running the project:
</p>

<pre class="lang:sh decode:true ">mvn clean package   
java -jar target/jax-rs-2.0-spring-security-0.0.1-SNAPSHOT.jar</pre>

Issuing the [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) **GET** call without providing username and password does not succeed and returns HTTP [status code 401](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

<pre class="lang:sh decode:true ">&gt; curl -i http://localhost:8080/rest/api/people

HTTP/1.1 401 Full authentication is required to access this resource
WWW-Authenticate: Basic realm="Realm"
Cache-Control: must-revalidate,no-cache,no-store
Content-Type: text/html; charset=ISO-8859-1
Content-Length: 339
Server: Jetty(9.2.2.v20140723)</pre>

The same [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) **GET** call with username and password provided returns successful response (with some JSON generated by the server).

<pre class="lang:sh decode:true ">&gt; curl -i -u user:password http://localhost:8080/rest/api/people

HTTP/1.1 200 OK
Date: Sun, 28 Sep 2014 20:07:35 GMT
Content-Type: application/json
Content-Length: 65
Server: Jetty(9.2.2.v20140723)

[{"firstName":"Tom","lastName":"Tommyknocker","email":"a@b.com"}]</pre>

Excellent, it works like a charm! Turns out, it is really very easy. Also, as it was mentioned before, the in-memory authentication could be replaced with user details service, here is an example how it could be done:

<pre class="lang:java decode:true ">package com.example.config;

import java.util.Arrays;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true)
public class UserDetailsSecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService( userDetailsService() );
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        return new UserDetailsService() {
            @Override
            public UserDetails loadUserByUsername( final String username ) 
                    throws UsernameNotFoundException {
                if( username.equals( "admin" ) ) {
                    return new User( username, "password", true, true, true, true,
                        Arrays.asList(
                            new SimpleGrantedAuthority( "ROLE_USER" ),
                            new SimpleGrantedAuthority( "ROLE_ADMIN" )
                        )
                    );
                } else if ( username.equals( "user" ) ) {
                    return new User( username, "password", true, true, true, true,
                        Arrays.asList(
                            new SimpleGrantedAuthority( "ROLE_USER" )
                        )
                    );
                } 
                    
                return null;
            }
        };
    }

    @Override
    protected void configure( HttpSecurity http ) throws Exception {
        http
           .httpBasic().and()
           .sessionManagement().sessionCreationPolicy( SessionCreationPolicy.STATELESS ).and()
           .authorizeRequests().antMatchers("/**").hasRole( "USER" );
    }
}
</pre>

<p style="text-align: justify;">
  Replacing the <b>@Import( InMemorySecurityConfig.class )</b> with <b>@Import( UserDetailsSecurityConfig.class )</b> in the <b>AppConfig</b> class leads to the same results, as both security configurations define the identical sets of users and their roles.
</p>

<p style="text-align: justify;">
  I hope, this blog post will save you some time and gives a good starting point, as <a href="http://cxf.apache.org/">Apache CXF</a> and <a href="http://projects.spring.io/spring-security/">Spring Security</a> are getting along very well under <a href="http://www.eclipse.org/jetty/">Jetty</a> umbrella!
</p>

The complete source code is available on [GitHub](https://github.com/reta/jax-rs-2.0-spring-security).

<p class="note_normal">
  Published on Codingpedia.org with the permission of Andriy RedkoAndriy Redko</a> – source <a title="http://aredko.blogspot.de/2014/09/embedded-jetty-and-apache-cxf-secure.html" href="http://aredko.blogspot.de/2014/09/embedded-jetty-and-apache-cxf-secure.html" target="_blank">Embedded Jetty and Apache CXF: secure REST services with Spring Security</a> from <a title="http://aredko.blogspot.com" href="http://aredko.blogspot.com" target="_blank">http://aredko.blogspot.com</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="http://1.bp.blogspot.com/_WNHv4iYKMe0/S2Rnco10R2I/AAAAAAAAAAc/eTh_Rkk8V_w/S220/photo.jpg" alt="Andriy Redko" /> 
  
  <p id="about_author_header">
    Andriy Redko {devmind}
  </p>
  
  <div id="author_details" style="text-align: justify;">
    15+ years of software development experience as Programmer/Software Developer/Senior Software Developer/Team Lead/Consultant. I am extensively working with Java EE, Microsoft .NET and Adobe Flex platforms using Java, C#, C++, Groovy, Scala, Ruby, Action Script, Grails, MySQL, PostreSQL, MongoDB, Redis, ...
  </div>
  
  <div id="follow_social" style="clear: both;">
    <div id="social_logos">
      <a class="icon-earth" href="http://aredko.blogspot.com" target="_blank"> </a> <a class="icon-github" href="https://github.com/reta" target="_blank"> </a>
    </div>
    
    <div class="clear">
    </div>
  </div>
</div>
