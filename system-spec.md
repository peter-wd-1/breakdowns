
# Table of Contents

1.  [System Estimation](#orgc907bf2)
2.  [Infrastructure](#org118da2f)
    1.  [Centralized Event message](#orgb3f2bac)
        1.  [Message Server, Event Queue](#org2973f05)
    2.  [Database](#orgfa4360a)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org0c525cb)
        2.  [MongoDB](#orgaeadd11)
    3.  [Deployment of Containers](#org3a4ae6d)
        1.  [Docker swarm](#orge65dc25)
        2.  [{ EC2 | Digital Ocean | ECS }](#org8fdcfbd)
    4.  [CI/CD](#org90ef4d4)
        1.  [Git lab(git action)](#org981658a)
        2.  [AWS codedeploy / codepipeline](#org7501273)
        3.  [Shell script](#org04eba3b)
    5.  [High availability](#org68912c6)
        1.  [ECS & ECR](#orgcf2c797)
        2.  [Health Check](#org29b4780)
        3.  [alert & monitoring](#org0f78ddd)
    6.  [Auto Scaling](#org051b18d)
    7.  [CDN](#org51e6db8)
    8.  [Scalability](#org391bc0e)
        1.  [Docker swarm + Nginx Load balancer](#org9cac786)
3.  [Backend Stack](#orge24acc7)
    1.  [Development](#org1242f4f)
    2.  [Nodejs](#org34b8bac)
        1.  [web framework](#org5d420c4)
        2.  [testing](#org8156b5c)
        3.  [API doc](#orgdb617b7)
        4.  [Data Validation/Schemas/serialize](#org00c35c8)
        5.  [Additional modules depending on each services](#orge8b44ed)
    3.  [Web server](#orge7aeaf0)
        1.  [Nginx](#org58509e6)
    4.  [SendGrid Email Support](#org98d77ca)
    5.  [Twilio SMS,MMS Support](#org5cd68c4)
4.  [Backend Application](#org991b390)
    1.  [Infra Services](#org316f8dc)
        1.  [Message Services : manage message queue](#org9d93516)
        2.  [Authentication/Authorization Services](#org906d6ca)
    2.  [Business Services](#orgc17df48)
        1.  [RFI Service](#org09a195c)
        2.  [Template/RFI Search Service](#org2f2a229)
        3.  [Vendor Manage Service](#org409e4af)
        4.  [Matching Service](#org583f45f)
        5.  [Static Content Manage Service](#org38d4216)
        6.  [Alert/notification Service](#org72bac21)
        7.  [Vendor/Customer Chat Service](#orga887e75)
        8.  [Payment/billing Service](#orge3c07e9)
    3.  [Entities](#orgbc54137)
        1.  [customer](#orgcd06a64)
        2.  [vendor](#org8b6fac4)
        3.  [admin](#org2db1569)
        4.  [RFI](#org0d5ab3d)
        5.  [project template](#org48ed4d4)
        6.  [notifications](#org2fa5908)
        7.  [static content](#org760eaa8)
    4.  [DB Schema](#org5da4a37)



<a id="orgc907bf2"></a>

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


<a id="org118da2f"></a>

# Infrastructure


<a id="orgb3f2bac"></a>

## Centralized Event message


<a id="org2973f05"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="orgfa4360a"></a>

## Database


<a id="org0c525cb"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orgaeadd11"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="org3a4ae6d"></a>

## Deployment of Containers


<a id="orge65dc25"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org8fdcfbd"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org90ef4d4"></a>

## CI/CD


<a id="org981658a"></a>

### Git lab(git action)

-   automate deployment with AWS codedeploy


<a id="org7501273"></a>

### AWS codedeploy / codepipeline


<a id="org04eba3b"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org68912c6"></a>

## High availability


<a id="orgcf2c797"></a>

### ECS & ECR


<a id="org29b4780"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org0f78ddd"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="org051b18d"></a>

## Auto Scaling


<a id="org51e6db8"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org391bc0e"></a>

## Scalability

Each services can be horizontally scaled


<a id="org9cac786"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="orge24acc7"></a>

# Backend Stack


<a id="org1242f4f"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org34b8bac"></a>

## Nodejs


<a id="org5d420c4"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org8156b5c"></a>

### testing

-   jest


<a id="orgdb617b7"></a>

### API doc

-   swagger


<a id="org00c35c8"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orge8b44ed"></a>

### Additional modules depending on each services


<a id="orge7aeaf0"></a>

## Web server


<a id="org58509e6"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org98d77ca"></a>

## SendGrid Email Support


<a id="org5cd68c4"></a>

## Twilio SMS,MMS Support


<a id="org991b390"></a>

# Backend Application


<a id="org316f8dc"></a>

## Infra Services


<a id="org9d93516"></a>

### Message Services : manage message queue


<a id="org906d6ca"></a>

### Authentication/Authorization Services


<a id="orgc17df48"></a>

## Business Services


<a id="org09a195c"></a>

### RFI Service

-   `create_RFI(RFIData)`
-   `delete_RFI(RFIID)`
-   `update_RFI(RFIID,RFIData)`
-   `create_user_RFI(userID, RFIID, RFIData)`
-   `delete_user_RFI(userID, RFIID)`
-   `update_user_RFI(userID, RFIID, RFIData)`
-   `create_project_template(vendorID, projectData)`
-   `delete_project_template(vendorID, projectID)`
-   `update_project_template(vendorID, projectID, projectData)`


<a id="org2f2a229"></a>

### Template/RFI Search Service


<a id="org409e4af"></a>

### Vendor Manage Service


<a id="org583f45f"></a>

### Matching Service


<a id="org38d4216"></a>

### Static Content Manage Service


<a id="org72bac21"></a>

### Alert/notification Service


<a id="orga887e75"></a>

### Vendor/Customer Chat Service


<a id="orge3c07e9"></a>

### Payment/billing Service


<a id="orgbc54137"></a>

## Entities

description of entities and its attridu


<a id="orgcd06a64"></a>

### customer


<a id="org8b6fac4"></a>

### vendor


<a id="org2db1569"></a>

### admin


<a id="org0d5ab3d"></a>

### RFI


<a id="org48ed4d4"></a>

### project template


<a id="org2fa5908"></a>

### notifications


<a id="org760eaa8"></a>

### static content


<a id="org5da4a37"></a>

## DB Schema

