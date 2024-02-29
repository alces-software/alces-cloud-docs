## Overview

A test of `mkdocs` for hosting the Alces cloud docs

## Setup

### Linux Machine/VM
- Prerequisite
  `gcc, g++, python3.10 and python-pip AND python3.10-devel`

- Create python virtualenv and activate virtualenv
  ```bash
  cd alces-cloud-docs
  python3.10 -m venv venv
  source venv/bin/activate
  ```

-  Install dependencies
   ```bash
   cd alces-cloud-docs
   pip install --upgrade pip
   pip install -r requirments.txt
   ```

- Ensure correct default branch being used by mike
  ```bash
  mike set-default latest
  ```

## Viewing Docs

If using python virtualenv, then source the environemnt.
  ```bash
  cd alces-cloud-docs
  source venv/bin/activate
  ```

To view your WIP documentation locally simply use `mkdocs serve` which will update as docs are changed. 

To view the versioned documentation (managed by `mike`) run `mike serve` (note: this will not auto-update as docs are changed and requires redeploying).

## Writing Docs

It's worth familiarising with the [available markdown formatting](https://www.mkdocs.org/user-guide/writing-your-docs/#writing-with-markdown) supported by mkdocs. This documentation has the following plugins installed:


## Deploying Docs

Using `mike` we can deploy documentation with version tagging

- Deploy WIP data to `staging` 
```
mike deploy --push --no-redirect --update-aliases 20XX.Y staging
```

- Deploy a new stable version of documentation
```
mike deploy --push --no-redirect --update-aliases 20XX.Y latest
```

- Removing staging tag upon release to latest 
```
# Set a different branch to `staging` tag, see previous deploy WIP step
```

- Resetting/removing a version 
```
mike delete --push 20XX.Y
```


## Content Things to Look Into

- Code Annotations (adds pop-ups with notes on commands/info) 
    - See "Code annotations" in material docs
- Rename sections to prettier naming
