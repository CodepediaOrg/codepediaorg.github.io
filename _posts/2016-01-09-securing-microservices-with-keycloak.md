---
id: 2605
title: Securing Microservices with Keycloak
date: 2016-01-09T18:50:16+00:00
author: keycloak keycloak
layout: post
guid: http://www.codingpedia.org/?p=2605
permalink: /keycloak/securing-microservices-with-keycloak/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4476603429
fsb_social_facebook:
  - 2
fsb_social_google:
  - 4
fsb_social_linkedin:
  - 6
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - Java EE
  - javascript
  - security
tags:
  - keycloak
  - micro-services
  - openid connect
  - security
  - sso
---
<p style="text-align: justify;">
  In a traditional multi-tiered architecture like the one shown in the picture below a server-side web tier deals with authenticating the user by calling out to a relational database or an LDAP server. An HTTP session is then created containing the required authentication and user details. The security context is propagated between the tiers within the application server so there&#8217;s no need to re-authenticate the user.
</p>

<p style="text-align: justify;">
  <img src="http://1.bp.blogspot.com/-036NE1K0vZs/VV29q9kDXoI/AAAAAAAAStI/RLBoK8ITwOE/s1600/old-architecture.png" /><!--more-->
</p>

<p style="text-align: justify;">
  <span>With a microservice architecture the server-side web application is gone, instead we have HTML5 and mobile applications on the client side. The applications invoke multiple services, which may in turn call other services. In this architecture there&#8217;s no longer a single layer that can deal with authentication and usually it&#8217;s stateless as well so there&#8217;s no HTTP session.</span>
</p>

<p style="text-align: justify;">
  <img src="http://4.bp.blogspot.com/-HGsYTjn6IFY/VV25MDoWHYI/AAAAAAAASs4/pjazmyksZDo/s1600/microservices-architecture.png" />
</p>

<p style="text-align: justify;">
  As microservices is all about having many smaller services each that deal with one distinct task the obvious solution to security is an authentication and authorization service. This is where Keycloak and OpenID Connect comes to the rescue. Keycloak provides the service you need to secure micro services.
</p>

<p style="text-align: justify;">
  The first step to securing micro services is authenticating the user. This is done by adding the Keycloak JavaScript adapter to your HTML5 application. For mobile applications there&#8217;s a Keycloak Cordova adapter, but there&#8217;s also native support though the <a href="http://www.aerogear.org/">AeroGear project</a>.
</p>

<p style="text-align: justify;">
  In an HTML5 application you just need to add a login button and that&#8217;s pretty much it. When the user clicks the login button the users browser is redirected to the login screen on the Keycloak server. The user then authenticates with the Keycloak server. As the authentication is done by the Keycloak server and not your application it&#8217;s easy to add support for multi-factor authentication or social logins without having to change anything in your application.
</p>

<p style="text-align: justify;">
  <img border="0" src="http://2.bp.blogspot.com/-NjOd_xAs0IQ/VV3EcENU3hI/AAAAAAAAStY/t_XPePYNhHs/s1600/auth.png" />
</p>

<p style="text-align: justify;">
  Once the user is authenticated, Keycloak gives the application a token. The token contains details about the user as well as permissions the user has.
</p>

<p style="text-align: justify;">
  <img border="0" src="http://1.bp.blogspot.com/-_kp0j2ylQoo/VV3EcWeIqJI/AAAAAAAAStc/y8uDGorcMKE/s1600/microservices-architecture-with-kc.png" />
</p>

<p style="text-align: justify;">
  The second step in securing micro services is to secure them. Again, with Keycloak this is easy to do with our adapters. We&#8217;ve got JavaEE adapters, NodeJS adapters and are planning to adding more in the future. If we don&#8217;t have an adapter it&#8217;s also relatively easy to verify the tokens yourself. A token is basically just a signed JSON document and can be verified by the services itself or by invoking the Keycloak server.
</p>

<p style="text-align: justify;">
  If a service needs to invoke another service it can pass on the token it received, which will invoke the other service with the users permissions. Soon we&#8217;ll add support for services to authenticate directly with Keycloak to be able to invoke other services with their own permissions, not just on behalf of users.
</p>

<p style="text-align: justify;">
  For more details about OpenID Connect you can look at the <a href="http://openid.net/connect/">OpenID Connect website</a>, but the nice thing is with Keycloak you don&#8217;t really need to know the low level details so it&#8217;s completely optional. As an alternative just go straight to the <a href="http://www.keycloak.org/">Keycloak website</a>, download the server and adapters, and check out our documentation and many examples.
</p>

<p style="text-align: justify;" class="note_normal">
  Published at Codingpedia.org with the permission of Keycloak&#8217;s team – source <a title="Are you getting worked up over code duplication?" href="http://blog.keycloak.org/2015/05/securing-microservices.html" target="_blank">Securing Microservices</a> from <a title="http://johannesbrodwall.com/" href="http://keycloak.org" target="_blank">http://keycloak.org</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="https://raw.githubusercontent.com/keycloak/keycloak/master/misc/logo/keycloak_icon_256px.png" alt="Keycloak-logo" /> 
  
  <p id="about_author_header">
    <strong>Keycloak</strong>
  </p>
  
  <div id="social_logos_up">
    <a class="icon-earth" href="http://keycloak.jboss.org/" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/keycloak" target="_blank"> </a> <a class="icon-github" href="https://github.com/keycloak" target="_blank"> </a>
  </div>
  
  <div id="author_details" style="text-align: justify;">
    Integrated SSO and IDM for browser apps and RESTful web services. Built on top of the OAuth 2.0, Open ID Connect, JSON Web Token (JWT) and SAML 2.0 specifications. Keycloak has tight integration with a variety of platforms and has a HTTP security proxy service where we don't have tight integration. Options are to deploy it with an existing app server, as a black-box appliance, or as an Openshift cloud service and/or cartridge.
  </div>
  
  <div class="clear">
  </div>
</div>
