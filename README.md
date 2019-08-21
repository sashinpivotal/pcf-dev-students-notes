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

## Basic Definitions - Getting Started w/ the CF CLI

### Lab Extras

-   Install and use the "Targets" plugin from [Cloud Foundry community](https://plugins.cloudfoundry.org/) using the following command

    ```bash
    cf install-plugin -r CF-Community "Targets"
    ```

    You can then use it as described below. More info [here](https://github.com/guidowb/cf-targets-plugin).

    | command | usage | description|
    | :--------------- |:---------------| :------------|
    |`targets`| `cf targets` |list all saved targets|
    |`save-target`|`cf save-target [-f] [<name>]`|save the current target for later use|
    |`set-target`|`cf set-target [-f] <name>`|restore a previously saved target|
    |`delete-target`|`cf delete-target <name>`|delete a previously saved target|


### Trouble-shooting

-   If you are using GitBash under Windows, you might
    experience a problem when you do
    `cf login -a api.run.pivotal.io`.
    Try the following if that happens.

    `cf auth <username> <password>`

## Lab extras

- Try [PCF AppManager](https://login.run.pivotal.io)

## Tech Overview - Pushing Apps

### Trouble-shooting

-   On Windows, the `curl` command with `POST` might result json
    conversion error

    ```bash
    curl -H "Content-Type: application/json" -X POST -d '{"firstName":"<some-key>","lastName":"<some-value>"}' http://<YOUR-APP-URL>/people
    ```
    When it happens, you can capture the data part below into a file, i.e.
    `person.json`

    ```bash
    {"firstName":"<some-key>","lastName":"<some-value>"}
    ```

    Then you can use `-d @<file-name>`

    ```bash
    curl -H "Content-Type: application/json" -X POST -d @person.json http://<YOUR-APP-URL>/people
    ```

    Alternatively escape the values in the command line (CMD)

    ```bash
    curl -H "Content-Type: application/json" -X POST -d "{\"firstName\":\"<some-key>\",\"lastName\":\"<some-value>\"}" http://<YOUR-APP-URL>/people
    ```

### Challenge questions

-   What is the PCF component that handles the requests coming
    from "cf cli"?
-   What is the PCF component that handles routing?
-   How many containers are needed for pushing an app?
-   Why do you see “container gets created and destroyed” as part of pushing app?
-   What is the “org”/“space” structure in your PCF installation?
-   Why do you have to use “--random-route”?
-   What is the `cf push` option that lets you specify the hostname
    part of a route?
    (`cf push -h`)
-   What is the difference between PAS and PKS?
-   What is a commonality between PAS and PKS?

### References

- [Containerization - P to V to C](https://www.youtube.com/watch?v=Z2oghhwoEO0)
- [PCF Architectural diagram](http://htmlpreview.github.io/?https://raw.githubusercontent.com/cloudfoundry-incubator/diego-design-notes/master/clickable-diego-overview/clickable-diego-overview.html)

## Core Themes - Scaling Apps

### Lab Extras

-   Install and use "open" plugin from [Cloud Foundry community](https://plugins.cloudfoundry.org/) using the following command

    ```bash
    cf install-plugin -r CF-Community "open"
    ```

    You can then use it as shown below, which will automatically open
    a browser with the correct URL of the `roster` app.

    ```bash
    cf open roster
    ```

-   Try PCF autoscaling feature in PWS App Manager
-   Try `app-autoscaler` service (do `cf marketplace`)
-   Review [App autoscaler cli plugin](https://docs.pivotal.io/pivotalcf/2-4/appsman-services/autoscaler/using-autoscaler-cli.html)

### Lab Extras

-   Review the `roster` app health status in App Manager.

-   See here [Spring Boot App Manager integration](https://docs.pivotal.io/pivotalcf/2-4/console/using-actuators.html)

## Logging Metrics - Application Logs and & Events

### Lab Extras

-   Visualizes logs and metrics via an app nozzle.

    Install and use the "Firehose Plugin" from [Cloud Foundry community](https://plugins.cloudfoundry.org/) using the following command

    ```bash
    cf install-plugin -r CF-Community "Firehose Plugin"
    ```

    Because you do not have enough rights to call 

    ```bash
    cf nozzle
    ```
    
    and see all the metrics and logs, you may use an app-nozzle to see app related information with an interactive prompt.

    ```bash
    cf app-nozzle roster
    ```

### Challenge questions

-   Which component of PCF architecture is responsible for
    collecting logs?
-   What are the other log types other than STG, APP, CELL, RTR?
    (Google `PCF log types`)
-   What is the responsibility of Doppler?
-   What is the responsibility of a developer to
    take advantage of PCF logging system?


### References

- [Why Loggregator may lose logs](https://community.pivotal.io/s/article/Why-Loggregator-may-Lose-Logs)

## Resiliency - Application Resiliency

### Trouble-shooting

-   If you see the following message when you send the `kill` request,
    it is expected behavior.

    ```bash
    502 Bad Gateway: Registered endpoint failed to handle the request
    ```

    The `kill` operation invokes a `System.exit()` command during the
    request, the request never sends a response to the go-router,
    and the go-router throws back the 502 to the client.

### Challenge questions

-   Which component of the PCF architecture maintains the `desired
    state` and `actual state` of the application?
-   What are the "4 levels of resiliency or recoverability" PCF provides?
-   When Diego system is asked to increase the number of an application
    instances by 3, does it create all three instances in a single cell?
-   What is the value proposition of PCF from the standpoint of
    2nd-day operation?
-   What are the health types supported by Cloud Foundry?
    When would you use each?

## Lab Extras

-   Configure the roster application with health check type of `HTTP` to
    use the roster application actuator `/health` endpoint.

## References

- [Cloud Foundry health checks](https://docs.cloudfoundry.org/devguide/deploy-apps/healthchecks.html)

## Services - Creating & Bind Services

### Chllenge questions

-   What is a service?
    What is a service instance?
-   What is the role of the service broker?
    What REST operations does it support?
-   What is a `marketplace`?
    What is a `plan`?
-   What is binding?
    What is the end result of a binding?
-   What `cf` command can you use to tell what services are bound to an
    application?
-   What `cf` command can you use to tell what applications are bound a
    service?
-   Once you bind an application to a service instance,
    do you do "restart" or "restaging"?
-   What is the difference between "brokered service"
    (the services you find under `cf marketplace`).
    and "user-provided service"?
-   What are the three different types of user-provided service?
    (Try `cf cups -h`)

### Lab Extras

-   Install "mysql" cf plugin and use it to access the database (you
    will have to install `mysql` client first)
-   Investigate Autoscaler plugin behavior - it is
    a backing service.


### Trouble-shooting

-   On Windows, the `curl` command with `POST` might result in JSON
    conversion error

    ```bash
    curl -H "Content-Type: application/json" -X POST -d '{"firstName":"<some-key>","lastName":"<some-value>"}' http://<YOUR-APP-URL>/people
    ```

    When it happens, you can capture the data part below into a file, i.e.
    `person.json`

    ```bash
    {"firstName":"<some-key>","lastName":"<some-value>"}
    ```

    Then you can use `-d @<file-name>`.

    ```bash
    curl -H "Content-Type: application/json" -X POST -d @person.json http://<YOUR-APP-URL>/people
    ```

### References

- [Cloud Foundry Service Broker API](https://github.com/openservicebrokerapi/servicebroker/blob/v2.12/spec.md)

## 12 Factor Applications - Environment Variables & App Manifest

### Challenge questions

-   What is the value proposition of setting environment variables
    in the manifest file regarding providing application
    configuration data?
    Is it better than setting configuration data in the
    `application.properties` file?
-   When you have many environment variables to set or complex
    configuration to do, what would be a better option than
    using environment variables?
    (Answer:
    Think of the usage of another backing service called config server -
    under PWS, it is called `p-config-server`.)

### Lab extras 1

-   Try to override the memory size by setting the `-m 768M`
    while using manifest.
-   Use `cf create-app-manifest` and use the newly created manifest file
    to push an application
-   Use `health-check-type` and `health-check-http-endpoint`

### Lab extras 2

-   Create and run Spring Boot application in both locally and PCF
    -   It is a RESTful application
    -   When accessed with "http://localhost:8080/hello",
        it displays greeting message
    -   The greeting message is maintained in the `application.yml` file
        when running locally.
    -   Deploy the same app in PCF
    -   For PCF, the greeting message must be set via environment
        variable
    -   Redeploy the application using a manifest file

### References

- [12 factor presentation](https://content.pivotal.io/slides/the-12-factors-for-building-cloud-native-software)
- [Beyond 12 Factor (15 Factor)](https://content.pivotal.io/blog/beyond-the-twelve-factor-app) or [Beyond 12 Factor (15 Factor)](https://www.oreilly.com/library/view/beyond-the-twelve-factor/9781492042631/)
- [Cloud Native Maturity Model](https://blogs.sap.com/2017/03/28/developing-cloud-native-apps-on-sap-cloud-platform/)
- [Spring External Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)
- [YAML tutorial site](https://gettaurus.org/docs/YAMLTutorial/)
- [Cloud Foundry Environment Variables](https://docs.pivotal.io/pivotalcf/2-4/devguide/deploy-apps/environment-variable.html)

## Log Drains

### Lab extras

- In the PaperTrail dashboard, try to filter only logs from `CELL`.
- Try different syslog drainers such as ELK/Kibana or Splunk.

### Trouble-shooting

-   It might take a few minutes before you see the logs in the
    Papertrail.
-   If you do not see the logs in the Papertrail even after a few
    minutes, make sure you create service using
    `syslog` (as shown below) or `syslog-tls`.

    ```bash
    cf cups my-drain-service -l syslog://<papertrail-url>
    ```

## Manipulating Routes - Blue/Green Deployment

### Supplemental topics

- Semantic versioning - Major.Minor.Patch
- Feature toggles

### Steps for blue-green deployment

1. V1 - R1 is currently running
1. Create V2 with R2 and make sure it is working
1. Add R1 to V2 - now V2 handles both R1 and R2
1. Remove R1 from V1 - now V2 handles 100% of R1
1. Remove R2 from V2 - now V2 handles only R1
1. Rename V2 to V1

### Lab extras

- Define a route mapping with a context path `/xyz`.
- Why does that not work e.g. returning a 404 page? How could you fix it? What are the advantages when you deploy multiple apps and make them available via context path? Hint: [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

### Challenge questions

-   How is a route constructed?
-   How do you verify the existence of a route?
    (See `cf check-route`)
-   Can an application have multiple routes?
-   Can a route be applied to multiple applications?
-   Can a route exist without an application associated with it?
    (See “cf routes” and “cf create-route” commands.)
-   Why might you use an internal route?
-   What are prerequisites to using an internal route?
-   What could be the use case of "cf create-route"?
-   When mapping or unmapping routes, do you have to restart
    or restage an application?
-   How can you delete all routes that are not associated with
    any apps?
-   What are the logical steps of blue-green deployment?
-   How can we control the ratio of the traffic between V1.0.1 (blue)
    versus V1.0.2 (green)?
-   Is blue-green deployment suitable for major feature change?
-   Can you describe which PCF components are responsible for
    updating the routing table whenever a new instance is created
    or old instance gets destroyed?

### References

- [Cloud Foundry Routes](https://docs.cloudfoundry.org/devguide/deploy-apps/routes-domains.html)
- [CanaryRelease](https://martinfowler.com/bliki/CanaryRelease.html)
- [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html)

## App Execution & Security Groups - Setting up App Monitoring with New Relic

### Supplemental topics

- Egress traffic
- Telemetry
- PCF Metrics vs. APM tool

### Challenge questions

- What are the default ASG's?

### Lab extras

- Try `cf` commands relevant to security groups
- Try PCF Metrics from the App Manager

## Staging and Running - Push the Web-UI

### Challenge questions

-   What could be use cases where you will have to do “cf restage”
    (as opposed to “cf restart”)?
-   Suppose you deployed an application with “cf push roster -m 768M”,
    what would be memory allocated when you re-deployed the same application with “cf push roster“?
-   When you delete an application using `cf delete <app-name>`, does the associated route get deleted as well?

### How to debug when things do not work in this lab

-   See if the `web-ui` app has `url` (`cf env web-ui`) environment var
    - The `rest-backend` upsi has to be created correctly
    - The `web-ui` is then bound to the `rest-backend` upsi
-   Make sure `web-ui` app was started with the correct path to the Ruby code
    - Otherwise, you will experience `GemFile locked..` error
-   See if `web-ui` has been restarted

### Lab extras

-   Try `cf buildpacks`.

-   Try `cf stacks`.

-   What is the difference between `offline buildpack`
    vs. `online buildpack`.

-   Use the online Java buildpack from the following website
    (Google `Java buildpack`).

    ```bash
    https://github.com/cloudfoundry/java-buildpack
    ```

-   Redeploy the application using JDK version

-   Rename the `rest-backend` service to something else,
    and watch how `web-ui` breaks.
    This will give you a better picture of how service bindings are
    consumed in the bound application through `VCAP_SERVICES`.

## Microservices - Container SSH

### Error in the lab document

- Any reference to `static-web-ui` is for `web-ui-static`

### Supplemental topics

- [Application continuum slides](http://deck.appcontinuum.io/assets/player/KeynoteDHTMLPlayer.html)
- [Application continuum website](http://www.appcontinuum.io/)
- [Independent deployability](https://blog.cleancoder.com/uncle-bob/2014/09/19/MicroServicesAndJars.html)

### Challenge questions (related to presentation)

- What are the complexities introduced in Microservices?
- Can "cloud-native app" be monolith app?

### Challenge questions (related to the lab)

-   What are the environment variables that are automatically set
    by PCF? (Google `Cloud Foundry Environment Variables`)
-   What is the auto-configuration which is being performed when
    you push an application?  See [here](https://github.com/cloudfoundry/java-buildpack-auto-reconfiguration) for an answer.

### Lab extras

-   Do `cf ssh roster` (or any Java app) and see which command
    is executed.
-   Do `cf ssh <app>` and display the values of these environments
    variables using `echo $<environment-variable-name>`.
-   Also, try the `ps -ef` command.
-   Hide the roster application from a public route,
    but keep it visible to the web-ui application.
    Hint:
    Use an internal route.

### References

- [Strangle Monolith](https://www.martinfowler.com/bliki/StranglerApplication.html)
- [Monolith first](https://www.martinfowler.com/bliki/MonolithFirst.html)
- [Microservices for greenfield](https://samnewman.io/blog/2015/04/07/microservices-for-greenfield/)
- [Do not start with a monolith](https://martinfowler.com/articles/dont-start-monolith.html)
- [Deploying Microservice Architectures with Cloud Foundry](https://www.youtube.com/watch?v=DBIm6gDpSNg&t=520s) video
- [Building Microservices by Sam Newman](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems-ebook/dp/B00T3N7XB4/ref=sr_1_1?ie=UTF8&qid=1539207745&sr=8-1&keywords=microservices+sam+newman) book
- [Cloud Native Application by Josh Long](https://www.oreilly.com/library/view/cloud-native-java/9781449374631/) book
- [Release it by Michael T. Nygard](https://pragprog.com/book/mnee/release-it) book
- [Domain Driven Design by Eric Evans](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?ie=UTF8&qid=1539295231&sr=8-1&keywords=domain+driven+design+eric+evans) book

## Route Services - Deploying a route service for rate limiting

### Error in the document

-   Rename `manifest.yml` to `manifest-old.yml' so that it does not
    get picked up when you deploy the route service application

### Solution Procedure

1.  Create Rate Limit App:

    `cf push {name of your rate limit app} -b java_buildpack -m 768M -p ./rate-limit-route-service.jar`

2.  Create a service instance

    `cf cups rate-limit-service -r https://{route from your pushed rate limit app}`.

    You can get the route of your rate limit app via
    `cf app {name of your rate limit app}`.

3.  Bind Route Service to Route

    `cf bind-route-service cfapps.io rate-limit-service --hostname {host name of your web-ui route}`.

    You can get the web-ui route via `cf app {web ui app name}`.

### Supplemental topics

Use cases:

- Denial of service attack.
- Multi-tenant rate quotas.

### Error in the lab document

-   The link to the `rate-limit-route-service.jar` is not set in
    the document.
    Download it from the lower-left corner of the
    course website.

### Challenge questions

-   When do you use "bind-service" and when do you use
    "bind-route-service" command?
-   What `cf` command do you use to find all the `route
    services` bound to a route?

### References

- [Route Services](https://docs.cloudfoundry.org/services/route-services.html)

## Docker - Pushing a Docker App

### Supplemental topics

- PKS vs. PAS (PCF) for running Docker apps
- Health check - default behavior in PCF is checking a port 8080

### Error in the lab document

-   Rename `manifest.yml` to `manifest-old.yml` so that it does not
    get picked up

-   You want to run the `engineerbetter/worker-image` docker app
    with `--health-check-type process`.

    ```bash
    cf push my-docker-sang-shin -o engineerbetter/worker-image --health-check-type process
    ```

    Otherwise, you will experience the following behavior. (This might
    result in `Waiting for the app to

    ```bash
    ...
    [HEALTH/0] ERR Failed to make TCP connection to port 8080: connection refused
    [CELL/0] ERR ... health check never passed.
    ```

### Lab Extras

-   Try `cf feature-flags` and note that `diego_docker` is enabled
-   Try the hello docker image `tutum/hello-world` (if `diego_docker`
    is enabled).

## Docker - Service Keys

### Challenge questions

- Can you create a service key from user-provided service instance?

## TCP Routing

### Lab extras

-   Try [tcp routing lab](http://dojoblog.dellemc.com/tcp-routing/tcp-routing-and-ssl/)
-   Try `cf -v org sashin-org` to find out `total_reserved_route_ports`
    in your PCF installation.
    (In PWS, it is set to 0; hence you will not be able to finish the
    lab above.)

## UAA - Deploying UAA as a CF app

### Error in the lab document

-   Increase the memory to 768M in the `uaa-manifest.yml` file as shown
    below.
    (Currently, it is set to `256M`.) Otherwise, the `cf push` will fail.

    ```yaml
    memory: 768M
    ```

## UAA - Deploying a Route service for authentication

### Explanation of the OAuth2 in the context of this Lab

#### Use case and Roles

Proxy (client) wants to access `/secure` (resource) of
`web-ui` (resource server)
on behalf of a user (resource owner) using OAuth2.

- You, as a user, play the role of the resource owner.
- Proxy plays the role of `client`.
- The `web-ui` plays the role of resource sever.
- The `UAA` plays the role of `OAuth2` server.

#### Service to Service communication

-   Before anything happens, any service (or microservice) that needs to
    directly communicate with UAA has to register with UAA and gets
    assigned with `client id` (sometimes called `client name`) and
    `client secret`.
    -   You can think of Proxy, `web-ui`, and UAA as
        services (or microservices) running in PCF
    -   Services communicate with each other using
        `Client credentials` grant type
    -   This communication is secure because of only
        the calling service knows about the `client secret`
-   The Proxy has been registered with the UAA and assigned
    with `client id` and `client secret` and configured with
    - GUARD_CLIENT_KEY: dashboard
    - GUARD_CLIENT_SECRET: dashboardsecret
-   The `web-ui` has to be registered with the UAA and assigned
    with its own  `client id` and `client secret`
    -   We need to configure `web-ui` with the `client id`
        and `client secret`
    -   This is the reason why we are creating `uaa-token` user
        provided service and binding `web-ui` to it
        - client_name - oauth_showcase_authorization_code
        - client_secret - secret
    -   We also need configure  `web-ui` with the URL of the UAA

#### Authorization code flow

 1.  User (resource owner) accesses the proxy (client) for
     the first time
 1.  The proxy `redirects` the request to the `GUARD_LOGIN_URL`
     the endpoint of the UAA for authentication
     -   This is the reason
         why the proxy has to be configured with `GUARD_LOGIN_URL`
     -   Note that client `redirects` so that browser displays
         the login screen - there is no direct communication between
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
         because it is given to it in step 2 above
 1.  The proxy then directly communicates with UAA asking for `access token`
     passing the `authorization code`
     -   This is direct communication between Proxy and UAA, hence
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
 1.  The proxy then access the `web-ui` resource server with the access token
 1.  The `web-ui` then verifies token and then returns the resource

### Summary steps of this lab

1.  Deploy UAA server
2.  Bind `web-ui to UAA
    - Create `uaa-tokens` user provided service instance(UPSI) capturing UAA route `client id` and `client secret` of the `web-ui`
    - Bind `web-ui` to the `uaa-tokens` upsi using `cf bind-service` command
3.  Create Route Service proxy
    - Start `uaa-guard-proxy` routing service app
    - Create `authz` UPSI route service
    - Bind-route-service `web-ui` to the `authz`

    (You might experience an error indicating only a single
      route service can be bound.
      You will have to
      `unbind-route-service <rate-limiting-route-service>`
      if this happens.)

### Lab extras

-   Observe the locally maintained access token after performing
    `cf login -a api.run.pivotal.io` (this is an example of
    `password` grant flow)

    ```bash
    cd <Home-directory>/.cf
    cat config.json (or type config.json for Windows)
    ```

-   Observe that every time a REST call is made to the `Cloud Controller`,
    an access token is sent in the `Authorization` field as a bearer token

    ```bash
    cf -v app roster
    ```

### References

- [OAuth2 Basics](https://www.slideshare.net/SangShin1/spring4-security-oauth2)
- [Enabling Cloud Native Security](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa) slides 7-37

## Cascading Failure - Preventing Cascading Failure

### Challenge questions

-   The lab demonstrates what happens when the roster app is "down".
    What are the various way the roster,
    a Java application,
    can fail?

-   What would happen to the web-ui application if the roster app slows
    down?
    Would the existing solution be acceptable to an end-user?

-   How might you protect the web-ui application against slow-down of
    the roster application?
    See
    [Circuit Breaker pattern](https://microservices.io/patterns/reliability/circuit-breaker.html)
    for more information.

-   What happens if there are "transient failures" accessing the roster
    application?
    Maybe there is a short duration of time when scaling down the roster
    application on Cloud Foundry.
    Is the existing solution optimal for the end-user?

-   How might you improve the user experience during transient failures?
    See [Retry Pattern](https://docs.microsoft.com/en-us/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling)
    for more information.

## After the training

### Course website

- The course website will be available to you even after the training
- This "student notes" page will also be available even after the training

### Cerfification

-   You can take the certification exam as described in the
    [Cloud Foundry website](https://www.cloudfoundry.org/certification/)

### PCF access

-   For classes where the instructor sets up access to PCF,
    you will have sufficient resources to run the labs.

-   For classes where you use a PWS free trial account,
    you get 2G limited memory in your org.
    You may encounter insufficient memory to run some of the labs given
    the 2G quota.

-   For the current Java buildpack memory calculator, for Spring app
    on Java 8+, need 768M container memory size at a minimum.

-   You can "tune down" requirements of you Java applications to
    reduce container memory footprints by one of two ways:
    -   Set `JAVA_OPTS` explicitly for your application.
    -   Set `JBP_CONFIG_OPEN_JDK_JRE` parameters for Java buildpack
        memory calculator.

-   You can use `JAVA_OPTS` method as follows:

    ```bash
    cf push roster -p ./rest-data-service.jar --random-route -m 512M --no-start
    cf set-env roster JAVA_OPTS -Xss228K
    cf start roster
    ```

    or set it in the manifest file

    ```yaml
    applications:
    - name: roster
      disk_quota: 1G
      env:
        JAVA_OPTS: -Xss228K
      instances: 1
      memory: 512M
      path: ./rest-data-service.jar
      ...
    ```

-   You can use `JBP_CONFIG_OPEN_JDK_JRE` method as follows:

    ```bash
    cf push roster -p ./rest-data-service.jar --random-route -m 512M --no-start
    cf set-env roster JBP_CONFIG_OPEN_JDK_JRE '{ memory_calculator: { stack_threads: 100 } }'
    cf restage roster
    ```

    or set it in the manifest file

    ```yaml
    applications:
    - name: roster
      disk_quota: 1G
      env:
        JBP_CONFIG_OPEN_JDK_JRE: '{ memory_calculator: { stack_threads: 100 } }'
      instances: 1
      memory: 512M
      path: ./rest-data-service.jar
      ...
    ```

-   Why do you need to restage when using `JBP_CONFIG_OPEN_JDK_JRE`
    option?

-   What is an advantage of using `JAVA_OPTS` over
    `JBP_CONFIG_OPEN_JDK_JRE`?

-   What is an advantage of using `JBP_CONFIG_OPEN_JDK_JRE` over
    `JAVA_OPTS`?

-   See the
    [Java build pack documentation](https://github.com/cloudfoundry/java-buildpack/blob/master/README.md)
    for more information.

### Contacting instructors

- Feel free to contact the instructor for any issues/ideas/suggestions
