#Creating an SSH Key
To securely connect to your instances you will need to create at least one SSH key pair. These are unique to your user and are available across all of your projects. Additional SSH keys can be also created if required.

=== "CLI"

    From `stack01`, ensure you have sourced the `openrc` file correctly and authenticated with Alces Cloud.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ source openrc
    Please enter your password:
    ```

    Create a new SSH key using the following command - the private key will be saved to the file `~/.ssh/mykeypair`.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ openstack keypair create mykeypair --private-key ~/.ssh/mykeypair
    +-------------+-------------------------------------------------+
    | Field       | Value                                           |
    +-------------+-------------------------------------------------+
    | created_at  | None                                            |
    | fingerprint | a1:b2:c3:d4:e5:f6:g7:h8:i9:j0:k1:l2:m3:n4:o5:p6 |
    | id          | mykeypair                                       |
    | is_deleted  | None                                            |
    | name        | mykeypair                                       |
    | type        | ssh                                             |
    | user_id     | a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6                |
    +-------------+-------------------------------------------------+
    ```

    Ensure the file permissions are set correctly.
    ```
    (openstack) [myuser@stack01[poc1] ~]$ chmod 600 ~/.ssh/mykeypair
    ```

    After creating your instances with the correct key pair `mykeypair`, you will be able to use this private key to access your instances with the `ssh` command.
    
=== "GUI"

    On the left side bar, navigate to `Compute` and then `Key Pairs`.

    [<img src="../img/keypair-overview.png" width="800px" />](img/keypair-overview.png)

    Click on the `Create Key Pair`, give your key pair a name and select `SSH Key` type.

    [<img src="../img/keypair-create.png" width="600px" />](img/keypair-create.png)

    Once you have created your key pair, a `.pem` file will be downloaded with your private key. Use this to later SSH to your instances.
