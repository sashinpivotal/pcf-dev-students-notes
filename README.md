# pcf-dev-students-notes

-   Supplemental lab notes
-   Challenge questions
-   Lab extras
-   Trouble-shooting tips
-   References

## Basic Definitions: Getting Started w/ the CF CLI

### References

-   IaaS
-   CaaS -> Docker, Kubernetes, PKS
-   PaaS -> Cloud Foundry, PAS (pre-2.0 Elastic Runtime), what
    we are teaching in this course
-   FaaS
-   BOSH - The secret sauce of Cloud Foundry
-   App Manager, demo PWS
-   Op Manager - value add of PCF (Operator)


## Tech Overview: Pushing Apps

### Supplemental lab notes

-   Consider adding a unique non-random route for easier postman automation.
-   There may be slight differences in the push output varying by the cli version:
    - url -> routes
    - memory usage -> usage

### Challenge questions

-   How many containers are needed for pushing an app?
-   Why do you see “container gets created and destroyed” as part of pushing app?
-   What is the “org”/“space” structure in your PCF installation?
-   Why do you have to use “--random-route”? 
-   What is the “cf push” option that lets you specify the hostname 
    part of a route? (“cf push -h”)

## Core Themes: Scaling Apps

### Lab Extras

-   Try PCF autoscaling feature in PWS App Manager

## Logging Metrics: Application Logs and & Events

### Challenge questions

-   What are the examples of PCF log types? (Google “PCF log types”)


## Resiliency: Application Resiliency

### Trouble-shooting

-  If you see the following message when you send the `kill` request,
   it is an expected behavior. 

   ```
   502 Bad Gateway: Registered endpoint failed to handle the request
   ```

## Services: Creating & Bind Services
    
### Lab Extras

-   Install "mysql" cf plugin and use it to access the database (you
    will have to install `mysql` client first)


## 12 Factor Applications: Environment Variables & App Manifest 
   
### Lab extras   

-   Use `cf create-app-manifest` and use the newly creted manifest file 
    to push an application    
-   Use `health-check-type` and `health-check-http-endpoint`   
    
### References

-   [12 factor presentation](https://content.pivotal.io/slides/the-12-factors-for-building-cloud-native-software)
-   [Beyond 12 Factor (15 Factor)](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)
-   [Beyond 12 Factor (15 factor)](https://www.oreilly.com/library/view/beyond-the-twelve-factor/9781492042631/)
    

## Log Drain 


## Manipulating Routes - Blue/Green Deployment

### Challenge questions

-   Can an application have multiple routes?
-   Can a route be applied to multiple applications?
-   Can a route exist without an application associated with it? 
    (See “cf routes” and “cf create-route” commands.)
-   When mapping or unmapping routes, do you have to restart
    or restage an application?
  
## App Execution & Security Groups: Setting up App Monitoring with New Relic
    

## Staging and Running: Push the Web-UI
    
### Challenge questions

-   What could be use cases where you will have to do “cf restage” 
    (as opposed to “cf restart”)?
-   Suppose you deployed an application with “cf push roster -m 768M”, 
    what would be memory allocated when you re-deployed the same application 
    with “cf push roster“?    

## Microservices: Container SSH

### References

-   [Monthlith First](https://martinfowler.com/bliki/MonolithFirst.html)
 
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
    

       
    
