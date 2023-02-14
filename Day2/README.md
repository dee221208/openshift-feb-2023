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
