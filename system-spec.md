
# Table of Contents

1.  [System Estimation](#org649f4c6)
2.  [Infrastructure](#org5219145)
    1.  [Centralized Event message](#org876754e)
        1.  [Message Server, Event Queue](#org1dea818)
    2.  [Database](#orga55a007)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org9cdb34e)
        2.  [MongoDB](#orgce3e9fa)
    3.  [Deployment of Containers](#orgc83122e)
        1.  [Docker swarm](#org84245a8)
        2.  [{ EC2 | Digital Ocean | ECS }](#org80f12b3)
    4.  [CI/CD](#org4674901)
        1.  [Git lab(git action)](#org400d920)
        2.  [AWS codedeploy / codepipeline](#org9a58d11)
        3.  [Shell script](#org87bc4e2)
    5.  [High availability](#orgc58acfd)
        1.  [ECS & ECR](#orgfb8c768)
        2.  [Health Check](#org3033b8f)
        3.  [alert & monitoring](#org4e829fe)
    6.  [Auto Scaling](#org1169a7d)
    7.  [CDN](#org2770b1e)
    8.  [Scalability](#org1797375)
        1.  [Docker swarm + Nginx Load balancer](#orgf6eca40)
3.  [Backend Stack](#org81036cb)
    1.  [Development](#orge3bb9bc)
    2.  [Nodejs](#org54cb34d)
        1.  [web framework](#orgfad61e0)
        2.  [testing](#org4e001c0)
        3.  [API doc](#orgfcba2a1)
        4.  [Data Validation/Schemas/serialize](#org67f248d)
        5.  [Additional modules depending on each services](#org8fe52ed)
    3.  [Web server](#org833627d)
        1.  [Nginx](#org730b8c3)
    4.  [SendGrid Email Support](#org1283103)
    5.  [Twilio SMS,MMS Support](#org0864e94)
4.  [Backend Application](#org9961dc5)
    1.  [Infra Services](#org498e64f)
        1.  [Message Services : manage message queue](#orgcd6b75b)
        2.  [Authentication/Authorization Services](#org00bbe52)
    2.  [Business Services](#org0f5897f)
        1.  [RFI Service](#orgd1ff5fa)
        2.  [Template/RFI Search Service](#org246e63d)
        3.  [Vendor Manage Service](#orgdb9ffb2)
        4.  [Matching Service](#org0cf2509)
        5.  [Static Content Manage Service](#org3e81faa)
        6.  [Alert/notification Service](#org95ef213)
        7.  [Vendor/Customer Chat Service](#orgb962512)
        8.  [Payment/billing Service](#org1ac1135)
    3.  [Entities](#orge0500be)
        1.  [customer](#org11a8ae6)
        2.  [vendor](#org68bf78f)
        3.  [admin](#org3b620e3)
        4.  [RFI](#org4bf1cc4)
        5.  [project<sub>template</sub>](#org74e3fd9)
        6.  [notifications](#orge7c99cf)
        7.  [static<sub>content</sub>](#org3a171f4)
    4.  [DB Schema](#orgbccb004)



<a id="org649f4c6"></a>

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

However, I decided to proceed without spending much time on estimating system capacity since our priority is having showcase page and we are designing MVP.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="org5219145"></a>

# Infrastructure


<a id="org876754e"></a>

## Centralized Event message


<a id="org1dea818"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="orga55a007"></a>

## Database


<a id="org9cdb34e"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orgce3e9fa"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="orgc83122e"></a>

## Deployment of Containers


<a id="org84245a8"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org80f12b3"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org4674901"></a>

## CI/CD


<a id="org400d920"></a>

### Git lab(git action)

-   automate deployment with AWS codedeploy


<a id="org9a58d11"></a>

### AWS codedeploy / codepipeline


<a id="org87bc4e2"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="orgc58acfd"></a>

## High availability


<a id="orgfb8c768"></a>

### ECS & ECR


<a id="org3033b8f"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org4e829fe"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="org1169a7d"></a>

## Auto Scaling


<a id="org2770b1e"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org1797375"></a>

## Scalability

Each services can be horizontally scaled


<a id="orgf6eca40"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org81036cb"></a>

# Backend Stack


<a id="orge3bb9bc"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org54cb34d"></a>

## Nodejs


<a id="orgfad61e0"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org4e001c0"></a>

### testing

-   jest


<a id="orgfcba2a1"></a>

### API doc

-   swagger


<a id="org67f248d"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="org8fe52ed"></a>

### Additional modules depending on each services


<a id="org833627d"></a>

## Web server


<a id="org730b8c3"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org1283103"></a>

## SendGrid Email Support


<a id="org0864e94"></a>

## Twilio SMS,MMS Support


<a id="org9961dc5"></a>

# Backend Application


<a id="org498e64f"></a>

## Infra Services


<a id="orgcd6b75b"></a>

### Message Services : manage message queue


<a id="org00bbe52"></a>

### Authentication/Authorization Services


<a id="org0f5897f"></a>

## Business Services


<a id="orgd1ff5fa"></a>

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


<a id="org246e63d"></a>

### Template/RFI Search Service


<a id="orgdb9ffb2"></a>

### Vendor Manage Service


<a id="org0cf2509"></a>

### Matching Service


<a id="org3e81faa"></a>

### Static Content Manage Service


<a id="org95ef213"></a>

### Alert/notification Service


<a id="orgb962512"></a>

### Vendor/Customer Chat Service


<a id="org1ac1135"></a>

### Payment/billing Service


<a id="orge0500be"></a>

## Entities


<a id="org11a8ae6"></a>

### customer


<a id="org68bf78f"></a>

### vendor


<a id="org3b620e3"></a>

### admin


<a id="org4bf1cc4"></a>

### RFI


<a id="org74e3fd9"></a>

### project<sub>template</sub>


<a id="orge7c99cf"></a>

### notifications


<a id="org3a171f4"></a>

### static<sub>content</sub>


<a id="orgbccb004"></a>

## DB Schema

