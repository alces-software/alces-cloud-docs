#Flavors
Alces Cloud instances are available in a number of flavors. The flavor chosen will define the resources available to your instance.

Currently, there are two flavors available on Alces Cloud with the following sizes:

| Flavor | VCPUs | RAM | Ephemeral Disk | Root Disk | I/O Limit |
|---|---|---|---|---|---|
| p1.small | 6 | 48 GB | 0 GB | 500 GBi | 50 MB/s Read + Write |
| p1.large | 12 | 96 GB | 0 GB | 1000 GB | 100 MB/s Read + Write |

!!! info
    I/O limits apply to instances booted using a local root disk. If you choose to boot your instance from a volume, the relevant volume limits will apply instead.

You can also view these using the command line:

```
(openstack) [user@stack01[poc1] ~]$ openstack flavor list
+--------------------------------------+----------+-------+------+-----------+-------+-----------+
| ID                                   | Name     |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------------------+----------+-------+------+-----------+-------+-----------+
| 22ce59c5-54bd-4fb2-b31d-2835397bffd1 | p1.large | 98304 |  500 |         0 |    12 | False     |
| a489b653-2740-4bf0-b2ea-a23a7f563361 | p1.small | 49152 | 1000 |         0 |     6 | False     |
+--------------------------------------+----------+-------+------+-----------+-------+-----------+
```