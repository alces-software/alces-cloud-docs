---
authors:
  - shubhamdang
date: 2024-03-20
categories:
  - se1
readtime: 5
---

# How to install and use Docker

In this blog post, we will provide a comprehensive guide on installing Docker, followed by a detailed walkthrough on its usage on a rocky based operating system. Our setup revolves around a single node with Docker installed, ready to facilitate the execution of Docker containers.


Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, followed by installation of docker and how to use docker cli to perform docker operations.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).


## Setup Docker

The Docker installation package provided in the official Rocky Linux 9 repository might not always be the most up-to-date version. For access to the latest features and updates, it's recommended to install Docker directly from the official Docker repository. Use below command to update package database and add docker repository.

```


sudo dnf check-update
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Now install the Docker using below commands.

```
sudo dnf update -y
sudo dnf install -y docker-ce docker-ce-cli containerd.io
```

After the installation of docker, enable and start the docker. 
```
sudo systemctl enable docker
sudo systemctl start docker
```

### Verify Docker
You can check the web server is up and running by using the below command 
```
systemctl status docker

Output
------
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
     Active: active (running) since Wed 2024-03-20 16:18:27 UTC; 40s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 50100 (dockerd)
      Tasks: 10
     Memory: 34.7M
        CPU: 395ms
     CGroup: /system.slice/docker.service
             └─50100 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```


### Execute Docker command without sudo (optional)
By default, the docker command requires root privileges to run, requiring the command to be prefixed with sudo.

To bypass the need for typing sudo each time you use the docker command, include your username in the docker group:
```
sudo usermod -aG docker $(whoami)
```
Afterwards, log out of the Droplet and log back in as the same user to activate this modification.


If you wish to add a user to the docker group for a user other than the one you're logged in as, specify the username explicitly:
```
sudo usermod -aG docker username
```

### Working with Docker command

You can use  the following command to display a list of all available subcommands and additional help:
```
docker --help
```

For more detailed information about a specific subcommand, you can type:
```
docker docker-subcommand --help
```

**Working with Docker images**

Docker containers operate on Docker images. Images are fetched from Docker Hub, a registry operated by Docker. Docker Hub enables anyone to construct and share their Docker images, making it a central repository for a wide array of applications and Linux distributions necessary for Docker container deployment.

To ensure Docker is functioning correctly, execute the following command:
```
docker run hello-world

Output
------
Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```

The displayed output confirms Docker's operational status.

To search for an image on Docker Hub, run the following command:
```
docker search ubuntu

Output
------
NAME                             DESCRIPTION                                     STARS     OFFICIAL
ubuntu                           Ubuntu is a Debian-based Linux operating sys…   16959     [OK]
ubuntu-debootstrap               DEPRECATED; use "ubuntu" instead                52        [OK]
websphere-liberty                WebSphere Liberty multi-architecture images …   298       [OK]
open-liberty                     Open Liberty multi-architecture images based…   64        [OK]
neurodebian                      NeuroDebian provides neuroscience research s…   107       [OK]
ubuntu-upstart                   DEPRECATED, as is Upstart (find other proces…   115       [OK]
ubuntu/nginx                     Nginx, a high-performance reverse proxy & we…   112
ubuntu/squid                     Squid is a caching proxy for the Web. Long-t…   86
ubuntu/cortex                    Cortex provides storage for Prometheus. Long…   4
ubuntu/prometheus                Prometheus is a systems and service monitori…   58
ubuntu/apache2                   Apache, a secure & extensible open-source HT…   71
ubuntu/kafka                     Apache Kafka, a distributed event streaming …   44
ubuntu/bind9                     BIND 9 is a very flexible, full-featured DNS…   80
ubuntu/zookeeper                 ZooKeeper maintains configuration informatio…   13
ubuntu/mysql                     MySQL open source fast, stable, multi-thread…   61
ubuntu/jre                       Distroless Java runtime based on Ubuntu. Lon…   13
ubuntu/postgres                  PostgreSQL is an open source object-relation…   35
```

Once we found the image, we can pull the image locally
```
docker pull ubuntu
```

To view the images downloaded to your system, use the command:
```
docker images

Output
------
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    ca2b0f26964c   3 weeks ago     77.9MB
hello-world   latest    d2c94e258dcb   10 months ago   13.3kB
```

**Running a Docker container**

In last steps we have successfully downloaded Docker images (`hello-world and ubuntu:latest`).
Now we will be running the Docker container using the latest image of ubuntu However, this time, we will interacting with the container shell by executing the following command:

```
docker run -it ubuntu

Output
------
[root@59839a1b7de2 /]#
```

To list all the docker containers in the system, run the below command 
```
docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED              STATUS                          PORTS     NAMES
f8b4958bcda6   ubuntu        "/bin/bash"   10 seconds ago       Exited (0) 5 seconds ago                  competent_nobel
dbcb92c48a44   hello-world   "/hello"      About a minute ago   Exited (0) About a minute ago             brave_carver
```
!!! note
    For more information about Docker, please refer to the official Docker documentation [link](https://docs.docker.com/get-started/overview/)





