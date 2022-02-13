#+TITLE: Taskbreakdown


Should user have RFI attribute?

* Scope :
** Includes
- all the milestones in this document.
- 6 months of defect liability
  - No Cost of Remedying Defects.
  - Defect is malfunction of listed functionalities
- *Significant improvements of functionalities.*
  - Login UI
  - RFI builder UI
  - Color Thema

** Not Included
- Additional functionalities that is not listed here.

* What We use :
** Backend
- Nodejs & Express / Nginx
- Docker compose
- mongoDB Atlas
- AWS Elastic Search
- Redis(no storage, single excitable services) on ECR
- Kafca on ECR
- DDoS Protection
- Static file server
- Jenkins
- Githib
- AWS ECR
- AWS Elastic beanstalk
- Docker
- Elasticsearch/Kibana
** Frontend :
- ?GraphQL | Swagger
- React | Next
- React Admin
- Framer & Emotion & Chakra-ui
- Redux | ( Reducer & Context API )



* ~MildStone: 4~6~ Microservice infrastructures [0%]
I'll demonstrate test app once the infrastructures is ready
** [ ] Microservice Architecture Production [0%]
*** [ ] ~0.2~ Web Server / Application Server
- Nodejs & Express / Nginx
*** [ ] ~0.5~ Dadabase Server
- mongoDB Atlas
- AWS Elastic Search
*** [ ] ~1~ Message Server, Event queue
- Redis(no storage, single excitable services) on ECR
- Kafca on ECR
*** [ ] ~0.5~ CloudeFlare CDN
- DDoS Protection
- Static file server
*** [ ] ~1~ CI/CD
- Jenkins
- Githib
- AWS ECR
- AWS Elastic beanstalk
- Docker
*** [ ] ~1~ Logging/Monitoring
- Elasticsearch/Kibana
* ~MildStone: 1~2~ Home, customer|vendor [0%]
** [ ] Backend [0%]
*** [ ] ~0.2~ Content management service for home [0%]
CRUD API for home page contents, only by API call this will be used at this stage. We'll have full Admin dashboard UI down the line.
- data set of key-value pair (content title - content)
  i.e. main title : RFIReady
*** [ ] ~0.2~ Alert(notification) service for vendor|customer at home
Get Notification from Notification MongoDB and send notification to client or store
- Web Socket | Polling
- Web Push Notification : send notification when the site is not open.
- =post= [ /customer|vendor ]/notification/ : for other service/client to post the notification
- =get= [ /customer|vendor ]/notification/ : for other service/client to post the notification
*** [ ] ~0.2~ RFI templates at home
RFI smaples on home page is regular RFI data that has been picked by human or algorism.
- =CRUD= RFI/curatedlist -> *don't use templates. Name should change to regular id or uuid* : this will be managed by algorism or by human
- Curator
*** [ ] ~0.2~ GA tag setup
need analytics server?
want to store log data?
- admin can track every user action on analytics web site
** [ ] Front [0%]
*** [ ] customer [0%]
- [ ] Responsive Layout
- [ ] Slider Component
  - [ ] RFI template Component
    - =get= RFI/{ templates }
  - [ ] Testimonials Component
    - =get= admin/homecontent/Testimonials
  - [ ] Vewe More Pop RFI Detail
- [ ] Animation Component
  - [ ] scroll up animation (fade in/out)
  - [ ] floating animation
  - [ ] map animation
*** [ ] vendor [0%]
- [ ] Plan Component
  can add plan more then 3?
  - =get= admin/homecontent/plan/{symbol}
- [ ] Contact RFI Team for onboarding and evaluation
*** [ ] common [0%]
- [ ] NavBar and page route setup
- [ ] Color Thema Context
  - Light/Dark Thema Context switch
- [ ] Alert Polling vendor/customer
  - websocket or regular polling(depends on user traffic and urgentness of alert)
  - [ ] websocket
  - [ ] polling
    - =get= [ customer|vendor ]/{who}/notification
- [ ] Search Button, need search? / temp search : two search boxs teamp : location :google api
- [ ] GA Tag setup
- [ ] vendor/customer Context switch
- [ ] url thumbnail meta tag
*** [ ] About Us [0%]
- [ ] Responsive Layout(need draft)
  - =get= admin/homecontent/aboutus
  - =post= contact/
*** [ ] SEO [0%]
- [ ] Meta data input
  - thumbname, details

