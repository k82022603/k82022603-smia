## Spring Microservices in Action - Second Edition. Chapter 10 Gatewayserver

# Introduction
Welcome to Spring Microservices in Action, Chapter 10.  Chapter 10 introduces the Spring Cloud Config service and how you can use it managed the configuration of your microservices.  By the time you are done reading this chapter you will have built and/or deployed:

1.  A Spring Cloud Config server that is deployed as Docker container and can manage a services configuration information using a file system/ classpath or GitHub-based repository.

## Initial Configuration
1.	Apache Maven (http://maven.apache.org)  All of the code examples in this book have been compiled with Java version 11.
2.	Git Client (http://git-scm.com)
3.  Docker(https://www.docker.com/products/docker-desktop)

## How To Use

To clone and run this application, you'll need [Git](https://git-scm.com), [Maven](https://maven.apache.org/), [Java 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html). From your command line:

```bash
# Clone this repository
$ git clone https://github.com/ihuaylupo/manning-smia

## Changes
1. chapter10/Gatewayserver/pom.xml 
- spring-boot-starter-parent 2.2.6.RELEASE --> 2.7.1
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.1</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
- java version & spring-cloud version
    <properties>
		<java.version>17</java.version>
		<spring-cloud.version>2021.0.3</spring-cloud.version>
	</properties>
2. chapter10/Gatewayserver/src/main/resources/application.yml
- move bootstrap.yml to application.yml

3. chapter10/gatewayserver/src/main/resources/application.yml
spring:
    main: 
      allow-bean-definition-overriding: true
    application:
     name: gateway-server 
    config: 
      import: "optional:configserver:http://configserver:8071,optional:configserver:http://localhost:8071"
    cloud:
      config:
        label: null 
        # label is an optional git label (defaults to master)
        # or use like belows in the cloudconfig server
        # /{application}/{profile}[/{label}]
        # /{application}-{profile}.yml
        # /{label}/{application}-{profile}.yml
        # /{application}-{profile}.properties
        # /{label}/{application}-{profile}.properties

4. try below
mvn clean install spring-boot:repackage

# The build command (1) using Dockerfile
## Changes
1. chapter10/Gatewayserver/Dockerfile
- java version & LABEL
FROM openjdk:17-slim as build
LABEL maintainer="k82022603 <k82022603@gmail.com>"
FROM openjdk:17-slim

## The build command
- mvn clean package dockerfile:build
- run "docker images" to find out the docker image
REPOSITORY                 TAG              IMAGE ID       CREATED         SIZE
ostock/Gatewayserver        0.0.1-SNAPSHOT   b03f832d2cfb   7 minutes ago   442MB


# The Run command using the docker
# The Run command
#$ docker run -d --name "Gatewayserver" -p 8071:8071 ostock/Gatewayserver:0.0.1-SNAPSHOT
$ docker run -d -p 8071:8071 ostock/Gatewayserver:0.0.1-SNAPSHOT
$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS         PORTS                                       NAMES
c3fbf002187c   ostock/Gatewayserver:0.0.1-SNAPSHOT   "java -cp app:app/li…"   3 minutes ago   Up 3 minutes   0.0.0.0:8071->8071/tcp, :::8071->8071/tcp   Gatewayserver


# The Run command using (2) mvn sprint-boot:run
# Go into the repository, by chaning to the directory where you have downloaded the 
# Chapter 10 source code
$ cd chapter10/Gatewayserver

# To run the code examples for Chapter 10 configsever, open a command-line 
# window and execute the following command(1):
$ mvn clean spring-boot:run

1. No spring.config.import property has been defined
https://stackoverflow.com/questions/67507452/no-spring-config-import-property-has-been-defined
1.1 add below
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
2. No spring.config.import property has been defined
Description:
No spring.config.import property has been defined
Action:
Add a spring.config.import=configserver: property to your configuration.
        If configuration is not required add spring.config.import=optional:configserver: instead.
        To disable this check, set spring.cloud.config.enabled=false or
        spring.cloud.config.import-check.enabled=false.
--> chapter10/Gatewayserver/src/main/resources/application.yml
     cloud:
         config: 
             uri: http://configserver:8071
------>
    config: 
      import: "optional:configserver:http://configserver:8071,optional:configserver:http://localhost:8071"

--> RESULT

2022-07-26 19:42:17.480  INFO 12661 --- [           main] c.o.eureka.GatewayserverApplication       : Starting GatewayserverApplication using Java 17.0.3 on jeff01 with PID 12661 (/home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter10/Gatewayserver/target/classes started by jeff in /home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter10/Gatewayserver)
2022-07-26 19:42:17.482  INFO 12661 --- [           main] c.o.eureka.GatewayserverApplication       : No active profile set, falling back to 1 default profile: "default"
2022-07-26 19:42:17.514  INFO 12661 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://localhost:8071
2022-07-26 19:42:17.514  INFO 12661 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Located environment: name=eureka-server, profiles=[default], label=null, version=null, state=null
2022-07-26 19:42:17.514  INFO 12661 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://configserver:8071
2022-07-26 19:42:17.514  INFO 12661 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Connect Timeout Exception on Url - http://configserver:8071. Will be trying the next url if available
2022-07-26 19:42:17.514  WARN 12661 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Could not locate PropertySource ([ConfigServerConfigDataResource@355e34c7 uris = array<String>['http://configserver:8071'], optional = true, profiles = list['default']]): I/O error on GET request for "http://configserver:8071/eureka-server/default": configserver; nested exception is java.net.UnknownHostException: configserver
2022-07-26 19:42:18.190  INFO 12661 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=2ff5d176-2a59-363f-be0c-057f2c2b78a5
2022-07-26 19:42:18.386  INFO 12661 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8070 (http)
2022-07-26 19:42:18.394  INFO 12661 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-07-26 19:42:18.394  INFO 12661 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.64]
2022-07-26 19:42:18.506  INFO 12661 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-07-26 19:42:18.506  INFO 12661 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 991 ms
2022-07-26 19:42:18.823  INFO 12661 --- [           main] c.s.j.s.i.a.WebApplicationImpl           : Initiating Jersey application, version 'Jersey: 1.19.4 05/24/2017 03:20 PM'
2022-07-26 19:42:18.864  INFO 12661 --- [           main] c.n.d.provider.DiscoveryJerseyProvider   : Using JSON encoding codec LegacyJacksonJson
2022-07-26 19:42:18.865  INFO 12661 --- [           main] c.n.d.provider.DiscoveryJerseyProvider   : Using JSON decoding codec LegacyJacksonJson
2022-07-26 19:42:19.074  INFO 12661 --- [           main] c.n.d.provider.DiscoveryJerseyProvider   : Using XML encoding codec XStreamXml
2022-07-26 19:42:19.074  INFO 12661 --- [           main] c.n.d.provider.DiscoveryJerseyProvider   : Using XML decoding codec XStreamXml
2022-07-26 19:42:19.825  INFO 12661 --- [           main] DiscoveryClientOptionalArgsConfiguration : Eureka HTTP Client uses Jersey
2022-07-26 19:42:19.865  WARN 12661 --- [           main] iguration$LoadBalancerCaffeineWarnLogger : Spring Cloud LoadBalancer is currently working with the default cache. While this cache implementation is useful for development and tests, it's recommended to use Caffeine cache in production.You can switch to using Caffeine cache, by adding it and org.springframework.cache.caffeine.CaffeineCacheManager to the classpath.
2022-07-26 19:42:19.878  INFO 12661 --- [           main] o.s.c.n.eureka.InstanceInfoFactory       : Setting initial instance status as: STARTING
2022-07-26 19:42:19.898  INFO 12661 --- [           main] com.netflix.discovery.DiscoveryClient    : Initializing Eureka in region us-east-1
2022-07-26 19:42:19.898  INFO 12661 --- [           main] com.netflix.discovery.DiscoveryClient    : Client configured to neither register nor query for data.
2022-07-26 19:42:19.904  INFO 12661 --- [           main] com.netflix.discovery.DiscoveryClient    : Discovery Client initialized at timestamp 1658832139903 with initial instances count: 0
2022-07-26 19:42:19.932  INFO 12661 --- [           main] c.n.eureka.DefaultGatewayserverContext    : Initializing ...
2022-07-26 19:42:19.933  WARN 12661 --- [           main] c.n.eureka.cluster.PeerEurekaNodes       : The replica size seems to be empty. Check the route 53 DNS Registry
2022-07-26 19:42:19.994  INFO 12661 --- [           main] c.n.e.registry.AbstractInstanceRegistry  : Finished initializing remote region registries. All known remote regions: []
2022-07-26 19:42:20.001  INFO 12661 --- [           main] c.n.eureka.DefaultGatewayserverContext    : Initialized
2022-07-26 19:42:20.007  INFO 12661 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 16 endpoint(s) beneath base path '/actuator'
2022-07-26 19:42:20.031  INFO 12661 --- [           main] o.s.c.n.e.s.EurekaServiceRegistry        : Registering application EUREKA-SERVER with eureka with status UP
2022-07-26 19:42:20.040  INFO 12661 --- [       Thread-9] o.s.c.n.e.server.GatewayserverBootstrap   : isAws returned false
2022-07-26 19:42:20.041  INFO 12661 --- [       Thread-9] o.s.c.n.e.server.GatewayserverBootstrap   : Initialized server context
2022-07-26 19:42:20.041  INFO 12661 --- [       Thread-9] c.n.e.r.PeerAwareInstanceRegistryImpl    : Got 1 instances from neighboring DS node
2022-07-26 19:42:20.041  INFO 12661 --- [       Thread-9] c.n.e.r.PeerAwareInstanceRegistryImpl    : Renew threshold is: 1
2022-07-26 19:42:20.041  INFO 12661 --- [       Thread-9] c.n.e.r.PeerAwareInstanceRegistryImpl    : Changing status to UP
2022-07-26 19:42:20.050  INFO 12661 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8070 (http) with context path ''
2022-07-26 19:42:20.051  INFO 12661 --- [           main] .s.c.n.e.s.EurekaAutoServiceRegistration : Updating port to 8070
2022-07-26 19:42:20.086  INFO 12661 --- [           main] c.o.eureka.GatewayserverApplication       : Started GatewayserverApplication in 3.32 seconds (JVM running for 3.586)
2022-07-26 19:42:20.091  INFO 12661 --- [       Thread-9] e.s.GatewayserverInitializerConfiguration : Started Eureka Server
2022-07-26 19:43:20.042  INFO 12661 --- [a-EvictionTimer] c.n.e.registry.AbstractInstanceRegistry  : Running the evict task with compensationTime 0ms



# or window and execute the following command(2):
$ mvn clean package spring-boot:repackage
$ java -jar target/licensing-service-0.0.1-SNAPSHOT.jar

# To check the Gatewayserver, run the application in the postman
1. http://localhost:8071/actuator
2. you can see the reponse like as
{
    "_links": {
        "self": {
            "href": "http://localhost:8071/actuator",
            "templated": false
        },
        "beans": {
            "href": "http://localhost:8071/actuator/beans",
            "templated": false
        },
        "caches-cache": {
            "href": "http://localhost:8071/actuator/caches/{cache}",
            "templated": true
        },
        "caches": {
            "href": "http://localhost:8071/actuator/caches",
            "templated": false
        },
        "health": {
            "href": "http://localhost:8071/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8071/actuator/health/{*path}",
            "templated": true
        },
        "info": {
            "href": "http://localhost:8071/actuator/info",
            "templated": false
        },
        "conditions": {
            "href": "http://localhost:8071/actuator/conditions",
            "templated": false
        },
        "configprops": {
            "href": "http://localhost:8071/actuator/configprops",
            "templated": false
        },
        "configprops-prefix": {
            "href": "http://localhost:8071/actuator/configprops/{prefix}",
            "templated": true
        },
        "env": {
            "href": "http://localhost:8071/actuator/env",
            "templated": false
        },
        "env-toMatch": {
            "href": "http://localhost:8071/actuator/env/{toMatch}",
            "templated": true
        },
        "loggers": {
            "href": "http://localhost:8071/actuator/loggers",
            "templated": false
        },
        "loggers-name": {
            "href": "http://localhost:8071/actuator/loggers/{name}",
            "templated": true
        },
        "heapdump": {
            "href": "http://localhost:8071/actuator/heapdump",
            "templated": false
        },
        "threaddump": {
            "href": "http://localhost:8071/actuator/threaddump",
            "templated": false
        },
        "metrics-requiredMetricName": {
            "href": "http://localhost:8071/actuator/metrics/{requiredMetricName}",
            "templated": true
        },
        "metrics": {
            "href": "http://localhost:8071/actuator/metrics",
            "templated": false
        },
        "scheduledtasks": {
            "href": "http://localhost:8071/actuator/scheduledtasks",
            "templated": false
        },
        "mappings": {
            "href": "http://localhost:8071/actuator/mappings",
            "templated": false
        },
        "refresh": {
            "href": "http://localhost:8071/actuator/refresh",
            "templated": false
        },
        "features": {
            "href": "http://localhost:8071/actuator/features",
            "templated": false
        }
    }
}
or
1. In the Body, write "test"
2. run http://localhost:8071/encrypt
3. you can see the respnse like as "5706784406e7a1a0347a20133939402a74bcd46d234e420f04e29083b96e2e9c"

