#Images
Alces Cloud includes a number of public images for common operating systems. Users are also able to upload their own images if required.

The following images are currently available on Alces Cloud:

| Name | Description | Default User |
|---|---|---|
| Rocky 9.3 | Rocky Linux release 9.3 (Blue Onyx) | rocky |
| Ubuntu 22.04.3 (LTS) | Ubuntu 22.04.3 (Long Term Support) | ubuntu |


## Upload Images
You can upload the image to Alces cloud.

!!!note
    Only images in RAW format are supported. You can convert images to this format using the `qemu-img convert` command.

=== "CLI"
    From the Alces Cloud login node, you can use the `openstack` command:

    ```
    usage: openstack image create [-h] [-f {json,shell,table,value,yaml}] [-c COLUMN] [--noindent]
                              [--prefix PREFIX] [--max-width <integer>] [--fit-width] [--print-empty]
                              [--id <id>] [--container-format <container-format>]
                              [--disk-format <disk-format>] [--min-disk <disk-gb>] [--min-ram <ram-mb>]
                              [--file <file> | --volume <volume>] [--force] [--progress]
                              [--sign-key-path <sign-key-path>] [--sign-cert-id <sign-cert-id>]
                              [--protected | --unprotected]
                              [--public | --private | --community | --shared] [--property <key=value>]
                              [--tag <tag>] [--project <project>] [--import]
                              [--project-domain <project-domain>]
                              <image-name>
    # For more information about the parameters, run command "openstack image create --help".
    ```

    To upload an image into Alces Cloud, the image needs to first exist on your machine, here we are following an example to upload the cirros image in Alces cloud.

    - Download the cirros Image, if not present on your local machine.
        ```
        wget https://download.cirros-cloud.net/0.5.2/cirros-0.5.2-x86_64-disk.img
        ```
    - Convert the image to RAW
        ```
        qemu-img convert cirros-0.5.2-x86_64-disk.img cirros-0.5.2-x86_64-disk.raw
        ```

    - Upload the image into Alces Cloud
        ```bash
        (openstack) [user@stack01[poc1] ~]$] ~]$ openstack image create cirros --container-format bare --disk-format raw --file cirros-0.5.2-x86_64-disk.raw
        +------------------+--------------------------------------------------------------------------------------+
        | Field            | Value                                                                                |
        +------------------+--------------------------------------------------------------------------------------+
        | container_format | bare                                                                                 |
        | created_at       | 2024-03-13T09:23:27Z                                                                 |
        | disk_format      | raw                                                                                  |
        | file             | /v2/images/5df0d202-c4c4-476c-ad04-cf7c41003df0/file                                 |
        | id               | 5df0d202-c4c4-476c-ad04-cf7c41003df0                                                 |
        | min_disk         | 0                                                                                    |
        | min_ram          | 0                                                                                    |
        | name             | cirros                                                                               |
        | owner            | 17234a6cc8954d748ed74a31680ea39b                                                     |
        | properties       | os_hidden='False', owner_specified.openstack.md5='',                                 |
        |                  | owner_specified.openstack.object='images/cirros',                                    |
        |                  | owner_specified.openstack.sha256=''                                                  |
        | protected        | False                                                                                |
        | schema           | /v2/schemas/image                                                                    |
        | status           | queued                                                                               |
        | tags             |                                                                                      |
        | updated_at       | 2024-03-13T09:23:27Z                                                                 |
        | visibility       | shared                                                                               |
        +------------------+--------------------------------------------------------------------------------------+
        ```

    - To confirm the image uploaded into the cloud, use `openstack image list`.


=== "GUI"

    - From the Alces Cloud dashboard, click `Compute` and then `Images`:
    - Click `Create Image`, then `Create An Image` dialog box will appear.
    - Enter the following values:

        | Field                | Description                                                                                                  |
        |----------------------|--------------------------------------------------------------------------------------------------------------|
        | Image Name           | Enter a name for the image.                                                                                  |
        | Image Description    | Enter a description of the image.                                                                            |
        | Image Source         | Choose the image source from the dropdown list. Your choices are Image Location and Image File.              |
        | Image File Location  | Based on your selection for Image Source, you either enter the location URL of the image or browse for the image file on your file system and add it. |
        | Format               | Select the image format for the image. Only RAW images are supported.                                        |
        | Architecture         | Specify the architecture. For example, i386 for a 32-bit architecture or x86_64 for a 64-bit architecture.  |
        | Minimum Disk (GB)    | Leave this field empty.                                                                                      |
        | Minimum RAM (MB)     | Leave this field empty.                                                                                      |
        | Copy Data            | Specify this option to copy image data to the Image service.                                                  |
        | Visibility           | The access permission for the image. Public or Private.                                                      |
        | Protected            | Select this check box to ensure that only users with permissions can delete the image. Yes or No.            |
        | Image Metadata       | Specify this option to add resource metadata. The glance Metadata Catalog provides a list of metadata image definitions. (Note: Not all cloud providers enable this feature.) |

    - Click `Create Image`, then the image is queued to be uploaded, it may take some time and status of image is 
      changed from `Queued` to `Active`. 


## List Images
You can also view your available images (including custom ones):

