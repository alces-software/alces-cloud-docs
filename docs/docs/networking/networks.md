#Networks
Alces Cloud provides you with the ability to create and manage your own private networks. This can be useful if you need to isolate an instances from other instances. Your project will come with a single network already configured, but additional networks can be created if required.

Alces Cloud also provides an external network in order for you to access your instances. Instances cannot be connected directly to this network and instead floating IPs are allocated to your project and then associated to your instances.

!!! info
    The Alces Cloud external network is limited to 30MB/s per instance when using a floating IP. Instances without floating IPs will share a combined 30MB/s limit.

## Create a Network

Your project will come with a default network already setup and configured, however additional networks can be created if required.

=== "CLI"

    First create a new network with the desired name.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack network create mynetwork
    +---------------------------+--------------------------------------+
    | Field                     | Value                                |
    +---------------------------+--------------------------------------+
    | id                        | 8fb2fcd6-3f56-44db-86a9-c5ba4d10df47 |
    | name                      | mynetwork                            |
    | status                    | ACTIVE                               |
    +---------------------------+--------------------------------------+
    ```

    Then create a subnet for the network with a suitable CIDR range.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack subnet create --network mynetwork --subnet-range 192.168.12.0/24 mysubnet
    +----------------------+--------------------------------------+
    | Field                | Value                                |
    +----------------------+--------------------------------------+
    | allocation_pools     | 192.168.12.2-192.168.12.254          |
    | cidr                 | 192.168.12.0/24                      |
    | gateway_ip           | 192.168.12.1                         |
    | name                 | mysubnet                             |
    +----------------------+--------------------------------------+
    ```

=== "GUI"
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Networks` on the left side bar.
    2. Click the `Create Network` button.
    3. Enter a network name, and then click `Next`.
    4. Enter a subnet name and then a suitable network address in CIDR format.
    5. Click `Next` and then `Create`.

## Connect a Network

In order for you to associate a floating IP with an instance, the network must be connected to a router than is also connected to the Alces Cloud external network.

=== "CLI"

    List the routers available to your project and make a note of the router you'd like to connect your network to. Your project will come with a default router already connected to the Alces Cloud external network for you to use.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack router list
    +--------------------------------------+-----------------+--------+-------+----------------------------------+
    | ID                                   | Name            | Status | State | Project                          |
    +--------------------------------------+-----------------+--------+-------+----------------------------------+
    | e543e469-791d-42cf-8f82-da384fc14348 | project-default | ACTIVE | UP    | a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6 |
    +--------------------------------------+-----------------+--------+-------+----------------------------------+
    ```

    To connect the network to a router, you must connect the associated subnet. List the subnets associated with your network and make a note of the one you'd like to connect.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack subnet list --network mynetwork
    +--------------------------------------+----------+--------------------------------------+-----------------+
    | ID                                   | Name     | Network                              | Subnet          |
    +--------------------------------------+----------+--------------------------------------+-----------------+
    | c7bc6fb4-4a37-40f2-9cfe-03def6cce8ab | mysubnet | 8fb2fcd6-3f56-44db-86a9-c5ba4d10df47 | 192.168.12.0/24 |
    +--------------------------------------+----------+--------------------------------------+-----------------+
    ```

    Connect the chosen subnet to the desired router.

    ```
    (openstack) [user@stack01[poc1] ~]$ openstack router add subnet myrouter mysubnet
    ```

=== "GUI"
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Routers`.
    2. Click on the router you wish to connect your network to.
    3. Click on the `interfaces` tab, and then the `Add Interface` button.
    4. Select the subnet associated with your network, and then click `Submit`.

## Create a Router

Your project will come with a default router that is already connected to the Alces Cloud external network, however additional routers can be created if required.

=== "CLI"

    Create a new router with the desired name.
    
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack router create myrouter
    +-------------------------+--------------------------------------+
    | Field                   | Value                                |
    +-------------------------+--------------------------------------+
    | id                      | 9d4588be-f5c7-4819-81a1-e347469ecb72 |
    | name                    | myrouter                             |
    | status                  | ACTIVE                               |
    +-------------------------+--------------------------------------+
    ```

    Connect the router to the Alces Cloud external network if desired.
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack router set myrouter --external-gateway external1
    ```

=== "GUI"
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Routers`.
    2. Click the `Create Router` button.
    3. Enter a router name and select the Alces Cloud external network.
    4. Click create router.

Routers must be connected to the Alces Cloud external network in order to use floating IPs. Internal routers between your private networks can also be created if required by not selecting any external network.
