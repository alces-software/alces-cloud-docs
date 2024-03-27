#Flavors
Alces Cloud instances are available in a number of flavors. The flavor chosen will define the resources available to your instance.

Currently, there are two flavors available on Alces Cloud with the following sizes:

| Flavor | VCPUs | RAM | Ephemeral Disk | Root Disk | I/O Limit |
|---|---|---|---|---|---|
| c1.small | 6 | 48 GB | 700 GB | 150 GBi | 50 MB/s Read + Write |
| c1.large | 12 | 96 GB | 1400 GB | 300 GB | 100 MB/s Read + Write |

!!! info
    I/O limits apply to instances booted using a local root disk. If you choose to boot your instance from a volume, the relevant volume limits will apply instead.

You can also view these using the command line:

```
(openstack) [user@stack01[poc1] ~]$ openstack flavor list
+--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+
| ID                                   | Name                            |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+
| 26b933e3-4fde-4e10-9556-16b2e470cadf | c1.small                        | 49152 |  150 |       700 |     6 | False     |
| d538bd2d-10ce-4555-8d1d-46535ee815ee | c1.large                        | 98304 |  300 |      1400 |    12 | False     |
+--------------------------------------+---------------------------------+-------+------+-----------+-------+-----------+
```

## Understanding Instance Storage Options

Choosing the right storage solution for your virtual machines (VMs) is crucial for optimal performance and data protection. Let's understand the key differences between local disks (root and ephemeral) and Ceph-backed volumes, helps you to make informed decisions based on your specific needs.

**Local Disk**

Local Disk is a storage in which data of virtual machines is present on the physical compute node due to which it provides good speed. It has some drawbacks as well i.e. lack of persistence of data when virtual machine is terminated and lack of redundancy that means if hardware on which id spawned faces failure then all data is lost.

Types of Local Disk are given below:
- **Root Disk:** Root Disk stores the operating system and essential boot files. Since it resides on the compute node's local disk, the root disk offers exceptional read/write speeds, making it ideal for applications requiring fast access to frequently used data.

- **Ephemeral Disk:** Ephemeral Disk provides additional temporary storage for your VM. Similar to the root disk, ephemeral disks provide impressive speeds, perfect for storing temporary files, caches, or workloads requiring quick data access.

In simpler terms, root disk is like the permanent hard drive on your personal computer, where your OS and core files reside whereas ephemeral disk is like a USB flash drive attached to your computer, offering extra space but temporary storage.

**Volumes Disk**
Volume-based disks utilize distributed storage systems spread across multiple nodes, offering a balance between performance, redundancy, and scalability. Data stored on volumes persists even after VM reboots or terminations, making them ideal for critical application data, databases, or user files.

While volumes may exhibit slightly lower read/write speeds compared to local disks due to the distributed storage architecture, they offer enhanced data protection and availability.

Your data is replicated across multiple Ceph nodes, ensuring high availability. Even in the event of a node failure, your data remains safeguarded on other nodes, highlighting the robust redundancy mechanisms inherent to Ceph-backed volumes.
