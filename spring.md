# microservices-with-spring-boot-and-spring-cloud


spring-cloud-starter-openfeign 作用

spring



## Official content

### 7 web services terminology

<https://www.w3.org/TR/2004/NOTE-ws-gloss-20040211/#webservice>

- request/response format(json/xml)
- request structure
- response structure
- endpoint: where find service

transport: MQ or HTTP

### SOAP web services

Simple Object Access Protocol

use xml as req res format

wsdl: web service definition language

wsdl: end point, all operations, req res structure

### 17 auto configuration

not fully understand

### 25 implementing generic exception handling for all resources

different respond code for different exception

### 30 HATEOAS

provide data or action the url can might need or perform

### 34 step 20

auto generate configuring documentation

springdoc-openapi

### 38 Static Filtering

JsonIgnore

### 42 Authentication

bf4bd79f-3fa1-43ca-8c86-2dc483f028c8

### 56 Microservices Challenges

configuation management: multiple environment and instance

scale up and scale down: auto open close and dynamically distribute

visibilty and debug, avoiding shutdown expansion

### 151 GateWay

API gateway can implement authentication logic

### 176 Connect Microservice with Zipkin

distributed tracing system

spring-cloud-starter-sleuth, assign unique ID to request.

### 180 Create docker image

https://www.youtube.com/watch?v=ck6xQqSOlpw&t=412s

```
FROM adoptopenjdk/openjdk16
COPY target/classes/ /tmp #
WORKDIR /tmp
CMD java com.in28minutes.microservices.currencyexchangeservice.CurrencyExchangeServiceApplication
```
https://www.youtube.com/watch?v=5Mm992VvBVE&t=1122s

### 182 Running Eureka Naming Server with Docker Compose
inside the Docker, the localhost is not meaning the same with outside.

### 190 Started with Docker & K8s
Service Discovery, Load Balancing, Auto Scale, Self Healing,

### 197 pods
inside pods, containers share resources, and can use localhost to call each other.

### 199 Understanding Deployment in Kubernetes
kubectl describe pod [name]

### 209 Deploy Microservices to k8s
K8s will create environment var follow by _SERVICE_HOST

### 214 Playing with kubernetes Declarative YAML
declarative deployment, service discovery, load balancing

### 215 Creating Environment Variables
Environment var will be creat after deployment start, So may cannot to find in some point of time.

Should use the 

distributed tracing system

### understanding centralized configuration

service discovery, load balancing

### Centrallized Configuration and monitor in GKE

sleuth tracing system

### Expoloring Microservices Deployments

Before. centralized logging, service discovery, naming servers, load balancing.

### Liveness and Readiness Probes

Avoid time gap between 2 pods Ready to service

### AutoScaling

manual scaling: yaml file or `kubectl scale deployment currency-exchange --replicas=2`

Auto scaling: `kubectl autoscale deployment currency-exchange --min=1 --max=3 --cpu-percent=70`

## Spring boot

### [why spring boot?](https://www.bilibili.com/video/BV1aE411A7pR)

#### spring feature:

* IOC (before: new Object by hardcode to create Instance. after: Configure xml file and mark by @ annotation.)
Control by the framework install of application.

* AOP (Meeting non-functional needs)

#### spring boot:

* easy to create a spring app
* include build in something like tomcat, jetty
* auto configure spring and 3rd party lib in maven file
* many configure are already done by default

spring boot devtool: HOT updates

spring configuration processor: help in .property and .yml file

### 226 spring boot goals

* dependency management(pom.xml)
* web app config(web.xml)
* spring bean manage(context.xml)
* implement non functional requirements

### 234 production 1 profiles

multi setting for different env(like dev, product, test)

`spring.profiles.active=prod`

level of log: trace, debug, info, warning, error, off

### 238 Spring boot vs spring vs spring MVC

spring: Dependency injection, spring modules and projects. @Componment, @Autowired, @ComponentScan

spring MVC: spring module. simplify building web apps and REST API. @Controller, @RestController, @RequestMapping("/coureses") 

spring boot: spring project. help to start and config projects. NFRs

## JPA

### 243 getting started with spring JDBC

spring JDBC need less java code than JDBC.

### 247 JPA and EntityManager

map java bean to db table @Entity() , @Id , @Column

spring.jpa.show-sql=true

### 248 magic of JPA

JPA don't need queries, by Map entities to table.

### 250 Exploring features of Spring Data JPA

How to implement such 'findbuAuthor'?

### 251 difference between Hibernatee and JPA

JPA defines the specification, is API, like interface.

Hibernate is one of the popular implementations of JPA


## Functional Programming

### 252 Introduction

lambda functions, Streams, Filters, Method references

### 253 start with functional programming with java

paradigm shift

### 255 functional Program with filter

in structured one, we need focus on how to loop the numbers.

In Functional One, we can only focus on what to do with.

### 256 lamdba Expression
numbers.stream()
.filter(number -> number % 2 == 1)//lamdba Expression
.forEach(System.out::println);

### 258 Using map
.mapping

### 259 Optional class
optional is to avoid error cause by null object
