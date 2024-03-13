---
authors:
  - shubhamdang
date: 2024-03-06
categories:
  - se1
readtime: 5
---

# Create a Single Node Slurm Cluster

In this blog, we'll guide you through the seamless upload of Flight solo image on alces cloud and then followed by the installation of a slurm standalone cluster installation. Our setup involves a single nodes that has slurm manager installed on it and that can be further used to execute HPC workloads.

Let's start with a step-by-step process, starting from uploading flight solo image, then creating virtual machines on the Alces Cloud platform, followed by verification of setup by running a simple HPC job.
<!-- more -->

## Upload Flight Solo Image

- Download the Flight Solo OpenStack image [here](https://repo.openflighthpc.org/?prefix=images/FlightSolo/)
- Upload the image by following the steps provided in the [link](../../docs/compute/images.md)


## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Create Slurm Cluster Using Flight Solo


1. Parse your node(s) with the command `flight hunter parse`.

    1. This will display a list of hunted nodes, for example
        ```bash 
        [flight@myinstance.novalocal ~]$ flight hunter parse
        Select nodes: (Scroll for more nodes)
        ‣ ⬡ myinstance.novalocal - 10.10.0.1
        ```

    1. Select the desired node to be parsed with ++space++, and you will be taken to the label editor
        ```bash
        Choose label: login-node.novalocal
        ```

    1. Here, you can edit the label like plain text
        ```bash
        Choose label: login1
        ```

        !!! tip
            You can clear the current node name by pressing ++down++ in the label editor.

    1. When done editing, press ++enter++ to save. The modified node label will appear next to the ip address and original node label.
        ```bash
        Select nodes: login-node.novalocal - 10.10.0.1 (login1) (Scroll for more nodes)
        ‣ ⬢ myinstance.novalocal - 10.10.0.1 (login1)
        ```

    1. From this point, you can either hit ++enter++ to finish parsing and process the selected nodes, or continue changing nodes. Either way, you can return to this list by running `flight hunter parse`.

    1. Save the node inventory before moving on to the next step.

        !!! tip
            See `flight hunter parse -h` for more ways to parse nodes.

### Add genders

1. **Optionally**, you may add genders to the newly parsed node. For example, in the case that the node should have the gender `cluster` and `all` then run the command:
    ```bash
    flight hunter modify-groups --add cluster,all login1
    ```

## SLURM Standalone Configuration


1. Configure profile

    ```bash
    flight profile configure
    ```

    1. This brings up a UI, where several options need to be set. Use up and down arrow keys to scroll through options and enter to move to the next option. Options in brackets coloured yellow are the default options that will be applied if nothing is entered.
        - Cluster type: The type of cluster setup needed, in this case select `Slurm Standalone`.
        - Cluster name: The name of the cluster.
        - Default user: The user that you log in with.
        - Set user password: Set a password to be used for the chosen default user.
        - IP or FQDN for Web Access: This could be the public IP or public hostname.

1. Apply an identity by running the command `flight profile apply`, E.g.
    ```bash
    flight profile apply login1 all-in-one
    ```

    !!! tip
        You can check all available identities for the current profile with `flight profile identities`

7. Wait for the identity to finish applying. You can check the status of all nodes with `flight profile list`.

    !!! tip
        You can watch the progress of the application with `flight profile view login1 --watch`

    !!! success
        Congratulations, you've now created a SLURM Standalone environment.


## Run Slurm Workload

1. Create a file called `simplejobscript.sh`, and copy this into it:
    ```
    #!/bin/bash -l
    echo "Starting running on host $HOSTNAME"
    sleep 30
    echo "Finished running - goodbye from $HOSTNAME"
    ```

2. Run the script with `sbatch simplejobscript.sh`, and to test all your nodes try queuing up enough jobs that all nodes will have to run.

3. In the directory that the job was submitted from there should be a `slurm-X.out` where `X` is the Job ID returned from the `sbatch` command. This will contain the echo messages from the script created in step 1 