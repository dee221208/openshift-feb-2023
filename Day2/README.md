# Day2

## What is Container Orchestration Platform
- container management platform
- manages containerized applications
- typically has a cluster of machine/nodes
- master/worker nodes
- nodes could be physical server, virtual machine from onPrem/public cloud
- supports
  1. Application deployment
  2. Exposing services
  3. Scale up/down
  4. Rolling update
  5. Application Health/Live Monitoring
  6. High Availability
  7. Self Healing
- Examples
  1. Docker SWARM - Free, Not Production Grade, good for Dev/QA, Prototype, learning
  2. Google Kubernetes - Opensource, no support, Production Grade
  3. Red Hat OpenShift - requires license, get support from Red Hat , Production Grade

## Kubernetes as a Managed Service
- AWS
   - EKS ( Elastic Kubernetes Service )
- Azure
   - AKS ( Azure Kubernetes Service )

## Red Hat OpenShift as a Managed Service
- AWS
  - ROSA ( Red Hat OpenShift as a Service )
  
- AZure
  - Azure Red Hat OpenShift (ARO)

## Kubernetes Overview
- Container Orchestration Platform
- supports many different types of Container Engine/Runtimes that implements CRI ( Container Runtime Interface )
- in other words, it's a mult-container orchestration platform
- Kubernetes Resources
  - doesn't manages containers directly
  - introduced new abstraction called Pod
  - Pod is a group of related containers
  - each Pod represents one single application
  - 
  - the smallest unit that can be managed indepedently in Kubernetes is Pod
  - IP address is assigned on the Pod level and not on the Container level
  - ReplicaSet
    - manages a group of Pods
  - Deployment
    - manages a one or more ReplicaSets
  - DaemonSet
  - Job
  - CronJob
  - Service
    - Internal
      - ClusterIP
    - External
      - NodePort
      - LoadBalancer
  - Kubernetes is primarily a CLI tool
  - it does support a minimal Web Dashboard but it is generally not used in production
  - allows us to extend the Kubernetes API by adding Custom Resources(CR)
  - we can add Custom Resources(CRs) by defining Custom Resource Definitions (CRDs)
  - Kubernetes uses Controller to monitor and manage each type of Resources
  - Deployment Controller is the one that manages the Deployment
  - ReplicaSet Controller is the one that manages the ReplicaSet
  - kubectl is the client tool used in Kubernetes to interact with the cluster

## Kubernetes/OpenShift Control Plane Components
1. API Server
2. etcd database
3. Scheduler
4. Controller Manager(s)

### API Server
- all the Kubernetes Functionalities are implemented as REST APIs
- API Server maintains the Cluster State and the application status in the etcd database
- API Server is the only component that will access to etcd database
- Whenever API Server updates/deletes/creates any database entry within etcd, it trigger an event
- the event type would vary depending on What operation(Create, Delete, Update) and What resource is impacted
- all the Kubernetes/Openshift components are allowed to talk to only API Server
- ie. Scheduler is not allowed to talk to Controller managers or controller managers aren't allowed to communicate with other components
- ie. all communication happens only via API Server

### etcd
- it is third-party opensource key/value datastore used by Kubenetes and OpenShift

### Scheduler
- this component is responsible to identify a health node where each new Pod can be deployed
- Scheduler sends the Scheduling recommendations to the API Server via REST call

### Controller Managers
- it is a combination of many Controllers
- each Controller manages a particular type of Resource
- For example
  - Deployment Controller is the one which monitors and manages the Deployment Resource
  - ReplicaSet Controller is the one which monitors and manages the ReplicaSet Resource
  - DaemonSet Controller manages DaemonSet Resource

### Kube-proxy(DaemonSet)
- this supports the load-balance of similar Pods
- this runs in every nodes ie all master and worker nodes

### Core DNS (DaemonSet)
- this supports Service Discovery
- this runs in every nodes ( master and Worker nodes )

## OpenShift Overview
- is developed on top of Kubernetes
- added many additional features on top of Kuberenetes
- they have added many Custom Resources by definining many Custom Resource Definitions(CRDs)
- to manage the Custom Resource, the OpenShift team also have added many Custom Controllers

## Kubernetes vs OpenShift
- OpenShift is Red Hat's Distribution of Kubernetes
- all the features of Kubernetes works in OpenShift
- OpenShift has many additional features
- Additional Features
  - User Management
  - Web Console and CLI
  - supports Private Container Registry out of the box
  - you get support from Red Hat ( an IBM company )

# OpenShift Command Reference

## Listing OpenShift Cluster nodes
```
oc get nodes
kubectl get nodes
```

Expected output
<pre>
(jegan@tektutor.org)$ oc get nodes
NAME                        STATUS   ROLES                         AGE     VERSION
master-1.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
master-2.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
master-3.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
worker-1.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9
worker-2.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9
(jegan@tektutor.org)$ kubectl get nodes
NAME                        STATUS   ROLES                         AGE     VERSION
master-1.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
master-2.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
master-3.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9
worker-1.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9
worker-2.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9

</pre>
