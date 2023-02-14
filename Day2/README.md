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

## Finding the openshift version
```
oc version
kubectl version
```

Expected output
<pre>
(jegan@tektutor.org)$ oc version
Client Version: 4.12.2
Kustomize Version: v4.5.7
Server Version: 4.12.2
Kubernetes Version: v1.25.4+a34b9e9

(jegan@tektutor.org)$ kubectl version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.2", GitCommit:"b05f7d40f9a2dac30771be620e9e9148d26ffd07", GitTreeState:"clean", BuildDate:"2023-01-31T16:04:34Z", GoVersion:"go1.19.4", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.4+a34b9e9", GitCommit:"b6d1f054747e9886f61dd85316deac3415e2726f", GitTreeState:"clean", BuildDate:"2023-01-10T15:55:28Z", GoVersion:"go1.19.4", Compiler:"gc", Platform:"linux/amd64"}
</pre>

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

## Listing the nodes in wider format
```
oc get nodes -o wide
kubectl get nodes -o wide
```
Expected output
<pre>
(jegan@tektutor.org)$ oc get nodes -o wide
NAME                        STATUS   ROLES                         AGE     VERSION           INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                        KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.59    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
master-2.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.113   <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
master-3.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.76    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
worker-1.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9   192.168.122.56    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
worker-2.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9   192.168.122.176   <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8

(jegan@tektutor.org)$ kubectl get nodes -o wide
NAME                        STATUS   ROLES                         AGE     VERSION           INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                        KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.59    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
master-2.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.113   <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
master-3.ocp.tektutor.org   Ready    control-plane,master,worker   3d20h   v1.25.4+a34b9e9   192.168.122.76    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
worker-1.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9   192.168.122.56    <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
worker-2.ocp.tektutor.org   Ready    worker                        3d20h   v1.25.4+a34b9e9   192.168.122.176   <none>        Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)   4.18.0-372.43.1.el8_6.x86_64   cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
</pre>

## Finding more details about an OpenShift node
```
oc describe node master-1.ocp.tektutor.org
```

Expected output
<pre>
(jegan@tektutor.org)$ oc describe node master-1.ocp.tektutor.org
Name:               master-1.ocp.tektutor.org
Roles:              control-plane,master,worker
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=master-1.ocp.tektutor.org
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node-role.kubernetes.io/master=
                    node-role.kubernetes.io/worker=
                    node.openshift.io/os_id=rhcos
Annotations:        machineconfiguration.openshift.io/controlPlaneTopology: HighlyAvailable
                    machineconfiguration.openshift.io/currentConfig: rendered-master-36f55ce1ce7f3c67a5c08014ac3e206b
                    machineconfiguration.openshift.io/desiredConfig: rendered-master-36f55ce1ce7f3c67a5c08014ac3e206b
                    machineconfiguration.openshift.io/desiredDrain: uncordon-rendered-master-36f55ce1ce7f3c67a5c08014ac3e206b
                    machineconfiguration.openshift.io/lastAppliedDrain: uncordon-rendered-master-36f55ce1ce7f3c67a5c08014ac3e206b
                    machineconfiguration.openshift.io/reason: 
                    machineconfiguration.openshift.io/state: Done
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Fri, 10 Feb 2023 15:42:39 +0530
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  master-1.ocp.tektutor.org
  AcquireTime:     <unset>
  RenewTime:       Tue, 14 Feb 2023 12:40:20 +0530
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 14 Feb 2023 12:38:52 +0530   Fri, 10 Feb 2023 15:57:59 +0530   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 14 Feb 2023 12:38:52 +0530   Fri, 10 Feb 2023 15:57:59 +0530   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 14 Feb 2023 12:38:52 +0530   Fri, 10 Feb 2023 15:57:59 +0530   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Tue, 14 Feb 2023 12:38:52 +0530   Fri, 10 Feb 2023 15:57:59 +0530   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.122.59
  Hostname:    master-1.ocp.tektutor.org