=== "CLI"

    From the Alces Cloud login node, you can use the `openstack` command:
    ```
    usage: openstack image list [-h] [-f {csv,df-to-csv,json,table,value,yaml}] [-c COLUMN]
                            [--format-config-file FORMAT_CONFIG]
                            [--quote {all,minimal,none,nonnumeric}] [--noindent]
                            [--max-width <integer>] [--fit-width] [--print-empty]
                            [--sort-column SORT_COLUMN] [--sort-ascending | --sort-descending]
                            [--public | --private | --community | --shared | --all]
                            [--property <key=value>] [--name <name>]
                            [--status <status>] [--member-status <member-status>]
                            [--project <project>] [--project-domain <project-domain>]
                            [--tag <tag>] [--hidden] [--long]
                            [--sort <key>[:<direction>]] [--limit <limit>]
                            [--marker <marker>]
    
    # For more information about the parameters, run command "openstack image list --help".
    ```
    To list images in Alces cloud, follow below example.
    
    ```
    (openstack) [user@stack01[poc1] ~]$  openstack image list
    +--------------------------------------+----------------------+--------+
    | ID                                   | Name                 | Status |
    +--------------------------------------+----------------------+--------+
    | b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Rocky 9.3            | active |
    | 8711c1da-71f8-4c09-85bd-6efdc2346188 | Ubuntu 22.04.3 (LTS) | active |
    +--------------------------------------+----------------------+--------+
    ```

=== "GUI"

    From the Alces Cloud dashboard, click `Compute` and then `Images`:

    [<img src="../img/images.png" width="800px" />](img/images.png)


## Update Images
You can update the image present in Alces cloud
=== "CLI"
    From the Alces Cloud login node, you can use the `openstack` command:

    ```
    usage: openstack image set [-h] [--name <name>] [--min-disk <disk-gb>] [--min-ram <ram-mb>]
                           [--container-format <container-format>] [--disk-format <disk-format>]
                           [--protected | --unprotected] [--public | --private | --community | --shared]
                           [--property <key=value>] [--tag <tag>] [--architecture <architecture>]
                           [--instance-id <instance-id>] [--kernel-id <kernel-id>] [--os-distro <os-distro>]
                           [--os-version <os-version>] [--ramdisk-id <ramdisk-id>] [--deactivate | --activate]
                           [--project <project>] [--project-domain <project-domain>] [--accept | --reject | --pending]
                           [--hidden | --unhidden]
                           <image>
    # For more information about the parameters, run command "openstack image delete --help".
    ```

    In current example, Image name is updated from `cirros` to `cirros-old`.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+----------------------+--------+
    | ID                                   | Name                 | Status |
    +--------------------------------------+----------------------+--------+
    | b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Rocky 9.3            | active |
    | 8711c1da-71f8-4c09-85bd-6efdc2346188 | Ubuntu 22.04.3 (LTS) | active |
    | 43c2618e-560e-4f33-8873-125f00e360ad | cirros               | active |
    +--------------------------------------+----------------------+--------+

    (openstack) [user@stack01[poc1] ~]$ openstack image set --name cirros  cirros-old

    (openstack) [user@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+----------------------+--------+
    | ID                                   | Name                 | Status |
    +--------------------------------------+----------------------+--------+
    | b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Rocky 9.3            | active |
    | 8711c1da-71f8-4c09-85bd-6efdc2346188 | Ubuntu 22.04.3 (LTS) | active |
    | 43c2618e-560e-4f33-8873-125f00e360ad | cirros-old           | active |
    +--------------------------------------+----------------------+--------+
    ```

=== "GUI"
    Image deletion is a permanent operation and it cannot be reversed. Specific users with the appropriate permissions can delete images.

    - From the Alces Cloud dashboard, click `Compute` and then `Images`:
    - Select the image that you want to edit.
    - In the Actions column, click the menu button and then select Edit Image from the list.
    - In the Edit Image dialog box, you can perform various actions. For example:
        - Change the name of the image.
        - Change the description of the image.
        - Change the format of the image.
        - Change the minimum disk of the image.
        - Change the minimum RAM of the image.
        - Select the `Public` button to make the image public.
        - Clear the `Private` button to make the image private.
        - Change the metadata of the image.
    - Click `Edit Image`.


## Delete Images
You can delete the image from Alces cloud
=== "CLI"

    From the Alces Cloud login node, you can use the `openstack` command:
    ```
    usage: openstack image delete [-h] [--store <STORE>] <image> [<image> ...]

    # For more information about the parameters, run command "openstack image delete --help".
    ```
    
    To delete cirros images from Alces cloud, follow below example.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack image list
    +--------------------------------------+----------------------+--------+
    | ID                                   | Name                 | Status |
    +--------------------------------------+----------------------+--------+
    | b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Rocky 9.3            | active |
    | 8711c1da-71f8-4c09-85bd-6efdc2346188 | Ubuntu 22.04.3 (LTS) | active |
    | 5df0d202-c4c4-476c-ad04-cf7c41003df0 | cirros               | active |
    +--------------------------------------+----------------------+--------+
    (openstack) [user@stack01[poc1] ~]$ openstack image delete cirros
    ```


=== "GUI"
    Image deletion is a permanent operation and it cannot be reversed. Specific users with the appropriate permissions can delete images.

    - From the Alces Cloud dashboard, click `Compute` and then `Images`:
    - Select the images that you want to delete.
    - Click `Delete Images`.
    - In the `Confirm Delete Images` dialog box, click `Delete Images` to confirm the deletion.

