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


## Various Operation on Instance

A user can observe various operation on instance like stop, start, pause, suspend, reboot, shutdown etc.

=== "CLI"
    | Operation                      | Description                                                                                                                                                                                                   | Command                         |
    |--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
    | Stop an instance               | Stops the instance.                                                                                                                                                                                           | `openstack server stop <server_uuid/name>`        |
    | Start an instance              | Starts a stopped instance.                                                                                                                                                                                    | `openstack server start <server_uuid/name>`       |
    | Pause a running instance       | Immediately pause a running instance, storing its state in memory (RAM) in a frozen state. No confirmation is needed.                                                                                       | `openstack server pause <server_uuid/name>`       |
    | Resume a paused instance       | Immediately resumes a paused instance. No confirmation is needed.                                                                                                                                             | `openstack server unpause <server_uuid/name>`     |
    | Suspend a running instance     | Immediately suspends a running instance, storing its state on the instance disk. No confirmation is needed.                                                                                                  | `openstack server suspend <server_uuid/name>`     |
    | Resume a suspended instance   | Immediately resumes a suspended instance. No confirmation is needed.                                                                                                                                          | `openstack server resume  <server_uuid/name>`      |
    | Delete an instance             | Permanently destroys the instance. No confirmation is needed.                                                                                                                                                 | `openstack server delete  <server_uuid/name>`      |
    | Edit instance metadata        | Use instance metadata to specify instance properties.                                                                                                                                                         | `openstack server set --property <key=value> [--property <key=value>]   <server_uuid/name>` |
    | Add security groups            | Adds specified security groups to the instance.                                                                                                                                                               | `openstack server add security group <server_uuid/name>` |
    | Remove security groups         | Removes specified security groups from the instance.                                                                                                                                                          | `openstack remove security group <server_uuid/name>` |
    | Rescue an instance             | Puts an instance in rescue mode, allowing system repair and data recovery.                                                                                                                                    | `openstack server rescue <server_uuid/name>`      |
    | Restore a rescued instance     | Reboots the rescued instance.                                                                                                                                                                                 | `openstack server unrescue <server_uuid/name>`    |
    | View instance logs             | Displays the most recent section of the instance console log.                                                                                                                                                  | `openstack console log show <server_uuid/name>`   |
    | Shelve an instance             | Retains instance data and resource allocations, but clears instance memory. Shelved instances are moved to SHELVED_OFFLOADED state. No confirmation is needed.                                               | `openstack server shelve <server_uuid/name>`      |
    | Unshelve an instance           | Restores the instance using the shelved instance's disk image.                                                                                                                                                | `openstack server unshelve <server_uuid/name>`    |
    | Lock an instance               | Prevents non-admin users from executing actions on the instance.                                                                                                                                              | `openstack server lock <server_uuid/name>`        |
    | Unlock an instance             | Unlocks a previously locked instance.                                                                                                                                                                         | `openstack server unlock <server_uuid/name>`      |
    | Soft reboot an instance        | Gracefully stops and restarts the instance.                                                                                                                                                                   | `openstack server reboot --soft  <server_uuid/name>` |
    | Hard reboot an instance        | Stops and restarts the instance by shutting down its power.                                                                                                                                                   | `openstack server reboot --hard  <server_uuid/name>` |
    | Rebuild an instance            | Rebuilds the instance using new image and disk-partition options. Useful for OS issues.                                                                                                                      | `openstack server rebuild <server_uuid/name>`     |


=== "GUI"

    - From the Alces Cloud dashboard, click `Compute` and then `Instances`:
    - Click on the dropdown menu within the "Actions" column corresponding to the specific instance requiring the operation.

    [<img src="../img/alces_cloud_win12.png" width="800px" />](img/alces_cloud_win12.png)

## Resizing an instance

If you wanted to increase or decrease the memory or CPU count of the instance you can use resize operation. While resizing the instance, select the new flavor for instance that matches the requirement. Resize will perform rebuild and restart of the instance.

