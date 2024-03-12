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

mike is a Python utility that makes it easy to deploy multiple versions of your MkDocs-powered docs to a Git branch, suitable for hosting on Github via gh-pages.

### Before Using mike
Before using mike for the first time, you may want to use mike delete --all to delete any old documentation on your gh-pages branch before building your new versioned docs.

```bash
mike delete --all
```

### Why Use mike?
mike is built around the idea that once you've generated your docs for a particular version, you should never need to touch that version again. This means you never have to worry about breaking changes in MkDocs, since your old docs (built with an old version of MkDocs) are already generated and sitting in your gh-pages branch.

### How It Works
mike works by creating a new Git commit on your gh-pages branch every time you deploy a new version of your docs using mike deploy (or other mike subcommands that change your gh-pages branch). When deploying a particular version, previously-deployed docs for that version are erased and overwritten, but docs for other versions remain untouched.

### Usage
Before using mike for the first time, you may want to use mike delete --all to delete any old documentation on your gh-pages branch before building your new versioned docs. (If you prefer, you can also manually move your old documentation to a subdirectory of your gh-pages branch so that it's still accessible.)

Building Your Docs
mike is designed to produce one version of your docs at a time. That way, you can easily deploy a new version without touching any older versions of your docs

#### Add/Update New Version
```bash
# Add new version (locally on gh-pages branch)
mike deploy [version]
mike deploy 2024.3

# Add new version (remotely on gh-pages branch)
mike deploy --push [version]
mike deploy --push 2024.3

# Add new version with aliases(locally on gh-pages branch)
mike deploy [version] [alias]
mike deploy 2024.3 latest

# Add new version with aliases(remotely on gh-pages branch)
mike deploy --push [version] [alias]
mike deploy --push 2024.3 latest
```

If [version] already exists, the above command will also update all of the pre-existing versions and aliases for it. Normally, if an alias specified on the command line is already associated with another version, this will return an error. If you do want to move an alias from another version to this version (including when the new version itself was previously an alias), you can pass -u/--update-aliases to allow this. For example, this can be useful when releasing a new version and updating the latest alias to point to this new version.

```bash
# let's say 2024.3 is associated with latest aliases and now we wanted to associate 2024.4 to latest aliases then we can run the command like below
mike deploy --update-aliases 2024.4 latest

# To deploy on code on remote gh-pages branch
mike deploy --update-aliases --push 2024.4 latest

# This is how we can point latest aliases to latest version of docs.
```

#### Delete Version
We can delete the version of docs following below command

```bash
# Delete specfic version from remote gh-pages branch
mike delete [version/aliases]
mike delete 2024.3

# Delete specfic version from remote gh-pages branch
mike delete --push [version/aliases]
mike delete --push 2024.3


# Delete all version from local gh-pages branch
mike delete --all

# Delete all version from remote gh-pages branch
mike delete --push --all
```

## Add New Blog

- Create blog file:
  `docs/blog/post/blog-file-name.md`: This is the template we are using for blog, add these lines at the top of each file and then start adding the blog content below.
  ```markdown
  ---
  authors:
    - shubhamdang
  date: 2024-03-03
  categories:
    - Kubernetes
  readtime: 2
  ---

  # Blog Title
  ...
  blog content start from here
  ```

    `author:` Name of the author of blog.
    `date`: Date of blog creation.
    `categories:` Category of the blog is added.
    `readtime:` Time will take to read the blog.

  In order to add new author, we need to add a entry in file `docs/blog/post/.authors.yml` like below

  ```markdown
  authors:
    shubhamdang:
      name: Shubham Dang
      description: Creator
      avatar: https://avatars.githubusercontent.com/u/15174448
      url: https://github.com/shubhamdang
  ```

- Test the Blog:
  Once the content is added we can test the blog by running the command `mkdocs serve` and go to Blogs section.

- Excerpt the Blog:
  In blog listing, by defualt all the content of the blog is shown. If we wanted to show some initial lines of the blogs then we can add a excerpt in the blog content.

  ```markdown
  ---
  authors:
    - shubhamdang
  date: 2024-03-03
  categories:
    - Kubernetes
  readtime: 2
  ---

  # Blog Title
  ...
  blog content start from here.
  <!-- more -->
  some more content is here.
  ```

  According to example it will show content above `<!-- more -->` in the blog listing and rest will come as `Continue reading`.


## Content Things to Look Into

- Code Annotations (adds pop-ups with notes on commands/info) 
    - See "Code annotations" in material docs
- Rename sections to prettier naming


## Steps to create/update docs

- Checkout to main branch and pull the latest code
  ```bash
  git checkout main && git pull
  ```

- Create a new branch from main branch.
  ```bash
  git checkout -b branch-name
  ```

- Add your changes for docs.

- Commit your changes and push the branch for PR
  ```bash
  # git status to check the files changed
  git status
  # Add files to the staging Area
  git add <file-name>
  # commit changes
  git commit -m "commit message"
  # push changes to remote
  git push -u origin branch-name
  ```

- Then raise the PR to merge the changes in main branch.
