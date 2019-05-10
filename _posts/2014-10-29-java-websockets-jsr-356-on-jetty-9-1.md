---
id: 2060
title: Java WebSockets (JSR-356) on Jetty 9.1
date: 2014-10-29T21:09:39+00:00
author: Andriy Redko
layout: post
guid: http://www.codingpedia.org/?p=2060
permalink: /aredko/java-websockets-jsr-356-on-jetty-9-1/
gr_overridden:
  - 1
gr_options:
  - 'a:3:{s:13:"enable-ribbon";s:4:"Show";s:10:"github-url";s:48:"https://github.com/reta/jetty-web-sockets-jsr356";s:11:"ribbon-type";i:10;}'
fsb_show_social:
  - 0
dsq_thread_id:
  - 3170165568
fsb_social_facebook:
  - 0
fsb_social_google:
  - 2
fsb_social_linkedin:
  - 2
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - java
  - spring
tags:
  - jetty
  - jetty9
  - JSON processing
  - jsr353
  - jsr356
  - WebSockets
---
<p style="text-align: justify;">
  <a style="color: #888855;" href="http://www.eclipse.org/jetty/documentation/current/jetty-javaee.html">Jetty 9.1</a> is finally released, bringing <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">Java WebSockets (JSR-356)</a> to non-EE environments. It&#8217;s awesome news and today&#8217;s post will be about using this great new API along with <a style="color: #888855;" href="http://projects.spring.io/spring-framework/">Spring Framework</a>.
</p>

<p style="text-align: justify;">
  <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">JSR-356</a> defines concise, annotation-based model to allow modern Java web applications easily create bidirectional communication channels using <a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets API</a>. It covers not only server-side, but client-side as well, making this API really simple to use everywhere.<!--more-->
</p>

<p style="color: #333333; text-align: justify;">
  Let&#8217;s get started! Our goal would be to build a <a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets</a> server which accepts messages from the clients and broadcasts them to all other clients currently connected. To begin with, let&#8217;s define the message format, which server and client will be exchanging, as this simple <b>Message</b> class. We can limit ourselves to something like a <b>String</b>, but I would like to introduce to you the power of another new API &#8211; <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=353">Java API for JSON Processing (JSR-353)</a>.
</p>

<pre class="lang:java decode:true ">package com.example.services;

public class Message {
    private String username;
    private String message;
 
    public Message() {
    }
 
    public Message( final String username, final String message ) {
        this.username = username;
        this.message = message;
    }
 
    public String getMessage() {
        return message;
    }
 
    public String getUsername() {
        return username;
    }

    public void setMessage( final String message ) {
        this.message = message;
    }
 
