
# Table of Contents

1.  [System Estimation](#org2c0405d)
2.  [Infrastructure](#org955c8ef)
    1.  [Centralized Event message](#org02dbaf5)
        1.  [Message Server, Event Queue](#org4f2d2e1)
    2.  [Database](#orgf7aaa74)
        1.  [{ Cloud Atlas | EC2 & Docker }](#orgb565d0a)
        2.  [MongoDB](#org0ad6d1a)
    3.  [Deployment of Containers](#orgcae5dee)
        1.  [Docker swarm](#orga8511d2)
        2.  [{ EC2 | Digital Ocean | ECS }](#org336d8ee)
    4.  [CI/CD](#org0205edf)
        1.  [Git lab(git action)](#org3e00946)
        2.  [AWS codedeploy / codepipeline](#orgd73b908)
        3.  [Shell script](#org6ff420d)
    5.  [High availability](#org7f65be2)
        1.  [ECS & ECR](#orgc4ff3e5)
        2.  [Health Check](#org74822ec)
        3.  [alert & monitoring](#orgf9ed4f3)
    6.  [Auto Scaling](#org2261f33)
    7.  [CDN](#orgf805dc2)
    8.  [Scalability](#orge00eb47)
        1.  [Docker swarm + Nginx Load balancer](#org3567b50)
3.  [Backend Stack](#orgdf48fb7)
    1.  [Development](#orgd5177ed)
    2.  [Nodejs](#org12366bb)
        1.  [web framework](#org22aa745)
        2.  [testing](#orgca741b6)
        3.  [API doc](#org414507e)
        4.  [Data Validation/Schemas/serialize](#orgce1ca7c)
        5.  [Additional modules depending on each services](#org2487965)
    3.  [Web server](#org2e78cc7)
        1.  [Nginx](#org6e4fddf)
    4.  [SendGrid Email Support](#org835a632)
    5.  [Twilio SMS,MMS Support](#orgfd62edf)
4.  [Backend Application](#org87258d7)
    1.  [Infra Services](#orgad90d87)
        1.  [Message Services : manage message queue](#orgf8f95ff)
        2.  [Authentication/Authorization Services](#org0cd7741)
    2.  [Business Services](#orgd03b237)
        1.  [RFI Service](#orgc0b580e)
        2.  [Template/RFI Search Service](#org547fe4b)
        3.  [Vendor Manage Service](#orgc9c3fb1)
        4.  [Matching Service](#org59604c5)
        5.  [Static Content Manage Service](#orge90ca59)
        6.  [Alert/notification Service](#org60fdd50)
        7.  [Vendor/Customer Chat Service](#orgc1e6401)
        8.  [Payment/billing Service](#org6e5988b)
    3.  [Entities](#orge00d6f0)
        1.  [customer](#org9d0688e)
        2.  [vendor](#org6dcd9f0)
        3.  [admin](#orgfc01dce)
        4.  [RFI](#org444a8e2)
        5.  [project<sub>template</sub>](#org1657d86)
        6.  [notifications](#org84a5d64)
        7.  [static<sub>content</sub>](#org60b8d48)
    4.  [DB Schema](#org2791a3f)



<a id="org2c0405d"></a>

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

From the given metric, we can derive the metrics such as read/write throughput, bandwidth.

However, I decided to proceed without spending much time on estimating system specs since our priority is having showcase page and we are designing MVP.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="org955c8ef"></a>

# Infrastructure


<a id="org02dbaf5"></a>

## Centralized Event message


<a id="org4f2d2e1"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="orgf7aaa74"></a>

## Database


<a id="orgb565d0a"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="org0ad6d1a"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="orgcae5dee"></a>

## Deployment of Containers


<a id="orga8511d2"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org336d8ee"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org0205edf"></a>

## CI/CD


<a id="org3e00946"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="orgd73b908"></a>

### AWS codedeploy / codepipeline


<a id="org6ff420d"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org7f65be2"></a>

## High availability


<a id="orgc4ff3e5"></a>

### ECS & ECR


<a id="org74822ec"></a>

### Health Check

-   docker swarm has health check functionality


<a id="orgf9ed4f3"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="org2261f33"></a>

## Auto Scaling


<a id="orgf805dc2"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="orge00eb47"></a>

## Scalability

Each services can be horizontally scaled


<a id="org3567b50"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="orgdf48fb7"></a>

# Backend Stack


<a id="orgd5177ed"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org12366bb"></a>

## Nodejs


<a id="org22aa745"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="orgca741b6"></a>

### testing

-   jest


<a id="org414507e"></a>

### API doc

-   swagger


<a id="orgce1ca7c"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="org2487965"></a>

### Additional modules depending on each services


<a id="org2e78cc7"></a>

## Web server


<a id="org6e4fddf"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org835a632"></a>

## SendGrid Email Support


<a id="orgfd62edf"></a>

## Twilio SMS,MMS Support


<a id="org87258d7"></a>

# Backend Application


<a id="orgad90d87"></a>

## Infra Services


<a id="orgf8f95ff"></a>

### Message Services : manage message queue


<a id="org0cd7741"></a>

### Authentication/Authorization Services


<a id="orgd03b237"></a>

## Business Services


<a id="orgc0b580e"></a>

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


<a id="org547fe4b"></a>

### Template/RFI Search Service


<a id="orgc9c3fb1"></a>

### Vendor Manage Service


<a id="org59604c5"></a>

### Matching Service


<a id="orge90ca59"></a>

### Static Content Manage Service


<a id="org60fdd50"></a>

### Alert/notification Service


<a id="orgc1e6401"></a>

### Vendor/Customer Chat Service


<a id="org6e5988b"></a>

### Payment/billing Service


<a id="orge00d6f0"></a>

## Entities


<a id="org9d0688e"></a>

### customer


<a id="org6dcd9f0"></a>

### vendor


<a id="orgfc01dce"></a>

### admin


<a id="org444a8e2"></a>

### RFI


<a id="org1657d86"></a>

### project<sub>template</sub>


<a id="org84a5d64"></a>

### notifications


<a id="org60b8d48"></a>

### static<sub>content</sub>


<a id="org2791a3f"></a>

## DB Schema