Capacity:
  cpu:                8
  ephemeral-storage:  104322028Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             64399800Ki
  pods:               250
Allocatable:
  cpu:                7500m
  ephemeral-storage:  95069439022
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             63248824Ki
  pods:               250
System Info:
  Machine ID:                             df6e475bdb344bb392671ace9e843910
  System UUID:                            df6e475b-db34-4bb3-9267-1ace9e843910
  Boot ID:                                0132eae9-0199-42ac-9567-b6201eb38824
  Kernel Version:                         4.18.0-372.43.1.el8_6.x86_64
  OS Image:                               Red Hat Enterprise Linux CoreOS 412.86.202301311551-0 (Ootpa)
  Operating System:                       linux
  Architecture:                           amd64
  Container Runtime Version:              cri-o://1.25.2-4.rhaos4.12.git66af2f6.el8
  Kubelet Version:                        v1.25.4+a34b9e9
  Kube-Proxy Version:                     v1.25.4+a34b9e9
Non-terminated Pods:                      (40 in total)
  Namespace                               Name                                                        CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                               ----                                                        ------------  ----------  ---------------  -------------  ---
  openshift-apiserver                     apiserver-7479789986-g5glp                                  110m (1%)     0 (0%)      250Mi (0%)       0 (0%)         3d20h
  openshift-authentication                oauth-openshift-6bdbbcb58-4ksmk                             10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         3d20h
  openshift-cluster-node-tuning-operator  tuned-f5nm8                                                 10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         3d20h
  openshift-cluster-storage-operator      csi-snapshot-controller-6587df558d-w26g2                    10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         3d20h
  openshift-cluster-storage-operator      csi-snapshot-webhook-8688766d4c-m7dj5                       10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         3d20h
  openshift-console                       console-67cb476f7-9b7fz                                     10m (0%)      0 (0%)      100Mi (0%)       0 (0%)         3d20h
  openshift-controller-manager            controller-manager-7df599c5c9-9s2fb                         100m (1%)     0 (0%)      100Mi (0%)       0 (0%)         2d21h
  openshift-dns                           dns-default-n8cks                                           60m (0%)      0 (0%)      110Mi (0%)       0 (0%)         3d20h
  openshift-dns                           node-resolver-98q9d                                         5m (0%)       0 (0%)      21Mi (0%)        0 (0%)         3d20h
  openshift-etcd                          etcd-guard-master-1.ocp.tektutor.org                        10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         3d20h
  openshift-etcd                          etcd-master-1.ocp.tektutor.org                              360m (4%)     0 (0%)      910Mi (1%)       0 (0%)         3d20h
  openshift-image-registry                image-registry-88694df94-kxp5f                              100m (1%)     0 (0%)      256Mi (0%)       0 (0%)         3d20h
  openshift-image-registry                node-ca-w794k                                               10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         3d20h
  openshift-ingress-canary                ingress-canary-tqxrz                                        10m (0%)      0 (0%)      20Mi (0%)        0 (0%)         3d20h
  openshift-ingress                       router-default-654bbc5f99-nrgs2                             100m (1%)     0 (0%)      256Mi (0%)       0 (0%)         3d20h
  openshift-kube-apiserver                kube-apiserver-guard-master-1.ocp.tektutor.org              10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         3d20h
  openshift-kube-apiserver                kube-apiserver-master-1.ocp.tektutor.org                    290m (3%)     0 (0%)      1224Mi (1%)      0 (0%)         2d21h
  openshift-kube-controller-manager       kube-controller-manager-guard-master-1.ocp.tektutor.org     10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         3d20h
  openshift-kube-controller-manager       kube-controller-manager-master-1.ocp.tektutor.org           80m (1%)      0 (0%)      500Mi (0%)       0 (0%)         3d20h
  openshift-kube-scheduler                openshift-kube-scheduler-guard-master-1.ocp.tektutor.org    10m (0%)      0 (0%)      5Mi (0%)         0 (0%)         3d20h
  openshift-kube-scheduler                openshift-kube-scheduler-master-1.ocp.tektutor.org          25m (0%)      0 (0%)      150Mi (0%)       0 (0%)         3d20h
  openshift-machine-config-operator       machine-config-controller-59786d68b6-bvtfm                  40m (0%)      0 (0%)      100Mi (0%)       0 (0%)         3d20h
  openshift-machine-config-operator       machine-config-daemon-gxvb9                                 40m (0%)      0 (0%)      100Mi (0%)       0 (0%)         3d20h
  openshift-machine-config-operator       machine-config-server-bvl7w                                 20m (0%)      0 (0%)      50Mi (0%)        0 (0%)         3d20h
  openshift-monitoring                    node-exporter-wjkwg                                         9m (0%)       0 (0%)      47Mi (0%)        0 (0%)         3d20h
  openshift-monitoring                    prometheus-adapter-b6b474d6f-drltr                          1m (0%)       0 (0%)      40Mi (0%)        0 (0%)         2d21h
  openshift-monitoring                    prometheus-operator-admission-webhook-7d4759d465-jf7x7      5m (0%)       0 (0%)      30Mi (0%)        0 (0%)         3d20h
  openshift-monitoring                    thanos-querier-687c464d4f-nbc6w                             15m (0%)      0 (0%)      92Mi (0%)        0 (0%)         3d20h
  openshift-multus                        multus-additional-cni-plugins-n92ts                         10m (0%)      0 (0%)      10Mi (0%)        0 (0%)         3d20h
  openshift-multus                        multus-admission-controller-84dfbc5587-nzmkv                20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         3d20h
  openshift-multus                        multus-smbfd                                                10m (0%)      0 (0%)      65Mi (0%)        0 (0%)         3d20h
  openshift-multus                        network-metrics-daemon-hlzrb                                20m (0%)      0 (0%)      120Mi (0%)       0 (0%)         3d20h
  openshift-network-diagnostics           network-check-source-b94cc7564-rnx44                        10m (0%)      0 (0%)      40Mi (0%)        0 (0%)         3d20h
  openshift-network-diagnostics           network-check-target-mkq5c                                  10m (0%)      0 (0%)      15Mi (0%)        0 (0%)         3d20h
  openshift-oauth-apiserver               apiserver-796d99cf99-7ls8k                                  150m (2%)     0 (0%)      200Mi (0%)       0 (0%)         3d20h
  openshift-operator-lifecycle-manager    packageserver-5dbd46fb6c-6njp4                              10m (0%)      0 (0%)      50Mi (0%)        0 (0%)         3d20h
  openshift-route-controller-manager      route-controller-manager-597f88967c-9qds5                   100m (1%)     0 (0%)      100Mi (0%)       0 (0%)         2d21h
  openshift-sdn                           sdn-controller-ztftb                                        20m (0%)      0 (0%)      70Mi (0%)        0 (0%)         3d20h
  openshift-sdn                           sdn-jwpcd                                                   110m (1%)     0 (0%)      220Mi (0%)       0 (0%)         3d20h
  openshift-service-ca                    service-ca-5d96446959-h5vpf                                 10m (0%)      0 (0%)      120Mi (0%)       0 (0%)         3d20h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests     Limits
  --------           --------     ------
  cpu                1950m (26%)  0 (0%)
  memory             5636Mi (9%)  0 (0%)
  ephemeral-storage  0 (0%)       0 (0%)
  hugepages-1Gi      0 (0%)       0 (0%)
  hugepages-2Mi      0 (0%)       0 (0%)
