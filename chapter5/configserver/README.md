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

# The build command


# The Run command
# Go into the repository, by chaning to the directory where you have downloaded the 
# chapter 5 source code
$ cd chapter5/configserver

# To run the code examples for Chapter 5 configsever, open a command-line 
# window and execute the following command:
$ mvn sprint-boot:run

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





    