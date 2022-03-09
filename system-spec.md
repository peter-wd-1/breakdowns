
# Table of Contents

1.  [System Estimation](#orgbd6710b)
2.  [Infrastructure](#orgb225532)
    1.  [Centralized Event message](#org83a9e5a)
        1.  [Message Server, Event Queue](#org8cabbb0)
    2.  [Database](#org87a489a)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org1d6ada6)
        2.  [MongoDB](#orgad39343)
    3.  [Deploy Container](#orgf01f7d7)
        1.  [Docker swarm](#org31dd110)
        2.  [{ EC2 | Digital Ocean | ECS }](#org1160523)
    4.  [CI/CD](#org2692424)
        1.  [Shell script](#org421d6d4)
        2.  [Git lab(git action)](#org6d7110b)
        3.  [AWS codedeploy / codepipeline](#orgae22c7d)
    5.  [High availability](#org63c1723)
        1.  [ECS & ECR](#orgda4f2b7)
        2.  [Health Check](#org57a2ca0)
        3.  [alert & monitoring](#orge04c125)
    6.  [Latency handling](#org40b3a70)
        1.  [Kafka](#orgc255cf0)
    7.  [Auto Scaling](#org9c5eafa)
    8.  [CDN](#org4379253)
    9.  [Scalability](#org680fc81)
        1.  [Docker swarm + Nginx Load balancer](#org4bfbd7c)
3.  [Backend Stack](#org16be2a2)
    1.  [Development](#orge4b5b31)
    2.  [Nodejs](#orgc8fa5bf)
        1.  [web framework](#org5be75d7)
        2.  [testing](#org5d63bf7)
        3.  [API doc](#orgecb8d96)
        4.  [Data Validation/Schemas/serialize](#orga854418)
        5.  [Additional modules depending on each services](#org677c636)
    3.  [Web server](#org30f4500)
        1.  [Nginx](#org3458ee3)
    4.  [SendGrid Email Support](#orgb284c3c)
    5.  [Twilio SMS,MMS Support](#org9f25921)
4.  [Backend Application](#orgba5f31c)
    1.  [Infra Services](#org9a86b0d)
        1.  [Message Services : manage message queue](#org4d8a083)
        2.  [Authentication/Authorization Services](#org0eeaddd)
    2.  [Business Services](#orgf686317)
        1.  [RFI Service](#org7689d55)
        2.  [Template/RFI Search Service](#org2502d76)
        3.  [Vendor Manage Service](#org0dd40a1)
        4.  [Matching Service](#org7e30477)
        5.  [Static Content Manage Service](#orgace8eb3)
        6.  [Alert/notification Service](#orga7a6718)
        7.  [Vendor/Customer Chat Service](#org67cb961)
        8.  [Payment/billing Service](#org5e7d70b)
    3.  [Entities](#org4bb7414)
        1.  [customer](#org54aa156)
        2.  [vendor](#org2a0f518)
        3.  [admin](#org8cb2d65)
        4.  [RFI](#orga87814c)
        5.  [project<sub>template</sub>](#orgce2fd71)
        6.  [notifications](#orgc9b43e5)
        7.  [static<sub>content</sub>](#org0d9846e)
    4.  [DB Schema](#org3be1841)



<a id="orgbd6710b"></a>

# System Estimation

We can assume some of metrics like

-   DAU
    -   vendor
    -   customer
-   \# of requests of complete RFI submission per user
-   \# of requests of complete RFI submission RFI per day
-   \# of messages between customers and vendors
-   \# of vendor registrations per day
-   \# of changes per RFI
-   size of RFI request per request
-   size of on boarding vendor data
-   size of changes of RFI

From the given metric, we can derive the metrics such as read/write throughput, bandwidth.

However, I decided to proceed without spending much time on estimating system specs since our priority is having showcase page and we are designing MVP.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="orgb225532"></a>

# Infrastructure


<a id="org83a9e5a"></a>

## Centralized Event message


<a id="org8cabbb0"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="org87a489a"></a>

## Database


<a id="org1d6ada6"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orgad39343"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="orgf01f7d7"></a>

## Deploy Container


<a id="org31dd110"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org1160523"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org2692424"></a>

## CI/CD


<a id="org421d6d4"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org6d7110b"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="orgae22c7d"></a>

### AWS codedeploy / codepipeline


<a id="org63c1723"></a>

## High availability


<a id="orgda4f2b7"></a>

### ECS & ECR


<a id="org57a2ca0"></a>

### Health Check

-   docker swarm has health check functionality


<a id="orge04c125"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   cloudwatch


<a id="org40b3a70"></a>

## Latency handling


<a id="orgc255cf0"></a>

### Kafka

-   message broker
-   message consumer & producer between service


<a id="org9c5eafa"></a>

## Auto Scaling


<a id="org4379253"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org680fc81"></a>

## Scalability

Each services can be horizontally scaled


<a id="org4bfbd7c"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org16be2a2"></a>

# Backend Stack


<a id="orge4b5b31"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="orgc8fa5bf"></a>

## Nodejs


<a id="org5be75d7"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org5d63bf7"></a>

### testing

-   jest


<a id="orgecb8d96"></a>

### API doc

-   swagger


<a id="orga854418"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="org677c636"></a>

### Additional modules depending on each services


<a id="org30f4500"></a>

## Web server


<a id="org3458ee3"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="orgb284c3c"></a>

## SendGrid Email Support


<a id="org9f25921"></a>

## Twilio SMS,MMS Support


<a id="orgba5f31c"></a>

# Backend Application


<a id="org9a86b0d"></a>

## Infra Services


<a id="org4d8a083"></a>

### Message Services : manage message queue


<a id="org0eeaddd"></a>

### Authentication/Authorization Services


<a id="orgf686317"></a>

## Business Services


<a id="org7689d55"></a>

### RFI Service

-   `create_RFI(RFIData)`
-   `delete_RFI(RFIID)`
-   `update_RFI(RFIID,RFIData)`
-   `create_user_RFI(userID, RFIID, RFIData)`
-   `delete_user_RFI(userID, RFIID, RFIData)`
-   `update_user_RFI(userID, RFIID, RFIData)`
-   `create_MVP_template()`
-   `delete_MVP_template()`
-   `updateMVP_template()`


<a id="org2502d76"></a>

### Template/RFI Search Service


<a id="org0dd40a1"></a>

### Vendor Manage Service


<a id="org7e30477"></a>

### Matching Service


<a id="orgace8eb3"></a>

### Static Content Manage Service


<a id="orga7a6718"></a>

### Alert/notification Service


<a id="org67cb961"></a>

### Vendor/Customer Chat Service


<a id="org5e7d70b"></a>

### Payment/billing Service


<a id="org4bb7414"></a>

## Entities


<a id="org54aa156"></a>

### customer


<a id="org2a0f518"></a>

### vendor


<a id="org8cb2d65"></a>

### admin


<a id="orga87814c"></a>

### RFI


<a id="orgce2fd71"></a>

### project<sub>template</sub>


<a id="orgc9b43e5"></a>

### notifications


<a id="org0d9846e"></a>

### static<sub>content</sub>


<a id="org3be1841"></a>

## DB Schema

