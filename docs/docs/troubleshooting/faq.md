#Frequently Asked Questions

Please see below for some of the most common issues that our users have (and solutions!)

## How do I connect to my instances?
Access to your instances is provided via floating IP addresses and the secure VPN service. Click [here](../starter/index.md) for our Getting Started guide.

## I'm get the message `ssh: connect to host se1.alces.network port 22: Connection refused` upon trying to SSH into my login node
For security, we run a service which temporarily blocks IP addresses after 3 failed login attempts. If you are struggling to login, please try again after 1 hour.

## I can't access my instance from my laptop terminal
* Make sure you are connected to the Alces Cloud VPN (and that your machine has a 10.199.X.X IP address)
* Ensure that you have configured the security groups for your instance correctly (eg. Add the 'ssh access' security group if you wish to ssh into your instance)

## My disk speeds aren't performing as well as I'd expect
We offer a number of different storage options on Alces Cloud - utilising the 'ephemeral disk' available with your flavor selection will offer greater performance

## What types of nodes are currently available through Alces Cloud?
Alces Cloud offers exclusive access to the CPU cores on the instances that you create, backed by physical resource, ensuring that you get the best performance per core possible

## I have available compute quota but I'm getting an error launching my instances
 
