#Flavors
Alces Cloud instances are available in a number of flavors. The flavor chosen will define the resources available to your instance.

Currently, there are four flavors available on Alces Cloud with the following sizes:

| Flavor | VCPUs | RAM | Ephemeral Disk |
|---|---|---|---|
| default.tiny | 2 | 12 GB | 0 GB |
| default.small | 4 | 24 GB | 0 GB |
| default.medium | 8 | 48 GB | 0 GB |
| default.large | 14 | 84 GB | 0 GB |

You can also view these using the command line:

```
(openstack) [user@stack01[poc1] ~]$ openstack flavor list
+--------------------------------------+----------------+-------+------+-----------+-------+-----------+
| ID                                   | Name           |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------------------+----------------+-------+------+-----------+-------+-----------+
| 13049915-8a25-4efa-b41b-91d88be6b987 | default.tiny   | 12288 |    0 |         0 |     2 | True      |
| 39169052-66d8-434a-8725-554e618e9dc2 | default.medium | 49152 |    0 |         0 |     8 | True      |
| c2ee8fe3-4a7a-4ea1-9e22-8c0a520e0dde | default.large  | 86016 |    0 |         0 |    14 | True      |
| fbbdd856-db71-4979-9d7d-5634feba6ef0 | default.small  | 24576 |    0 |         0 |     4 | True      |
+--------------------------------------+----------------+-------+------+-----------+-------+-----------+
```
