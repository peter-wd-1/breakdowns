
# Table of Contents

1.  [System Estimation](#orgc36e818)
2.  [Infrastructure](#orgd2930ad)
    1.  [Centralized Event message](#org9c68191)
        1.  [Message Server, Event Queue](#orgf7b6b51)
    2.  [Database](#orgfa9654f)
        1.  [{ Cloud Atlas | EC2 & Docker }](#orgbca085e)
        2.  [MongoDB](#orgb3d3938)
    3.  [Deployment of Containers](#orgd530ac0)
        1.  [Docker swarm](#orgc6853e7)
        2.  [{ EC2 | Digital Ocean | ECS }](#org424b43a)
    4.  [CI/CD](#orgaf48d00)
        1.  [Git lab(git action)](#org4b47451)
        2.  [AWS codedeploy / codepipeline](#orgdeacc0e)
        3.  [Shell script](#org744d224)
    5.  [High availability](#org577e6f9)
        1.  [ECS & ECR](#orgf6308dc)
        2.  [Health Check](#org0734a6e)
        3.  [alert & monitoring](#orgf989d72)
    6.  [Auto Scaling](#org82fe870)
    7.  [CDN](#orgf86ce54)
    8.  [Scalability](#org2113793)
        1.  [Docker swarm + Nginx Load balancer](#org0f0d58a)
3.  [Backend Stack](#org2a29fed)
    1.  [Development](#org797ea26)
    2.  [Nodejs](#orge8094ac)
        1.  [web framework](#orgd237535)
        2.  [testing](#org2a2db3b)
        3.  [API doc](#org7cccf4a)
        4.  [Data Validation/Schemas/serialize](#org756989d)
        5.  [Additional modules depending on each services](#orge52dc54)
    3.  [Web server](#org3014fa6)
        1.  [Nginx](#org3eccd21)
    4.  [SendGrid Email Support](#orgbc94051)
    5.  [Twilio SMS,MMS Support](#org51ff197)
4.  [Backend Application](#org5634393)
    1.  [Infra Services](#orgf7a0786)
        1.  [Message Services : manage message queue](#org7f88845)
        2.  [Authentication/Authorization Services](#org90c954c)
    2.  [Business Services](#orgb38ce2c)
        1.  [RFI Service](#org3466a05)
        2.  [Template/RFI Search Service](#org917e823)
        3.  [Vendor Manage Service](#org354c106)
        4.  [Matching Service](#org771f412)
        5.  [Static Content Manage Service](#org1b672aa)
        6.  [Alert/notification Service](#orga39e1db)
        7.  [Vendor/Customer Chat Service](#orgde1487f)
        8.  [Payment/billing Service](#org26da24f)
    3.  [Entities](#orgdd1984c)
        1.  [customer](#org5a2b3ae)
        2.  [vendor](#orgb06d6d0)
        3.  [admin](#org2efae54)
        4.  [RFI](#orgac39461)
        5.  [project<sub>template</sub>](#org4b1bd67)
        6.  [notifications](#org58f9760)
        7.  [static<sub>content</sub>](#org1e14f14)
    4.  [DB Schema](#orgf5bb65c)



<a id="orgc36e818"></a>

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


<a id="orgd2930ad"></a>

# Infrastructure


<a id="org9c68191"></a>

## Centralized Event message


<a id="orgf7b6b51"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   { Kafka | Redis } event message broker for conveying messages between services.


<a id="orgfa9654f"></a>

## Database


<a id="orgbca085e"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="orgb3d3938"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="orgd530ac0"></a>

## Deployment of Containers


<a id="orgc6853e7"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org424b43a"></a>

### { EC2 | Digital Ocean | ECS }


<a id="orgaf48d00"></a>

## CI/CD


<a id="org4b47451"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="orgdeacc0e"></a>

### AWS codedeploy / codepipeline


<a id="org744d224"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org577e6f9"></a>

## High availability


<a id="orgf6308dc"></a>

### ECS & ECR


<a id="org0734a6e"></a>

### Health Check

-   docker swarm has health check functionality


<a id="orgf989d72"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="org82fe870"></a>

## Auto Scaling


<a id="orgf86ce54"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="org2113793"></a>

## Scalability

Each services can be horizontally scaled


<a id="org0f0d58a"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org2a29fed"></a>

# Backend Stack


<a id="org797ea26"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="orge8094ac"></a>

## Nodejs


<a id="orgd237535"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org2a2db3b"></a>

### testing

-   jest


<a id="org7cccf4a"></a>

### API doc

-   swagger


<a id="org756989d"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orge52dc54"></a>

### Additional modules depending on each services


<a id="org3014fa6"></a>

## Web server


<a id="org3eccd21"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="orgbc94051"></a>

## SendGrid Email Support


<a id="org51ff197"></a>

## Twilio SMS,MMS Support


<a id="org5634393"></a>

# Backend Application


<a id="orgf7a0786"></a>

## Infra Services


<a id="org7f88845"></a>

### Message Services : manage message queue


<a id="org90c954c"></a>

### Authentication/Authorization Services


<a id="orgb38ce2c"></a>

## Business Services


<a id="org3466a05"></a>

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


<a id="org917e823"></a>

### Template/RFI Search Service


<a id="org354c106"></a>

### Vendor Manage Service


<a id="org771f412"></a>

### Matching Service


<a id="org1b672aa"></a>

### Static Content Manage Service


<a id="orga39e1db"></a>

### Alert/notification Service


<a id="orgde1487f"></a>

### Vendor/Customer Chat Service


<a id="org26da24f"></a>

### Payment/billing Service


<a id="orgdd1984c"></a>

## Entities


<a id="org5a2b3ae"></a>

### customer


<a id="orgb06d6d0"></a>

### vendor


<a id="org2efae54"></a>

### admin


<a id="orgac39461"></a>

### RFI


<a id="org4b1bd67"></a>

### project<sub>template</sub>


<a id="org58f9760"></a>

### notifications


<a id="org1e14f14"></a>

### static<sub>content</sub>


<a id="orgf5bb65c"></a>

## DB Schema

