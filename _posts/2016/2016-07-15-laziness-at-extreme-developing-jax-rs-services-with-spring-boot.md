---
layout: post
title: Laziness at extreme - developing JAX-RS services with Spring Boot
description: "In this post we are going to take a look at how we can use Spring Boot in conjunction with Apache CXF project for a rapid REST(ful) web services development."
author: aredko
permalink: /aredko/laziness-at-extreme-developing-jax-rs-services-with-spring-boot
modified: 2016-07-15
categories: [article]
tags:
    - java
    - spring-boot
    - jax-rs
    - rest
    - apache-cxf

# properties only relevant if the post is being republished
republished: true
original_title: Laziness at extreme - developing JAX-RS services with Spring Boot
original_url: https://aredko.blogspot.ch/2016/04/laziness-at-extreme-developing-jax-rs.html
---


I think it would be fair to state that we, as software developers, are always looking for the ways to write less code
which does more stuff, automagically or not. With this regards, [Spring Boot](https://projects.spring.io/spring-boot/) project, proud member of the Spring portfolio, disrupted the traditional approaches, dramatically speeding up and simplifying Spring-based applications development.

There is a lot to be said about Spring Boot, intrinsic details of how it works and its seamless integration with most if not all Spring projects. But its capabilities go far beyond that, supporting first-class integration with popular Java frameworks.

In this post we are going to take a look at how we can use Spring Boot in conjunction with [Apache CXF](https://cxf.apache.org/)
project for a rapid [REST(ful) web services](https://en.wikipedia.org/wiki/Representational_state_transfer) development. As we are going to see very soon, Spring Boot takes care of quite a lot of boilerplate, letting us to concentrate on the parts of the application which do have real value. Hopefully, at the end of this post the benefits of adopting Spring Boot for your projects become apparent.

With that, let us get started by developing a simple people management REST(ful) web service, wrapped up into familiar **PeopleRestService** [JAX-RS](https://jax-rs-spec.java.net/) resource:
<!--more-->

```java
@Path("/people")
@Component
public class PeopleRestService {
    @GET
    @Produces({MediaType.APPLICATION_JSON})
    public Collection<Person> getPeople() {
        return Collections.singletonList(new Person("a@b.com", "John", "Smith"));
    }
}
```

Not much to add here, pretty simple implementation which returns the hard-coded collection of people.
There are a couple of ways we can package and deploy this JAX-RS service,
but arguably the simplest one is by hosting it inside embedded servlet container
like [Tomcat](https://tomcat.apache.org/), [Jetty](https://www.eclipse.org/jetty/) or [Undertow](https://undertow.io/). With that comes the routine: container initialization, configuring Spring context locations, registering listeners, ... Let us see how Spring Boot can help here by dissecting the Spring context configuration below.

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan(basePackageClasses = PeopleRestService.class)
public class AppConfig {
    @Autowired private PeopleRestService peopleRestService;

    @Bean(destroyMethod = "shutdown")
    public SpringBus cxf() {
        return new SpringBus();
    }

    @Bean(destroyMethod = "destroy") @DependsOn("cxf")
    public Server jaxRsServer() {
        final JAXRSServerFactoryBean factory = new JAXRSServerFactoryBean();

        factory.setServiceBean(peopleRestService);
        factory.setProvider(new JacksonJsonProvider());
        factory.setBus(cxf());
        factory.setAddress("/");

        return factory.create();
    }

    @Bean
    public ServletRegistrationBean cxfServlet() {
        final ServletRegistrationBean servletRegistrationBean =
            new ServletRegistrationBean(new CXFServlet(), "/api/*");
        servletRegistrationBean.setLoadOnStartup(1);
        return servletRegistrationBean;
    }
}
```

The `AppConfig` class looks like a typical Spring Java-based configuration except this unusual `@EnableAutoConfiguration` annotation, which with no surprise comes from Spring Boot module. Under the hood, this annotation enables a complex and intelligent process of guessing, among many other things, what kind of the application we are going to run and what kind of Spring beans we may need for our application. With this configuration in place, we just need to have a runner for our application, also with a bit of Spring Boot flavor:

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(AppConfig.class, args);
    }
}
```

Having @SpringBootApplication meta-annotation and using SpringApplication to initialize our Spring context, we have a full-fledged runnable Java application, which could be run from Apache Maven using Spring Boot plugin:

```shell
mvn spring-boot:run
```

Or packaged as a single runnable uber-JAR and invoked from command line:

```shell
mvn package
java -jar target/jax-rs-2.0-cxf-spring-boot-0.0.1-SNAPSHOT.jar
```

And that's it, just a couple of annotations along with a single line of code (main method). Once we run the application, we could make sure that our people management REST(ful) web service is deployed properly and is fully operational:

```shell
$ curl -i http://localhost:8080/api/people

HTTP/1.1 200 OK
Content-Type: application/json;charset=utf-8
Transfer-Encoding: chunked
Server: Jetty(9.3.8.v20160314)

[{"email":"a@b.com","firstName":"John","lastName":"Smith"}]
```

At this point you may wonder how does it work? We have not dealt with servlet container anywhere so how come Jetty is serving our requests? The truth is, we only need to include our container of choice as a dependency, for example using Apache Maven's `pom.xml` file:

```xml
<dependency>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-server</artifactId>
    <version>9.3.8.v20160314</version>
</dependency>
```

Spring Boot along with `@EnableAutoConfiguration/@SpringBootApplication` does the rest: it detects the presence of Jetty in the classpath, comes to a valid conclusion that our intention is to run web application and complement the Spring context with the necessary pieces. Isn't it just brilliant?

It would be unfair to finish up without covering yet another important feature of the Spring Boot project: integration testing support. In this regards Spring Boot takes the same approach and provides a couple of annotations to take off all the scaffolding we would have to write ourselves otherwise. For example:

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = AppConfig.class)
@WebIntegrationTest(randomPort = true)
public class PeopleRestServiceIntegrationTest {
    @Value("${local.server.port}") private int port;

    @Before
    public void setUp() {
        RestAssured.port = port;
    }

    @Test
    public void testListOfPersonsIsBeingReturnedSuccessfuly() {
        given()
            .when()
            .contentType(ContentType.JSON)
            .get("/api/people")
            .then()
            .statusCode(200)
            .log()
            .ifValidationFails();
    }
}
```

Just two annotations, @SpringApplicationConfiguration (please notice that we are using the same configuration in test as for the main application) and @WebIntegrationTest (which takes the specifics of the web application testing into account and runs the embedded servlet container on random port), and we have full-fledged integration test against our people management JAX-RS service. The port which servlet container is running on is available through local.server.port environment property so we can configure REST-assured in the test background. Easy and simple.

In this post we have just looked at the one specific use case of using Spring Boot to increase the development velocity of your JAX-RS projects. Many, many things become very trivial with Spring Boot, with more and more intelligence being added with every single release, not to mention excellent integration with your IDE of choice. I hope you really got excited about Spring Boot and eager to learn more about it. It is worth the time and effort.

The complete project is available on [Github](https://github.com/reta/jax-rs-2.0-cxf-spring-boot)
