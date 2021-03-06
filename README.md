# Creating Excellent Experiences for API Consumers

## About
Creating REST APIs that are simple for developers to consume can be a 
challenge, but Pivotal provides a number of frameworks and solutions to 
automate much of the process for your clients.

This repository contains an example of a service that provides information 
about electronic bills to a downstream banking application.  

### eBill Service
The eBill service (servfin-epay-service) uses 
[Spring Boot](https://projects.spring.io/spring-boot/), 
[Spring Data JPA](https://projects.spring.io/spring-data-jpa/), and
[Spring Data REST](https://projects.spring.io/spring-data-rest/) to 
quickly expose a [HAL](http://stateless.co/hal_specification.html) 
compliant, data-focused service endpoint for working with eBills.

The service also uses the [SpringFox](http://springfox.github.io/springfox/) 
project to automatically generate and expose REST API documentation to 
developers.

### Service Broker
The eBill service provisioning lifecycle is exposed via an 
[Open Service Broker API](https://github.com/openservicebrokerapi/servicebroker) 
compatible REST service created with the 
[Spring Cloud Cloud Foundry Service Broker](http://cloud.spring.io/spring-cloud-cloudfoundry-service-broker/)
framework.  This endpoint can be registered with platforms such as 
[Pivotal Cloud Foundry](https://pivotal.io/platform) to provide 
self-service access for API consumers to the eBill service via the 
[service marketplace](https://docs.run.pivotal.io/marketplace/).

The service broker can expose information about the service API URI, and 
links to documentation, management consoles, and support channels 
for the service.

### Client App
The eBill service is consumed by a client application (bank-client-app) 
that uses the service to display a list of eBills in its interface.

This application includes a library that would be provided by the eBill 
service provider that leverages the 
[Spring Cloud Connectors](http://cloud.spring.io/spring-cloud-connectors/) 
framework to automatically create the client-side components that developers 
can use to access the eBill service, and inject them into the Spring 
application context for the client application.  This allows developers 
to not have to worry about the details of creating the client, and simply 
wire it into their application by simply including the library with their 
application, binding the eBill service to their application in Cloud 
Foundry, and then allowing Spring to inject the client into their 
application components via `@Autowired` or equivalent mechanisms.

## Building and Deploying
1. Clone this repository locally.
2. Open a terminal, and cd into the directory you cloned this respository to.
3. Run `./gradlew build` to build all the projects and run tests.
4. Run `./push-service.sh <hostname-for-service> <hostname-for-service-broker> <domain>` 
replacing the arguments with the hostnames and domain you want to use for the 
service and service broker.  For example, on Pivotal Web Services you could try 
`./push-service.sh epay-service servfin-service-broker cfapps.io` but you 
might need to select different host names for your service and broker endpoints.
5. Run `./push-app.sh` to push the bank client application, and bind it to 
the eBill service.  You can navigate to the apps manager for your Cloud Foundry 
instance (https://console.run.pivotal.io for Pivotal Web Services) to see the 
bank client app hostname and access it's web interface.

## Tear Down
Use the `./cleanup-all.sh` script to remove the bank client app, service broker, 
and eBill Service.