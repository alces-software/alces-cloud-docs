---
authors:
  - shubhamdang
date: 2024-03-21
categories:
  - se1
readtime: 5
---

# Installation and configuration of Minio 
In this blog, we'll guide you through the installation of a MiniIO on a linux based operating system.

Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, then attaching and mounting a block volume and leading up to the installation of MinIO storage and its operations.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Create, attach and mount volume to instance
All the steps to create, attach and mounting of volume to instance is provided in [link](../../docs/storage/volumes.md)

!!! note
    Just Make sure port `22, 9000, 9001, 80, 443` is opened as ingress rule in security group that is attached to the instance.
    If `firewalld` is enabled on the server, open the ports `9000` and `9001` required by the MinIO server. You also need to open port `80` and `443` required for SSL access.

    Use the below commands to open the required ports.
    ```
    sudo firewall-cmd --zone=public --add-port=9000/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=9001/tcp --permanent
    sudo firewall-cmd --permanent --add-service=http
    sudo firewall-cmd --permanent --add-service=https
    ```

    To apply the changes and reload the firewalld run below command
    ```
    sudo firewall-cmd --reload
    ```

## Setup MinIO

MinIO is a S3-compatible object storage server, It supports clients for multiple platforms and offers a web interface for managing objects and users.

**MinIO Installation Steps**

- MinIO comes in the form of a binary file, You can download it from the official website.
    ```
    sudo wget https://dl.min.io/server/minio/release/linux-amd64/minio -O /usr/local/bin/minio
    ```

- Make the file executable by adjusting its permissions.
    ```
    sudo chmod +x /usr/local/bin/minio
    ```

- Ensure proper SELinux permissions are set for the file.
    ```
    sudo restorecon -v /usr/local/bin/minio
    ```

- To confirm the installation, check the version information.
    ```
    minio --version
    minio version RELEASE.2024-03-15T01-07-19Z
    ```


**Configure and Setup Service of MinIO**

- Set up a  user to operate the MinIO server.
    ```
    sudo useradd -r minio-user -s /sbin/nologin
    ```

- Adjust the ownership of the MinIO binary.
    ```
    sudo chown minio-user:minio-user /usr/local/bin/minio
    ```

- Establish a directory to store MinIO server configuration files.
    ```
    sudo mkdir /etc/minio
    ```

- Ensure the ownership of the configuration directory is appropriately set.
    ```
    sudo chown minio-user:minio-user /etc/minio
    ```

- Assign ownership to the directory where data will be mounted.
    ```
    sudo chown minio-user:minio-user /data
    ```

- Create the default environment file and open it for update.
    ```
    sudo nano /etc/default/minio
    ```

- Within the file, insert the following lines:
    ```
    MINIO_VOLUMES="/data"
    MINIO_OPTS="-C /etc/minio --address :9000 --console-address :9001"
    MINIO_ROOT_USER="minio"
    MINIO_ROOT_PASSWORD="ChooseAVeryStrongPassword"
    ```

- Retrieve the service unit file from MinIO's GitHub repository.
    ```
    sudo wget https://raw.githubusercontent.com/minio/minio-service/master/linux-systemd/minio.service -O /etc/systemd/system/minio.service
    ```

- Adjust SELinux settings to authorize the service execution.
    ```
    sudo restorecon -v /etc/systemd/system/minio.service
    ```

- Refresh the service daemon.
    ```
    sudo systemctl daemon-reload
    ```

- Enable and start MinIO service.
    ```
    sudo systemctl enable minio
    sudo systemctl start minio
    ```

- Verify the status of MinIO Service
    ```
    systemctl status minio
    ```

You can now access the MinIO console through the following URL in your web browser: `http://your_server_ip:9001`. Log in using the root username and password you previously configured.


**Enable SSL on MinIO(Optional)**

We are using Certbot and Let's Encrypt API to enable SSL, Make sure Certbot is installed on the system, if it is not installed follow below steps.

- Install the EPEL repository and Certbot
    ```
    sudo dnf install epel-release
    sudo dnf install certbot
    ```

- Request an SSL certificate for MinIO using standalone mode:
    ```
    sudo certbot certonly --standalone --agree-tos --no-eff-email --preferred-challenges http -m name@alces.com -d minio.alces.com
    ```

- The SSL certificate is now accessible from the /etc/letsencrypt/live/minio.alces.com directory, Copy the certificate files to the /etc/minio/certs folder:
    ```
    sudo cp /etc/letsencrypt/live/minio.alces.com/privkey.pem /etc/minio/certs/private.key
    sudo cp /etc/letsencrypt/live/minio.alces.com/fullchain.pem /etc/minio/certs/public.crt
    ```

- Adjust ownership of the certificates:
    ```
    sudo chown minio-user:minio-user /etc/minio/certs/private.key
    sudo chown minio-user:minio-user /etc/minio/certs/public.crt
    ```

- Open the MinIO default configuration file, Add the following line at the bottom and Save the file by pressing CTRL+X, then Y.
    ```
    sudo nano /etc/default/minio
    MINIO_SERVER_URL="https://minio.alces.com:9000"
    ```

- Restart the MinIO Server to apply the changes:
    ```
    sudo systemctl restart minio
    ```

To access MinIO, simply navigate to the URL `https://minio.alces.com:9001` in your web browser. Log in using the root username and password you previously configured.

Once logged in, you can begin utilizing MinIO to create buckets and store your data. Whether you prefer the official MinIO client or any S3 compatible tool, you'll have full access to manage and interact with your uploaded data.