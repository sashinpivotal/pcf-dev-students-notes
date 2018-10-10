# PCF Developer Training 

The purpose of this document to supplement the official training materials
with the following items.

-   Supplemental topics
-   Challenge questions
-   Lab extras
-   Trouble-shooting tips
-   References

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

-   If you are using GitBash under Windows, try the following in order to
    establish a connection to the PCF
    
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

-   Try PCF autoscaling feature in PWS App Manager


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
-   What is a `marketplace`? What is a plan?
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
   
### Lab extras   

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
-   [Beyond 12 Factor (15 Factor)](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)
-   [Beyond 12 Factor (15 Factor)](https://www.oreilly.com/library/view/beyond-the-twelve-factor/9781492042631/)
    

## Log Drains 



## Manipulating Routes - Blue/Green Deployment

### Challenge questions

-   Can an application have multiple routes?
-   Can a route be applied to multiple applications?
-   Can a route exist without an application associated with it? 
    (See “cf routes” and “cf create-route” commands.)
-   When mapping or unmapping routes, do you have to restart
    or restage an application?
-   How can you delete all routes that are not associated with
    any apps?
-   What is the logical steps of blue-green deployment?
-   Is blue-green deployment suitable for major feature change?
-   Can you describe which PCF components are responsible for
    updating the routing table whenever a new instance is created
    or old instance gets destroyed?
  
## App Execution & Security Groups: Setting up App Monitoring with New Relic

### Challenge questions

-   What are the default ASG's?
    
### Lab extras

-   Try cf commands relevant to security groups    

## Staging and Running: Push the Web-UI
    
### Challenge questions

-   What could be use cases where you will have to do “cf restage” 
    (as opposed to “cf restart”)?
-   Suppose you deployed an application with “cf push roster -m 768M”, 
    what would be memory allocated when you re-deployed the same application 
    with “cf push roster“?    

## Microservices: Container SSH

### Challenge questions (related to presentation)

-   What are the complexities introduced in Microservices?

### Challenge questions (related to lab)

-   What are the environment variables that are automatically set
    by PCF? (Google `Cloud Foundry Environment Variables`)

### Lab extras

-   Do `cf ssh <app>` and display the values of these environment
    variables using `echo $<environment-variable-name>`

### References

-   [Monthlith First](https://martinfowler.com/bliki/MonolithFirst.html)
-   [Application continuum](http://www.appcontinuum.io/)
-   [Deploying Microservice Architectures with Cloud Foundry](https://www.youtube.com/watch?v=DBIm6gDpSNg&t=520s)
 
## Route Services: Deploying a route service for rate limiting

### Challenge questions

-   When do you use "bind-service" and when do you use 
    "bind-route-service" command?
-   How do you find all the route services bound to a route?
         
## Docker: Pusing a Docker App

    
### Trouble-shooting

-   You want to run the docker app with `--health-check-type process`. 
    Otherwise, you will experience `[HEALTH/0] ERR Failed to make TCP connection to port 8080: connection refused`
    
    
### Lab Extras

-   Try `cf feature-flags` and note that `diego_docker` is enabled
-   Try hello docker image: tutum/hello-world
    
## Docker: Service Keys

### Challenge question

-   Can you create service key from user provided service instance?
            
## TCP Routing


### Lab extras

-   Try try tcp [tcp routing lab]
(http://dojoblog.dellemc.com/tcp-routing/tcp-routing-and-ssl/)
-   Try `cf -v org sashin-org` to find out `total_reserved_route_ports` in PWS

     
    
## UAA: Deploying UAA as a CF app

### Trouble-shooting

-   Set the memory to 768M when deploying UAA application

  

## UAA: Deplpying a Route service for authentication

### Challenge questions

-   Which grant types are used in this lab?
    

       
    
