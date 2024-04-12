---
authors:
  - danshaw
date: 2024-04-08
categories:
  - se1
readtime: 10
---

# Running a parallel OpenFoam job on Alces Cloud
In this blog, we'll guide you through the installation of OpenFOAM and then running a multinode job. We will then view our job results using Alces Cloud's desktop capabilities.

We will be exploring a quick route into getting started running a HPC workflow on Alces Cloud.
<!-- more -->

## Build your HPC platform using Flight Solo

Setting up a HPC environment can be complicated and time consuming so in order to reduce the time to getting started running my workflow, I've opted to utilise the Flight Solo image in this blog. 

I prefer to use the CLI, so firstly I accessed the Alces Cloud (region se1) using my credentials - for the purposes of this blog, I've redacted my actual username!
```
dshaw@localmachine ~ % ssh myusername@se1.alces.network
```
Next, I wanted to upload the latest version of the Flight Solo image to my Openstack project:
```
(openstack) [myusername@stack01[poc1] ~]$ wget https://repo.openflighthpc.org/images/FlightSolo/2024.1/Flight_Solo_2024-1_generic-cloudinit.raw
(openstack) [myusername@stack01[poc1] ~]$ source openrc
(openstack) [myusername@stack01[poc1] ~]$ openstack image create --disk-format raw --min-disk 10 --min-ram 2048 --file Flight_Solo_2024-1_generic-cloudinit.raw Flight_Solo_2024.1
```
I can verify that my Flight Solo image has uploaded sucessfully with:
```
(openstack) [myusername@stack01[poc1] ~]$ openstack image list
+--------------------------------------+----------------------+--------+
| ID                                   | Name                 | Status |
+--------------------------------------+----------------------+--------+
| 2b5f1096-8c9f-4f39-89bc-e6f95b844c21 | Flight_Solo_2024.1   | active |
...
```

With this image now in place we can now get ready to launch our Flight Solo cluster. For our OpenFOAM workflow, I'm going to launch a cluster with one "login" node (which we'll use later) and two large "compute" nodes (which will be what we run our workload on). 

Firstly, let's create our 'cluster login' node.



```

(openstack) [myusername@stack01[poc1] ~]$ openstack keypair create mykeypair --private-key ~/.ssh/mykeypair
```



## Installing your OpenFOAM version

## Running your job/workflow

## Viewing your results
