---
authors:
  - shubhamdang
date: 2024-03-28
categories:
  - se1
readtime: 2
---

# Train a Machine Learning Model With MNIST Dataset Using PyTorch
In this blog, we'll guide you through the installation of PyTorch on a linux based operating system, then followed by training Machine Learning Model with MNIST dataset using PyTorch.

Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, leading up to the installation of PyTorch and then train a machine learning model with MNIST dataset using PyTorch.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

!!! note
    Make sure  `python3` and `virtualenv` are installed on the system.

## Setting Up PyTorch and Examples Repository

- Create a Python Virtual Environment and Install PyTorch:
  ```
  python3 -m venv myenv 
  source myenv/bin/activate
  pip install torch
  ```
- Clone the PyTorch Examples Repository:
  ```
  git clone https://github.com/pytorch/examples.git
  ```
- Navigate to the Example You Wish to Explore:
  ```
  cd examples/mnist
  ```
- Install Required Dependencies and Run the Training Model
  ```
  pip install -r requirements.txt
  python main.py
  ```

!!! note
    For additional information and to explore other training model examples, please visit the PyTorch Examples GitHub repository [here](https://github.com/pytorch/examples). This repository offers a diverse range of examples for various machine learning models and use cases, providing valuable resources for further experimentation and learning.
