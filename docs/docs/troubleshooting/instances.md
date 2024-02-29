#Instances
One of the basic principles of cloud computing is that things can go wrong. This section is designed to help with common issues you may face when creating and using your instances.

### Connection refused / Connection timed out
When trying to SSH to your instances, you may be faced with a connection error.

```
[user@fedora ~]$ ssh -i MyKeypair.pem rocky@10.199.31.5
ssh: connect to host 10.199.31.5 port 22: Connection refused
```

- Ensure your instance has properly booted by waiting a few minutes and then trying again. You can also use the Alces Cloud dashboard to view the remote console and confirm the instance has correctly booted.
- Ensure that you instance has been configured with the appropriate security groups. Further information on security groups can be found [here](../networking/secgroups.md).

### Host key verification failed
When reusing an existing floating IP that has previously been used for another instance, your ssh client may prevent you from connecting to the new instance with a host key error:

```
[user@fedora ~]$ ssh -i MyKeypair.pem rocky@10.199.31.5
Host key verification failed.
```

To resolve this, remove the previous host key for that floating IP address and then try to connect to your instance again. You may be asked to accept the new host key as well.

```
[user@fedora ~]$ ssh-keygen -R 10.199.31.5
```

### Associate Floating IP - No ports available
When associating a floating IP to an instance, you will be given a dropdown to select the port on the instance to use. If no ports are available, then it is likely the network chosen has not been connected properly.

[<img src="../img/floating-ip-no-ports.png" width="600px" />](img/floating-ip-no-ports.png)

- Check the network being used is connected to a router.
- Check the router the network is connected to is also connected to the Alces Cloud external network.

More information on networks can be found [here](../networking/networks.md).
