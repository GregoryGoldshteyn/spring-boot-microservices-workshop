# Spring Boot Microservices using Netflix Eureka on Apps Service

This is a sample project to demonstrate how to configure Netflix Eureka to build microservices on Azure Apps Service

## Building

Building requires Java 1.8

Run `mvn clean package -DskipTests`

## Deploying

Deploying is similar to [deploying a spring boot application using maven](https://docs.microsoft.com/en-us/azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin). Make sure you are logged in through the Azure cli and have the correct subscription set

Then, run `mvn azure-webapp:deploy`

To build and run the project in one command, use `mvn clean package azure-webapp:deploy -DskipTests`

## The Service

After deployment, the project will be hosted on four Azure webapps.

These can be accessed at:

- http://(TIMESTAMP)-discovery-server.azurewebsites.net/
- http://(TIMESTAMP)-movie-catalog-service.azurewebsites.net/
- http://(TIMESTAMP)-movie-info-service.azurewebsites.net/
- http://(TIMESTAMP)-ratings-data-service.azurewebsites.net/

Where (TIMESTAMP) is the automatically generated timestamp to ensure the webapps have a unique identifier.

The URLS can be accessed either through the output of the maven command, or on the Azure portal under the new resource group `(TIMESTAMP)-example-springboot-microservices-rg`

IMPORTANT: Ensure that you visit the discovery server first. This ensures that the services are able to register with it and discover each other. Then visit the movie info and ratings data services before querying the movie catalog service. This is because the movie catalog service requires data from the movie info service and ratings data service. Querying the movie catalog service first results in a 500 error.

A powershell script which performs the requests in correct order:

```powershell
#Replace with your timestamp
$timestamp=200408173336

Invoke-Webrequest -URI https://$timestamp-discovery-server.azurewebsites.net
Invoke-Webrequest -URI https://$timestamp-movie-info-service.azurewebsites.net
Invoke-Webrequest -URI https://$timestamp-ratings-data-service.azurewebsites.net
Invoke-Webrequest -URI https://$timestamp-movie-catalog-service.azurewebsites.net
```
