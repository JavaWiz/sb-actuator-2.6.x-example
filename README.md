### Spring Boot info actuator endpoint is enabled, but I don’t see any environment details!
Did you recently upgrade your Spring Boot application to version 2.6.x? After the upgrade, you can still reach the Spring Boot `/info` actuator endpoint but don’t see any details?

Be aware that all actuators except for/health are disabled by default for security reasons. They might expose sensitive information. You can use the `management.endpoints.web.exposure.include` property to enable the actuators.

Spring Boot includes several built-in endpoints. One of the endpoints is the info actuator to display arbitrary application.
To expose the info actuator endpoint in your Spring Boot application:

```
management:
  endpoints:
    web:
      exposure:
        include: info
```
We can reach the info actuator on your local machine: `http://localhost:8080/actuator/info` once we start your Spring Boot application.

### Spring Boot info actuator contributors
There are different info contributors that `contribute` to the information exposed by the actuator info endpoint:

**build**
  - Enabled by default: true
  - Goal: Exposes build information.
  - Requires: you to generate build information
  - Configuration property: management.info.build.enabled
  
**env**
  - Enabled by default: false(since Spring Boot 2.6.x. For the older Spring Boot version, this contributor is enabled by default!)
  - Goal: Exposes any property from the Spring Environment.
  - Required: to specify properties with names that start with info.* in you application.yml or application.properties
  - Configuration property: `management.info.env.enabled`.
  
**git**
   * Enabled by default: true
   - Goal: exposes git information.
   - Requires: you to generate git information
   - Configuration property: `management.info.git.enabled`.
   
**java**
  - Enabled by default: false
  - Goal: exposes Java runtime information.
  - Configuration property: `management.info.java.enabled`

It’s up to us to decide what information we would like to expose.

Whether or not an individual contributor is enabled is controlled by its `management.info.<id>.enabled` property.

We can customize the environment info contributor by adding `info.*properties` to our `application.yml` or `application.properties` . Here is an example:

```
spring:
  application:
    name: sb-actuator-2.6.x-example-app
    
info:
  application:
    name: ${spring.application.name}
    description: Very cool Spring Boot application
    version: '@project.version@'
    spring-cloud-version: '@spring-cloud.version@'
    spring-boot-version: '@project.parent.version@'
```
That’s all we need to do to make the information available on Spring Boot Actuators’ `/info` endpoint.

### What changed in Spring Boot 2.6?
Actuator `env` Info Contributor Disabled by Default. To enable it, set `management.info.env.enabled` to true.
Before Spring Boot 2.6.x:

```
{
	"application": {
		"name": "sb-actuator-2.6.x-example-app",
		"description": "Spring Boot Actuator Application",
		"version": "0.0.1-SNAPSHOT",
		"spring-cloud-version": "@spring-cloud.version@",
		"spring-boot-version": "2.6.7"
	}
}
```
After Spring Boot 2.6.x (without configuring `management.info.env.enabled` set to true .)
```
{}
```
### How to show the environment details in the info actuator endpoint again?
So although you exposed the `/info` endpoint using the property `management.endpoints.web.exposure.include` you now have to enable the environment contributor (explicitly `management.info.env.enabled`).
To enable it, set `management.info.env.enabled` to true.


### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/2.6.7/maven-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/2.6.7/maven-plugin/reference/html/#build-image)
* [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/2.6.7/reference/htmlsingle/#production-ready)

### Guides
The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service with Spring Boot Actuator](https://spring.io/guides/gs/actuator-service/)

