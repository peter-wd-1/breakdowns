
# Table of Contents

1.  [System Estimation](#org9cc6a48)
2.  [Infrastructure](#org22c10ea)
    1.  [Centralized Event message](#orgf226a73)
        1.  [Message Server, Event Queue](#org05e2c85)
    2.  [Database](#org8d9216c)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org4e4bd5e)
        2.  [MongoDB](#orgeeccd81)
    3.  [Deploy Container](#orgdb16d39)
        1.  [Docker swarm](#org9f6f5e0)
        2.  [{ EC2 | Digital Ocean | ECS }](#orgd207d2b)
    4.  [CI/CD](#org526d141)
        1.  [Shell script](#org3cc32d9)
        2.  [Git lab(git action)](#org6402019)
        3.  [AWS codedeploy / codepipeline](#org49d706a)
    5.  [High availability](#org77844fe)
        1.  [ECS & ECR](#orge643a56)
        2.  [Health Check](#orgc0036ed)
        3.  [alert & monitoring](#org1b8aad1)
    6.  [Latency handling](#org858e890)
        1.  [Kafka](#org3b50c73)
    7.  [Auto Scaling](#orgcaa6613)
    8.  [CDN](#orgad2c56a)
    9.  [Scalability](#orgc67f650)
        1.  [Docker swarm + Nginx Load balancer](#orga181233)
3.  [Backend Stack](#orga137dc0)
    1.  [Development](#org9678b38)
    2.  [Nodejs](#orgfe7c7b6)
        1.  [web framework](#org1fbd2f7)
        2.  [testing](#org7e5f937)
        3.  [API doc](#org377c95f)
        4.  [Data Validation/Schemas/serialize](#orgdfd244e)
        5.  [Additional modules depending on each services](#orgd95c1c1)
    3.  [Web server](#org50e6160)
        1.  [Nginx](#orgf913c45)
    4.  [SendGrid Email Support](#org0d12e54)
    5.  [Twilio SMS,MMS Support](#org27c981c)
4.  [Backend Application](#orgc8c4125)
    1.  [Infra Services](#org76ed516)
        1.  [Message Services : manage message queue](#org75abbdb)
        2.  [Authentication/Authorization Services](#orge8c3281)
    2.  [Business Services](#org1fd3665)
        1.  [RFI Service](#org28e850e)
        2.  [Template/RFI Search Service](#org9182196)
        3.  [Vendor Manage Service](#org1e65ba5)
        4.  [Matching Service](#orgdb4eeb2)
        5.  [Static Content Manage Service](#org182e193)
        6.  [Alert/notification Service](#org9607080)
        7.  [Vendor/Customer Chat Service](#org5c23e9c)
        8.  [Payment/billing Service](#orgb158b66)
    3.  [Entities](#org1c7ba19)
        1.  [customer](#org01550cc)
        2.  [vendor](#org951f18e)
        3.  [admin](#orgf050c2e)
        4.  [RFI](#org56c647e)
        5.  [project<sub>template</sub>](#org32e5b0c)
        6.  [notifications](#org24713ef)
        7.  [static<sub>content</sub>](#org659fb3d)
    4.  [DB Schema](#orgc0eda44)



<a id="org9cc6a48"></a>

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

From the given metric, we can drive read/write throughput, bandwidth.

However, our priority is having showcase page and we are designing MVP, I decided to proceed without spending much time on estimating system specs.
Rather, I would focus on designing more flexible system for horizontal scaling.

(If you think that considering large mount of read/write throughput per user, I would reconsider spending more time on estimating)

I think I can achieve this by using follow up high level design.


<a id="org22c10ea"></a>

# Infrastructure


<a id="orgf226a73"></a>

## Centralized Event message


<a id="org05e2c85"></a>

### Message Server, Event Queue

-   Services can send request to event queue
    and polling from there.
-   { Kafka | Redis } event message broker for conveying messages between services
-   Event will be stored and handled and will be eventually deleted as time decay


<a id="org8d9216c"></a>

## Database


<a id="org4e4bd5e"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orgeeccd81"></a>

### MongoDB

-   Shading
-   Geospacial queries
    **this can be used for searching the closest vendor**


<a id="orgdb16d39"></a>

## Deploy Container


<a id="org9f6f5e0"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : staging


<a id="orgd207d2b"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org526d141"></a>

## CI/CD


<a id="org3cc32d9"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org6402019"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="org49d706a"></a>

### AWS codedeploy / codepipeline


<a id="org77844fe"></a>

## High availability


<a id="orge643a56"></a>

### ECS & ECR


<a id="orgc0036ed"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org1b8aad1"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   cloudwatch


<a id="org858e890"></a>

## Latency handling


<a id="org3b50c73"></a>

### Kafka

-   message broker
-   message consumer & producer between service


<a id="orgcaa6613"></a>

## Auto Scaling


<a id="orgad2c56a"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="orgc67f650"></a>

## Scalability

Each services can be horizontally scaled


<a id="orga181233"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="orga137dc0"></a>

# Backend Stack


<a id="org9678b38"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="orgfe7c7b6"></a>

## Nodejs


<a id="org1fbd2f7"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org7e5f937"></a>

### testing

-   jest


<a id="org377c95f"></a>

### API doc

-   swagger


<a id="orgdfd244e"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orgd95c1c1"></a>

### Additional modules depending on each services


<a id="org50e6160"></a>

## Web server


<a id="orgf913c45"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org0d12e54"></a>

## SendGrid Email Support


<a id="org27c981c"></a>

## Twilio SMS,MMS Support


<a id="orgc8c4125"></a>

# Backend Application


<a id="org76ed516"></a>

## Infra Services


<a id="org75abbdb"></a>

### Message Services : manage message queue


<a id="orge8c3281"></a>

### Authentication/Authorization Services


<a id="org1fd3665"></a>

## Business Services


<a id="org28e850e"></a>

### RFI Service

-   create<sub>RFI</sub>(RFIData)
-   delete<sub>RFI</sub>(RFIID)
-   update<sub>RFI</sub>(RFIID,RFIData)
-   create<sub>user</sub><sub>RFI</sub>(userID, RFIID, RFIData)
-   delete<sub>user</sub><sub>RFI</sub>(userID, RFIID, RFIData)
-   update<sub>user</sub><sub>RFI</sub>(userID, RFIID, RFIData)
-   create<sub>MVP</sub><sub>template</sub>()
-   delete<sub>MVP</sub><sub>template</sub>()
-   update<sub>MVP</sub><sub>template</sub>()


<a id="org9182196"></a>

### Template/RFI Search Service


<a id="org1e65ba5"></a>

### Vendor Manage Service


<a id="orgdb4eeb2"></a>

### Matching Service


<a id="org182e193"></a>

### Static Content Manage Service


<a id="org9607080"></a>

### Alert/notification Service


<a id="org5c23e9c"></a>

### Vendor/Customer Chat Service


<a id="orgb158b66"></a>

### Payment/billing Service


<a id="org1c7ba19"></a>

## Entities


<a id="org01550cc"></a>

### customer


<a id="org951f18e"></a>

### vendor


<a id="orgf050c2e"></a>

### admin


<a id="org56c647e"></a>

### RFI


<a id="org32e5b0c"></a>

### project<sub>template</sub>


<a id="org24713ef"></a>

### notifications


<a id="org659fb3d"></a>

### static<sub>content</sub>


<a id="orgc0eda44"></a>

## DB Schema

