#Quotas
## Checking your current Quota
Each project is assigned a quota that defines how much of each resource a project can use. For each resource consuming operation (such as creating an instance), the request is checked against your maximum permitted quota.
  
Your project quota and usage can be checked using:
=== "CLI"

    To view your current quota and usage:

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack quota show --usage
    +----------------------+--------+--------+----------+
    | Resource             |  Limit | In Use | Reserved |
    +----------------------+--------+--------+----------+
    | cores                |     48 |     42 |        0 |
    | instances            |      8 |      7 |        0 |
    | ram                  | 393216 | 344064 |        0 |
    | volumes              |      8 |      8 |        0 |
    | snapshots            |     10 |      0 |        0 |
    | gigabytes            |   1000 |    800 |        0 |
    | backups              |     10 |      0 |        0 |
    | volumes_hdd.1        |     -1 |      8 |        0 |
    | gigabytes_hdd.1      |     -1 |    800 |        0 |
    | snapshots_hdd.1      |     -1 |      0 |        0 |
    | volumes_default      |     -1 |      0 |        0 |
    | gigabytes_default    |     -1 |      0 |        0 |
    | snapshots_default    |     -1 |      0 |        0 |
    | groups               |     10 |      0 |        0 |
    | networks             |      3 |      1 |        0 |
    | ports                |     30 |     10 |        0 |
    | rbac_policies        |     10 |      0 |        0 |
    | routers              |     10 |      1 |        0 |
    | subnets              |      5 |      1 |        0 |
    | subnet_pools         |     -1 |      0 |        0 |
    | fixed-ips            |     -1 |      0 |        0 |
    | injected-file-size   |  10240 |      0 |        0 |
    | injected-path-size   |    255 |      0 |        0 |
    | injected-files       |      5 |      0 |        0 |
    | key-pairs            |     20 |      0 |        0 |
    | properties           |    128 |      0 |        0 |
    | server-groups        |     10 |      0 |        0 |
    | server-group-members |     10 |      0 |        0 |
    | floating-ips         |      8 |      8 |        0 |
    | secgroup-rules       |    100 |      8 |        0 |
    | secgroups            |     10 |      3 |        0 |
    | backup-gigabytes     |   1000 |      0 |        0 |
    | per-volume-gigabytes |     -1 |      0 |        0 |
    +----------------------+--------+--------+----------+
    ```

=== "GUI"
    You can also view some of the key quotas from the Alces Cloud dashboard overview page.

    [<img src="../img/quotas.png" width="800px" />](img/quotas.png)

## Image Quotas

Your project also has some additional image quotas limiting the amount and size of images you are allowed to upload.

| Limit | Quota |
|---|---|
| Total Images | 10 |
| Maximum Image Size | 100GB |
| Total Image Storage | 250GB |
| Maximum Parallel Uploads | 3 |
