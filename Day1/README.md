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