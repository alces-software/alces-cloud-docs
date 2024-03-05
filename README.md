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
