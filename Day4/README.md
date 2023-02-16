# Day4

## Lab - Deploying mysql db server into OpenShift that uses a Persistent Storage from NFS

First you need to edit the yml files and replace 'jegan' with your name and the IP address with your centos IP.

```
cd ~/openshift-feb-2023
git pull

cd Day4/mysql-with-persistent-storage
oc create -f mysql-pv.yml
oc create -f mysql-pvc.yml
oc create -f mysql-deploy.yml
```
