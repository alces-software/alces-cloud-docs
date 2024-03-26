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


## Mount attached volume to the instance

Once Volume is attached to the instance, let's login to instance `myinstance` using SSH. There will be additional disk available in the instance `/dev/vdb`.

!!! note
    `/dev/vdb` is used as an example here, but please ensure to verify the device name to avoid accidentally formatting your other volumes.

- To verify if the volume was attached properly, run the command
    ```
    [rocky@myinstance ~]$ sudo fdisk -l
    Disk /dev/vda: 10 GiB, 10737418240 bytes, 20971520 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: gpt
    Disk identifier: 07B147F9-8655-4FB3-8D30-B2F796D32ED8

    Device       Start      End  Sectors  Size Type
    /dev/vda1     2048   204799   202752   99M EFI System
    /dev/vda2   204800  2252799  2048000 1000M Linux filesystem
    /dev/vda3  2252800  2260991     8192    4M PowerPC PReP boot
    /dev/vda4  2260992  2263039     2048    1M BIOS boot
    /dev/vda5  2265088 20969471 18704384  8.9G Linux filesystem


    Disk /dev/vdb: 100 GiB, 107374182400 bytes, 209715200 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    ```


- In order to format device `/dev/vdb`, run the below commands
    ```
    [rocky@myinstance ~]$ sudo fdisk /dev/vdb

    Welcome to fdisk (util-linux 2.37.4).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.

    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x76b921f3.

    Command (m for help): g
    Created a new GPT disklabel (GUID: B95E1AF7-B041-0243-BD25-C43676192090).

    Command (m for help): n
    Partition number (1-128, default 1):
    First sector (2048-209715166, default 2048):
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-209715166, default 209715166):

    Created a new partition 1 of type 'Linux filesystem' and of size 100 GiB.

    Command (m for help): w
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.
    ```

    !!! note 
        For additional details regarding the formatting information for `fdisk`, refer to: 
        ```
        Help:

        GPT
        M   enter protective/hybrid MBR

        Generic
        d   delete a partition
        F   list free unpartitioned space
        l   list known partition types
        n   add a new partition
        p   print the partition table
        t   change a partition type
        v   verify the partition table
        i   print information about a partition

        Misc
        m   print this menu
        x   extra functionality (experts only)

        Script
        I   load disk layout from sfdisk script file
        O   dump disk layout to sfdisk script file

        Save & Exit
        w   write table to disk and exit
        q   quit without saving changes

        Create a new label
        g   create a new empty GPT partition table
        G   create a new empty SGI (IRIX) partition table
        o   create a new empty DOS partition table
        s   create a new empty Sun partition table
        ```

- Create ext4 file system on /dev/vdb disk:
    ```
    [rocky@myinstance ~]$ sudo mkfs -t ext4 /dev/vdb
    mke2fs 1.46.5 (30-Dec-2021)
    Found a gpt partition table in /dev/vdb
    Proceed anyway? (y,N) y
    Creating filesystem with 26214400 4k blocks and 6553600 inodes
    Filesystem UUID: 3e30912b-e49b-4640-acf3-647bc28271ca
    Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (131072 blocks):
    done
    Writing superblocks and filesystem accounting information: done
    ```

- Now mount the device to a folder. here device `/dev/vdb` is mounted at folder `/data`.
    ```
    sudo mkdir /data
    sudo mount /dev/vdb /data
    ```

- Verify that volume is mounted successfully, use command below
    ```
    [rocky@myinstance ~]$ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        4.0M     0  4.0M   0% /dev
    tmpfs           7.7G     0  7.7G   0% /dev/shm
    tmpfs           3.1G  8.6M  3.1G   1% /run
    /dev/vda5       8.9G  1.1G  7.9G  12% /
    /dev/vda2       936M  175M  762M  19% /boot
    /dev/vda1        99M  7.0M   92M   8% /boot/efi
    tmpfs           1.6G     0  1.6G   0% /run/user/1000
    /dev/vdb         98G   24K   93G   1% /data
    ```


## Extend Volume

Sometimes virtual machines need more space to accommodate increasing data, Extending volume is best solution to handle the increasing data.

!!! note
    Before you resize the volume make sure to take care of below conditions.

    - **Detach the Volume:** Resizing only works on unattached volumes. Ensure the volume you want to resize is detached from any instances.
    - **New Size:** The new size must be greater than the current size. Shrinking volumes is not currently supported.

