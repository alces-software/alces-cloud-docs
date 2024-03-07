#Images
Alces Cloud includes a number of maintained images for supported operating systems. It is recommended to use an Alces supported image, however you are also able to upload you own images if required.

The following images are currently available on Alces Cloud:

| Name | Description | Default User |
|---|---|---|
| Rocky 9.3 | Rocky Linux release 9.3 (Blue Onyx) | rocky |
| Ubuntu 22.04.3 (LTS) | Ubuntu 22.04.3 (Long Term Support) | ubuntu |

You can also view your available images (including custom ones):

=== "CLI"

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
    Alces Cloud dashboard overview page / Images:.

