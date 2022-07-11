## Spring Microservices in Action - Second Edition. Chapter 5 licensing-service

# Introduction
Welcome to Spring Microservices in Action, Chapter 5.  Chapter 5 introduces the Spring Cloud Config service and how you can use it managed the configuration of your microservices like as the licensing-service . By the time you are done reading this chapter you will have built and/or deployed:

1. The licensing-service that is deployed as Docker container and can manage a services configuration information using a file system/ classpath or GitHub-based repository.

## Initial Configuration
1.	Apache Maven (http://maven.apache.org)  All of the code examples in this book have been compiled with Java version 11.
2.	Git Client (http://git-scm.com)
3.  Docker(https://www.docker.com/products/docker-desktop)

## How To Use

To clone and run this application, you'll need [Git](https://git-scm.com), [Maven](https://maven.apache.org/), [Java 11](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html). From your command line:

```bash
# Clone this repository
$ git clone https://github.com/ihuaylupo/manning-smia

# Go into the repository, by chaning to the directory where you have downloaded the 
# chapter 5 source code
$ cd chapter5/licensing-service/

# Database
You can find the database script as well in the docker directory.
-- If you want "dev" environment, create schema "ostock_dev" and run the DDL/DML scripts in the "ostock_dev". It uses "spring.datasource.url=jdbc:postgresql://localhost:5432/ostock_dev"

If not running postgresql, start it 
$ sudo systemctl start postgresql.service

## Changes
1. chapter5/configserver/pom.xml 
- java version & spring-cloud version
	<properties>
		<java.version>17</java.version>
		<docker.image.prefix>ostock</docker.image.prefix>
		<spring-cloud.version>2021.0.3</spring-cloud.version>
	</properties>
2. chapter5/configserver/src/main/resources/application.yml
- move bootstrap.yml to application.yml




# The build command (1) using Dockerfile
## Changes
1. chapter5/licensing-service/Dockerfile
- java version & LABEL
FROM openjdk:17-slim as build
LABEL maintainer="k82022603 <k82022603@gmail.com>"
FROM openjdk:17-slim

## The build command
- mvn clean package dockerfile:build
- run "docker images" to find out the docker image
REPOSITORY                 TAG              IMAGE ID       CREATED          SIZE
ostock/licensing-service   0.0.1-SNAPSHOT   dd6fafa1cadc   33 seconds ago   459MB

# The Run command using the docker
## Changes to fix trouble shootings
1. $ docker run ostock/licensing-service:0.0.1-SNAPSHOT
2022-07-07 08:08:28.193 WARN 1 --- [ main] o.s.c.c.c.ConfigServerConfigDataLoader : Could not locate PropertySource ([ConfigServerConfigDataResource@1eb6749b uris = array<String>['[http://localhost:8071](http://localhost:8071/)'], optional = true, profiles = list['dev']]): I/O error on GET request for "[http://localhost:8071/licensing-service/dev](http://localhost:8071/licensing-service/dev)": Connection refused; nested exception is java.net.ConnectException: Connection refused
--> 
chapter5/licensing-service/src/main/resources/application.yml
    config: 
      import: "optional:configserver:http://192.168.5.7:8071"

2. org.postgresql.util.PSQLException: Connection to 192.168.5.7:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections
-->
https://stackoverflow.com/questions/25641047/org-postgresql-util-psqlexception-fatal-no-pg-hba-conf-entry-for-host

$ vi /etc/postgresql/14/main/postgresql.conf
listen_addresses = 'localhost' --> listen_addresses = '*'

3. org.postgresql.util.PSQLException: FATAL: no pg_hba.conf entry for host "172.17.0.2", user "postgres", database "ostock_dev", SSL encryption
--> 
https://stackoverflow.com/questions/25641047/org-postgresql-util-psqlexception-fatal-no-pg-hba-conf-entry-for-host
$ vi /etc/postgresql/14/main/pg_hba.conf
add a new row :
host all all 0.0.0.0/0 md5


# The Run command
$ docker run -d --name "licensing_service" -p 8080:8080 ostock/licensing-service:0.0.1-SNAPSHOT
$ docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
adb8f7af4e57   ostock/licensing-service:0.0.1-SNAPSHOT   "java -cp app:app/li…"   13 seconds ago   Up 10 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   licensing_service


# The Run command using (2) mvn sprint-boot:run
# To run the code examples for Chapter 5 configsever, open a command-line 
# window and execute the following command:
1. without running Spring Cloud Config service (chapter5/configserver)
$ mvn sprint-boot:run
2022-07-11 14:07:01.798  INFO 16563 --- [           main] c.o.license.LicenseServiceApplication    : Starting LicenseServiceApplication using Java 17.0.3 on jeff01 with PID 16563 (/home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service/target/classes started by jeff in /home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service)
2022-07-11 14:07:01.800  INFO 16563 --- [           main] c.o.license.LicenseServiceApplication    : The following 1 profile is active: "dev"
2022-07-11 14:07:01.840  INFO 16563 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://192.168.5.7:8071
2022-07-11 14:07:01.840  INFO 16563 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Connect Timeout Exception on Url - http://192.168.5.7:8071. Will be trying the next url if available
2022-07-11 14:07:01.840  WARN 16563 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Could not locate PropertySource ([ConfigServerConfigDataResource@14f5da2c uris = array<String>['http://192.168.5.7:8071'], optional = true, profiles = list['dev']]): I/O error on GET request for "http://192.168.5.7:8071/licensing-service/dev": Connection refused; nested exception is java.net.ConnectException: Connection refused
....

2. with running Spring Cloud Config service (chapter5/configserver)
$ mvn sprint-boot:run
2022-07-11 15:19:23.614  INFO 23926 --- [           main] c.o.license.LicenseServiceApplication    : Starting LicenseServiceApplication using Java 17.0.3 on jeff01 with PID 23926 (/home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service/target/classes started by jeff in /home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service)
2022-07-11 15:19:23.616  INFO 23926 --- [           main] c.o.license.LicenseServiceApplication    : The following 1 profile is active: "dev"
2022-07-11 15:19:23.652  INFO 23926 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://192.168.5.7:8071
2022-07-11 15:19:23.653  INFO 23926 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Located environment: name=licensing-service, profiles=[dev], label=null, version=null, state=null
....


# To check the configserver, run the application in the postman
1. http://localhost:8080/actuator
2. you can see the reponse like as
{
    "_links": {
        "self": {
            "href": "http://localhost:8080/actuator",
            "templated": false
        },
        "health": {
            "href": "http://localhost:8080/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8080/actuator/health/{*path}",
            "templated": true
        }
    }
}
or
1. http://localhost:8080/v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3/license/f2a9c9d4-d2c0-44fa-97fe-724d77173c62
2. you can see the respnse like as 
{
    "licenseId": "f2a9c9d4-d2c0-44fa-97fe-724d77173c62",
    "description": "Software Product",
    "organizationId": "d898a142-de44-466c-8c88-9ceb2c2429d3",
    "productName": "Ostock",
    "licenseType": "complete",
    "comment": null,
    "_links": {
        "self": {
            "href": "http://localhost:8080/v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3/license/f2a9c9d4-d2c0-44fa-97fe-724d77173c62"
        },
        "createLicense": {
            "href": "http://localhost:8080/v1/organization/{organizationId}/license",
            "templated": true
        },
        "updateLicense": {
            "href": "http://localhost:8080/v1/organization/{organizationId}/license",
            "templated": true
        },
        "deleteLicense": {
            "href": "http://localhost:8080/v1/organization/{organizationId}/license/f2a9c9d4-d2c0-44fa-97fe-724d77173c62",
            "templated": true
        }
    }
}

# To check referencing the configserver, change like below and run 
1. chapter5/licensing-service/src/main/resources/application.yml
- profiles:
      active: prod 
2. run Spring Cloud Config service (chapter5/configserver)
3. run chapter5/licensing-service like as
$ mvn spring-boot:run
2022-07-11 14:16:41.576  INFO 17656 --- [           main] c.o.license.LicenseServiceApplication    : Starting LicenseServiceApplication using Java 17.0.3 on jeff01 with PID 17656 (/home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service/target/classes started by jeff in /home/jeff/SpringMicroservicesInAction/k82022603-smia/chapter5/licensing-service)
2022-07-11 14:16:41.578  INFO 17656 --- [           main] c.o.license.LicenseServiceApplication    : The following 1 profile is active: "prod"
2022-07-11 14:16:41.615  INFO 17656 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://192.168.5.7:8071
....
org.postgresql.util.PSQLException: The connection attempt failed.





    