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

## Clean up all resources in your project
```
oc delete deploy/spring-ms svc/spring-ms bc/spring-ms is/spring-ms
```


## Lab - Deploying application using S2I(Source to Image) strategy
```
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/spring-ms.git --strategy=source
```
In the above command, we have requested OpenShift to use registry.access.redhat.com/ubi8/openjdk-11 as a S2I image to perform the build and deploy it.

## Lab - Deploying an application using readily available Container Image from Docker Hub
```
oc new-app tektutor/spring-ms:1.0
```

Expected output
<pre>
(jegan@tektutor.org)$ oc new-app tektutor/spring-ms:1.0
--> Found container image 9175b94 (6 months old) from Docker Hub for "tektutor/spring-ms:1.0"

    * An image stream tag will be created as "spring-ms:1.0" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "spring-ms" created
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "spring-ms" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "spring-ms" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "spring-ms" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "spring-ms" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
    deployment.apps "spring-ms" created
    service "spring-ms" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/spring-ms' 
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ oc status
In project jegan on server https://api.ocp.tektutor.org:6443

http://spring-ms-jegan.apps.ocp.tektutor.org to pod port 8080-tcp (svc/spring-ms)
  deployment/spring-ms deploys istag/spring-ms:1.0 
    deployment #2 running for 3 seconds - 1 pod
    deployment #1 deployed 7 seconds ago

pod/example runs tektutor/spring-ms:1.0

1 info identified, use 'oc status --suggest' to see details.
</pre>
