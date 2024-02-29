#Creating an instance
## Launching an Instance
To launch your first instance on Alces Cloud you can utilise the Launch Instance wizard.

On the left side bar, navigate to `Compute` and then `Instances`. On the top right, click `Launch Instance` to open the wizard. Choose a suitable name for your instance, then then click `Next`.

[<img src="../img/instance-wizard-1.png" width="800px" />](img/instance-wizard-1.png)

Choose an image from the available list by pressing the up arrow next to the desired image, as well as a suitable volume size. Note: different images may have different requirements - you can view these by pressing the arrow to the left of the image name. Click `Next`.

[<img src="../img/instance-wizard-2.png" width="800px" />](img/instance-wizard-2.png)

Choose the flavor size for your instance. More information on the available flavors can be found [here](../compute/flavors.md). Click `Next`.

[<img src="../img/instance-wizard-3.png" width="800px" />](img/instance-wizard-3.png)

Choose the network to attach your instance to. Your project will come with a default network already configured for you to use. More information on networks can be found [here](../networking/networks.md). 

Ensure the `default` network is selected and then click `Next` twice to move to the `Security Group` section.

[<img src="../img/instance-wizard-4.png" width="800px" />](img/instance-wizard-4.png)

Choose the security groups to apply to your instance. Your project will come with 3 security groups by default. More information on security groups can be found [here](../networking/secgroups.md).

Select the `default` and `allow-ssh` security groups by pressing the up arrow next to these, then click `Next`.

[<img src="../img/instance-wizard-5.png" width="800px" />](img/instance-wizard-5.png)

Ensure the key pair you created earlier is selected, and then click on `Launch Instance` to launch it.

[<img src="../img/instance-wizard-6.png" width="800px" />](img/instance-wizard-6.png)

Launching your instance may take a few moments, after which it will be shown in the `Running` state on your `Instances` page.

##Connecting to your Instance

In order to connect to your instance, you will need to associate a floating IP with the instance. Click the drop down arrow to the right of your instance and then select `Associate Floating IP`.

[<img src="../img/instance-wizard-7.png" width="800px" />](img/instance-wizard-7.png)

To allocate a new floating IP to your project, press the `+` button and then `Allocate IP`.

[<img src="../img/instance-wizard-8.png" width="600px" />](img/instance-wizard-8.png)

Ensure the IP that was just allocated is selected, and then select your instance from the drop down menu. Click `Associate` to then associate the IP.

[<img src="../img/instance-wizard-9.png" width="600px" />](img/instance-wizard-9.png)

You should now be able to SSH to your instance using floating IP and the `.pem` file downloaded earlier. This can be done using the `ssh` command on most Linux systems. Different images may require you to login as a different user - more information on the standard Alces images can be found [here](../compute/images.md).

You may also be asked to accept the instance host key the first time you connect to a new instance. Type `yes` here to continue.

```
[user@fedora ~]$ ssh -i MyKeypair.pem rocky@10.199.31.5
The authenticity of host '10.199.31.5 (10.199.31.5)' can't be established.
ED25519 key fingerprint is SHA256:87OxF7SkU/D6jXbY9XmwElnME9fK6qUaBVlTDlbVnDQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.199.31.5' (ED25519) to the list of known hosts.
[rocky@myinstance ~]$ 
```
