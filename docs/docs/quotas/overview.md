#Quotas
Each project is assigned a quota that defines how much of each resource a project can use. For each resource consuming operation (such as creating an instance), the request is checked against your maximum permitted quota.

Your project quota and usage can be checked using:

```
(openstack) [user@stack01[poc1] ~]$ openstack quota show --usage
+----------------------+--------+--------+----------+
| Resource             |  Limit | In Use | Reserved |
+----------------------+--------+--------+----------+
| cores                |     56 |      0 |        0 |
| instances            |     28 |      0 |        0 |
| ram                  | 415802 |      0 |        0 |
| volumes              |     28 |      0 |        0 |
| snapshots            |     10 |      0 |        0 |
| gigabytes            |    500 |      0 |        0 |
| backups              |     10 |      0 |        0 |
| volumes_default      |     -1 |      0 |        0 |
| gigabytes_default    |     -1 |      0 |        0 |
| snapshots_default    |     -1 |      0 |        0 |
| groups               |     10 |      0 |        0 |
| networks             |      5 |      1 |        0 |
| ports                |    100 |      3 |        0 |
| rbac_policies        |     10 |      0 |        0 |
| routers              |     10 |      1 |        0 |
| subnets              |     20 |      1 |        0 |
| subnet_pools         |     -1 |      0 |        0 |
| fixed-ips            |     -1 |      0 |        0 |
| injected-file-size   |  10240 |      0 |        0 |
| injected-path-size   |    255 |      0 |        0 |
| injected-files       |      5 |      0 |        0 |
| key-pairs            |     20 |      0 |        0 |
| properties           |    128 |      0 |        0 |
| server-groups        |     10 |      0 |        0 |
| server-group-members |     10 |      0 |        0 |
| floating-ips         |     28 |      1 |        0 |
| secgroup-rules       |    100 |     10 |        0 |
| secgroups            |     10 |      3 |        0 |
| backup-gigabytes     |    500 |      0 |        0 |
| per-volume-gigabytes |     -1 |      0 |        0 |
+----------------------+--------+--------+----------+
```

You can also view some of the key quotas from the Alces Cloud dashboard overview page.

[<img src="../img/quotas.png" width="800px" />](img/quotas.png)
