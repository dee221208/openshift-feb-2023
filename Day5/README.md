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
