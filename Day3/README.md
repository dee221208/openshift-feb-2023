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

## Lab - Deploying application using S2I(Source to Image) strategy
```
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/spring-ms.git --strategy=source
```
In the above command, we have requested OpenShift to use registry.access.redhat.com/ubi8/openjdk-11 as a S2I image to perform the build and deploy it.
