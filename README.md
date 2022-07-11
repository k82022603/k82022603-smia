# k82022603-smia
Spring Microservices In Action 2nd Edition

1. Change java version & Spring Cloud Version
	<properties>
		<java.version>17</java.version>
		<spring-cloud.version>2021.0.3</spring-cloud.version>
	</properties>

2. Use application.yml instead of bootstrap.yml
- A bootstrap file (properties or yaml) is not needed for the Spring Boot Config Data method of import via spring.config.import. (https://docs.spring.io/spring-cloud-config/docs/current/reference/html/#_spring_cloud_config_client)