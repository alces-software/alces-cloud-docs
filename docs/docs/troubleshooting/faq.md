#Frequently Asked Questions

## General
### How do I connect to my instances?
Access to your instances is provided via floating IP addresses and the secure VPN service. Click [here](../starter/index.md) for our Getting Started guide.

### I'm get the message `ssh: connect to host se1.alces.network port 22: Connection refused` upon trying to SSH into my login node
For security, we run a service which temporarily blocks IP addresses after 3 failed login attempts. If you are struggling to login, please try again after 1 hour.

### I can't access my instance from my laptop terminal
* Make sure you are connected to the Alces Cloud VPN (and that your machine has a 10.199.X.X IP address)
* Ensure that you have configured the security groups for your instance correctly (eg. Add the 'ssh access' security group if you wish to ssh into your instance)