    public void setUsername( final String username ) {
        this.username = username;
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">To separate the declarations related to the server and the client, </span><a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">JSR-356</a><span style="color: #333333;"> defines two basic annotations: </span><b style="color: #333333;">@ServerEndpoint</b><span style="color: #333333;"> and </span><b style="color: #333333;">@ClientEndpoit</b><span style="color: #333333;"> respectively. Our client endpoint, let&#8217;s call it <b>BroadcastClientEndpoint</b>, will simply listen for the messages the server sends: </span>
</p>

<pre class="lang:java decode:true ">package com.example.services;

import java.io.IOException;
import java.util.logging.Logger;

import javax.websocket.ClientEndpoint;
import javax.websocket.EncodeException;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;

@ClientEndpoint
public class BroadcastClientEndpoint {
    private static final Logger log = Logger.getLogger( 
        BroadcastClientEndpoint.class.getName() );
 
    @OnOpen
    public void onOpen( final Session session ) throws IOException, EncodeException  {
        session.getBasicRemote().sendObject( new Message( "Client", "Hello!" ) );
    }

    @OnMessage
    public void onMessage( final Message message ) {
        log.info( String.format( "Received message '%s' from '%s'",
            message.getMessage(), message.getUsername() ) );
    }
}</pre>

&nbsp;

That&#8217;s literally it! Very clean, self-explanatory piece of code:

  * **@OnOpen** is being called when client got connected to the server and
  * **@OnMessage** is being called every time server sends a message to the client.

<p style="color: #333333; text-align: justify;">
  Yes, it&#8217;s very simple but there is a caveat: <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">JSR-356</a> implementation can handle any simple objects but not the complex ones like <b>Message</b> is. To manage that, <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">JSR-356</a> introduces the concept of <b>encoders</b> and <b>decoders</b>.
</p>

<p style="color: #333333; text-align: justify;">
  We all love <a style="color: #888855;" href="http://www.json.org/">JSON</a>, so why don&#8217;t we define our own <a style="color: #888855;" href="http://www.json.org/">JSON</a> encoder and decoder? It&#8217;s an easy task which <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=353">Java API for JSON Processing (JSR-353)</a> can handle for us. To create an encoder, you only need to implement <b>Encoder.Text<Message></b> and basically serialize your object to some string, in our case to <a style="color: #888855;" href="http://www.json.org/">JSON</a> string, using <a style="color: #888855;" href="http://docs.oracle.com/javaee/7/api/javax/json/JsonObjectBuilder.html">JsonObjectBuilder</a>.
</p>

<pre class="lang:java decode:true ">package com.example.services;

import javax.json.Json;
import javax.json.JsonReaderFactory;
import javax.websocket.EncodeException;
import javax.websocket.Encoder;
import javax.websocket.EndpointConfig;

public class Message {
    public static class MessageEncoder implements Encoder.Text&lt; Message &gt; {
        @Override
        public void init( final EndpointConfig config ) {
        }
  
        @Override
        public String encode( final Message message ) throws EncodeException {
            return Json.createObjectBuilder()
                .add( "username", message.getUsername() )
                .add( "message", message.getMessage() )
                .build()
                .toString();
        }
  
        @Override
        public void destroy() {
        }
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">For decoder part, everything looks very similar, we have to implement </span><b style="color: #333333;">Decoder.Text< Message ></b><span style="color: #333333;"> and deserialize our object from string, this time using </span><a style="color: #888855;" href="http://docs.oracle.com/javaee/7/api/javax/json/JsonReader.html">JsonReader</a><span style="color: #333333;">.</span>
</p>

<pre class="lang:java decode:true">package com.example.services;

import javax.json.JsonObject;
import javax.json.JsonReader;
import javax.json.JsonReaderFactory;
import javax.websocket.DecodeException;
import javax.websocket.Decoder;

public class Message {
    public static class MessageDecoder implements Decoder.Text&lt; Message &gt; {
        private JsonReaderFactory factory = Json.createReaderFactory( Collections.&lt; String, Object &gt;emptyMap() );
  
        @Override
        public void init( final EndpointConfig config ) {
        }
  
        @Override
        public Message decode( final String str ) throws DecodeException {
            final Message message = new Message();
   
            try( final JsonReader reader = factory.createReader( new StringReader( str ) ) ) {
                final JsonObject json = reader.readObject();
                message.setUsername( json.getString( "username" ) );
                message.setMessage( json.getString( "message" ) );
            }
   
            return message;
        }
  
        @Override
        public boolean willDecode( final String str ) {
            return true;
        }
  
        @Override
        public void destroy() {
        }
    }
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">And as a final step, we need to tell the client (and the server, they share same decoders and encoders) that we have encoder and decoder for our messages. The easiest thing to do that is just by declaring them as part of </span><b style="color: #333333;">@ServerEndpoint</b><span style="color: #333333;"> and </span><b style="color: #333333;">@ClientEndpoit</b><span style="color: #333333;"> annotations.</span>
</p>

<pre class="lang:java decode:true ">import com.example.services.Message.MessageDecoder;
import com.example.services.Message.MessageEncoder;

@ClientEndpoint( encoders = { MessageEncoder.class }, decoders = { MessageDecoder.class } )
public class BroadcastClientEndpoint {
}</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">To make client&#8217;s example complete, we need some way to connect to the server using </span><b style="color: #333333;">BroadcastClientEndpoint </b><span style="color: #333333;">and basically exchange messages. The </span><b style="color: #333333;">ClientStarter</b><span style="color: #333333;"> class finalizes the picture:</span>
</p>

<pre class="lang:java decode:true">package com.example.ws;

import java.net.URI;
import java.util.UUID;

import javax.websocket.ContainerProvider;
import javax.websocket.Session;
import javax.websocket.WebSocketContainer;

import org.eclipse.jetty.websocket.jsr356.ClientContainer;

import com.example.services.BroadcastClientEndpoint;
import com.example.services.Message;

public class ClientStarter {
    public static void main( final String[] args ) throws Exception {
        final String client = UUID.randomUUID().toString().substring( 0, 8 );
  
        final WebSocketContainer container = ContainerProvider.getWebSocketContainer();    
        final String uri = "ws://localhost:8080/broadcast";  
  
        try( Session session = container.connectToServer( BroadcastClientEndpoint.class, URI.create( uri ) ) ) {
            for( int i = 1; i &lt;= 10; ++i ) {
                session.getBasicRemote().sendObject( new Message( client, "Message #" + i ) );
                Thread.sleep( 1000 );
            }
        }
  
        // Application doesn't exit if container's threads are still running
        ( ( ClientContainer )container ).stop();
    }
}</pre>

<p style="text-align: justify;">
  Just couple of comments what this code does: we are connecting to <a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets</a> endpoint at <b>ws://localhost:8080/broadcast</b>, randomly picking some <b>client</b> name (from UUID) and generating 10 messages, every with 1 second delay (just to be sure we have time to receive them all back).
</p>

<p style="color: #333333; text-align: justify;">
  Server part doesn&#8217;t look very different and at this point could be understood without any additional comments (except may be the fact that server just broadcasts every message it receives to all connected clients). Important to mention here: a new instance of the server endpoint is created every time new client connects to it (that&#8217;s why <b>peers</b> collection is static), it&#8217;s a default behavior and could be easily changed.
</p>

<pre class="lang:java decode:true ">package com.example.services;

import java.io.IOException;
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;

import javax.websocket.EncodeException;
import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;

import com.example.services.Message.MessageDecoder;
import com.example.services.Message.MessageEncoder;

@ServerEndpoint( 
    value = "/broadcast", 
    encoders = { MessageEncoder.class }, 
    decoders = { MessageDecoder.class } 
) 
public class BroadcastServerEndpoint {
    private static final Set&lt; Session &gt; sessions = 
        Collections.synchronizedSet( new HashSet&lt; Session &gt;() ); 
   
    @OnOpen
    public void onOpen( final Session session ) {
        sessions.add( session );
    }

    @OnClose
    public void onClose( final Session session ) {
        sessions.remove( session );
    }

    @OnMessage
    public void onMessage( final Message message, final Session client ) 
            throws IOException, EncodeException {
        for( final Session session: sessions ) {
            session.getBasicRemote().sendObject( message );
        }
    }
}</pre>

<span style="color: #333333;">In order this endpoint to be available for connection, we should start the </span><a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets</a><span style="color: #333333;"> container and register this endpoint inside it. As always, </span><a style="color: #888855;" href="http://www.eclipse.org/jetty/documentation/current/jetty-javaee.html">Jetty 9.1</a><span style="color: #333333;"> is runnable in embedded mode effortlessly:</span>

<pre class="lang:java decode:true">package com.example.ws;

import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.servlet.DefaultServlet;
import org.eclipse.jetty.servlet.ServletContextHandler;
import org.eclipse.jetty.servlet.ServletHolder;
import org.eclipse.jetty.websocket.jsr356.server.deploy.WebSocketServerContainerInitializer;
import org.springframework.web.context.ContextLoaderListener;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;

import com.example.config.AppConfig;

public class ServerStarter  {
    public static void main( String[] args ) throws Exception {
        Server server = new Server( 8080 );
        
        // Create the 'root' Spring application context
        final ServletHolder servletHolder = new ServletHolder( new DefaultServlet() );
        final ServletContextHandler context = new ServletContextHandler();

        context.setContextPath( "/" );
        context.addServlet( servletHolder, "/*" );
        context.addEventListener( new ContextLoaderListener() );   
        context.setInitParameter( "contextClass", AnnotationConfigWebApplicationContext.class.getName() );
        context.setInitParameter( "contextConfigLocation", AppConfig.class.getName() );
   
        server.setHandler( context );
        WebSocketServerContainerInitializer.configureContext( context );        
        
        server.start();
        server.join(); 
    }
}</pre>

&nbsp;

<p style="text-align: justify;">
  <span style="color: #333333;">The most important part of the snippet above is </span><b style="color: #333333;">WebSocketServerContainerInitializer.configureContext</b><span style="color: #333333;">: it&#8217;s actually creates the instance of </span><a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets</a><span style="color: #333333;"> container. Because we haven&#8217;t added any endpoints yet, the container basically sits here and does nothing. </span><a style="color: #888855;" href="http://projects.spring.io/spring-framework/">Spring Framework</a><span style="color: #333333;"> and </span><b style="color: #333333;">AppConfig</b><span style="color: #333333;"> configuration class will do this last wiring for us.</span>
</p>

<pre class="lang:java decode:true ">package com.example.config;

import javax.annotation.PostConstruct;
import javax.inject.Inject;
import javax.websocket.DeploymentException;
import javax.websocket.server.ServerContainer;
import javax.websocket.server.ServerEndpoint;
import javax.websocket.server.ServerEndpointConfig;

import org.eclipse.jetty.websocket.jsr356.server.AnnotatedServerEndpointConfig;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.context.WebApplicationContext;

import com.example.services.BroadcastServerEndpoint;

@Configuration
public class AppConfig  {
    @Inject private WebApplicationContext context;
    private ServerContainer container;
 
    public class SpringServerEndpointConfigurator extends ServerEndpointConfig.Configurator {
        @Override
        public &lt; T &gt; T getEndpointInstance( Class&lt; T &gt; endpointClass ) 
                throws InstantiationException {
            return context.getAutowireCapableBeanFactory().createBean( endpointClass );   
        }
    }
 
    @Bean
    public ServerEndpointConfig.Configurator configurator() {
        return new SpringServerEndpointConfigurator();
    }
 
    @PostConstruct
    public void init() throws DeploymentException {
        container = ( ServerContainer )context.getServletContext().
            getAttribute( javax.websocket.server.ServerContainer.class.getName() );
  
        container.addEndpoint( 
            new AnnotatedServerEndpointConfig( 
                BroadcastServerEndpoint.class, 
                BroadcastServerEndpoint.class.getAnnotation( ServerEndpoint.class )  
            ) {
                @Override
                public Configurator getConfigurator() {
                    return configurator();
                }
            }
        );
    }  
}</pre>

<p style="text-align: justify;">
  As we mentioned earlier, by default container will create new instance of server endpoint every time new client connects, and it does so by calling constructor, in our case <b>BroadcastServerEndpoint.class.newInstance()</b>. It might be a desired behavior but because we are using <a style="color: #888855;" href="http://projects.spring.io/spring-framework/">Spring Framework</a> and dependency injection, such new objects are basically unmanaged beans. Thanks to very well-thought (in my opinion) design of <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">JSR-356</a>, it&#8217;s actually quite easy to provide your own way of creating endpoint instances by implementing <b>ServerEndpointConfig.Configurator</b>.
</p>

<p style="text-align: justify;">
  The <b>SpringServerEndpointConfigurator</b> is an example of such implementation: it&#8217;s creates new managed bean every time new endpoint instance is being asked (if you want single instance, you can create singleton of the endpoint as a bean in <b>AppConfig</b> and return it all the time).
</p>

<p style="text-align: justify;">
  The way we retrieve the <a style="color: #888855;" href="http://www.w3.org/TR/websockets/">WebSockets</a> container is <a style="color: #888855;" href="http://www.eclipse.org/jetty">Jetty</a>-specific: from the attribute of the context with name<b>&#8220;javax.websocket.server.ServerContainer&#8221;</b> (it probably might change in the future). Once container is there, we are just adding new (managed!) endpoint by providing our own <b>ServerEndpointConfig</b> (based on <b>AnnotatedServerEndpointConfig</b> which <a style="color: #888855;" href="http://www.eclipse.org/jetty">Jetty</a> kindly provides already).
</p>

<p style="color: #333333;">
  To build and run our server and clients, we need just do that:
</p>

<pre class="lang:sh decode:true ">mvn clean package
java -jar target\jetty-web-sockets-jsr356-0.0.1-SNAPSHOT-server.jar // run server
java -jar target/jetty-web-sockets-jsr356-0.0.1-SNAPSHOT-client.jar // run yet another client</pre>

<p style="text-align: justify;">
  <span style="color: #333333;">As an example, by running server and couple of clients (I run 4 of them, &#8216;</span><b style="color: #333333;">392f68ef</b><span style="color: #333333;">&#8216;, &#8216;</span><b style="color: #333333;">8e3a869d</b><span style="color: #333333;">&#8216;, &#8216;</span><b style="color: #333333;">ca3a06d0</b><span style="color: #333333;">&#8216;, &#8216;</span><b style="color: #333333;">6cb82119</b><span style="color: #333333;">&#8216;) you might see by the output in the console that each client receives all the messages from all other clients (including its own messages):</span>
</p>

<pre class="lang:sh decode:true">Nov 29, 2013 9:21:29 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Hello!' from 'Client'
Nov 29, 2013 9:21:29 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #1' from '392f68ef'
Nov 29, 2013 9:21:29 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #2' from '8e3a869d'
Nov 29, 2013 9:21:29 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #7' from 'ca3a06d0'
Nov 29, 2013 9:21:30 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #4' from '6cb82119'
Nov 29, 2013 9:21:30 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #2' from '392f68ef'
Nov 29, 2013 9:21:30 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #3' from '8e3a869d'
Nov 29, 2013 9:21:30 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #8' from 'ca3a06d0'
Nov 29, 2013 9:21:31 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #5' from '6cb82119'
Nov 29, 2013 9:21:31 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #3' from '392f68ef'
Nov 29, 2013 9:21:31 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #4' from '8e3a869d'
Nov 29, 2013 9:21:31 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #9' from 'ca3a06d0'
Nov 29, 2013 9:21:32 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #6' from '6cb82119'
Nov 29, 2013 9:21:32 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #4' from '392f68ef'
Nov 29, 2013 9:21:32 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #5' from '8e3a869d'
Nov 29, 2013 9:21:32 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #10' from 'ca3a06d0'
Nov 29, 2013 9:21:33 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #7' from '6cb82119'
Nov 29, 2013 9:21:33 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #5' from '392f68ef'
Nov 29, 2013 9:21:33 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #6' from '8e3a869d'
Nov 29, 2013 9:21:34 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #8' from '6cb82119'
Nov 29, 2013 9:21:34 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #6' from '392f68ef'
Nov 29, 2013 9:21:34 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #7' from '8e3a869d'
Nov 29, 2013 9:21:35 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #9' from '6cb82119'
Nov 29, 2013 9:21:35 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #7' from '392f68ef'
Nov 29, 2013 9:21:35 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #8' from '8e3a869d'
Nov 29, 2013 9:21:36 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #10' from '6cb82119'
Nov 29, 2013 9:21:36 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #8' from '392f68ef'
Nov 29, 2013 9:21:36 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #9' from '8e3a869d'
Nov 29, 2013 9:21:37 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #9' from '392f68ef'
Nov 29, 2013 9:21:37 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #10' from '8e3a869d'
Nov 29, 2013 9:21:38 PM com.example.services.BroadcastClientEndpoint onMessage
INFO: Received message 'Message #10' from '392f68ef'
2013-11-29 21:21:39.260:INFO:oejwc.WebSocketClient:main: Stopped org.eclipse.jetty.websocket.client.WebSocketClient@3af5f6dc</pre>

<p style="text-align: justify;">
  Awesome! I hope this introductory blog post shows how easy it became to use modern web communication protocols in Java, thanks to <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=356">Java WebSockets (JSR-356)</a>, <a style="color: #888855;" href="http://jcp.org/en/jsr/detail?id=353">Java API for JSON Processing (JSR-353)</a> and great projects such as <a style="color: #888855;" href="http://www.eclipse.org/jetty/documentation/current/jetty-javaee.html">Jetty 9.1</a>!
</p>

<p style="color: #333333;">
  As always, complete project is available on <a style="color: #888855;" href="https://github.com/reta/jetty-web-sockets-jsr356">GitHub</a>.
</p>

<p class="note_normal" style="color: #333333;">
  Published at Codingpedia.org with the permission of Andriy RedkoAndriy Redko</a> – source <a title="http://aredko.blogspot.de/2013/11/java-websockets-jsr-356-on-jetty-91.html" href="http://aredko.blogspot.de/2013/11/java-websockets-jsr-356-on-jetty-91.html" target="_blank">Java WebSockets (JSR-356) on Jetty 9.1</a> from <a title="http://aredko.blogspot.com" href="http://aredko.blogspot.com/" target="_blank">http://aredko.blogspot.com</a>
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
