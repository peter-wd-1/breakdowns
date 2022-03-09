
# Table of Contents

1.  [System Estimation](#orgd0d7d77)
2.  [Infrastructure](#orgda28efe)
    1.  [Centralized Event message](#org30196a0)
        1.  [Message Server, Event Queue](#org9d415d1)
    2.  [Database](#org1514f2d)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org3db9f89)
        2.  [MongoDB](#orga23d733)
    3.  [Deploy Container](#orgf3a9167)
        1.  [Docker swarm](#orga736356)
        2.  [{ EC2 | Digital Ocean | ECS }](#org86f3264)
    4.  [CI/CD](#org6a442eb)
        1.  [Shell script](#org7a87c48)
        2.  [Git lab(git action)](#orgf41c8d2)
        3.  [AWS codedeploy / codepipeline](#orgc32f7e2)
    5.  [High availability](#org100b6cb)
        1.  [ECS & ECR](#org16904ab)
        2.  [Health Check](#orgc69d6ce)
        3.  [alert & monitoring](#orgc0e3880)
    6.  [Latency handling](#orgc724893)
        1.  [Kafka](#org5e05e66)
    7.  [Auto Scaling](#org4a8c184)
    8.  [CDN](#org81fa771)
    9.  [Scalability](#org6e43a93)
        1.  [Docker swarm + Nginx Load balancer](#orga3dec87)
3.  [Backend Stack](#orgec8f952)
    1.  [Development](#org2bac490)
    2.  [Nodejs](#org9a19d4c)
        1.  [web framework](#orgbb9174d)
        2.  [testing](#org6902a44)
        3.  [API doc](#org8632881)
        4.  [Data Validation/Schemas/serialize](#org7f2c926)
        5.  [Additional modules depending on each services](#org8dde3f3)
    3.  [Web server](#org915eb22)
        1.  [Nginx](#org72c2a38)
    4.  [SendGrid Email Support](#orgbe255e3)
    5.  [Twilio SMS,MMS Support](#orgcf204ba)
4.  [Backend Application](#org7db5894)
    1.  [Infra Services](#org2ea0669)
        1.  [Message Services : manage message queue](#org1419e44)
        2.  [Authentication/Authorization Services](#orgab2ec71)
    2.  [Business Services](#orgd8647e0)
        1.  [RFI Service](#orgb06a404)
        2.  [Template/RFI Search Service](#org58493c5)
        3.  [Vendor Manage Service](#org6b35548)
        4.  [Matching Service](#orge6e7d2c)
        5.  [Static Content Manage Service](#org3002be6)
        6.  [Alert/notification Service](#org140a3d1)
        7.  [Vendor/Customer Chat Service](#org1e8380a)
        8.  [Payment/billing Service](#orge9c4c81)
    3.  [Entities](#org7ec5ee9)
        1.  [customer](#org591faed)
        2.  [vendor](#org80ffa51)
        3.  [admin](#orgd35a17e)
        4.  [RFI](#orge3d0de4)
        5.  [project<sub>template</sub>](#orgefff319)
        6.  [notifications](#org11186e0)
        7.  [static<sub>content</sub>](#org06069e9)
    4.  [DB Schema](#orgca5c7a5)



<a id="orgd0d7d77"></a>

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

However, our priority is having showcase page and we are designing MVP, I decided to proceed without spending much time on estimating system specs.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="orgda28efe"></a>

# Infrastructure


<a id="org30196a0"></a>

## Centralized Event message


<a id="org9d415d1"></a>

### Message Server, Event Queue

-   Services can send request to event queue
    and polling from there.
-   { Kafka | Redis } event message broker for conveying messages between services
-   Event will be stored and handled and will be eventually deleted as time decay


<a id="org1514f2d"></a>

## Database


<a id="org3db9f89"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orga23d733"></a>

### MongoDB

-   Shading
-   Geospacial queries
    **this can be used for searching the closest vendor**


<a id="orgf3a9167"></a>

## Deploy Container


<a id="orga736356"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : staging


<a id="org86f3264"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org6a442eb"></a>

## CI/CD


<a id="org7a87c48"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="orgf41c8d2"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="orgc32f7e2"></a>

### AWS codedeploy / codepipeline


<a id="org100b6cb"></a>

## High availability


<a id="org16904ab"></a>

### ECS & ECR


<a id="orgc69d6ce"></a>

### Health Check

-   docker swarm has health check functionality


<a id="orgc0e3880"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   cloudwatch


<a id="orgc724893"></a>

## Latency handling


<a id="org5e05e66"></a>

### Kafka

-   message broker
-   message consumer & producer between service


<a id="org4a8c184"></a>

## Auto Scaling


<a id="org81fa771"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org6e43a93"></a>

## Scalability

Each services can be horizontally scaled


<a id="orga3dec87"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="orgec8f952"></a>

# Backend Stack


<a id="org2bac490"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org9a19d4c"></a>

## Nodejs


<a id="orgbb9174d"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org6902a44"></a>

### testing

-   jest


<a id="org8632881"></a>

### API doc

-   swagger


<a id="org7f2c926"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="org8dde3f3"></a>

### Additional modules depending on each services


<a id="org915eb22"></a>

## Web server


<a id="org72c2a38"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="orgbe255e3"></a>

## SendGrid Email Support


<a id="orgcf204ba"></a>

## Twilio SMS,MMS Support


<a id="org7db5894"></a>

# Backend Application


<a id="org2ea0669"></a>

## Infra Services


<a id="org1419e44"></a>

### Message Services : manage message queue


<a id="orgab2ec71"></a>

### Authentication/Authorization Services


<a id="orgd8647e0"></a>

## Business Services


<a id="orgb06a404"></a>

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


<a id="org58493c5"></a>

### Template/RFI Search Service


<a id="org6b35548"></a>

### Vendor Manage Service


<a id="orge6e7d2c"></a>

### Matching Service


<a id="org3002be6"></a>

### Static Content Manage Service


<a id="org140a3d1"></a>

### Alert/notification Service


<a id="org1e8380a"></a>

### Vendor/Customer Chat Service


<a id="orge9c4c81"></a>

### Payment/billing Service


<a id="org7ec5ee9"></a>

## Entities


<a id="org591faed"></a>

### customer


<a id="org80ffa51"></a>

### vendor


<a id="orgd35a17e"></a>

### admin


<a id="orge3d0de4"></a>

### RFI


<a id="orgefff319"></a>

### project<sub>template</sub>


<a id="org11186e0"></a>

### notifications


<a id="org06069e9"></a>

### static<sub>content</sub>


<a id="orgca5c7a5"></a>

## DB Schema

