---
authors:
  - shubhamdang
date: 2024-03-03
categories:
  - se1
readtime: 5
---


# Kubernetes Installation With RKE
In this blog, we'll guide you through the seamless installation of a Kubernetes cluster on a RHEL-based operating system. Our setup involves two nodes; one serving as both the RKE and master node, and the other dedicated as a worker node.

Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, leading up to the installation of the Kubernetes cluster using RKE.
<!-- more -->

## Launch the Instance

Launch 2 instances for our setup where one node will act as a rke and master node, whereas second node will act as worker node in the kubernetes cluster.

All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Kubernetes Installation with RKE
Here we have 2 types of nodes:
rke node: node where rke utility is installed that is responsible for installation of k8s cluster on other nodes.
k8s nodes: nodes that are the part of k8s cluster where installation takes place.

**Prerequisites**

- Docker must be installed on all the nodes of k8s cluster.
  ```bash
  sudo dnf check-update
  sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  sudo dnf install docker-ce docker-ce-cli containerd.io
  sudo systemctl start docker
  sudo systemctl status docker
  sudo systemctl enable docker

  # Execute docker cli without sudo
  sudo usermod -aG docker $(whoami)

  # You will need to log out of the Droplet and back in as the same user to enable this change.
  ```

- password less ssh to all k8s nodes from rke node.

Here we are creating a cluster with one master node and one worker node, so number of vm required here is 2. we also need to setup a rke node as well.

**Initial setup for RKE node**

- Install RKE 
  On the first instance we will be installing rke cli to install our cluster, below are the instructions.

  ```bash 
  wget https://github.com/rancher/rke/releases/download/v1.2.9/rke_linux-amd64 -O rke chmod +x rke
  ```
- Perform a password-less ssh from rke node to k8s nodes.
- Install kubectl to access k8s resources.

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


