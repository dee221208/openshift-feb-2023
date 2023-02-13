# Day 1

## What is Hypervisor?
- aka Virtualization
- Hardware + Software Technology
  General Purpose Processors
  1. AMD - virtualization feature is AMD-V
  2. Intel - Virtualization feature is VT-X
- two types
  1. Type 1 - Meant for Servers/Workstations ( Type 1 hypervisor is directly installed on the Hardware - no OS is required)
  2. Type 2 - Meant for Laptops/Desktops/Workstations( Type2 hypervisor is installed on top of Host OS )
- Examples
  1. Oracle VirtualBox (Free)
  2. VMWare ( Commercial - need license )
     - Fusion ( Type 2 - Mac OS-X)
     - Workstation ( Type 2 - Windows & Linux ) 
     - vSphere/Vcenter ( Type 1 )
  3. KVM - Kernel Virtual Manager (Opensource)   
  4. Parallels ( Type 2 - Mac OS-X )   
- allows running many Operating System within Virtual Machine side-side i.e many OS can be active on the same system
- each Virtual Machine is allocated with dedicated hardware resources
  - CPU
  - RAM
  - Storage
- hence, this type of virtualization is called as Heavy weight virtualization
- The OS installed with VM/Guest OS is a fully functional Operating Sytem

## What is Application Virtualization?
- Container technology
- each container represents one single application
- each container, runs in its namespace
- every container has its own dedicated Network Stack and Virtual Network Card, hence an IP is allocated for each container
- every container get a file system 
- When Docker for Windows is installed it installs a thin-linux layer which has the Linux Kernel
- When Docker for Mac is installed, it installs a thin-linux layer which has the Linux Kernel
- in Linux Host OS, we can only run Linux containerized application as we have only Linux Kernel
- in Windows Host OS, we can run both Windows containerized applications as well Linux containerized applications
 
## Docker Overview
- developed by Docker Inc organization in Go Programming language
- it comes in two flavours
  - Docker Community Edition(CE) - you get opensource Docker Images with limited officially certied Docker images
  - Docker Enterprise Edition(EE) - you get officially certified Docker Images, you get support from Docker Inc
- follows client/server architecture
  Client - docker utility
  Server - dockerd or Daemon which runs as a Daemon service

## What is a Docker Image?
- specification of a Docker Container
- the software tools required on a container can be installed on the Docker Image
- Using Docker Image, we can create any number of Containers

## What is a Docker Container?
- an instance of Docker Image
- each running container, get atleast one IP address as container may have more than one Network Interface Cards (NIC)

## What is a Docker Container Registry?
- this is where Docker Images are hosted
- three types of Docker Registry
  - Local Docker Registry ( /var/lib/docker folder on your system where Docker is installed )
  - Remote Docker Registry - Docker Hub (Web portal) that has many opensource Docker Images
  - Private Docker Registry - Optional ( can be setup using Sonatype Nexus, JFrog Artifactory, for learning or R&D purpose you could also use the registry:2 Docker Image from Docker Hub

## What is Container Runtime?
- this is the software utility that manages the containers
  - creating container
  - start/stop/restart/abort/kill/delete containers
  - are normally used only by Container Engine
 - Example
   - runC - used by Docker Container Engine
   - CRI-O - used by Podman Container Engine

## What is Container Engine?
- this a high-level software used by end-users like us
- Container Engines dependes on Container Runtimes to manage containers
- Container Engines also depends on other tools to manage Container Images
- Container Engines provide easy-to-use user-friendly commands 
- Without knowing lowel-level container internals, we can manage container images/containers using this tool
- Example
  - Docker
  - Podman

## What is a Container Orchestration Platform?
Examples
1. Docker SWARM
2. Google Kubernetes
3. Red Hat OpenShift

### Common Features supported by any Container Orchestration Platform
1. Manages containerized application taking help of Container Engines/Runtimes
2. In built monitoring features 
   - checks the health of each containerized applications and repairs then if they are found unhealthy
   - check if the containerized application is live, ie is it responding to user request if not it will be replaced with another healthy live instance of the same containerized application
3. High Availability (HA)
   - it ensures, the desired number of application instances are always running
   - whenever the Orchestration plaform finds either less number of instances running or more than the desired number of application instances are running, it ensure the desired instances matches with the actual number of instances
4. Supports Service abstraction
   - Service represents a group of load-balanced application instances of same type
   - End-user application that needs let's say mysql db server, they will access the mysql db instance via Service
   - each service has a name and IP address
6. Supports Service Discovery
   - nothing but accessing the Service by its name
   - Orchestration Platforms support DNS - Domain Naming Server 
     - translates the name of the service to its corresponding IP address
7. Scale up/down 
   - Whenever the traffic to your web applications/microservices increase, you may scale up the number of application instance to handle the user traffic with less response time
   - Whenever the traffic to your web applications/microservices decrease, you may scale down the number of application instances
8. Rolling update
   - assume you have alreay deployed some microservice whose version is 1.0, if you wish to deploy v2.0 without any down time, you can use the rolling update feature of the Orchestration Platform
   - support rollback
 

# Docker Commands

## Finding the docker version
```
docker --version
```

## Listing the Docker images
```
docker images
```

## Finding more detailed information about your docker installation
```
docker info
```

Expected output
<pre>
(jegan@tektutor.org)$ docker info
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 6
  Running: 4
  Paused: 0
  Stopped: 2
 Images: 116
 Server Version: 20.10.7
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 5.4.0-120-generic
 Operating System: Ubuntu 18.04.6 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 48
 Total Memory: 125.6GiB
 Name: tektutor.org
 ID: I3XQ:RESC:AIUK:6RVT:T34U:3CP5:GHLH:QWET:J52D:UV3O:G5B6:3WGE
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Username: tektutor
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  image-registry-openshift-imclearage-registry.apps.ocp.tektutor.org
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: API is accessible on http://0.0.0.0:4243 without encryption.
         Access to the remote API is equivalent to root access on the host. Refer
         to the 'Docker daemon attack surface' section in the documentation for
         more information: https://docs.docker.com/go/attack-surface/
WARNING: No swap limit support
</pre>
