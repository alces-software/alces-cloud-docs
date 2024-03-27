---
authors:
  - shubhamdang
date: 2024-03-27
categories:
  - se1
readtime: 2
---

# Setup and Run Local LLM using Ollama
In this blog, we'll guide you through the installation of Ollama and running a local LLM using Ollama on a Linux-based operating system. Our setup involves a single node.

Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, leading up to the installation of Ollama and then running a LLM model.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

!!! note
    Just Make sure port `22, 11434` is opened as ingress rule in security group that is attached to the instance.
    If `firewalld` is enabled on the server, in order to allow external connection on your ollama api endpoint runs on port `11434` by default we need to update the setting by running the below command
    ```
    $ sudo firewall-cmd --zone=public --add-port=11434/tcp --permanent
    ```

    To apply the changes and reload the firewalld run below command
    ```
    $ sudo firewall-cmd --reload
    ```

## Setup Ollama
Ollama can be quickly installed using a one-liner command, use below command.
```
$ curl -fsSL https://ollama.com/install.sh | sh
```

!!! note
    In order to install on other Operating System, use the [link](https://ollama.com/download/linux)


## Execute Model

In our example we will pulling and running a the `gemma:2b` model using ollama, Gemma is a family of lightweight, state-of-the-art open models built by Google DeepMind. These models run on diverse devices and are adaptable through fine-tuning, making AI accessible to more users.

**Models Supported by Ollama**

Ollama provides a selection of models accessible via ollama.com/library.

Below are a few sample models that users can use:

| Model              | Parameters | Size  | Download                       |
| ------------------ | ---------- | ----- | ------------------------------ |
| Llama 2            | 7B         | 3.8GB | `ollama run llama2`            |
| Mistral            | 7B         | 4.1GB | `ollama run mistral`           |
| Dolphin Phi        | 2.7B       | 1.6GB | `ollama run dolphin-phi`       |
| Phi-2              | 2.7B       | 1.7GB | `ollama run phi`               |
| Neural Chat        | 7B         | 4.1GB | `ollama run neural-chat`       |
| Starling           | 7B         | 4.1GB | `ollama run starling-lm`       |
| Code Llama         | 7B         | 3.8GB | `ollama run codellama`         |
| Llama 2 Uncensored | 7B         | 3.8GB | `ollama run llama2-uncensored` |
| Llama 2 13B        | 13B        | 7.3GB | `ollama run llama2:13b`        |
| Llama 2 70B        | 70B        | 39GB  | `ollama run llama2:70b`        |
| Orca Mini          | 3B         | 1.9GB | `ollama run orca-mini`         |
| Vicuna             | 7B         | 3.8GB | `ollama run vicuna`            |
| LLaVA              | 7B         | 4.5GB | `ollama run llava`             |
| Gemma              | 2B         | 1.4GB | `ollama run gemma:2b`          |
| Gemma              | 7B         | 4.8GB | `ollama run gemma:7b`          |

!!!note 
    To effectively operate the 7B models, a minimum of 8 GB of RAM is recommended, while 16 GB is advisable for the 13B models, and 32 GB is necessary for the 33B models.



**Pull Model**

- To begin, let's ensure that the model is available on the local machine. Run the following command:
    ```
    $ ollama list
    NAME    	ID          	SIZE  	MODIFIED
    ```

- If the model is not present, we can fetch it locally using the command:
    ```
    $ ollama pull gemma:2b
    ```

- Once the model has been pulled, verify its presence on the machine by running:
    ```
    $ ollama list
    NAME    	ID          	SIZE  	MODIFIED
    gemma:2b	b50d6c999e59	1.7 GB	About a minute ago
    ```

**Run Model**

- To execute the model, simply use the following command. If the model is not already available locally, it will be automatically pulled before initiating the execution:
    ```
    $ ollama run gemma:2b
    >>> What is linux?
    Linux is an open-source operating system that is used on a wide range of computers, including desktops, laptops, and servers. It is known for its flexibility, reliability, and security. Linux is also used in
    a variety of software applications, such as web browsers, operating systems, and productivity tools.
    ```

- Above command starts the model and provides a prompt in the terminal, allowing users to input queries that can be answered by the Language Model (LLM).
    

!!! note
    For more details on Ollama, please refer to [link](https://github.com/ollama/ollama)