* ~MildStone: 1~ Authentication, Authorization
** [ ] Backend [0%]
*** [ ] ~0.5~ User authentication service
User Delete?
- Authentication service
  - Google Login
  - Email Verification
    - url w/code generate : one time url
    - expiration of code
    - [ polling | websocket ] for real-time user image verification
- Authorization service
  - Customer, Vendor, Admin
- User DB
  - Enabled Service
  - Token
*** [ ] ~0.2~ Email Service
- Sending email to user
- No-reply
** [ ] Front [0%]
*** [ ] ~0.5~ Sign Up UI
Survey like UI
Only one question at a time.
- Customer / Vendor
- Email insert / Email Verification / Email Expiration
- Password / Error Validation
- Google Authentication

* ~MildStone: 0.5~ Payment Server [0%]
User can use their card without re-registering their card information
** [ ] Backend [0%]
*** ~0.25~ Payment service
3rd Party Integration
Stripe Checkout, paypal integration, make user session get result via webhook
- [ ] Create user session for payment
- [ ] Store user card token
** [ ] Front [0%]
From where witch button leads to payament?
*** ~0.25~ Payment Page [0%]

* ~MildStone: 3~4~ Instant RFI Box, Template Search [0%]
** [ ] Backend [0%]
*** [ ] ~0.5~ RFI Serive
RFI DB provider
All the RFI Data will be stored and managed here.
- RFI Data storage =CRUD= API,
  - need JSON data format data to input DB
  - need to check the spread sheet of the actual data.
    - I might need extra work for converting dataset to JSON
*** [ ] ~1.5~ Template Search engine Service
- Text Search request for user input
  - Response per user input
    - Search from Indexed Template DB for Elastic Search
      - Original data will be stored MongoDB
- Address Search Get full address
  - Google Geographic API
    *Make sure the template data compatibility with google geographic address data*
- User can search separately.
- User can search with combination of both address and text
- Response with template result
*** [ ] ~?~ ML Server for User upload Template Document
ML model will read user uploaded document to create RFI and server will send to client.
- output should be JSON format data that client can parse it and input RFI
** [ ] Front [0%]
*** [ ] ~1~ Instant RFI Box
This will completed RFI build process with minimum mount of parameters
- [ ] Input Form Component [0%]
  Type form like questionare UI
  Send RFI =POST= RFI
- ~improvement~ Project Type UI : images or visualizable presets
  i.e. user can swipe to next to see key features of MVP site with image.
*** [ ] ~0.2~ Template Search Bar
*** [ ] ~0.3~ Template Result List
This shows search results
- [ ] RFI List View
- [ ] Send RFI =POST= RFI
*** [ ] ~0.5~ Template Details Page
- [ ] Accordion View
- [ ] Document File upload
* ~MildStone: 7~8~ RFI Builder
~improvement~ There should be a significant improvement in perspective of UI/UX for this, Therefore I included time for designing the UI/UX
** [ ] Backend [0%]
*** [ ] ~1~ Builder Auto Input Service
Business rule for auto input rest of parameters.
Pre-defined Excel sheet rules will be applied
*** [ ] ~2~ Custom Questions ( Real-time Chat ) Service
*Chat server will open chat per companies that answered custome questions*
this chat will be change matching score.
** [ ] Front [%]
*** [ ] ~1~ RFI builder
**** User action
- [ ] Send RFI
- [ ] Contact for Vendor Selection Free Service pops up,
*** [ ] ~1~ RFI Editor
**** User action
- [ ] Change parameters
*** [ ] ~2~ Custom Questions ( Real-time Chat ) UI
User will see chat list with corresponding companies that answerd thier custome quetion.

* ~MildStone: 8~9~ Vendor OnBoarding/Dashboard
** [ ] Backend [0%]
*** [ ] ~1~ Vendor Service
vendor DB provider.
statick data provider
*** [ ] ~3~ Vendor dashboard
** [ ] Front [0%]
*** [ ] ~1~ Vendor On-boarding UI Form
- document uploader
*** [ ] ~3~ Vendor dashboard
* ~MildStone:~
* ~MildStone:~
* ~MildStone:~ Results Screen [0%]
** Contact RFITeam button and Form for clients
** User(Customer) Page
*** Template List Status
* ~MildStone:~ Registration/Email Verification
* ~MildStone:~ Vendor Details Page
* ~MildStone:~ Recommendation Engine
* ~MildStone:~ Registration/Email Verification
