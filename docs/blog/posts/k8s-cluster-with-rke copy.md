---
authors:
  - shubhamdang
date: 2024-03-03
categories:
  - Kubernetes
readtime: 2
---


# Kubernetes Installation With RKE

### OpenStack CLI User Guide: Launching Instances
This guide provides instructions for launching instances in OpenStack using the command-line interface (CLI). Ensure that you have the OpenStack command-line client installed and configured before proceeding.

**Step 1: Source the OpenStack RC File**
Before using the CLI, source the OpenStack RC file to set environment variables:

```bash
source <your-openstack-rc-file>
Replace <your-openstack-rc-file> with the path to your OpenStack RC file.
```
<!-- more -->

**Step 2: List Available Images, Flavors, and Networks**
To list available images, flavors, and networks, use the following commands:

```bash
openstack image list
openstack flavor list
openstack network list
```
Note the IDs or names of the image, flavor, and network you want to use for your instance.

**Step 3: Launch an Instance**
Use the openstack server create command to launch an instance:

``` bash
openstack server create --flavor <flavor> --image <image> --nic net-id=<network> --key-name <keypair> <instance-name> 
```

Replace <flavor>, <image>, <network>, <keypair>, and <instance-name> with the appropriate values. 
For example:

```bash

openstack server create --flavor m1.small --image cirros --nic net-id=my_network --key-name my_keypair my_instance
```
**Step 4: Associate Floating IP to Instance**
Allocate a floating IP from the floating IP pool:

```bash
openstack floating ip create <floating-ip-pool>
```
Associate the floating IP with your instance:

```bash
openstack server add floating ip <instance-name> <floating-ip-address>
```
Replace <floating-ip-pool>, <instance-name>, and <floating-ip-address> accordingly.

Congratulations! You have successfully launched an instance in OpenStack using the CLI. Adjust the commands and parameters based on your specific requirements.

For more details and options, refer to the [OpenStack User Guide.](https://docs.openstack.org/ocata/user-guide/cli-launch-instances.html)

### K8SInstallation with RKE
Here we have two types of nodes.
rke node: node where rke utility is installed that is responsible for installation of k8s cluster on other nodes.
k8s nodes: nodes that are the part of k8s cluster where installation takes place.

**Prerequisites**
Docker must be installed on all the nodes of k8s cluster.
password less ssh to all k8s nodes from rke node.

Here we are creating a cluster with one master node and one worker node, so number of vm required here is 2. we also need to setup a rke node as well.

**Initial setup for RKE node**
1. Install RKE

    ```bash wget https://github.com/rancher/rke/releases/download/v1.2.9/rke_linux-amd64 -O rke
    chmod +x rke
    ```
2. Perform a password-less ssh from rke node to k8s nodes.
3. Install kubectl to access k8s resources.

    ```bash
    yum install epel-release
    yum install snapd
    systemctl enable --now snapd.socket
    ln -s /var/lib/snapd/snap /snap
    snap install kubectl --classic
     ```


**Initial setup for k8s node**
1. Disabled selinux on all nodes and firewalld.
    ```bash
    setenforce 0  
    systemctl stop firewalld
    ```
    
2. Install Docker on all nodes.

    ```bash
    yum update –y 
    yum install -y yum-utils  
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
    yum install docker-ce docker-ce-cli containerd.io 
    systemctl start docker  
    systemctl enable docker
    ``` 


**Steps for installation of K8s cluster**

On the rke node, create a cluster.yml file specifying the user , ip of k8s nodes, type of networking , roles and version of k8s we want to install.

    ```yaml
    # cluster.yml
    nodes: 
    - address: 10.151.0.xx
      user: rke 
      role: 
        - controlplane 
        - etcd 
      hostname_override: master
      
    - address: 10.151.0.xx
      user: rke 
      role: 
        - worker
      hostname_override: worker
      
    kubernetes_version: v1.20.8-rancher1-1
    network: 
        plugin: canal
    ```

2. Then execute ./rke up
Sometimes it will say that docker version is not supported, then execute ./rke up --ignore-docker-version.

3. Once the installation is complete you will get a file named kube_config_cluster.yml in the same directory i.e. the kubeconfig of cluster we have created.


