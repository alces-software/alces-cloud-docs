#Images
Alces Cloud includes a number of maintained images for supported operating systems. It is recommended to use an Alces supported image, however you are also able to upload you own images if required.

The following images are currently available on Alces Cloud:

| Name | Description | Default User |
|---|---|---|
| Alces - Rocky 9.3 | Rocky Linux release 9.3 (Blue Onyx) | rocky |
| Alces - Ubuntu 22.04.3 (LTS) | Ubuntu 22.04.3 (Long Term Support) | ubuntu |
| Alces - Flight Solo 2023.6 | Flight Solo 2023.6 release | flight |

Further documentation on using "Flight Solo" (a HPC-ready, platform-agnostic image) can be found [here](https://www.openflighthpc.org/latest/docs/flight-solo/).

You can also view these using the command line:

```
(openstack) [user@stack01[poc1] ~]$ openstack image list --public
+--------------------------------------+------------------------------+--------+
| ID                                   | Name                         | Status |
+--------------------------------------+------------------------------+--------+
| 856ad2cc-3c8c-4914-b3ba-b226634d8158 | Alces - Flight Solo 2023.6   | active |
| b7e0d27f-1266-4069-a9b8-fb8a71f25421 | Alces - Rocky 9.3            | active |
| 8711c1da-71f8-4c09-85bd-6efdc2346188 | Alces - Ubuntu 22.04.3 (LTS) | active |
+--------------------------------------+------------------------------+--------+
```
