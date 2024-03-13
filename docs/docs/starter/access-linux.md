# Accessing Alces Cloud - Linux/MacOS
This documentation provides guidance on how to access Alces Cloud, currently there are two routes to accessing Alces Cloud depending on your usage preferences. Users wishing to access the Alces Cloud via the command line should follow instructions under the "CLI" tab and users wishing to access the Alces Cloud via a GUI (Openstack Horizon Dashboard) should follow instructions under the "GUI" tab.

## First Time Login
All new users will be required to update their initial password before using the service.


From a command line :
```
[user@fedora ~]$ ssh myuser@se1.alces.network
```
This will then prompt for your password (as provided in your onboarding email) and your "Verification Code" (your MFA code from your TOTP authentication app). Once you have correctly provided these, the "login node" will prompt you to update your initial password.

With your password configured, you are now able to access the Alces Cloud service via the following methods:

=== "CLI"
    The Alces Cloud service provides an interactive login node for users who prefer to use the command line instead of the dashboard to manage their instances.

    From a command line:
    ```
    [user@fedora ~]$ ssh myuser@se1.alces.network
    (myuser@se1.alces.network) Password:
    (myuser@se1.alces.network Verification code:
    Last login: Thu Jan 25 15:33:51 2024 from 213.83.69.6
    -[ alces flight ]-
    Welcome to poc1, based on Rocky Linux 9.2.
    ```

    Once connected to the interactive login node (`stack01`) you will able to use the Openstack CLI using our supported version of the CLI.

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


=== "GUI"
    For users wishing to access Alces Cloud via a GUI (using Openstack Horizon), users must first connect to the Alces Cloud VPN service.

    ## Secure VPN
    To connect to this, SSH to the login node as above and copy the `cloudvpn.conf` file available in your home directory to your local machine. This can be done using the `scp` command.

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

    ##Using the Dashboard
    Once connected to the secure VPN, you will be able to access the [Alces Cloud Dashboard](https://cloud.se1.alces.network/) on a browser. This will take you to a login page and enter your username and password.
    
    [<img src="../img/login.png" width="800px" />](img/login.png)
    
    Once you have logged in successfully, you'll be presented with the overview screen.
    
    [<img src="../img/overview.png" width="800px" />](img/overview.png)
