#Networks
Alces Cloud provides you with the ability to create and manage your own private networks. This can be useful if you need to isolate an instances from other instances. Your project will come with a single network already configured, but additional networks can be created if required.

Alces Cloud also provides an external network in order for you to access your instances. Instances cannot be connected directly to this network and instead floating IPs are allocated to your project and then associated to your instances.

!!! info
    The Alces Cloud external network is limited to 30MB/s per instance when using a floating IP. Instances without floating IPs will share a combined 30MB/s limit.

=== "CLI"
    ## Create a Network
    ```
    #Create a network
    #Create a subnet
    #Create a router
    openstack router create <router-name>
    ```

=== "GUI"
    ## Create a Network
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Networks` on the left side bar.
    2. Click the `Create Network` button.
    3. Enter a network name, and then click `Next`.
    4. Enter a subnet name and then a suitable network address in CIDR format.
    5. Click `Next` and then `Create`.

    In order for you to associate a floating IP with an instance, the network must be connected to a router than is also connected to the Alces Cloud external network.

    ## Connect a Network
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Routers`.
    2. Click on the router you wish to connect your network to.
    3. Click on the `interfaces` tab, and then the `Add Interface` button.
    4. Select the subnet associated with your network, and then click `Submit`.

    Your project will come with a default router that is already connected to the Alces Cloud external network, however additional routers can be created if required.

    ## Create a Router
    1. From the Alces Cloud dashboard, navigate to `Network` and then `Routers`.
    2. Click the `Create Router` button.
    3. Enter a router name and select the Alces Cloud external network.
    4. Click create router.

    Routers must be connected to the Alces Cloud external network in order to use floating IPs. Internal routers between your private networks can also be created if required by not selecting any external network.