Events:              <none>
</pre>

## Editing node
```
oc edit node/master-2.ocp.tektutor.org
```


## Lab - Deploying nginx web server into OpenShift cluster
```
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3

oc get deployments
oc get deployment
oc get deploy

oc get replicasets
oc get replicaset
oc get rs

oc get pods
oc get pod
oc get po
```

Expected output
<pre>
(jegan@tektutor.org)$ oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
Warning: would violate PodSecurity "restricted:latest": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx created

(jegan@tektutor.org)$ oc get deployments
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3           25s
(jegan@tektutor.org)$ oc get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3           27s
(jegan@tektutor.org)$ oc get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3           29s

(jegan@tektutor.org)$ oc get replicasets
NAME               DESIRED   CURRENT   READY   AGE
nginx-66664d749f   3         3         3       52s
(jegan@tektutor.org)$ oc get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-66664d749f   3         3         3       55s
(jegan@tektutor.org)$ oc get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-66664d749f   3         3         3       58s

(jegan@tektutor.org)$ oc get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-66664d749f-fggcz   1/1     Running   0          77s
nginx-66664d749f-fz569   1/1     Running   0          77s
nginx-66664d749f-wwq9m   1/1     Running   0          77s
(jegan@tektutor.org)$ oc get pod
NAME                     READY   STATUS    RESTARTS   AGE
nginx-66664d749f-fggcz   1/1     Running   0          80s
nginx-66664d749f-fz569   1/1     Running   0          80s
nginx-66664d749f-wwq9m   1/1     Running   0          80s
(jegan@tektutor.org)$ oc get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-66664d749f-fggcz   1/1     Running   0          82s
nginx-66664d749f-fz569   1/1     Running   0          82s
nginx-66664d749f-wwq9m   1/1     Running   0          82s
</pre>


