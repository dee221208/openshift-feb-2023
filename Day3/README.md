# Day3

## Deploying an application using github repo

Deploying using docker strategy i.e using Dockerfile
```
oc new-project jegan
oc project jegan
oc new-app https://github.com/tektutor/spring-ms.git --strategy=docker
```

Checking the deployment status
```
oc status
oc logs -f bc/spring-ms 
oc expose svc/spring-ms
```
