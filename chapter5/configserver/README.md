## Spring Microservices in Action - Second Edition. Chapter 5 Configserver

# Introduction
Welcome to Spring Microservices in Action, Chapter 5.  Chapter 5 introduces the Spring Cloud Config service and how you can use it managed the configuration of your microservices.  By the time you are done reading this chapter you will have built and/or deployed:

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
1. chapter5/configserver/pom.xml 
- java version & spring-cloud version
    <properties>
		<java.version>17</java.version>
		<spring-cloud.version>2021.0.3</spring-cloud.version>
	</properties>
2. chapter5/configserver/src/main/resources/application.yml
- move bootstrap.yml to application.yml

# The build command (1) using Dockerfile
## Changes
1. chapter5/configserver/Dockerfile
- java version & LABEL
FROM openjdk:17-slim as build
LABEL maintainer="k82022603 <k82022603@gmail.com>"
FROM openjdk:17-slim

## The build command
- mvn clean package dockerfile:build
- run "docker images" to find out the docker image
REPOSITORY                 TAG              IMAGE ID       CREATED         SIZE
ostock/configserver        0.0.1-SNAPSHOT   b03f832d2cfb   7 minutes ago   442MB


# The Run command using the docker
# The Run command
#$ docker run -d --name "configserver" -p 8071:8071 ostock/configserver:0.0.1-SNAPSHOT
$ docker run -d -p 8071:8071 ostock/configserver:0.0.1-SNAPSHOT
$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS         PORTS                                       NAMES
c3fbf002187c   ostock/configserver:0.0.1-SNAPSHOT   "java -cp app:app/li…"   3 minutes ago   Up 3 minutes   0.0.0.0:8071->8071/tcp, :::8071->8071/tcp   configserver


# The Run command using (2) mvn sprint-boot:run
# Go into the repository, by chaning to the directory where you have downloaded the 
# chapter 5 source code
$ cd chapter5/configserver

# To run the code examples for Chapter 5 configsever, open a command-line 
# window and execute the following command(1):
$ mvn clean spring-boot:run

# or window and execute the following command(2):
$ mvn clean package spring-boot:repackage
$ java -jar target/licensing-service-0.0.1-SNAPSHOT.jar

# To check the configserver, run the application in the postman
1. http://localhost:8071/actuator
2. you can see the reponse like as
{
    "_links": {
        "self": {
            "href": "http://localhost:8071/actuator",
            "templated": false
        },
        "health": {
            "href": "http://localhost:8071/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8071/actuator/health/{*path}",
            "templated": true
        }
    }
}
or
1. In the Body, write "test"
2. run http://localhost:8071/encrypt
3. you can see the respnse like as "a77a03ca0269ae9a1eb214bdeee1e82a7feb2f643ee8e7f0e3cf5d8b91a60f15"