## What happens when you create a deployment?
```
oc create deploy nginx --image=bitnami/nginx:latest --replicas=3
```

When we issue the above command, the following things happen with the OpenShift cluster
<pre>

1. The oc client tool will make a REST API Call to API Server, requesting to create a deployment with bitnami/nginx image with 3 Pods.

2. API Server receives the request, and it creates a Deployment database entry in the etcd database.

3. API Server trigger an event something like "New Deployment" Created.

4. Deployment receives the "New Deployment" created event, and then it will make a REST call to API Server requesting it to create a ReplicaSet with Desired count as 3 Pods.

5. API Server receives the event, it then creates a ReplicaSet database entry in the etcd database.

6. API Server tirggers an event something like "New ReplicaSet" created.

7. ReplicaSet Controller receives the "New ReplicaSet" event, and then it fetches the desired count and then it will make a REST call to API Server requesting it to create 3 Pods.

8. API Server receives the event and it creates 3 Pod database entries in the etcd database.

9. API Server triggers an event something like "New Pod" created. This event is triggered for every Pod.

10. Scheduler receives the "New Pod" created event and it identifies healthy node where those individual Pods can be deploymed.  Scheduler then makes a REST call to API Server sending its scheduling recommendations for every new Pod.

11. API Server will retreive the Pod database entry from etcd and updates the scheduling info on the Pod entries stored in etcd.

12. API Server triggers "Pod scheduled" events.

13. Kubelet Container Agent running on respective nodes receive the "Pod Scheduled" event and it interacts with the Container Runtime to pull and deploy the containers for those Pods.

14. Kubelet monitors the status of the Pod and periodically keeps reporting the status of all the Pods that are running on the node where Kubelet is running like a heart-beat notification.
</pre>


## Scaling up/down
- Increasing/Decreasing the Pod count
- When the user traffic increase, we do scale up i.e add more Pods to handle the traffic quickly and efficiently
- When the user traffic decrease, we do scale down ie. delete some Pods
- supported by ReplicaSet Controller

## Rolling update
- helps in upgrading your already deployed application from one version to another version without any downtime
- also supports rolling back to previous or any particular version
- supported by Deployment Controller

## Deleting a deployment
```
oc delete deploy/nginx
```

Expected output
<pre>
(jegan@tektutor.org)$ oc delete deploy nginx
deployment.apps "nginx" deleted

(jegan@tektutor.org)$ oc get deploy,rs,po
No resources found in default namespace.
</pre>

## Lab - Scaling up the deployment
```
oc delete deploy/nginx

oc create deploy/nginx --image=bitnami/nginx:1.18 --replicas=3
oc get deploy,rs,po

oc set image deploy/nginx nginx=bitnami/ngin:1.19
```

