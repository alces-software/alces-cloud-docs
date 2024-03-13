#Creating your first Instance
## Launching an Instance

With access to Alces Cloud sorted - let's get started launching some instances!

=== "CLI"
    To launch an instance from the command line, you will first need to identify which image you wish to use. Make a note of the relevant ID to use later on.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+----------------------+--------+
    | ID                                   | Name                 | Status |
    +--------------------------------------+----------------------+--------+
    | b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Rocky 9.3            | active |
    | 8711c1da-71f8-4c09-85bd-6efdc2346188 | Ubuntu 22.04.3 (LTS) | active |
    +--------------------------------------+----------------------+--------+
    ```

    Identify the flavor you wish to use and make a note of the name. More information on the available flavors can be found [here](../compute/flavors.md).
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack flavor list
    +--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+
    | ID                                   | Name                            |   RAM | Disk | Ephemeral | VCPUs | Is Public |
    +--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+
    | 26b933e3-4fde-4e10-9556-16b2e470cadf | c1.small                        | 49152 |  150 |       700 |     6 | False     |
    | d538bd2d-10ce-4555-8d1d-46535ee815ee | c1.large                        | 98304 |  300 |      1400 |    12 | False     |
    +--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+ 
    ```

    Identify the network to attach your instance to and make a note of the name. Your project will come with a default network already configured for you to use. More information on networks can be found [here](../networking/networks.md).
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack network list --internal
    +--------------------------------------+---------------------+--------------------------------------+
    | ID                                   | Name                | Subnets                              |
    +--------------------------------------+---------------------+--------------------------------------+
    | cff8f325-db1b-4812-974a-9bee346a84bd | my-project--default | 920ba89b-4f27-4c35-8862-9d457bc6a677 |
    +--------------------------------------+---------------------+--------------------------------------+
    ```

    Identify the key pair you will use to access the instance and make a note of the name. For more information on creating a key pair, see  [Creating an SSH Key](../starter/key.md).
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack keypair list
    +-----------+-------------------------------------------------+------+
    | Name      | Fingerprint                                     | Type |
    +-----------+-------------------------------------------------+------+
    | mykeypair | a1:b2:c3:d4:e5:f6:g7:h8:i9:j0:k1:l2:m3:n4:o5:p6 | ssh  |
    +-----------+-------------------------------------------------+------+
    ```

    Launch your instance using the image ID, and flavor, network and key pair name previously selected. As there are quite a few parameters, we've put a description of each below.
    ```
    openstack server create
    --flavor: The name or ID of the flavor you wish to use.
    --image: The name of ID of the image you wish to use.
    --boot-from-volume: The size disk you wish to create for the instance.
    --network: The name or ID of the network to attach the instance to.
    --key-name: The name of the key pair you wish to use to connect to the instance.
    --security-group: The name or ID of the security group to assign to this instance (repeat option to set multiple groups).
    --wait: Wait for the instance to be in the `ACTIVE` state.
    <name>: The name of your instance.
    ```

    In our example, we use the following command to create our instance and assign the default security groups in order to allow SSH access. The command may take a few minutes to run, and will return a table with your instance details once it has been created.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack server create --flavor c1.small --image b7e0d27f-1266-4069-a9b8-fb8a71f25421 --boot-from-volume 100 --network my-project-default --key-name mykeypair --security-group default --security-group allow-ssh --wait myinstance
    +-----------------------------+----------------------------------------------------------+
    | Field                       | Value                                                    |
    +-----------------------------+----------------------------------------------------------+
    | flavor                      | c1.small (26b933e3-4fde-4e10-9556-16b2e470cadf)          |
    | name                        | myinstance                                               |
    | status                      | ACTIVE                                                   |
    +-----------------------------+----------------------------------------------------------+
    ```

    In order to connect to your instance, you will now need to associate a floating IP with the instance. First, allocate a new floating IP to your project and make a note of the ID.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack floating ip create external1
    +---------------------+--------------------------------------+
    | Field               | Value                                |
    +---------------------+--------------------------------------+
    | floating_ip_address | 10.199.31.8                          |
    | id                  | 9968df09-178d-48d5-a6b9-f82e996a2f4a |
    +---------------------+--------------------------------------+
    ```

    Assign this floating IP to your instance.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack server add floating ip myinstance 9968df09-178d-48d5-a6b9-f82e996a2f4a
    ```

    You should now be able to SSH to your instance using this floating IP and the SSH key created earlier. Different images may require you to login as a different user - more information on the standard Alces images can be found [here](../compute/images.md).

    You may also be asked to accept the instance host key the first time you connect to a new instance. Type `yes` here to continue.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ ssh -i ~/.ssh/mykeypair rocky@10.199.31.8
    The authenticity of host '10.199.31.8 (10.199.31.8)' can't be established.
    ED25519 key fingerprint is SHA256:87OxF7SkU/D6jXbY9XmwElnME9fK6qUaBVlTDlbVnDQ.
    This key is not known by any other names
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '10.199.31.8' (ED25519) to the list of known hosts.
    [rocky@myinstance ~]$
    ```

=== "GUI"
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

    In order to connect to your instance, you will need to associate a floating IP with the instance. Click the drop down arrow to the right of your instance and then select `Associate Floating IP`.

    [<img src="../img/instance-wizard-7.png" width="800px" />](img/instance-wizard-7.png)

    To allocate a new floating IP to your project, press the `+` button and then `Allocate IP`.

    [<img src="../img/instance-wizard-8.png" width="600px" />](img/instance-wizard-8.png)

    Ensure the IP that was just allocated is selected, and then select your instance from the drop down menu. Click `Associate` to then associate the IP.

    [<img src="../img/instance-wizard-9.png" width="600px" />](img/instance-wizard-9.png)

    You should now be able to SSH to your instance using this floating IP and the `.pem` file downloaded earlier. This can be done using the `ssh` command on most Linux systems. Different images may require you to login as a different user - more information on the standard Alces images can be found [here](../compute/images.md).
