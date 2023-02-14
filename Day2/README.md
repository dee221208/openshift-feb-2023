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
## OpenShift Overview
- is developed on top of Kubernetes
- added many additional features on top of Kuberenetes


## Kubernetes vs OpenShift
