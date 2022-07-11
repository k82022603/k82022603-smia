# k82022603-smia
Spring Microservices In Action 2nd Edition

1. Change java version & Spring Cloud Version
	<properties>
		<java.version>17</java.version>
		<spring-cloud.version>2021.0.3</spring-cloud.version>
	</properties>

2. Use application.yml instead of bootstrap.yml
- A bootstrap file (properties or yaml) is not needed for the Spring Boot Config Data method of import via spring.config.import. (https://docs.spring.io/spring-cloud-config/docs/current/reference/html/#_spring_cloud_config_client)

3. Find out images like as "ostock/configserver" and "ostock/licensing-service"
$ docker images
REPOSITORY                 TAG              IMAGE ID       CREATED          SIZE
ostock/configserver        0.0.1-SNAPSHOT   b03f832d2cfb   8 minutes ago    442MB
ostock/licensing-service   0.0.1-SNAPSHOT   dd6fafa1cadc   2 hours ago      459MB

4. compare the TAG(VERSION)'s with docker/docker-compose.yml's

5. If running postgresql, stop it 
$ sudo systemctl stop postgresql.service

6. run "docker-compose -f docker/docker-compose.yml up"
jeff@jeff01:~/SpringMicroservicesInAction/k82022603-smia/chapter5$ docker-compose -f docker/docker-compose.yml up
Starting docker_database_1 ...
Starting docker_database_1 ... done
Creating docker_licensingservice_1 ... done
Attaching to docker_configserver_1, docker_database_1, docker_licensingservice_1
database_1          | The files belonging to this database system will be owned by user "postgres".
database_1          | This user must also own the server process.
database_1          |
database_1          | The database cluster will be initialized with locale "en_US.utf8".
database_1          | The default database encoding has accordingly been set to "UTF8".
database_1          | The default text search configuration will be set to "english".
database_1          |
database_1          | Data page checksums are disabled.
database_1          |
database_1          | fixing permissions on existing directory /var/lib/postgresql/data ... ok
database_1          | creating subdirectories ... ok
database_1          | selecting dynamic shared memory implementation ... posix
database_1          | selecting default max_connections ... 100
database_1          | selecting default shared_buffers ... 128MB
configserver_1      |
configserver_1      |   .   ____          _            __ _ _
configserver_1      |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
configserver_1      | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
configserver_1      |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
configserver_1      |   '  |____| .__|_| |_|_| |_\__, | / / / /
configserver_1      |  =========|_|==============|___/=/_/_/_/
configserver_1      |  :: Spring Boot ::                (v2.7.1)
configserver_1      |
configserver_1      | 2022-07-11 07:42:23.754  INFO 1 --- [           main] c.o.c.ConfigurationServerApplication     : Starting ConfigurationServerApplication using Java 17.0.2 on 724c9e670335 with PID 1 (/app started by root in /)
configserver_1      | 2022-07-11 07:42:23.757  INFO 1 --- [           main] c.o.c.ConfigurationServerApplication     : The following 1 profile is active: "native"
configserver_1      | 2022-07-11 07:42:24.791  INFO 1 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=d3a8e23b-55fe-3d69-9c4f-ea77909c7b9f
configserver_1      | 2022-07-11 07:42:25.059  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8071 (http)
configserver_1      | 2022-07-11 07:42:25.068  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
configserver_1      | 2022-07-11 07:42:25.068  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.64]
configserver_1      | 2022-07-11 07:42:25.152  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
configserver_1      | 2022-07-11 07:42:25.153  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1336 ms
configserver_1      | 2022-07-11 07:42:25.956  INFO 1 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 1 endpoint(s) beneath base path '/actuator'
configserver_1      | 2022-07-11 07:42:25.996  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8071 (http) with context path ''
configserver_1      | 2022-07-11 07:42:26.015  INFO 1 --- [           main] c.o.c.ConfigurationServerApplication     : Started ConfigurationServerApplication in 2.919 seconds (JVM running for 3.218)
database_1          | selecting default time zone ... Etc/UTC
database_1          | creating configuration files ... ok
database_1          | running bootstrap script ... ok
database_1          | performing post-bootstrap initialization ... ok
database_1          | syncing data to disk ... ok
database_1          |
database_1          |
database_1          | Success. You can now start the database server using:
database_1          |
database_1          |     pg_ctl -D /var/lib/postgresql/data -l logfile start
database_1          |
database_1          | initdb: warning: enabling "trust" authentication for local connections
database_1          | You can change this by editing pg_hba.conf or using the option -A, or
database_1          | --auth-local and --auth-host, the next time you run initdb.
database_1          | waiting for server to start....2022-07-11 07:43:48.305 UTC [48] LOG:  starting PostgreSQL 14.4 (Debian 14.4-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
database_1          | 2022-07-11 07:43:48.349 UTC [48] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
database_1          | 2022-07-11 07:43:48.485 UTC [49] LOG:  database system was shut down at 2022-07-11 07:43:46 UTC
database_1          | 2022-07-11 07:43:48.546 UTC [48] LOG:  database system is ready to accept connections
database_1          |  done
database_1          | server started
database_1          | CREATE DATABASE
database_1          |
database_1          |
database_1          | /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/1-init.sql
database_1          | CREATE TABLE
database_1          | ALTER TABLE
database_1          | CREATE TABLE
database_1          | ALTER TABLE
database_1          |
database_1          |
database_1          | /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/2-data.sql
database_1          | INSERT 0 1
database_1          | INSERT 0 1
database_1          | INSERT 0 1
database_1          | INSERT 0 1
database_1          | INSERT 0 1
database_1          |
database_1          |
database_1          | 2022-07-11 07:43:50.400 UTC [48] LOG:  received fast shutdown request
database_1          | waiting for server to shut down....2022-07-11 07:43:50.448 UTC [48] LOG:  aborting any active transactions
database_1          | 2022-07-11 07:43:50.451 UTC [48] LOG:  background worker "logical replication launcher" (PID 55) exited with exit code 1
database_1          | 2022-07-11 07:43:50.451 UTC [50] LOG:  shutting down
database_1          | 2022-07-11 07:43:50.941 UTC [48] LOG:  database system is shut down
database_1          |  done
database_1          | server stopped
database_1          |
database_1          | PostgreSQL init process complete; ready for start up.
database_1          |
database_1          | 2022-07-11 07:43:51.131 UTC [1] LOG:  starting PostgreSQL 14.4 (Debian 14.4-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
database_1          | 2022-07-11 07:43:51.131 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
database_1          | 2022-07-11 07:43:51.131 UTC [1] LOG:  listening on IPv6 address "::", port 5432
database_1          | 2022-07-11 07:43:51.230 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
database_1          | 2022-07-11 07:43:51.344 UTC [66] LOG:  database system was shut down at 2022-07-11 07:43:50 UTC
database_1          | 2022-07-11 07:43:51.405 UTC [1] LOG:  database system is ready to accept connections
configserver_1      | 2022-07-11 07:44:00.249  INFO 1 --- [nio-8071-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
configserver_1      | 2022-07-11 07:44:00.250  INFO 1 --- [nio-8071-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
configserver_1      | 2022-07-11 07:44:00.251  INFO 1 --- [nio-8071-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
configserver_1      | 2022-07-11 07:44:00.304  INFO 1 --- [nio-8071-exec-1] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: Config resource 'class path resource [config/licensing-service-dev.properties]' via location 'classpath:/config/'
configserver_1      | 2022-07-11 07:44:00.304  INFO 1 --- [nio-8071-exec-1] o.s.c.c.s.e.NativeEnvironmentRepository  : Adding property source: Config resource 'class path resource [config/licensing-service.properties]' via location 'classpath:/config/'
licensingservice_1  |
licensingservice_1  |   .   ____          _            __ _ _
licensingservice_1  |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
licensingservice_1  | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
licensingservice_1  |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
licensingservice_1  |   '  |____| .__|_| |_|_| |_\__, | / / / /
licensingservice_1  |  =========|_|==============|___/=/_/_/_/
licensingservice_1  |  :: Spring Boot ::                (v2.7.1)
licensingservice_1  |
licensingservice_1  | 2022-07-11 07:44:00.637  INFO 1 --- [           main] c.o.license.LicenseServiceApplication    : Starting LicenseServiceApplication using Java 17.0.2 on c4d6b2dddc8c with PID 1 (/app started by root in /)
licensingservice_1  | 2022-07-11 07:44:00.640  INFO 1 --- [           main] c.o.license.LicenseServiceApplication    : The following 1 profile is active: "dev"
licensingservice_1  | 2022-07-11 07:44:00.709  INFO 1 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Fetching config from server at : http://192.168.5.7:8071
licensingservice_1  | 2022-07-11 07:44:00.709  INFO 1 --- [           main] o.s.c.c.c.ConfigServerConfigDataLoader   : Located environment: name=licensing-service, profiles=[dev], label=null, version=null, state=null
licensingservice_1  | 2022-07-11 07:44:01.494  INFO 1 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
licensingservice_1  | 2022-07-11 07:44:01.541  INFO 1 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 39 ms. Found 1 JPA repository interfaces.
licensingservice_1  | 2022-07-11 07:44:01.745  INFO 1 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=c8588742-c79d-3cab-8c6f-61b34e80dc2b
licensingservice_1  | 2022-07-11 07:44:02.121  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
licensingservice_1  | 2022-07-11 07:44:02.132  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
licensingservice_1  | 2022-07-11 07:44:02.133  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.64]
licensingservice_1  | 2022-07-11 07:44:02.225  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
licensingservice_1  | 2022-07-11 07:44:02.226  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1515 ms
licensingservice_1  | 2022-07-11 07:44:02.531  INFO 1 --- [           main] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
licensingservice_1  | 2022-07-11 07:44:02.616  INFO 1 --- [           main] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 5.6.9.Final
licensingservice_1  | 2022-07-11 07:44:02.748  INFO 1 --- [           main] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.1.2.Final}
licensingservice_1  | 2022-07-11 07:44:02.826  INFO 1 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
licensingservice_1  | 2022-07-11 07:44:02.932  INFO 1 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
licensingservice_1  | 2022-07-11 07:44:02.953  INFO 1 --- [           main] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.PostgreSQLDialect
licensingservice_1  | 2022-07-11 07:44:03.418  INFO 1 --- [           main] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
licensingservice_1  | 2022-07-11 07:44:03.427  INFO 1 --- [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
licensingservice_1  | 2022-07-11 07:44:03.730  WARN 1 --- [           main] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning
licensingservice_1  | 2022-07-11 07:44:04.222  INFO 1 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 1 endpoint(s) beneath base path '/actuator'
licensingservice_1  | 2022-07-11 07:44:04.266  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
licensingservice_1  | 2022-07-11 07:44:04.290  INFO 1 --- [           main] c.o.license.LicenseServiceApplication    : Started LicenseServiceApplication in 4.933 seconds (JVM running for 5.274)

7. Find out the containers
$ docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED          STATUS                    PORTS                                       NAMES
c4d6b2dddc8c   ostock/licensing-service:0.0.1-SNAPSHOT   "java -cp app:app/li…"   14 minutes ago   Up 12 seconds             0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   docker_licensingservice_1
724c9e670335   ostock/configserver:0.0.1-SNAPSHOT        "java -cp app:app/li…"   15 minutes ago   Up 24 seconds             0.0.0.0:8071->8071/tcp, :::8071->8071/tcp   docker_configserver_1
f121fbefead9   postgres:latest                           "docker-entrypoint.s…"   15 minutes ago   Up 24 seconds (healthy)   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   docker_database_1


8. run http://localhost:8080/v1/organization/d898a142-de44-466c-8c88-9ceb2c2429d3/license/f2a9c9d4-d2c0-44fa-97fe-724d77173c62

console log:
licensingservice_1  | Hibernate: select license0_.license_id as license_1_0_, license0_.comment as comment2_0_, license0_.description as descript3_0_, license0_.license_type as license_4_0_, license0_.organization_id as organiza5_0_, license0_.product_name as product_6_0_ from licenses license0_ where license0_.organization_id=? and license0_.license_id=?

response:
{
    "licenseId": "f2a9c9d4-d2c0-44fa-97fe-724d77173c62",
    "description": "Software Product",
    "organizationId": "d898a142-de44-466c-8c88-9ceb2c2429d3",
    "productName": "Ostock",
    "licenseType": "complete",
    "comment": "I AM DEV",
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
