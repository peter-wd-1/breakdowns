
# Table of Contents

1.  [System Estimation](#org6f9f451)
2.  [Infrastructure](#org6f83439)
    1.  [Docker Swarm on EC2](#org1fe9378)
        1.  [Docker Swarm](#org5830cfa)
        2.  [How many EC2 instance?](#org7a98d94)
    2.  [CDN](#orgb82b80f)
    3.  [API proxy](#org54f573b)
        1.  [Deploy](#org9aa20e5)
        2.  [Route Request from Client](#orgdf2fe07)
    4.  [Centralized Event message](#org95e6c77)
        1.  [EC2](#orgf97d75c)
        2.  [Message Server, Event Queue](#orga442e2b)
    5.  [Database](#org24fbfdd)
        1.  [Deploy : EC2 & Docker Swarm](#org07bd563)
        2.  [MongoDB](#org136bdc3)
    6.  [Deployment of Containers](#org0814ab6)
        1.  [Docker swarm](#orged5ca48)
        2.  [{ EC2 | Digital Ocean | ECS }](#org5c12b01)
    7.  [CI/CD](#orgc09152c)
        1.  [Git lab(git action)](#orgf3db4e4)
        2.  [AWS codedeploy / codepipeline](#orgb4f4c87)
        3.  [Shell script](#org68a132c)
    8.  [High availability](#org6ecfd04)
        1.  [ECS & ECR](#orgb4a749b)
        2.  [Health Check](#org869267d)
        3.  [alert & monitoring](#org2c67184)
    9.  [Auto Scaling](#org6d0e095)
    10. [Scalability](#org8cfb299)
        1.  [Docker swarm + Nginx Load balancer](#org3376310)
    11. [Frontend deploy](#org20f780e)
        1.  [Vercel](#orgc125554)
3.  [Backend Stack](#org19c4287)
    1.  [Development](#org5c2b4b7)
    2.  [Nodejs](#org32dc646)
        1.  [web framework](#org102efaf)
        2.  [testing](#org8504fa1)
        3.  [API doc](#org14de200)
        4.  [Data Validation/Schemas/serialize](#orgb1a0bed)
        5.  [Additional modules depending on each services](#orgbc2ce4a)
    3.  [Web server](#orga2655c7)
        1.  [Nginx](#orgddef5df)
    4.  [SendGrid Email Support](#org4b4ced2)
    5.  [Twilio SMS,MMS Support](#org69c8209)



<a id="org6f9f451"></a>

# System Estimation

We can assume some of metrics like

-   User
    -   vendor : 25000
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


<a id="org6f83439"></a>

# Infrastructure


<a id="org1fe9378"></a>

## Docker Swarm on EC2


<a id="org5830cfa"></a>

### Docker Swarm

-   Docker Compose is not designed for production environment
    -   No fail over response
    -   No rolling update
        -   Need to write solution for staging : Zero down time update
-   Docker Swarm have out-of-box solution For
    -   Health Check
    -   Zero downtime update & rolling
    -   LB for Container


<a id="org7a98d94"></a>

### How many EC2 instance?

-   We&rsquo;re going to use minimum number of EC2 host for our development.
-   It&rsquo;s okay to use multiple container on the same host


<a id="orgb82b80f"></a>

## CDN

We will need this if we want to publish out API to public

-   { cloudflare | cloudfront }
-   Enhance Security
    -   DDOS protection


<a id="org54f573b"></a>

## API proxy


<a id="org9aa20e5"></a>

### Deploy

-   EC2
-   Docker Swarm


<a id="orgdf2fe07"></a>

### Route Request from Client


<a id="org95e6c77"></a>

## Centralized Event message


<a id="orgf97d75c"></a>

### EC2


<a id="orga442e2b"></a>

### Message Server, Event Queue

-   Services can send(producer) request to event queue and polling(comsumer, in case of Kafka) event from there.
-   Kafka event message broker for conveying messages between services.


<a id="org24fbfdd"></a>

## Database


<a id="org07bd563"></a>

### Deploy : EC2 & Docker Swarm


<a id="org136bdc3"></a>

### MongoDB

-   Shading, if needed
-   Geospacial queries
    -   **this can be used for searching the closest vendor**


<a id="org0814ab6"></a>

## Deployment of Containers


<a id="orged5ca48"></a>

### Docker swarm

-   auto-recovery of services
-   rollbacks
-   health checks is available
-   rolling updates : zero downtime


<a id="org5c12b01"></a>

### { EC2 | Digital Ocean | ECS }


<a id="orgc09152c"></a>

## CI/CD


<a id="orgf3db4e4"></a>

### Git lab(git action)

-   automate deployment with AWS codedeploy


<a id="orgb4f4c87"></a>

### AWS codedeploy / codepipeline


<a id="org68a132c"></a>

### Shell script

-   for more customized way for deployment and server setting


<a id="org6ecfd04"></a>

## High availability


<a id="orgb4a749b"></a>

### ECS & ECR


<a id="org869267d"></a>

### Health Check

-   docker swarm has health check functionality


<a id="org2c67184"></a>

### alert & monitoring

-   slack for urgent alert.
-   grafana & telegraf & influxDB for system details monitoring
-   AWS cloudwatch


<a id="org6d0e095"></a>

## Auto Scaling


<a id="org8cfb299"></a>

## Scalability

Each services can be horizontally scaled


<a id="org3376310"></a>

### Docker swarm + Nginx Load balancer

nodejs cluster module can only distribute traffic on a single machine. But by using load balancer we can achieve horizontal scaling.


<a id="org20f780e"></a>

## Frontend deploy


<a id="orgc125554"></a>

### Vercel


<a id="org19c4287"></a>

# Backend Stack


<a id="org5c2b4b7"></a>

## Development

I have my own simple boilerplate for my work env to work with typescript nodejs project.

-   doom emacs
-   typescript
-   nodejs
-   CI/CD (refer to cloud infrastructure)


<a id="org32dc646"></a>

## Nodejs


<a id="org102efaf"></a>

### web framework

if routes are less then 40

-   express

if routes are more then 40~50

-   Fastify


<a id="org8504fa1"></a>

### testing

-   jest


<a id="org14de200"></a>

### API doc

-   swagger


<a id="orgb1a0bed"></a>

### Data Validation/Schemas/serialize

-   Avsc | Joi
-   mongoose


<a id="orgbc2ce4a"></a>

### Additional modules depending on each services


<a id="orga2655c7"></a>

## Web server


<a id="orgddef5df"></a>

### Nginx

-   cache response
-   reverse proxy
-   load balancer


<a id="org4b4ced2"></a>

## SendGrid Email Support


<a id="org69c8209"></a>

## Twilio SMS,MMS Support

