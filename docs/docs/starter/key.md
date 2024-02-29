#Creating an SSH Key
To securely connect to your instances you will need to create at least one SSH key pair. These are unique to your user and are available across all of your projects. Additional SSH keys can be also created if required.

On the left side bar, navigate to `Compute` and then `Key Pairs`.

[<img src="../img/keypair-overview.png" width="800px" />](img/keypair-overview.png)

Click on the `Create Key Pair`, give your key pair a name and select `SSH Key` type.

[<img src="../img/keypair-create.png" width="600px" />](img/keypair-create.png)

Once you have created your key pair, a `.pem` file will be downloaded with your private key. Use this to SSH to your instances.

[Next](instance.md){: .md-banner__button}