=== "CLI"
    1. List your volumes to identify the volume ID or Name using the following command:
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume list
        +--------------------------------------+----------+-----------+------+-------------+
        | ID                                   | Name     | Status    | Size | Attached to |
        +--------------------------------------+----------+-----------+------+-------------+
        | e2b54ae8-98bc-475a-b644-906f8a46b... | myvolume | available |  100 |             |
        +--------------------------------------+----------+-----------+------+-------------+
        ```
    2. Once you have the volume ID, use the following command to resize the volume:
        ```
        openstack volume set --size <new_size> <volume_id_or_name>
        e.g.
        (openstack) [user@stack01[poc1] ~]$ openstack volume set --size 105 e2b54ae8-98bc-475a-b644-906f8a46b...
        ```
    3. Now Verify the size of volume.
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume list
        +--------------------------------------+----------+-----------+------+-------------+
        | ID                                   | Name     | Status    | Size | Attached to |
        +--------------------------------------+----------+-----------+------+-------------+
        | e2b54ae8-98bc-475a-b644-906f8a46b... | myvolume | available |  105 |             |
        +--------------------------------------+----------+-----------+------+-------------+
        ```




=== "GUI"
    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Volumes` on the left side menu.
    2. On the action dropdown menu for your volume, click `Extend Volume`.
    3. Enter the New Size (in GB) and click `Extend Volume`.

    The GUI will initiate the resize operation and provide you with progress updates.


## Volume Snapshot  
Snapshot capture a specific state of a volume at a given time. They're ideal for quick rollbacks or creating disaster recovery points.

=== "CLI"
    1. List your volumes to identify the volume ID using the following command:
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume list
        +--------------------------------------+----------+-----------+------+-------------+
        | ID                                   | Name     | Status    | Size | Attached to |
        +--------------------------------------+----------+-----------+------+-------------+
        | b3aa108a-e35a-4686-ab33-761dbb653... | myvolume | available |   10 |             |
        +--------------------------------------+----------+-----------+------+-------------+
        ```
    2. Once you have the volume ID or Name, use the following command to create a snapshot:
        ```
        openstack volume snapshot create --volume <volume_id_or_name> --description "<snapshot_description>"  <snapshot_name> 
        e.g. 
        (openstack) [user@stack01[poc1] ~]$ openstack volume snapshot create --volume b3aa108a-e35a-4686-ab33-761dbb653b60 --description "myvolume snapshot"  myvolume-snapshot
        +-------------+--------------------------------------+
        | Field       | Value                                |
        +-------------+--------------------------------------+
        | created_at  | 2024-03-26T11:17:34.366796           |
        | description | myvolume snapshot                    |
        | id          | 8c2635ef-2a04-4ab9-84aa-0ab601dcc... |
        | name        | myvolume-snapshot                    |
        | properties  |                                      |
        | size        | 10                                   |
        | status      | creating                             |
        | updated_at  | None                                 |
        | volume_id   | b3aa108a-e35a-4686-ab33-761dbb653... |
        +-------------+--------------------------------------+
        ```
        <snapshot_name> with a desired name for your snapshot.
        <snapshot_description> with an optional description (in quotes)
        <volume_id_or_name> with the actual ID or Name  of your volume.
    
    3. Now Verify the snapshot of volume `myvolume`.
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume snapshot list
        +--------------------------------------+-------------------+-------------------+-----------+------+
        | ID                                   | Name              | Description       | Status    | Size |
        +--------------------------------------+-------------------+-------------------+-----------+------+
        | 8c2635ef-2a04-4ab9-84aa-0ab601dcc... | myvolume-snapshot | myvolume snapshot | available |   10 |
        +--------------------------------------+-------------------+-------------------+-----------+------+
        ```

=== "GUI"
    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Volumes` on the left side menu.
    2. On the action dropdown menu for your volume, click `Create Snapshot`.
    3. Enter Name for your snapshot and optionally add a Description. and click `Create Volume Snapshot`.

    The GUI will initiate the snapshot operation and provide you with progress updates.


## Volume Backup
Backup create a copy of a volume that can be stored offsite or in a separate system. Backups provide a more robust solution for long-term data preservation and disaster recovery scenarios involving complete system outages.