## Lab - Rolling update, rollback
```
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc delete deploy/nginx</b>
deployment.apps "nginx" deleted

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
No resources found in jegan namespace.
(jegan@tektutor.org)$ oc create deploy nginx --image=bitnami/nginx:1.18
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx created

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           5s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   1         1         1       5s

NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-7fc97856d6-lh8qk   1/1     Running   0          5s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.19</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx image updated

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           34s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       34s
replicaset.apps/nginx-86dfdd65c9   1         1         1       6s

NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-86dfdd65c9-rw7b6   1/1     Running   0          5s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.20</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx image updated

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           68s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       68s
replicaset.apps/nginx-865bc46655   1         1         0       2s
replicaset.apps/nginx-86dfdd65c9   1         1         1       40s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   0/1     ContainerCreating   0          2s
pod/nginx-86dfdd65c9-rw7b6   1/1     Running             0          39s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           87s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       87s
replicaset.apps/nginx-865bc46655   1         1         1       21s
replicaset.apps/nginx-86dfdd65c9   0         0         0       59s

NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running   0          21s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.21</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx image updated

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           96s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       96s
replicaset.apps/nginx-865bc46655   1         1         1       30s
replicaset.apps/nginx-86dfdd65c9   0         0         0       68s
replicaset.apps/nginx-c95694c6b    1         1         0       2s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          30s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          2s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           104s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       104s
replicaset.apps/nginx-865bc46655   1         1         1       38s
replicaset.apps/nginx-86dfdd65c9   0         0         0       76s
replicaset.apps/nginx-c95694c6b    1         1         0       10s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          38s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          10s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           105s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       105s
replicaset.apps/nginx-865bc46655   1         1         1       39s
replicaset.apps/nginx-86dfdd65c9   0         0         0       77s
replicaset.apps/nginx-c95694c6b    1         1         0       11s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          39s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          11s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           106s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       106s
replicaset.apps/nginx-865bc46655   1         1         1       40s
replicaset.apps/nginx-86dfdd65c9   0         0         0       78s
replicaset.apps/nginx-c95694c6b    1         1         0       12s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          40s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          12s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           107s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       107s
replicaset.apps/nginx-865bc46655   1         1         1       41s
replicaset.apps/nginx-86dfdd65c9   0         0         0       79s
replicaset.apps/nginx-c95694c6b    1         1         0       13s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          41s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          13s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           108s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       108s
replicaset.apps/nginx-865bc46655   1         1         1       42s
replicaset.apps/nginx-86dfdd65c9   0         0         0       80s
replicaset.apps/nginx-c95694c6b    1         1         0       14s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Running             0          42s
pod/nginx-c95694c6b-625vk    0/1     ContainerCreating   0          14s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/1     1            2           109s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-7fc97856d6   0         0         0       109s
replicaset.apps/nginx-865bc46655   0         1         1       43s
replicaset.apps/nginx-86dfdd65c9   0         0         0       81s
replicaset.apps/nginx-c95694c6b    1         1         1       15s

NAME                         READY   STATUS        RESTARTS   AGE
pod/nginx-865bc46655-zrknc   1/1     Terminating   0          43s
pod/nginx-c95694c6b-625vk    1/1     Running       0          15s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.22</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx image updated

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           115s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-56f59c7849   1         1         0       2s
replicaset.apps/nginx-7fc97856d6   0         0         0       115s
replicaset.apps/nginx-865bc46655   0         0         0       49s
replicaset.apps/nginx-86dfdd65c9   0         0         0       87s
replicaset.apps/nginx-c95694c6b    1         1         1       21s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-56f59c7849-427ts   0/1     ContainerCreating   0          2s
pod/nginx-c95694c6b-625vk    1/1     Running             0          21s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           117s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-56f59c7849   1         1         0       4s
replicaset.apps/nginx-7fc97856d6   0         0         0       117s
replicaset.apps/nginx-865bc46655   0         0         0       51s
replicaset.apps/nginx-86dfdd65c9   0         0         0       89s
replicaset.apps/nginx-c95694c6b    1         1         1       23s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-56f59c7849-427ts   0/1     ContainerCreating   0          4s
pod/nginx-c95694c6b-625vk    1/1     Running             0          23s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           2m1s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-56f59c7849   1         1         0       8s
replicaset.apps/nginx-7fc97856d6   0         0         0       2m1s
replicaset.apps/nginx-865bc46655   0         0         0       55s
replicaset.apps/nginx-86dfdd65c9   0         0         0       93s
replicaset.apps/nginx-c95694c6b    1         1         1       27s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-56f59c7849-427ts   0/1     ContainerCreating   0          8s
pod/nginx-c95694c6b-625vk    1/1     Running             0          27s

(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   1/1     1            1           2m2s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-56f59c7849   1         1         0       9s
replicaset.apps/nginx-7fc97856d6   0         0         0       2m2s
replicaset.apps/nginx-865bc46655   0         0         0       56s
replicaset.apps/nginx-86dfdd65c9   0         0         0       94s
replicaset.apps/nginx-c95694c6b    1         1         1       28s

NAME                         READY   STATUS              RESTARTS   AGE
pod/nginx-56f59c7849-427ts   0/1     ContainerCreating   0          9s
pod/nginx-c95694c6b-625vk    1/1     Running             0          28s

(jegan@tektutor.org)$ <b>oc get po -w</b>
NAME                     READY   STATUS        RESTARTS   AGE
nginx-56f59c7849-427ts   1/1     Running       0          15s
nginx-c95694c6b-625vk    1/1     Terminating   0          34s
nginx-c95694c6b-625vk    0/1     Terminating   0          35s
nginx-c95694c6b-625vk    0/1     Terminating   0          35s
nginx-c95694c6b-625vk    0/1     Terminating   0          35s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.23</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx image updated

(jegan@tektutor.org)$ <b>oc get po -w</b>
NAME                     READY   STATUS              RESTARTS   AGE
nginx-56f59c7849-427ts   1/1     Running             0          22s
nginx-fdf6fcbd4-sjpch    0/1     ContainerCreating   0          1s
nginx-fdf6fcbd4-sjpch    0/1     ContainerCreating   0          2s
nginx-fdf6fcbd4-sjpch    1/1     Running             0          11s
nginx-56f59c7849-427ts   1/1     Terminating         0          32s
nginx-56f59c7849-427ts   0/1     Terminating         0          33s
nginx-56f59c7849-427ts   0/1     Terminating         0          33s
nginx-56f59c7849-427ts   0/1     Terminating         0          33s

^C(jegan@tektutor.org)$ <b>oc get po</b>
NAME                    READY   STATUS    RESTARTS   AGE
nginx-fdf6fcbd4-sjpch   1/1     Running   0          16s

(jegan@tektutor.org)$ <b>oc get rs</b>
NAME               DESIRED   CURRENT   READY   AGE
nginx-56f59c7849   0         0         0       41s
nginx-7fc97856d6   0         0         0       2m34s
nginx-865bc46655   0         0         0       88s
nginx-86dfdd65c9   0         0         0       2m6s
nginx-c95694c6b    0         0         0       60s
nginx-fdf6fcbd4    1         1         1       20s

(jegan@tektutor.org)$ <b>oc set image deploy/nginx nginx=bitnami/nginx:1.23</b>

(jegan@tektutor.org)$ <b>oc rollout history deploy/nginx</b>
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>
4         <none>
5         <none>
6         <none>

(jegan@tektutor.org)$ <b>oc edit deploy/nginx</b>
Edit cancelled, no changes made.

(jegan@tektutor.org)$ <b>oc rollout undo deploy/nginx</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx rolled back

(jegan@tektutor.org)$ <b>oc edit deploy/nginx</b>
Edit cancelled, no changes made.

(jegan@tektutor.org)$ <b>oc rollout undo deploy/nginx --to-revision=1</b>
Warning: would violate PodSecurity "restricted:v1.24": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
deployment.apps/nginx rolled back
</pre>

## Lab - Finding the IP Address of Pod(s)
```
oc get po -o wide
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>oc get po -o wide</b>
NAME                     READY   STATUS    RESTARTS   AGE   IP             NODE                        NOMINATED NODE   READINESS GATES
nginx-7fc97856d6-77z48   1/1     Running   0          25s   10.131.0.41    worker-2.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-fqspq   1/1     Running   0          25s   10.130.0.88    master-3.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-tq8x5   1/1     Running   0          25s   10.128.0.104   master-2.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-zqnkl   1/1     Running   0          17m   10.128.3.168   worker-1.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-zx2v8   1/1     Running   0          25s   10.129.0.93    master-1.ocp.tektutor.org   <none>           <none>
</pre>


