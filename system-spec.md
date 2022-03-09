
# Table of Contents

1.  [System Estimation](#org3e8edf0)
2.  [Infrastructure](#org6016423)
    1.  [Centralized Event message](#org0c882b5)
        1.  [Message Server, Event Queue](#orgf4d250b)
    2.  [Database](#orgab772f6)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org532cab4)
        2.  [MongoDB](#org38256c8)
    3.  [Deployment of Containers](#orgf9ea7e1)
        1.  [Docker swarm](#org1b5eaef)
        2.  [{ EC2 | Digital Ocean | ECS }](#orgce5b863)
    4.  [CI/CD](#org61975cb)
        1.  [Git lab(git action)](#org18904dc)
        2.  [AWS codedeploy / codepipeline](#orga975408)
        3.  [Shell script](#org277906e)
    5.  [High availability](#orgc4d5dcc)
        1.  [ECS & ECR](#orgee072fa)
        2.  [Health Check](#org99d8ec0)
        3.  [alert & monitoring](#org987d677)
    6.  [Auto Scaling](#orgf8023ea)
    7.  [CDN](#org228f6e3)
    8.  [Scalability](#org117fd99)
        1.  [Docker swarm + Nginx Load balancer](#orgcf7088c)
3.  [Backend Stack](#org56c9e10)
    1.  [Development](#orgfc33b21)
    2.  [Nodejs](#orge27ad41)
        1.  [web framework](#org42a4617)
        2.  [testing](#org6afe48a)
        3.  [API doc](#org7cc0773)
        4.  [Data Validation/Schemas/serialize](#orgb8a5a7f)
        5.  [Additional modules depending on each services](#orgc773dd5)
    3.  [Web server](#org70bd1dd)
        1.  [Nginx](#orgf7e20f7)
    4.  [SendGrid Email Support](#org512fcd3)
    5.  [Twilio SMS,MMS Support](#org3e1b243)
4.  [Backend Application](#org2fc636e)
    1.  [Infra Services](#org45b84c6)
        1.  [Message Services : manage message queue](#org5604db3)
        2.  [Authentication/Authorization Services](#orgbb216fa)
    2.  [Business Services](#orgb683d62)
        1.  [RFI Service](#orge9a69f8)
        2.  [Template/RFI Search Service](#org55a66d1)
        3.  [Vendor Manage Service](#org9e3113e)
        4.  [Matching Service](#orgc1be137)
        5.  [Static Content Manage Service](#org783fee6)
        6.  [Alert/notification Service](#org271822b)
        7.  [Vendor/Customer Chat Service](#org305a1ab)
        8.  [Payment/billing Service](#orgfc2f79f)
    3.  [Entities](#org49b5047)
        1.  [customer](#org2dcad23)
        2.  [vendor](#orgd2de110)
        3.  [admin](#orgf8cda2f)
        4.  [RFI](#org2b79e05)
        5.  [project<sub>template</sub>](#orgbef579f)
        6.  [notifications](#org44f8a0b)
        7.  [static<sub>content</sub>](#org975058a)
    4.  [DB Schema](#org35207ff)



<a id="org3e8edf0"></a>

# System Estimation

We can assume some of metrics like

-   DAU
    -   vendor
    -   customer
-   \# of requests of complete RFI submission per user
-   \# of requests of complete RFI submission per day
-   \# of messages between customers and vendors
-   \# of vendor registrations per day
-   \# of changes per RFI
-   size of RFI request per request
-   size of on boarding vendor data
-   size of changes of RFI

From the given metric, we can derive estimation metrics such as read/write throughput, bandwidth.

However, I decided to proceed without spending much time on estimating system capacity since our priority is having showcase page and we are designing MVP.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="org6016423"></a>

# Infrastructure


<a id="org0c882b5"></a>

## Centralized Event message


<a id="orgf4d250b"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="orgab772f6"></a>

## Database


<a id="org532cab4"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="org38256c8"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="orgf9ea7e1"></a>

## Deployment of Containers


<a id="org1b5eaef"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="orgce5b863"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org61975cb"></a>

## CI/CD


<a id="org18904dc"></a>

### Git lab(git action)

-   automate deployment with AWS codedeploy


<a id="orga975408"></a>

### AWS codedeploy / codepipeline


<a id="org277906e"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="orgc4d5dcc"></a>

## High availability


<a id="orgee072fa"></a>

### ECS & ECR


<a id="org99d8ec0"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org987d677"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="orgf8023ea"></a>

## Auto Scaling


<a id="org228f6e3"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org117fd99"></a>

## Scalability

Each services can be horizontally scaled


<a id="orgcf7088c"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org56c9e10"></a>

# Backend Stack


<a id="orgfc33b21"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="orge27ad41"></a>

## Nodejs


<a id="org42a4617"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org6afe48a"></a>

### testing

-   jest


<a id="org7cc0773"></a>

### API doc

-   swagger


<a id="orgb8a5a7f"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orgc773dd5"></a>

### Additional modules depending on each services


<a id="org70bd1dd"></a>

## Web server


<a id="orgf7e20f7"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org512fcd3"></a>

## SendGrid Email Support


<a id="org3e1b243"></a>

## Twilio SMS,MMS Support


<a id="org2fc636e"></a>

# Backend Application


<a id="org45b84c6"></a>

## Infra Services


<a id="org5604db3"></a>

### Message Services : manage message queue


<a id="orgbb216fa"></a>

### Authentication/Authorization Services


<a id="orgb683d62"></a>

## Business Services


<a id="orge9a69f8"></a>

### RFI Service

-   `create_RFI(RFIData)`
-   `delete_RFI(RFIID)`
-   `update_RFI(RFIID,RFIData)`
-   `create_user_RFI(userID, RFIID, RFIData)`
-   `delete_user_RFI(userID, RFIID, RFIData)`
-   `update_user_RFI(userID, RFIID, RFIData)`
-   `create_MVP_template()`
-   `delete_MVP_template()`
-   `update_MVP_template()`


<a id="org55a66d1"></a>

### Template/RFI Search Service


<a id="org9e3113e"></a>

### Vendor Manage Service


<a id="orgc1be137"></a>

### Matching Service


<a id="org783fee6"></a>

### Static Content Manage Service


<a id="org271822b"></a>

### Alert/notification Service


<a id="org305a1ab"></a>

### Vendor/Customer Chat Service


<a id="orgfc2f79f"></a>

### Payment/billing Service


<a id="org49b5047"></a>

## Entities


<a id="org2dcad23"></a>

### customer


<a id="orgd2de110"></a>

### vendor


<a id="orgf8cda2f"></a>

### admin


<a id="org2b79e05"></a>

### RFI


<a id="orgbef579f"></a>

### project<sub>template</sub>


<a id="org44f8a0b"></a>

### notifications


<a id="org975058a"></a>

### static<sub>content</sub>


<a id="org35207ff"></a>

## DB Schema

