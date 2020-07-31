# Spring Boot Microservices using Netflix Eureka on Apps Service

This is a sample project to demonstrate how to configure Netflix Eureka to build microservices on Azure Apps Service

## Building

Building requires Java 1.8

Run `mvn clean package -DskipTests`

To run with tests, use

Run `mvn clean package -Dserver.port=999`

## Deploying

Deploying is similar to [deploying a spring boot application using maven](https://docs.microsoft.com/en-us/azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin). Make sure you are logged in through the Azure cli and have the correct subscription set

IMPORTANT: It is likely that the names for these services are already in use in azure

For this reason, the following lines in the following files need to be changed before deploying:

```yml
discovery-server/pom.xml :
  <appName>example-discovery-server</appName> -> <appName>[SOME_UNIQUE_IDENTIFIER]-example-discovery-server</appName>
movie-catalog-service/pom.xml :
  <appName>example-movie-catalog-service</appName> -> <appName>[SOME_UNIQUE_IDENTIFIER]-example-movie-catalog-service</appName>
movie-catalog-service/src/main/resources/application.properties :
  eureka.client.serviceUrl.defaultZone=https://example-discovery-server.azurewebsites.net:443/eureka -> eureka.client.serviceUrl.defaultZone=https://[SOME_UNIQUE_IDENTIFIER]-example-discovery-server.azurewebsites.net:443/eureka
  eureka.instance.hostname=example-movie-catalog-service.azurewebsites.net -> eureka.instance.hostname=[SOME_UNIQUE_IDENTIFIER]-example-movie-catalog-service.azurewebsites.net
movie-info-service/pom.xml :
  <appName>example-movie-info-service</appName> -> <appName>[SOME_UNIQUE_IDENTIFIER]-example-movie-info-service</appName>
movie-info-service/src/main/resources/application.properties :
  eureka.client.serviceUrl.defaultZone=https://example-discovery-server.azurewebsites.net:443/eureka -> eureka.client.serviceUrl.defaultZone=https://[SOME_UNIQUE_IDENTIFIER]-example-discovery-server.azurewebsites.net:443/eureka
  eureka.instance.hostname=example-movie-info-service.azurewebsites.net -> eureka.instance.hostname=[SOME_UNIQUE_IDENTIFIER]-example-movie-info-service.azurewebsites.net
ratings-data-service/pom.xml :
  <appName>example-ratings-data-service</appName> -> <appName>[SOME_UNIQUE_IDENTIFIER]-example-ratings-data-service</appName>
ratings-data-service/src/main/resources/application.properties :
  eureka.client.serviceUrl.defaultZone=https://example-discovery-server.azurewebsites.net:443/eureka -> eureka.client.serviceUrl.defaultZone=https://[SOME_UNIQUE_IDENTIFIER]-example-discovery-server.azurewebsites.net:443/eureka
  eureka.instance.hostname=example-ratings-data-service.azurewebsites.net -> eureka.instance.hostname=[SOME_UNIQUE_IDENTIFIER]-example-ratings-data-service.azurewebsites.net
```

Then, run `mvn azure-webapp:deploy`