!!! note
    To resize an instance you will need sufficient quota and resource. Existing usage is not released until after the resize is complete. Resizing an instance with local root or ephemeral storage will also take considerable time and is not recommended.

=== "CLI"
    -  Fetch the name and ID of instances.
    ```
    openstack server list
    ```

    -  Fetch the name and ID of flavors.
    ```
    openstack flavor list
    ```
    - Resize the instance.
    ```
    openstack server resize --flavor <flavor_name_or_id> --wait <instance_name_or_id>
    ```

    !!! note
        Restarting of instance may some time, instance is first powered off and then resize take place. During this time status of instance is `RESIZE`.
        
        ```
        openstack server list
        +----------------------+----------------+--------+------------------------------------+
        | ID                   | Name           | Status | Networks                           |
        +----------------------+----------------+--------+------------------------------------+
        | 67bc9a9a-5928-47c... | myCirrosServer | RESIZE | admin_internal_net=192.168.111.139 |
        +----------------------+----------------+--------+------------------------------------+
        ```

    - When resize is complete, the status of instance will be changed to `VERIFY_RESIZE`, you can confirm or revert the resize.
        - Confirm the resize.
        ```
        openstack server resize confirm <instance_name_or_id>
        ```
        - Revert the resize.
        ```
        openstack server resize revert <instance_name_or_id>
        ```

    In the alces cloud we can perform resize of the instance by following the example below
    ```
    # Fetch all the instances
    (openstack) [myuser@stack01[poc1] ~]$ openstack server list
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | ID                                   | Name     | Status | Networks                          | Image                    | Flavor   |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | 7d611d7a-0595-4c4e-86e4-d536... | cirrosvm | ACTIVE | alces-testing-default=172.16.0.66 | N/A (booted from volume) | c1.small |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+

    # Fetch all the flavors 
    (openstack) [myuser@stack01[poc1] ~]$ openstack flavor list
    +--------------------------------------+----------+-------+------+-----------+-------+-----------+
    | ID                                   | Name     |   RAM | Disk | Ephemeral | VCPUs | Is Public |
    +--------------------------------------+----------+-------+------+-----------+-------+-----------+
    | 26b933e3-4fde-4e10-9556-16b2e... | c1.small | 49152 |  150 |       700 |     6 | False     |
    | d538bd2d-10ce-4555-8d1d-46535... | c1.large | 98304 |  300 |      1400 |    12 | False     |
    +--------------------------------------+----------+-------+------+-----------+-------+-----------+

    # Now pass the instance ID that we wanted to resize and then select the flavor that we want for our instance, here cirrosvm is selected 
    # to get resized from c1.small to c1.large.
    (openstack) [myuser@stack01[poc1] ~]$  openstack server resize --flavor d538bd2d-10ce-4555-8d1d-46535... --wait 7d611d7a-0595-4c4e-86e4-d536...

    # Now the cirrosvm status is changed to RESIZE from ACTIVE.
    (openstack) [myuser@stack01[poc1] ~]$ openstack server list
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | ID                                   | Name     | Status | Networks                          | Image                    | Flavor   |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | 7d611d7a-0595-4c4e-86e4-d536... | cirrosvm | RESIZE | alces-testing-default=172.16.0.66 | N/A (booted from volume) | c1.small |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+

    # When the resize is complete then status of cirrosvm is changed to VERIFY_RESIZE, next step is to approve/cancel the resize.
    (openstack) [myuser@stack01[poc1] ~]$ openstack server list
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+-----------------+
    | ID                                   | Name     | Status | Networks                          | Image                    | Flavor          |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+-----------------+
    | 7d611d7a-0595-4c4e-86e4-d536... | cirrosvm | VERIFY_RESIZE | alces-testing-default=172.16.0.66 | N/A (booted from volume) | c1.small |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+-----------------+

    # Resize of cirrosvm is approved, then status is changed from VERIFY_RESIZE to ACTIVE
    openstack server resize confirm 7d611d7a-0595-4c4e-86e4-d536...

    (openstack) [myuser@stack01[poc1] ~]$ openstack server list
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | ID                                   | Name     | Status | Networks                          | Image                    | Flavor   |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    | 7d611d7a-0595-4c4e-86e4-d536... | cirrosvm | ACTIVE | alces-testing-default=172.16.0.66 | N/A (booted from volume) | c1.large |
    +--------------------------------------+----------+--------+-----------------------------------+--------------------------+----------+
    ```

