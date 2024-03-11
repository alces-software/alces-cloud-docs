# Accessing Alces Cloud - Windows
This documentation provides guidance on how to access Alces Cloud, currently there are two routes to accessing Alces Cloud depending on your usage preferences. Users wishing to access the Alces Cloud via the command line should follow instructions under the "CLI" tab and users wishing to access the Alces Cloud via a GUI (Openstack Horizon Dashboard) should follow instructions under the "GUI" tab.

## First Time Login
All new users will be required to update their initial password before using the service.

From your installed terminal such as putty, console emulator, etc, all new users will be required to update their initial password before using the service.

[<img src="../img/access_cloud_win4.png" width="800px" />](img/access_cloud_win4.png)

This will then prompt for your password (as provided in your onboarding email) and your "Verification Code" (your MFA code from your TOTP authentication app). Once you have correctly provided these, the "login node" will prompt you to update your initial password.

With your password configured, you are now able to access the Alces Cloud service via the following methods:


=== "CLI"
    The Alces Cloud service provides an interactive login node for users who prefer to use the command line instead of the dashboard to manage their instances. This is also used for your initial onboarding process.

    [<img src="../img/access_cloud_win5.png" width="800px" />](img/access_cloud_win5.png)

    Once connected to the interactive login node (`stack01`) you will able to use the Openstack CLI using our supported version of the CLI.

    Once you are logged in, an `openrc` file will be available in your home directory that will setup the required authentication for your SSH session. Source the file and enter your password and a valid token from your authenticator app.

    For security, this is only valid for 24 hours and does not persist after you disconnect from your SSH session.

    [<img src="../img/access_cloud_win6.png" width="800px" />](img/access_cloud_win6.png)

=== "GUI"
    For users wishing to access Alces Cloud via a GUI (using Openstack Horizon) then we provide a secure VPN in order for you to access this service.

        
    ## Secure VPN
    To connect to this, SSH to the login node as above and copy the `cloudvpn.conf` file available in your home directory to your local machine. This can be done using the `scp` command.

    [<img src="../img/access_cloud_win8.png" width="800px" />](img/access_cloud_win8.png)

    If you then check your Downloads you should see the CONF file which you will need to import into a suitable VPN Client.

    [<img src="../img/access_cloud_win9.png" width="800px" />](img/access_cloud_win9.png)

    Once this has been copied to your local machine, use a suitable VPN client to connect to the service. This can be done using the OpenVPN client application, where you will need to move the CONF file into your config and change to a .ovpn.

    [<img src="../img/access_cloud_win10.png" width="350px" />](img/access_cloud_win10.png)

    Then check your OpenVPN client where you should see 'cloudvpn' as an option.

    [<img src="../img/access_cloud_win11.png" width="400px" />](img/access_cloud_win11.png)

    To connect through you will then need to use your username, password and your 2FA authenticator code.

## Using the Dashboard
Once connected to the secure VPN, you will be able to access the [Alces Cloud Dashboard](https://cloud.se1.alces.network/) on a browser. This will take you to a login page and enter your username and password.
    
[<img src="../img/login.png" width="800px" />](img/login.png)
    
Once you have logged in successfully, you'll be presented with the overview screen.
    
[<img src="../img/overview.png" width="800px" />](img/overview.png)
