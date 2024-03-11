#Security Groups
Security Groups allow control over the traffic permitted to and from your instances on Alces Cloud. By default, your project will come with 3 example security groups to help get you started:

| Name | Description | Ingress | Egress
|---|---|---|---|
| default | Default Security Group | All from instances in default security group | All to everywhere |
| allow-ssh | Allows SSH (22/tcp) access. | SSH (22/tcp) from everywhere | All to everywhere |
| allow-https | Allows HTTP (80/tcp) and HTTPS (443/tcp).  | HTTP(80/tcp) and HTTPS(443/tcp) from everywhere | All to everywhere |

Multiple security groups can be assigned to your instances at once. For example, to allow SSH and HTTP(S) access to your instance, you could stack the `allow-ssh` and `allow-https` security groups.

It is not recommended to modify the default security groups, and instead create additional security groups where required.

=== "CLI"

    ```
    (openstack) [user@stack01[poc1] ~]$  openstack magic secgroup create things
    ```
    View your security groups with:
    ```
    (openstack) [user@stack01[poc1] ~]$ openstack security group list
    +--------------------------------------+-------------+-------------------------------------------+----------------------------------+------+
    | ID                                   | Name        | Description                               | Project                          | Tags |
    +--------------------------------------+-------------+-------------------------------------------+----------------------------------+------+
    | a5c693ab-431c-4059-b0a2-e64e9636e51d | allow-https | Allows HTTP (80/tcp) and HTTPS (443/tcp). | 17234a6cc8954d748ed74a31680ea39b | []   |
    | cc99563e-1110-494b-8b3f-d411c8ece659 | allow-ssh   | Allows SSH (22/tcp) access.               | 17234a6cc8954d748ed74a31680ea39b | []   |
    | df9dc0fb-0d91-4e06-8aa5-477c6b1fe254 | default     | Default security group                    | 17234a6cc8954d748ed74a31680ea39b | []   |
    +--------------------------------------+-------------+-------------------------------------------+----------------------------------+------+
    ```
    
=== "GUI"
    ## Create a Security Group

    1. From the Alces Cloud dashboard, navigate to Network and then `Security Groups` on the left side bar.
    2. Click the `Create Security Group` button.
    3. Give the security group a name and an optional description.
    4. Click `Create Security Group`.

    By default, every security group will be created with two existing rules permitting all outbound traffic.

    ## Add Security Group Rules

    1. From the Alces Cloud dashboard, navigate to Network and then `Security Groups` on the left side bar.
    2. Click `Manage Rules` on the desired security group.
    3. Click the `Add Rule` button.
    4. Configure the desired rule. For example, select the SSH rule and configure a CIDR of 0.0.0.0/0 to allow all SSH traffic on port 22.
    5. Click the `Add` button.

Further information on creating security groups can be found [here](https://docs.openstack.org/nova/2023.1/admin/security-groups.html).