=== "GUI"
    - From the Alces Cloud dashboard, click `Compute` and then `Instances`:
    - Click on the dropdown menu within the "Actions" column then click on `Resize Instance`.

        [<img src="../img/alces_cloud_win12.png" width="800px" />](img/alces_cloud_win12.png)

    - Choose a new flavor and click on the “Resize” button.
    
        [<img src="../img/alces_cloud_win14.png" width="800px" />](img/alces_cloud_win13.png)

    - Resizing will require some time to process, and once completed, you will be prompted for approval or to revert the changes. Simply click "Approve" to confirm the resizing, or select "Revert" from the dropdown menu if you wish to undo the changes.
    
        [<img src="../img/alces_cloud_win13.png" width="800px" />](img/alces_cloud_win14.png)

## Create an instance snapshot

Instance snapshot is a image that captures the state of the running instance disk. You can take snapshot of an instance and store it as a image, further that image can be used as a template to create new instances. Snapshots enable us to create new instances from another instances. If we wanted to preserve the state of instance for later use snapshot is good option to use.

=== "CLI"
    - Scan all the servers and choose either the UUID or the name of the instance for which you intend to create a snapshot.
        ```
        openstack server list
        ```
    - Create snapshot of the instance using below command, replace <image_name> with a name for the new snapshot image and replace <instance> with the name or ID of the instance that you want to create the snapshot from.
        ```
        openstack server image create --name <image_name> <instance_name_or_uuid>
        ```

    In the below example we are creating snapshot with name `alces-docs-snap` of the instance names `alces-docs`.

    Fetch all the instances and image in the Alces cloud.
    ```

    (openstack) [user@stack01[poc1] ~]$ openstack server list
    +--------------------------------------+------------+--------+-------------------------------------------------+--------------------------+-----------+
    | ID                                   | Name       | Status | Networks                                        | Image                    | Flavor    |
    +--------------------------------------+------------+--------+-------------------------------------------------+--------------------------+-----------+
    | ea89d5b1-52c3-4d52-b6e4-1fc288947... | alces-docs | ACTIVE | engineering-default=10.151.17.115, 172.16.0.182 | N/A (booted from volume) | m1.medium |
    +--------------------------------------+------------+--------+-------------------------------------------------+--------------------------+-----------+

    (openstack) [user@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+--------------------+--------+
    | ID                                   | Name               | Status |
    +--------------------------------------+--------------------+--------+
    | 804716e7-257d-4975-8ae5-bead93a2e... | CirrOS             | active |
    | 8fdd6edc-e876-4a7c-b623-37d6555d0... | Flight Solo 2024.1 | active |
    | 15c14506-11e7-4294-af5e-abff71f63... | Rocky 9.3          | active |
    +--------------------------------------+--------------------+--------+
    ```

    Now Creating Snapshot of instance `alces-docs` with name `alces-docs-snap`.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack server image create --name alces-docs-snap alces-docs
    +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | Field            | Value                                                                                                                                                                                           |
    +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | checksum         | d41d8cd98f00b204e9800998ecf84...                                                                                                                                                                |
    | container_format | bare                                                                                                                                                                                            |
    | created_at       | 2024-03-19T09:49:54Z                                                                                                                                                                            |
    | disk_format      | raw                                                                                                                                                                                             |
    | file             | /v2/images/45ebfe87-9920-40c1-8177-54ecd7692.../file                                                                                                                                            |
    | id               | 45ebfe87-9920-40c1-8177-54ecd7692...                                                                                                                                                            |
    | min_disk         | 10                                                                                                                                                                                              |
    | min_ram          | 0                                                                                                                                                                                               |
    | name             | alces-docs-snap                                                                                                                                                                                 |
    | owner            | ce5b5d47a47348d59dd3bdcd21267...                                                                                                                                                                |
    | properties       | base_image_ref='', bdm_v2='True', block_device_mapping='[{"tag": null, "encryption_format": null, "image_id": null, "volume_id": null, "device_type": "disk", "destination_type": "volume",     |
    |                  | "volume_type": null, "device_name": "/dev/vda", "volume_size": 10, "encryption_options": null, "no_device": null, "source_type": "snapshot", "boot_index": 0, "guest_format": null,             |
    |                  | "encryption_secret_uuid": null, "delete_on_termination": true, "snapshot_id": "215bf45f-26c6-4516-8d99-4586dacfd9c1", "encrypted": null, "disk_bus": "virtio"}]', boot_roles='member,reader',   |
    |                  | direct_url='rbd://e4f759f8-da56-11ee-a031-525400dffa1c/hdd/45ebfe87-9920-40c1-8177-54ecd7692.../snap', hw_cdrom_bus='ide', hw_disk_bus='virtio', hw_input_bus='usb', hw_machine_type='pc',      |
    |                  | hw_pointer_model='usbtablet', hw_video_model='virtio', hw_vif_model='virtio', os_hash_algo='sha512',                                                                                            |
    |                  | os_hash_value='cf83e1357eefb8bdf1542850d66d8007d620e4050b5715dc83f4a921d36ce9ce47d0d13c5d85f2b0ff8318d2877eec2f63b931bd47417a81a538327af927da3e', os_hidden='False',                            |
    |                  | owner_project_name='engineering', owner_specified.openstack.md5='', owner_specified.openstack.object='images/Rocky 9.3', owner_specified.openstack.sha256='', owner_user_name='shubham.dang',   |
    |                  | root_device_name='/dev/vda', stores='rbd'                                                                                                                                                       |
    | protected        | False                                                                                                                                                                                           |
    | schema           | /v2/schemas/image                                                                                                                                                                               |
    | size             | 0                                                                                                                                                                                               |
    | status           | active                                                                                                                                                                                          |
    | tags             |                                                                                                                                                                                                 |
    | updated_at       | 2024-03-19T09:49:55Z                                                                                                                                                                            |
    | visibility       | private                                                                                                                                                                                         |
    +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    ```

    Now we can verifying the snapshot `alces-docs-snap` in image list, later this image will be used to create instances.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+--------------------+--------+
    | ID                                   | Name               | Status |
    +--------------------------------------+--------------------+--------+
    | 804716e7-257d-4975-8ae5-bead93a2e... | CirrOS             | active |
    | 8fdd6edc-e876-4a7c-b623-37d6555d0... | Flight Solo 2024.1 | active |
    | 15c14506-11e7-4294-af5e-abff71f63... | Rocky 9.3          | active |
    | 45ebfe87-9920-40c1-8177-54ecd7692... | alces-docs-snap    | active |
    +--------------------------------------+--------------------+--------+
    ```

=== "GUI"

    - From the Alces Cloud dashboard, click `Compute` and then `Instances`:
    - Click on the dropdown menu within the "Actions" column then click on `Create Snapshot`.

        [<img src="../img/alces_cloud_win17.png" width="800px" />](img/alces_cloud_win17.png)

    - Enter the Snapshot Name and click on the `Create Snapshot` button.

        [<img src="../img/alces_cloud_win16.png" width="800px" />](img/alces_cloud_win16.png)

    - Snapshot Creation will take some time, In order to check image click `Compute` and then `Images`.

        [<img src="../img/alces_cloud_win15.png" width="800px" />](img/alces_cloud_win15.png)
