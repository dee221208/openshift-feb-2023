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

## Listing the buildconfigs
```
oc get buildconfigs
oc get buildconfig
oc get bc
```

## Listing the builds
```
oc get builds
oc get build
```

## Starting a build from buildconfig
```
oc start-build bc/spring-ms
oc logs -f bc/spring-ms
```
