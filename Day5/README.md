# Day5

## What is a Monolithic Architecture
- pretty much all legacy applications that you use tody could be following this architecture
- monolithic architecture, we divide modules
- 2-tier or 3-tier 
- use of one single centralized database
- the application would be deployed into a single app server
- Benefits
  - debugging is easier
  - RBMS is used as Centralized Backend
    - we get ACID properties out of the box
    - ACID
      - Atomiticy
      - Consistency
      - Isolation
      - Durability
    - Better Performance as components are implemented within the same binary
    - Transaction Management
      - rollback when one or more steps in a transaction fails
- Drawbacks
  - mostly one single programming language stack is used like Java, C++, etc.,
  - product was developed over a period of 10/20 years
  - migrating from one technology(language stack) to other language stack is not easy also it is expensive
  - a single architecture is followed
  - when small changes are done in a module, entire application must be tested
  - new engineer joining the team will have to undergo a steep learning curve

## What is Microservice ?
- a self-contained independent feature can be implemented as a Microservice
- typically microservices support REST API for other microservices to communicate
Benefits of following Microservice Architecture
- Microservices(REST API) are langauge agnostic(independent)
- Microservice implemented in Java can easily communicate with another Microservice implemented in C++/Python/.Net, etc.,
- each Microservice can be implemented in different programming langauage
- each Microservice can follow different Architecture if required
- easy to understand the entire functionality of a single Microservice, hence even a person who is relative new to project team, can also learn quickly and start contributing productively to the project
- microservices can be scaled up/down independent of other Microservices
- microservices redesign can be done in less time without much impact on other Microservices
- we could even rewrite a microservice in another suitable programming language without much effort and risk

Drawbacks
- No centralized database
- No ACID properties guaranteed by NoSql databases
- debugging a functionality is very difficult
- checking application logs are difficult, we need ELK/EFK, Splunk kind of tools to check the logs from a centralized location
- if a functionality involves calling many microservices, too many hops might bring down the overall performance

## Launch Jenkins as a docker container
If your login is user01, then user the folder /tmp/jenkins01. If your login is user17 then use the folder /tmp/jenkins17.  

The port 8080 on the left, you will have to change it to 8001 if user is user01, if it is user15 then change the 8080 port on the left side to 8015 to avoid port conflicts.  Same way the left side 50000 port to 50001 if your user is user01, change it 50017 if your user is user17.

Append your name with the jenkins container name like jenkins-jegan

```
mkdir -p /tmp/jenkins
chmod -R /tmp/jenkins

docker run --name jenkins --hostname jenkins -p 8080:8080 -p 50000:50000 --restart=on-failure -v /tmp/jenkins:/var/jenkins_home jenkins/jenkins:lts-jdk11
```

Expected output
<pre>
(jegan@tektutor.org)$ docker run --name jenkins --hostname jenkins -p 8080:8080 -p 50000:50000 --restart=on-failure -v /tmp/jenkins:/var/jenkins_home jenkins/jenkins:lts-jdk11
Running from: /usr/share/jenkins/jenkins.war
webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
2023-02-17 08:26:37.244+0000 [id=1]	INFO	winstone.Logger#logInternal: Beginning extraction from war file
2023-02-17 08:26:37.316+0000 [id=1]	WARNING	o.e.j.s.handler.ContextHandler#setContextPath: Empty contextPath
2023-02-17 08:26:37.381+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: jetty-10.0.12; built: 2022-09-14T01:54:40.076Z; git: 408d0139887e27a57b54ed52e2d92a36731a7e88; jvm 11.0.18+10
2023-02-17 08:26:37.678+0000 [id=1]	INFO	o.e.j.w.StandardDescriptorProcessor#visitServlet: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
2023-02-17 08:26:37.745+0000 [id=1]	INFO	o.e.j.s.s.DefaultSessionIdManager#doStart: Session workerName=node0
2023-02-17 08:26:38.294+0000 [id=1]	INFO	hudson.WebAppMain#contextInitialized: Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
2023-02-17 08:26:38.483+0000 [id=1]	INFO	o.e.j.s.handler.ContextHandler#doStart: Started w.@5eccd3b9{Jenkins v2.375.3,/,file:///var/jenkins_home/war/,AVAILABLE}{/var/jenkins_home/war}
2023-02-17 08:26:38.508+0000 [id=1]	INFO	o.e.j.server.AbstractConnector#doStart: Started ServerConnector@192c3f1e{HTTP/1.1, (http/1.1)}{0.0.0.0:8080}
2023-02-17 08:26:38.524+0000 [id=1]	INFO	org.eclipse.jetty.server.Server#doStart: Started Server@157853da{STARTING}[10.0.12,sto=0] @1863ms
2023-02-17 08:26:38.528+0000 [id=44]	INFO	winstone.Logger#logInternal: Winstone Servlet Engine running: controlPort=disabled
2023-02-17 08:26:38.831+0000 [id=51]	INFO	jenkins.InitReactorRunner$1#onAttained: Started initialization
2023-02-17 08:26:38.912+0000 [id=71]	INFO	jenkins.InitReactorRunner$1#onAttained: Listed all plugins
2023-02-17 08:26:39.795+0000 [id=78]	INFO	jenkins.InitReactorRunner$1#onAttained: Prepared all plugins
2023-02-17 08:26:39.808+0000 [id=88]	INFO	jenkins.InitReactorRunner$1#onAttained: Started all plugins
2023-02-17 08:26:39.823+0000 [id=96]	INFO	jenkins.InitReactorRunner$1#onAttained: Augmented all extensions
2023-02-17 08:26:40.037+0000 [id=115]	INFO	jenkins.InitReactorRunner$1#onAttained: System config loaded
2023-02-17 08:26:40.039+0000 [id=120]	INFO	jenkins.InitReactorRunner$1#onAttained: System config adapted
2023-02-17 08:26:40.041+0000 [id=125]	INFO	jenkins.InitReactorRunner$1#onAttained: Loaded all jobs
2023-02-17 08:26:40.046+0000 [id=132]	INFO	jenkins.InitReactorRunner$1#onAttained: Configuration for all jobs updated
2023-02-17 08:26:40.095+0000 [id=151]	INFO	hudson.util.Retrier#start: Attempt #1 to do the action check updates server
2023-02-17 08:26:40.210+0000 [id=133]	INFO	jenkins.install.SetupWizard#init: 

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

6b02cd175cf04611b7b6feb4fe008ab8

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************

WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.codehaus.groovy.vmplugin.v7.Java7$1 (file:/var/jenkins_home/war/WEB-INF/lib/groovy-all-2.4.21.jar) to constructor java.lang.invoke.MethodHandles$Lookup(java.lang.Class,int)
WARNING: Please consider reporting this to the maintainers of org.codehaus.groovy.vmplugin.v7.Java7$1
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
2023-02-17 08:26:58.362+0000 [id=159]	INFO	jenkins.InitReactorRunner$1#onAttained: Completed initialization
2023-02-17 08:26:58.447+0000 [id=35]	INFO	hudson.lifecycle.Lifecycle#onReady: Jenkins is fully up and running
2023-02-17 08:27:28.244+0000 [id=151]	INFO	h.m.DownloadService$Downloadable#load: Obtained the updated data file for hudson.tasks.Maven.MavenInstaller
2023-02-17 08:27:28.245+0000 [id=151]	INFO	hudson.util.Retrier#start: Performed the action check updates server successfully at the attempt #1
</pre>

Once you see your Jenkins 
