#Volumes
A volume acts as the virtual hard drive for your instances. All instances are required to be booted from a volume and additional volumes can be attached to your instances if you require additional disks.

Available volume types:

| Volume Type | I/O | IOPs |
|---|---|---|
| hdd.1 | 20 MB/s Read + Write | 60 IOPs Read + Write |

## Create a new volume

Volume creation is useful for attaching a second data volume to your instance.

=== "CLI"

    Create a new volume by specifying a suitable size and a name.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack volume create --size 100 myvolume
    +---------------------+--------------------------------------+
    | Field               | Value                                |
    +---------------------+--------------------------------------+
    | name                | myvolume                             |
    | size                | 100                                  |
    | status              | creating                             |
    | type                | hdd.1                                |
    +---------------------+--------------------------------------+
    ```

    After a few moments, the volume should change to the `available` state.
    
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack volume list
    +--------------------------------------+----------+-----------+------+-------------+
    | ID                                   | Name     | Status    | Size | Attached to |
    +--------------------------------------+----------+-----------+------+-------------+
    | e2b54ae8-98bc-475a-b644-906f8a46b1b2 | myvolume | available |  100 |             |
    +--------------------------------------+----------+-----------+------+-------------+
    ```

=== "GUI"

    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Volumes` on the left side menu.
    2. Click the `Create Volume` button.
    3. Enter a volume name and size.
    4. Click create volume.

## Attach a volume to an instance

=== "CLI"

    Attach the desired volume to your instance.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack server add volume myinstance myvolume
    +-----------------------+--------------------------------------+
    | Field                 | Value                                |
    +-----------------------+--------------------------------------+
    | Server ID             | b98a496d-13f7-4ce0-8a38-87e87fe56c2a |
    | Volume ID             | e2b54ae8-98bc-475a-b644-906f8a46b1b2 |
    | Device                | /dev/vdb                             |
    +-----------------------+--------------------------------------+
    ```

=== "GUI"

    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Volumes` on the left side menu.
    2. On the action dropdown menu for your volume, click `Manage Attachments`.
    3. Select the instance to attach the volume to, and then press `Attach Volume`.

Once you have attached your volume, login to your instance to format and mount the volume as required.