## Lab - Testing if you are able to access the Web page from an nginx Pod

The below command is a blocking command, so you may have to use another terminal if you prefer curl or once you tested with web browser you can come back to the same terminal and press Ctrl + C
```
oc port-forward nginx-7fc97856d6-77z48 8080:8080
```


Expected output
<pre>
(jegan@tektutor.org)$ <b>oc port-forward nginx-7fc97856d6-77z48 8080:8080</b>
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
Handling connection for 8080
</pre>

You may now access the web page either using curl or on lab machine web browser using localhost:8080


## Creating a ClusterIP Internal Service for your nginx deployment
```
```

Expected output
```
(jegan@tektutor.org)$ oc get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   5/5     5            5           36m
(jegan@tektutor.org)$ oc expose deploy/nginx --type=ClusterIP --port=8080
service/nginx exposed

(jegan@tektutor.org)$ oc get services
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.19.152   <none>        8080/TCP   4s

(jegan@tektutor.org)$ oc get service
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.19.152   <none>        8080/TCP   6s

(jegan@tektutor.org)$ oc get svc
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
nginx   ClusterIP   172.30.19.152   <none>        8080/TCP   8s

(jegan@tektutor.org)$ oc describe svc/nginx
Name:              nginx
Namespace:         jegan
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.19.152
IPs:               172.30.19.152
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.0.104:8080,10.128.3.168:8080,10.129.0.93:8080 + 2 more...
Session Affinity:  None
Events:            <none>

(jegan@tektutor.org)$ oc get po -o wide
NAME                     READY   STATUS    RESTARTS   AGE   IP             NODE                        NOMINATED NODE   READINESS GATES
nginx-7fc97856d6-77z48   1/1     Running   0          16m   10.131.0.41    worker-2.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-fqspq   1/1     Running   0          16m   10.130.0.88    master-3.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-tq8x5   1/1     Running   0          16m   10.128.0.104   master-2.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-zqnkl   1/1     Running   0          33m   10.128.3.168   worker-1.ocp.tektutor.org   <none>           <none>
nginx-7fc97856d6-zx2v8   1/1     Running   0          16m   10.129.0.93    master-1.ocp.tektutor.org   <none>           <none>

(jegan@tektutor.org)$ oc expose svc/nginx
route.route.openshift.io/nginx exposed

(jegan@tektutor.org)$ oc get route
NAME    HOST/PORT                           PATH   SERVICES   PORT   TERMINATION   WILDCARD
nginx   nginx-jegan.apps.ocp.tektutor.org          nginx      8080                 None
(jegan@tektutor.org)$ curl nginx-jegan.apps.ocp.tektutor.org
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

## Lab - Understanding Service Discovery
The below docker image has curl utility installed in it, we need curl utility to test the service discovery.
```
oc create deploy hello --image=tektutor/spring-ms:1.0
```
Open a shell inside the hello pod
```
oc rsh deploy/hello
curl http://nginx:8080
```

In the above, nginx is the name of the ClusterIP service and port 8080 is the service port.

Service Discovery is nothing but accessing a service using its name, the Core DNS in OpenShift helps resolve the name of the service into its corresponding IP address on the fly.


