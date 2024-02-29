# Accessing Alces Cloud
This documentation provides guidance on how to access Alces Cloud via the secure VPN and login node services.

## Login Node
The Alces Cloud service provides an interactive login node for users who prefer to use the command line instead of the dashboard to manage their instances. This is also used for your initial onboarding process.

The login node can be accessed via SSH from a standard SSH client using your username, password and a valid token from your authenticator app.

```
[user@fedora ~]$ ssh myuser@se1.alces.network
(myuser@se1.alces.network) Password: 
(myuser@se1.alces.network Verification code: 
Last login: Thu Jan 25 15:33:51 2024 from 213.83.69.6
 -[ alces flight ]- 
Welcome to poc1, based on Rocky Linux 9.2.
```

## Secure VPN
The Alces Cloud service also provides a secure VPN in order for you to access your instances via their floating IP addresses. To connect to this, SSH to the login node as above and copy the `cloudvpn.conf` file available in your home directory to your local machine. This can be done using the `scp` command.

```
[user@fedora ~]$ scp myuser@se1.alces.network:~/cloudvpn.conf ~/
(myuser@se1.alces.network) Password: 
(myuser@se1.alces.network) Verification code: 
```

Once this has been copied to your local machine, use a suitable VPN client to connect to the service. This can be done using the `openvpn` command on most Linux systems:

```
[user@fedora ~]$ sudo openvpn --config cloudvpn.conf 
Enter Auth Username: myuser
Enter Auth Password:
CHALLENGE: Enter 2FA Authenticator code:
```

Once connected, ensure you can reach the Alces Cloud dashboard. You can do this either using your browser, or by using the `ping` command.

```
[user@fedora ~]$ ping cloud.se1.alces.network
PING cloud.se1.alces.network (10.199.0.6) 56(84) bytes of data.
64 bytes from 10.199.0.6: icmp_seq=1 ttl=63 time=44.1 ms
```

## Openstack CLI
A supported version of the Openstack CLI is available on the cloud login node.

Once you are logged in, an `openrc` file will be available in your home directory that will setup the required authentication for your SSH session. Source the file and enter your password and a valid token from your authenticator app. For security, this is only valid for 24 hours and does not persist after you disconnect from your SSH session.

```
(openstack) [myuser@stack01[poc1] ~]$ source openrc
Please enter your password: 
Passcode: 
(openstack) [myuser@stack01[poc1] ~]$ openstack network list
+--------------------------------------+-----------+--------------------------------------+
| ID                                   | Name      | Subnets                              |
+--------------------------------------+-----------+--------------------------------------+
| c681d94b-e2ec-4b73-89bf-9943bcce3255 | external1 | 8a1961f9-030c-419e-9bf0-66875031073f |
| d5732f52-84ed-4a9f-a73e-11a2938ac7c2 | default   | dc7f9f3f-61d7-44a0-b1ef-db3a554a461f |
+--------------------------------------+-----------+--------------------------------------+
```

[Next](dashboard.md){: .md-banner__button}