=== "CLI"
    **Backing Up Volume**

    1. List your volumes to identify the volume ID or Name using the following command:
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume list
        +--------------------------------------+----------+-----------+------+-------------+
        | ID                                   | Name     | Status    | Size | Attached to |
        +--------------------------------------+----------+-----------+------+-------------+
        | b3aa108a-e35a-4686-ab33-761dbb653... | myvolume | available |   10 |             |
        +--------------------------------------+----------+-----------+------+-------------+        
        ```

    2. Once you have the volume ID or Name, use the following command to create a volume backup:
        ```
        openstack volume backup create --name <backup_name> --description "<backup description>" <volume_id_or_name>
        e.g.
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup create --name myvolume-backup --description "myvolume backup" myvolume
        +-----------+--------------------------------------+
        | Field     | Value                                |
        +-----------+--------------------------------------+
        | id        | aa150436-7098-4af5-a5f6-73eeec0de... |
        | name      | myvolume-backup                      |
        | volume_id | b3aa108a-e35a-4686-ab33-761dbb653... |
        +-----------+--------------------------------------+
        ```
    3. Now Verify the backup of volume `myvolume`.
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup list
        +--------------------------------------+-----------------+-----------------+-----------+------+-------------+
        | ID                                   | Name            | Description     | Status    | Size | Incremental |
        +--------------------------------------+-----------------+-----------------+-----------+------+-------------+
        | aa150436-7098-4af5-a5f6-73eeec0de... | myvolume-backup | myvolume backup | available |   10 | False       |
        +--------------------------------------+-----------------+-----------------+-----------+------+-------------+        
        ```
    
    **Backing Up Snapshot**

    1. List your snapshots to identify the snapshot ID or Name using the following command:
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume snapshot list
        +--------------------------------------+-------------------+-------------+-----------+------+
        | ID                                   | Name              | Description | Status    | Size |
        +--------------------------------------+-------------------+-------------+-----------+------+
        | 5dc327db-c1c7-4593-a60e-57508fa4b... | myvolume-snapshot |             | available |   10 |
        +--------------------------------------+-------------------+-------------+-----------+------+        
        ```
    2. Once you have the snapshot ID or Name, use the following command to create a snapshot backup:
        ```
        openstack volume backup create --name <backup_name> --description "<backup description>" --snapshot <snapshot_id_or_name> <volume_id_or_name>
        e.g.
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup create --name backup_name --description "<backup description>" --snapshot  5dc327db-c1c7-4593-a60e-57508fa4b... myvolume
        +-----------+--------------------------------------+
        | Field     | Value                                |
        +-----------+--------------------------------------+
        | id        | 39e2f814-5985-4a85-8963-3168efb99... |
        | name      | backup_name                          |
        | volume_id | b3aa108a-e35a-4686-ab33-761dbb653... |
        +-----------+--------------------------------------+
        ```
    3. Now Verify the backup of volume `myvolume`.
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup list
        +--------------------------------------+-----------------+----------------------+-----------+------+-------------+
        | ID                                   | Name            | Description          | Status    | Size | Incremental |
        +--------------------------------------+-----------------+----------------------+-----------+------+-------------+
        | 39e2f814-5985-4a85-8963-3168efb99... | backup_name     | <backup description> | available |   10 | False       |
        +--------------------------------------+-----------------+----------------------+-----------+------+-------------+
        ```

=== "GUI"
    **Backing Up Volume**

    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Volumes` on the left side menu.
    2. On the action dropdown menu for your volume, click `Create Backup`.
    3. Enter Name for your backup and optionally add a Description. and click `Create Volume Backup`.

    The GUI will initiate the backup operation and provide you with progress updates.

    **Backing Up Snapshot**

    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Snapshots` on the left side menu.
    2. On the action dropdown menu for your snapshot, click `Create Backup`.
    3. Enter Name for your backup, optionally add a Description and optionally add container name. and click `Create Volume Backup`.

    The GUI will initiate the backup operation and provide you with progress updates.


## Volume Restore

=== "CLI"
    1. List your volume backups to identify the volume backup ID or Name using the following command:
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup list
        +--------------------------------------+-----------------+-------------+-----------+------+-------------+
        | ID                                   | Name            | Description | Status    | Size | Incremental |
        +--------------------------------------+-----------------+-------------+-----------+------+-------------+
        | 078d3bc2-cc87-4fe7-b916-551e72f660d4 | myvolume-backup |             | available |   10 | False       |
        +--------------------------------------+-----------------+-------------+-----------+------+-------------+
        ```

    2. Once you have the volume backup ID or Name, use the following command to restore backup to volume:
        ```
        openstack volume backup restore <backup_id_or_name> <volume_id_or_name>

        # <volume_id_or_name>: name or ID for existing volume, name only for new volume
        e.g.
        (openstack) [user@stack01[poc1] ~]$ openstack volume backup restore myvolume-backup new-volume-restore
        ```

    3. Now Verify the backup of volume `myvolume`.
        ```
        (openstack) [user@stack01[poc1] ~]$ openstack volume list
        +--------------------------------------+--------------------+-----------+------+-------------+
        | ID                                   | Name               | Status    | Size | Attached to |
        +--------------------------------------+--------------------+-----------+------+-------------+
        | 0447f7e6-1bb8-4938-8c73-ce21db45184c | new-volume-restore | available |   10 |             |
        | b3aa108a-e35a-4686-ab33-761dbb653b60 | myvolume           | available |   10 |             |
        +--------------------------------------+--------------------+-----------+------+-------------+        
        ```
    
=== "GUI"

    1. On the Alces Cloud dashboard, navigate to `Volume` and then `Backups` on the left side menu.
    2. On the action dropdown menu for your backup, click `Restore Backup`.
    3. You may select create a new volume or select an existing volume to overwrite, then click `Restore Backup to Volume`.

    The GUI will initiate the restore operation and provide you with progress updates.

