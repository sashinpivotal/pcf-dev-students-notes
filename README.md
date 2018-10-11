# PCF Developer Training 

The purpose of this document to supplement the official training materials
with the following items.

-   Supplemental topics
-   Error in the lab document
-   Challenge questions
-   Lab extras
-   Trouble-shooting tips
-   References
-   Solutions to the lab
-   After the training (certification, etc.)

## Basic Definitions: Getting Started w/ the CF CLI

### Supplemental topics

-   IaaS
-   CaaS -> Docker, Kubernetes, PKS
-   PaaS -> Cloud Foundry, PAS (pre-2.0 Elastic Runtime)
-   FaaS
-   BOSH - The secret sauce of Cloud Foundry
-   App Manager
-   Op Manager - value add of PCF (Operator)

### Trouble-shooting

-   If you are using GitBash under Windows, you might 
    experience a problem when you do 
    `cf login -a api.run.pivotal.io`.
    Try the following if that happens.
    
    `cf auth <username> <password>`

## Lab extras

-   Try [PCF AppManager](https://login.run.pivotal.io)

## Tech Overview: Pushing Apps

### Challenge questions

-   What is the PCF component that handles the requests coming 
    from "cf cli"?
-   What is the PCF component that handles routing?
-   How many containers are needed for pushing an app?
-   Why do you see “container gets created and destroyed” as part of pushing app?
-   What is the “org”/“space” structure in your PCF installation?
-   Why do you have to use “--random-route”? 
-   What is the “cf push” option that lets you specify the hostname 
    part of a route? (“cf push -h”)
    
### References

-   [PCF Architectural diagram](http://htmlpreview.github.io/?https://raw.githubusercontent.com/cloudfoundry-incubator/diego-design-notes/master/clickable-diego-overview/clickable-diego-overview.html)

## Core Themes: Scaling Apps

### Lab Extras

-   Install and use "open" plugin from [Cloud Foundry community](https://plugins.cloudfoundry.org/) using the following command

    ```
    cf install-plugin -r CF-Community "open"
    ```
    
    You can then use it as shown below, which will automatically opens
    a browser with the correct URL of the `roster` app.
    
    ```
    cf open roster
    ```
    
-   Try PCF autoscaling feature in PWS App Manager
-   Try `app-autoscaler` service (do `cf marketplace`)


## Logging Metrics: Application Logs and & Events

### Challenge questions

-   Which component of PCF architecture is responsible for 
    collecting logs?
-   What are the other log types other than STG, APP, CELL, RTR?
    (Google `PCF log types`)
-   What is the responsibility of Doppler?
-   What is the responsibility of a developer in order to 
    take advantage of PCF logging system?


## Resiliency: Application Resiliency

### Trouble-shooting

-  If you see the following message when you send the `kill` request,
   it is an expected behavior. 

   ```
   502 Bad Gateway: Registered endpoint failed to handle the request
   ```

    The `kill` operation invokes a `System.exit` command during the
    request, the request never sends a response to the go-router,
    and the go-router throws back the 502 to the client.
    
### Challenge questions

-  Which component of the PCF architecture maintains the `desired
   state` and `actual state` of the an application?
-  What are 4 levels of resilency (or recoverability) PCF provides?
-  When Diego system is asked to increase number of an application
   instances by 3, does it create all 3 instances in a single cell?
-  What are the value proposition of PCF from the standpoint of
   2nd-day operation? 

## Services: Creating & Bind Services
    
### Chllenge questions

-   What is a service? What is a service instance?
-   What is the role of service broker?  What rest 
    operations does it support?
-   What is a `marketplace`? What is a `plan`?
-   What is binding?  What is the end result of a binding?
-   What `cf` command can you use to tell what services are bound to an application?
-   What `cf` command can you use to tell what applications are bound a service?
-   Once you bind an application to a service instance, do
    you do "restart" or "restaging"?
-   What is the difference between "brokered service" (the 
    services you find under `cf marketplace`) and "user provided
    service"?
-   What are the three different types of user provided service?
    (Try `cf cups -h`)

### Lab Extras

-   Install "mysql" cf plugin and use it to access the database (you
    will have to install `mysql` client first)
    
### Trouble-shooting

-   On Windows, the `curl` command with `POST` might result json   
    conversion error

    ```
    curl -H "Content-Type: application/json" -X POST -d '{"firstName":"<some-key>","lastName":"<some-value>"}' http://<YOUR-APP-URL>/people
    ```
    
    When it happens, you can capture the data part below into a file, i.e. 
    `person.json`
    
    ```
    {"firstName":"<some-key>","lastName":"<some-value>"}
    ```
    
    Then you can use `-d @<file-name>`
    
    ```
    curl -H "Content-Type: application/json" -X POST -d @person.json http://<YOUR-APP-URL>/people
    ```
    
### Refereneces

-   [Cloud Foundry Service Broker API](https://github.com/openservicebrokerapi/servicebroker/blob/v2.12/spec.md)

## 12 Factor Applications: Environment Variables & App Manifest 

### Challenge questions

-   What is the value proposition of setting environment variables
    in the manifest file in regard to providing application
    configuration data?  Is it better than setting configuration
    data in the `application.properties` file?
-   When you have many environment variables to set or complex 
    configuration to do, what would be a better option than
    using environment variables?  (Answer: Think of the usage of
    another backing service called config server - under PWS, it
    is called `p-config-server`.)
   
### Lab extras 1  

-   Try to overide the memory size by setting the `-m 768M` 
    while using manifest
-   Use `cf create-app-manifest` and use the newly creted manifest file 
    to push an application    
-   Use `health-check-type` and `health-check-http-endpoint` 

### Lab extras 2

-   Create and run Spring Boot application in both locally and PCF  
    -   It is a RESTful application
    -   When accessed with "http://localhost:8080/hello", it displays
        greeting message
    -   Locally the greeting message should be maintained in the 
        application.yml file
    -   Deploy the same app in PCF
    -   For PCF, the greeting message should be set via environment
        variable
    -   Redeploy the application using a manifest file
    
### References

-   [12 factor presentation](https://content.pivotal.io/slides/the-12-factors-for-building-cloud-native-software) 
-   [Beyond 12 Factor (15 Factor)](https://content.pivotal.io/blog/beyond-the-twelve-factor-app) or [Beyond 12 Factor (15 Factor)](https://www.oreilly.com/library/view/beyond-the-twelve-factor/9781492042631/)
-   [Spring External Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)
-   [YMAL tutorial site](https://gettaurus.org/docs/YAMLTutorial/)
    

## Log Drains 

### Lab extras

-   In the PaperTrail dashboard, try to filter only logs from `CELL`
-   Try different syslog drainers such as ELK/Kibana or Splunk

### Trouble-shooting

-   It might take a few minutes before you see the logs in the Papertrail 
-   If you do not see the logs in the Papertrail even after a few
    minutes, make sure you create service using
    `syslog` (as shown below) or `syslog-tls` 

    ```
    cf cups my-drain-service -l syslog://<papertrail-url>
    ```

## Manipulating Routes - Blue/Green Deployment

### Supplemental topics

-   Semantic versioning - Major.Minor.Patch
-   Feature toggles

### Steps for blue-green deployment

1.  V1 - R1 is currently running
1.  Create V2 with R2 and make sure it is working
1.  Add R1 to V2 - now V2 handles both R1 and R2
1.  Remove R1 from V1 - now V2 handles 100% of R1
1.  Remove R2 from V2 - now V2 handles only R1
2.  Rename V2 to V1

### Challenge questions

-   Can an application have multiple routes?
-   Can a route be applied to multiple applications?
-   Can a route exist without an application associated with it? 
    (See “cf routes” and “cf create-route” commands.)
-   What could be the use case of "cf create-route"?
-   When mapping or unmapping routes, do you have to restart
    or restage an application?
-   How can you delete all routes that are not associated with
    any apps?
-   What is the logical steps of blue-green deployment?
-   How can we control the ratio of the traffic between V1.0.1 (blue) 
    vs. V1.0.2 (green)?
-   Is blue-green deployment suitable for major feature change?
-   Can you describe which PCF components are responsible for
    updating the routing table whenever a new instance is created
    or old instance gets destroyed?
    
### Referrences

-   [CanaryRelease](https://martinfowler.com/bliki/CanaryRelease.html)
-   [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html)
  
## App Execution & Security Groups: Setting up App Monitoring with New Relic

### Supplemental topics

-   Egress traffic
-   Telemetry
-   PCF Metrics vs APM tool

### Challenge questions

-   What are the default ASG's?
    
### Lab extras

-   Try `cf` commands relevant to security groups   
-   Try PCF Metrics from the App Manager

## Staging and Running: Push the Web-UI
    
### Challenge questions

-   What could be use cases where you will have to do “cf restage” 
    (as opposed to “cf restart”)?
-   Suppose you deployed an application with “cf push roster -m 768M”, 
    what would be memory allocated when you re-deployed the 
    same application with “cf push roster“?  
-   When you delete an application using `cf delete <app-name>`, is
    the associated route gets deleted as well?
    
### How to debug when things don't work in this lab

-   See if the `web-ui` app has `url` (`cf env web-ui`) environment var
    -   The `rest-backend` upsi has to be created correctly
    -   The `web-ui` is then bound to the `rest-backend` upsi
-   Make sure `web-ui` app was started with correct path to the Ruby code
    -   Oherwise, you will experience `GemFile locked..` error
-   See if `web-ui` has been restarted
    
### Lab extras

-   Try `cf buildpacks`
-   Try `cf stacks` 
-   What is the difference between `offline buildpack` 
    vs `online buildpack`
-   Use the online Java buildpack from the following website
    (Google `Java buildpack`)

    ```
    https://github.com/cloudfoundry/java-buildpack
    ```  

-   Redeploy the application using JDK version

### Where `url` is used in the `web-ui` app

-  Assuming you did `cf bind-service web-ui rest-backend`,
   the `web-ui` should have the following `VCAP_SERVICES`

   ```
   {
 "VCAP_SERVICES": {
  "user-provided": [
   {
    "binding_name": null,
    "credentials": {
     "url": "http://roster_sang_shin.cfapps.io"
    },
    "instance_name": "rest-backend",
    "label": "user-provided",
    "name": "rest-backend",
    "syslog_drain_url": "",
    "tags": [],
    "volume_mounts": []
   },
   ```

-  See `app/controllers/application_controller.rb` file

   This code below is invoked when `People` button is clicked.
   Inside this code `rest_backend_url` method is invoked.
   
   ```
   get '/people' do
     logged_in.check_token(request)
     url = rest_backend_url
     unless url
      halt 500, "Please connect your app to the rest data service\n"
      return
     end
     service = PeopleDataService.new(url)
     @people = service.people
     erb :people
   end
   ```
   
   The `rest_backend_url` method returns the value of
   `user-provided`-->`rest-backend`-->`credentials`-->`url`.

   ```
   def rest_backend_url
     return nil if vcap_services.nil?
     json = JSON.parse(vcap_services)
     service_array = json['user-provided']
     service_array.select do |service|
       service['name'].start_with?('rest-backend')
     end.first['credentials']['url']
   end
   ```

## Microservices: Container SSH

### Error in the lab document

-   Any reference to `static-web-ui` should have been `web-ui-static`

### Supplemental topics

-   [Application continuum slides](http://deck.appcontinuum.io/assets/player/KeynoteDHTMLPlayer.html)
-   [Application continuum website](http://www.appcontinuum.io/)

### Challenge questions (related to presentation)

-   What are the complexities introduced in Microservices?
-   Can "cloud native app" be monolith app?

### Challenge questions (related to lab)

-   What are the environment variables that are automatically set
    by PCF? (Google `Cloud Foundry Environment Variables`)
-   What is the auto-configuration is being performed when
    you push an application?  See [here](https://github.com/cloudfoundry/java-buildpack-auto-reconfiguration) for an anwser.

### Lab extras

-   Do `cf ssh roster` (or any Java app) and see which command 
    is executed
-   Do `cf ssh <app>` and display the values of these environment
    variables using `echo $<environment-variable-name>`
-   Also try `ps -ef` command 

### References

-   [Monthlith First](https://martinfowler.com/bliki/MonolithFirst.html)
-   [Deploying Microservice Architectures with Cloud Foundry](https://www.youtube.com/watch?v=DBIm6gDpSNg&t=520s) video
-   [Building Microservices by Sam Newman](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems-ebook/dp/B00T3N7XB4/ref=sr_1_1?ie=UTF8&qid=1539207745&sr=8-1&keywords=microservices+sam+newman) book
-   [Cloud Native Application by Josh Long](https://www.oreilly.com/library/view/cloud-native-java/9781449374631/) book
-   [Release it by Michael T. Nygard](https://pragprog.com/book/mnee/release-it) book
-   [Domain Driven Design by Eric Evans](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?ie=UTF8&qid=1539295231&sr=8-1&keywords=domain+driven+design+eric+evans) book
 
## Route Services: Deploying a route service for rate limiting

### Error in the document

-   Rename `manifest.yml` to `manifest-old.yml' so that it does not
    get picked up when you deploy the route service application

### Solution Procedure

1.  Create Rate Limit App:

    `cf push {name of your rate limit app} -b java_buildpack -m 768M -p ./rate-limit-route-service.jar`

2.  Create Service instance

    `cf cups rate-limit-service -r https://{route from your pushed rate limit app}`

    You can get the route of your rate limit app via
    `cf app {name of your rate limit app}`.

3.  Bind Route Service to Route

    `cf bind-route-service cfapps.io rate-limit-service --hostname {host name of your web-ui route}`

    You can get the web-ui route via `cf app {web ui app name}`

### Supplemental topcs

Use cases:
-   Denial of service attack
-   Multi-tenant rate quotas

### Error in the lab document

-   The link to the `rate-limit-route-service.jar` is not set in 
    the document.  Download it from the lower-left corner of the 
    course website.

### Challenge questions

-   When do you use "bind-service" and when do you use 
    "bind-route-service" command?
-   What `cf` command do you use to find all the `route 
    services` bound to a route?
    
### Solution to the lab

(to be added)
         
## Docker: Pushing a Docker App

### Supplemental topics

-   PKS vs PAS (PCF) for running Docker apps
-   Health check - default behavior in PCF is checking a port 8080

### Error in the lab document

-   Rename `manifest.yml` to `manifest-old.yml' so that it does not
    get picked up

-   You want to run the `engineerbetter/worker-image` docker app 
    with `--health-check-type process`. 
    
    ```
    cf push my-docker-sang-shin -o engineerbetter/worker-image --health-check-type process
    ```
    
    Otherwise, you will experience the following behavior. (This might
    result in `Waiting the app to
    
    ```
    ...
    [HEALTH/0] ERR Failed to make TCP connection to port 8080: connection refused
    [CELL/0] ERR ... health check never passed.
    ```
    
    
### Lab Extras

-   Try `cf feature-flags` and note that `diego_docker` is enabled
-   Try hello docker image: `tutum/hello-world`
    
## Docker: Service Keys

### Challenge questions

-   Can you create service key from user provided service instance?
            
## TCP Routing

### Lab extras

-   Try [tcp routing lab]
(http://dojoblog.dellemc.com/tcp-routing/tcp-routing-and-ssl/)
-   Try `cf -v org sashin-org` to find out `total_reserved_route_ports` 
    in your PCF installation.  (In PWS, it is set to 0, hence you
    will not be able to finish the lab above.)

     
    
## UAA: Deploying UAA as a CF app

### Error in the lab document

-   Increase the memory to 768M in the `uaa-manifest.yml` file as shown below.
    (Currently it is set to `256M`.) Otherwise, the `cf push` will fail.
    
    ```
    memory: 768M
    ```


## UAA: Deploying a Route service for authentication

### Explanation of the OAuth2 in the context of this Lab

#### Use case and Roles

Proxy (client) wants to access `/secure` (resource) of 
`web-ui` (resource server) 
on behalf of a user (resource owner) using OAuth2.

-   You, as a user, play the role of resource owner
-   Proxy plays the role of `client`
-   The `web-ui` plays the role of resource sever
-   The `UAA` plays the role of `OAuth2` server

#### Service to Service communication

-   Before anything happens, any service (or microservice) that needs to directly
    communicate with UAA has to register with UAA and gets assigned 
    with `client id` (sometimes called `client name`) and 
    `client secret`. 
    -   You can think of Proxy, `web-ui`, and UAA as 
        services (or microservices) running in PCF
    -   Services communicate with each other using 
        `Client credentials` grant type
    -   This communication is secure because of only
        the calling service knows about the `client secret`
-   The Proxy has been registered with the UAA and assigned
    with `client id` and `client secret` and configured with
    -   GUARD_CLIENT_KEY: dashboard
    -   GUARD_CLIENT_SECRET: dashboardsecret
-   The `web-ui` has to be registered with the UAA and assigned
    with its own  `client id` and `client secret`
    -   We need to configure `web-ui` with the `client id` 
        and `client secret`
    -   This is the reason why we are creating `uaa-token` user
        provided service and binding `web-ui` to it
        -   client_name - oauth_showcase_authorization_code
        -   client_secret - secret
    -   We also need configure  `web-ui` with the URL of the UAA 

#### Authorization code flow

 1.  User (resource owner) accesses the proxy (client) for
     the first time
 1.  The proxy `redirects` the request to the `GUARD_LOGIN_URL`
     endpoint of the UAA for authentication
     -   This is the reason
         why the proxy has to be configured with `GUARD_LOGIN_URL`
     -   Note that client `redirects` so that browser displays
         the login screen - there is no direct communucation between
         proxy and the UAA yet
     -   During this redirection to the UAA, the proxy also passes 
         its callback address,
         which is configured with `GUARD_DEFAULT_CALLBACK_URL`
 1.  User logs in with `marissa` and `koala`
 1.  User is then presented with `do you approve ..` pop-up screen
 1.  User approves
 1.  UAA then redirects the request back to the Proxy
     with `authorization code`
     -   UAA knows the redirect target location, `GUARD_DEFAULT_CALLBACK_URL`,
         because it is given to it in the step 2 above
 1.  Proxy then directly communicates with UAA asking for `access token`
     passing the `authorization code`
     -   This is a direct communcation between Proxy and UAA, hence
         Proxy has to send `client id` (`GUARD_CLIENT_KEY`) and 
         `client secret` (`GUARD_CLIENT_SECRET`)
     -   Because `client secret` is secured in the Proxy, this is 
         secure communication - in other words, even if someone has
         the `authorization code`, which is indeed visible to
         the public eye since it is passed as part of redirected
         request, he would not be able to perform
         this action because he does not have the `client secret`
 1.  UAA verifies the `authorization code` and then issues 
     `access token` to the Proxy
 1.  Proxy then access the `web-ui` resource server with the access token
 1.  The `web-ui` then verifies token and then returns the resource

### Summary steps of this lab

1.  Deploy UAA server
2.  Bind `web-ui to UAA
    -   Create `uaa-tokens` user provided service instance(UPSI) capturing UAA route `client id` and `client secret` of the `web-ui`
    -   Bind `web-ui` to the `uaa-tokens` upsi using `cf bind-service` command
3.  Create Route Service proxy
    - Start `uaa-guard-proxy` routing service app
    - Create `authz` UPSI route service
    - Bind-route-service `web-ui` to the `authz`  

    (You might experience an error indicating only a sinle 
      route service can be bound. 
      You will have to 
      `unbind-route-service <rate-limiting-route-service>` 
      if this happens.)

### Lab extras

-   Observe the locally maintained access token after performing
    `cf login -a api.run.pivotal.io` (this is an example of
    `password` grant flow)

    ```
    cd <Home-directory>/.cf
    cat config.json (or type config.json for Windows)
    ```
    
-   Observe that everytime a REST call is made to the `Cloud Controller',
    access token is sent in the `Authorization` field as a bearer token
    
    ```
    cf -v app roster
    ```

### References

-   [OAuth2 Basics](https://www.slideshare.net/SangShin1/spring4-security-oauth2)
-   [Enabling Cloud Native Security](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa) slides 7-37
    
## After the training

### Course website

-   The course website will be available to you even after the training
-   This "student notes" page will be also available even after the training

### Cerfification

-   You can take certification exam as described in the 
    [Cloud Foundry website](https://www.cloudfoundry.org/certification/)

### PCF access

-   The 5G memory given to you during the training will be 
    deleted right after the training
-   You can still do use PWS 2G free account to do the lab
    on your own 
-   Please note that Spring app needs 768M at a minimum, which
    means you should be able to do most labs without a problem
    except blue-green deployment
-   You can also use Java option to reduce the memory requirement
    of your Spring app as following - with this setting, you
    should be able to do even blue-green deployment
    
    ```
    cf push spring-music -p ./build/libs/spring-music-1.0.jar --random-route -m 512M --no-start
    cf set-env spring-music JAVA_OPTS -Xss228K
    cf start spring-music
    ```
    
    or set it in the manifest file
    
    ```
    applications:
    - name: spring-music
      disk_quota: 1G
      env:
        JAVA_OPTS: -Xss228K
      instances: 1
      memory: 512M
      path: ./build/libs/spring-music-1.0.jar
      routes:
      - route: spring-music-unemigrant-nontransportation.cfapps.io
    ```
    
### Contacting instructors

-   Feel free to contact the instructor for any issues/ideas/suggestions
       
    
