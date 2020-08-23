# Spring Cloud Gateway -Reverse proxy with Static and Dynamic Routing
In this tutorial we are going to a run reverse proxy with Static and Dynamic Routing
- Static Routing
    - Gateway runs on port 8080
    - All the requests that are coming on 8080 is reverse proxied (forwarded) to rest api running on 9090 and 9091
    - Requests are routed to only health tomcats running on 9090 and 9091
    - Health Check is verified before a request is forwarded to tomcat
    - Netty play a role of Gateway
    - Netflix Ribbon client is used as Client Side Load Balancer 
    - Routes are explicitly mentioned in configuration files
    - New servers cannot be dynamically added with our restart of the servers
    - Ribbon configuration are defined in application.yml
    - Legacy application that cannot use Eureka Client can use this type of deployment 
- Dynamic Routing
    - Gateway runs on port 8080
    - Requests on 8080 are reverse proxied (forwarded) to  rest api running on 9090 and 9091
    - Eureka Registry Server is running on port 8761
    - When Gateways starts up on port 8080 registers with Eureka Registry
    - When rest api servers starts on 9090 and 9091 register with Eureak Server
    - Spring Cloud gateway uses Ribbon Configurations and routes all the requests that are coming on 8080 in a round robin fashion to 9090 and 9091
    - When are new rest api instance starts on 9092 it registers with Eureka and routing is dynamically enabled by Gateway 
    - New application servers can be dynamically added
    - Application servers (service urls) are maintained in Registry
    - Netflix Eureka component plays a role of registry
    - Netflix Ribbon client plays a role of Client side load balancer
    - Netflix Eureka Client is included in all the application servers (services)  and Gateway
# Source Code - Static Routing
    git clone https://github.com/balajich/reverse-proxy-spring-cloud-gateway-enhanced-routing.git
# Source Code - Dynamic Routing
    git clone https://github.com/balajich/reverse-proxy-spring-cloud-gateway-registry.git
# Architecture
# Prerequisite
- JDK 1.8 or above
- Apache Maven 3.6.3 or above
# Clean and Build
    mvn clean install
# Running components
- Registry: java -jar .\registry\target\registry-0.0.1-SNAPSHOT.jar
- Gateway:  java -jar .\gateway\target\gateway-0.0.1-SNAPSHOT.jar
- Rest API instance 1: java -jar .\restapi\target\restapi-0.0.1-SNAPSHOT.jar
- Rest API instance 2:  java -jar '-Dserver.port=9091' .\restapi\target\restapi-0.0.1-SNAPSHOT.jar
# Using curl to test environment
- Access rest api via gateway:  curl http://localhost:8080/
- Access rest api directly on instance1 : curl http://localhost:9090/
- Access rest api directly on instance2 : curl http://localhost:9090/
# Whats next?
- Netflix components are currently under maintainence mode user should move to spring cloud supported components
- Currently, there are several applications using Netflix components for Dynamic Routing that the reason this tutorial explaning routing with these components   
