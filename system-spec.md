
# Table of Contents

1.  [System Estimation](#orgb5dbc72)
2.  [Infrastructure](#org696bb85)
    1.  [Centralized Event message](#org0ddbd63)
        1.  [Message Server, Event Queue](#org40dc7ac)
    2.  [Database](#org3d513c5)
        1.  [{ Cloud Atlas | EC2 & Docker }](#org2e43a00)
        2.  [MongoDB](#org5cb0ced)
    3.  [Deploy Container](#orgb0656a6)
        1.  [Docker swarm](#orgeadda6d)
        2.  [{ EC2 | Digital Ocean | ECS }](#org323a0b7)
    4.  [CI/CD](#org00cefea)
        1.  [Shell script](#org065fe8c)
        2.  [Git lab(git action)](#orgd383ba8)
        3.  [AWS codedeploy / codepipeline](#org4b95876)
    5.  [High availability](#orgdbd21a2)
        1.  [ECS & ECR](#org7ac60dc)
        2.  [Health Check](#orgabce4a6)
        3.  [alert & monitoring](#org1adaffb)
    6.  [Latency handling](#orgc65b2cf)
        1.  [Kafka](#org234ef16)
    7.  [Auto Scaling](#org16f94c6)
    8.  [CDN](#org7124f5f)
    9.  [Scalability](#orgdab736f)
        1.  [Docker swarm + Nginx Load balancer](#orgcd222d9)
3.  [Backend Stack](#org57bde63)
    1.  [Development](#orgb8773bb)
    2.  [Nodejs](#org4bf287d)
        1.  [web framework](#org6dd660b)
        2.  [testing](#orgbc73227)
        3.  [API doc](#org3eb7e10)
        4.  [Data Validation/Schemas/serialize](#org4cc12ef)
        5.  [Additional modules depending on each services](#orge462b44)
    3.  [Web server](#org93d68f2)
        1.  [Nginx](#org7eada90)
    4.  [SendGrid Email Support](#org918217d)
    5.  [Twilio SMS,MMS Support](#orgc113e8a)
4.  [Backend Application](#orgadefcfe)
    1.  [Infra Services](#org4fa26c6)
        1.  [Message Services : manage message queue](#org7ac172f)
        2.  [Authentication/Authorization Services](#org0fb4377)
    2.  [Business Services](#orge8515a9)
        1.  [RFI Service](#orgea3f846)
        2.  [Template/RFI Search Service](#orgb233c24)
        3.  [Vendor Manage Service](#orgef0f9e8)
        4.  [Matching Service](#org7a27da4)
        5.  [Static Content Manage Service](#orgfeb2980)
        6.  [Alert/notification Service](#orgee664fc)
        7.  [Vendor/Customer Chat Service](#orgf4b578e)
        8.  [Payment/billing Service](#orgccd7b92)
    3.  [Entities](#org7206d84)
        1.  [customer](#orgb676d96)
        2.  [vendor](#orga0d9c20)
        3.  [admin](#org1cf421c)
        4.  [RFI](#org18e83db)
        5.  [project<sub>template</sub>](#org5927ad9)
        6.  [notifications](#org7dc39cc)
        7.  [static<sub>content</sub>](#org6eebd84)
    4.  [DB Schema](#orgb66af4a)



<a id="orgb5dbc72"></a>

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


<a id="org696bb85"></a>

# Infrastructure


<a id="org0ddbd63"></a>

## Centralized Event message


<a id="org40dc7ac"></a>

### Message Server, Event Queue

-   Services can send request to event queue
    and polling from there.
-   { Kafka | Redis } event message broker for conveying messages between services
-   Event will be stored and handled and will be eventually deleted as time decay


<a id="org3d513c5"></a>

## Database


<a id="org2e43a00"></a>

### { Cloud Atlas | EC2 & Docker }


<a id="org5cb0ced"></a>

### MongoDB

-   Shading
-   Geospacial queries
    **this can be used for searching the closest vendor**


<a id="orgb0656a6"></a>

## Deploy Container


<a id="orgeadda6d"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : staging


<a id="org323a0b7"></a>

### { EC2 | Digital Ocean | ECS }


<a id="org00cefea"></a>

## CI/CD


<a id="org065fe8c"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="orgd383ba8"></a>

### Git lab(git action)

-   deploy automation with AWS codedeploy


<a id="org4b95876"></a>

### AWS codedeploy / codepipeline


<a id="orgdbd21a2"></a>

## High availability


<a id="org7ac60dc"></a>

### ECS & ECR


<a id="orgabce4a6"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org1adaffb"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   cloudwatch


<a id="orgc65b2cf"></a>

## Latency handling


<a id="org234ef16"></a>

### Kafka

-   message broker
-   message consumer & producer between service


<a id="org16f94c6"></a>

## Auto Scaling


<a id="org7124f5f"></a>

## CDN

-   { cloudflare | cloudfront }


<a id="orgdab736f"></a>

## Scalability

Each services can be horizontally scaled


<a id="orgcd222d9"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org57bde63"></a>

# Backend Stack


<a id="orgb8773bb"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org4bf287d"></a>

## Nodejs


<a id="org6dd660b"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="orgbc73227"></a>

### testing

-   jest


<a id="org3eb7e10"></a>

### API doc

-   swagger


<a id="org4cc12ef"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orge462b44"></a>

### Additional modules depending on each services


<a id="org93d68f2"></a>

## Web server


<a id="org7eada90"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org918217d"></a>

## SendGrid Email Support


<a id="orgc113e8a"></a>

## Twilio SMS,MMS Support


<a id="orgadefcfe"></a>

# Backend Application


<a id="org4fa26c6"></a>

## Infra Services


<a id="org7ac172f"></a>

### Message Services : manage message queue


<a id="org0fb4377"></a>

### Authentication/Authorization Services


<a id="orge8515a9"></a>

## Business Services


<a id="orgea3f846"></a>

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


<a id="orgb233c24"></a>

### Template/RFI Search Service


<a id="orgef0f9e8"></a>

### Vendor Manage Service


<a id="org7a27da4"></a>

### Matching Service


<a id="orgfeb2980"></a>

### Static Content Manage Service


<a id="orgee664fc"></a>

### Alert/notification Service


<a id="orgf4b578e"></a>

### Vendor/Customer Chat Service


<a id="orgccd7b92"></a>

### Payment/billing Service


<a id="org7206d84"></a>

## Entities


<a id="orgb676d96"></a>

### customer


<a id="orga0d9c20"></a>

### vendor


<a id="org1cf421c"></a>

### admin


<a id="org18e83db"></a>

### RFI


<a id="org5927ad9"></a>

### project<sub>template</sub>


<a id="org7dc39cc"></a>

### notifications


<a id="org6eebd84"></a>

### static<sub>content</sub>


<a id="orgb66af4a"></a>

## DB Schema

