---
authors:
  - shubhamdang
date: 2024-04-10
categories:
  - se1
readtime: 5
---

# How to Install Jupyter Notebook and Jupyter Lab

In this blog post, we will provide a comprehensive guide on installing jupyter notebook and jupyter lab on a rocky based operating system. Our setup revolves around a single node for the installation.


Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, followed by installation of jupyter lab and notebook.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Installation Steps

- First update package lists and install essential dependencies for jupyter lab and notebook.
    ```
    sudo dnf update
    sudo dnf install python3 python3-devel gcc libffi-devel openssl-devel
    ```


- Install Jupyter lab and notebook using pip:
```
python3 -m pip install jupyter jupyterlab
```

- Verification can be done using below command.
```
jupyter notebook --version
jupyter lab --version 
```

### Launching Jupyter Notebook:

- Once the installation is complete launching Jupyter Notebook, open a terminal in the directory you want to use as your working space Start Jupyter Notebook with the command:

    ```
    jupyter notebook
    ```

This will launch a web interface (usually at `http://localhost:8888/`) where you can create and manage Jupyter notebooks.

!!! note
    If you wanted to run jupyter notebook on remote server on port 8080 with no browser mode i.e. headless from the remote,  run the below command.
    ```
    jupyter notebook --no-browser --ip='*' --port 8080
    ```


### Launching JupyterLab:

- Once the installation is complete launching Jupyter Notebook, open a terminal in the directory you want to use as your working space Start Jupyter Notebook with the command:

    ```
    jupyter lab
    ```
    A web interface (often at `http://localhost:8888/lab`) will open, providing a more advanced environment for data science exploration.

!!! note
    If you wanted to run jupyter lab on remote server on port 8080 with no browser mode i.e. headless from the remote,  run the below command.
    ```
    jupyter lab --no-browser --ip='*' --port 8080
    ```


!!! note
    For more detials, please refer to offical documentation.

    - [Jupyter Notebook Documentation](https://docs.jupyter.org/)
    - [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)