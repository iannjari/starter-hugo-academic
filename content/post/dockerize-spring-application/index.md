---
title: Dockerizing a Spring Boot Application

# Date published
date: "2022-04-07T00:00:00Z"


# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: true

authors:
- admin

tags:
- Spring

categories:
---

Containerizing applications is the to go method of shipping applications from their development environments to deployment ones such as cloud without worrying about 
dependencies breaking due to a change of environments.

Docker is the most widely tool for this purpose. 

This guide assumes you have a fully functional Spring Boot Application.


## Docker Installation
Visit [Get Docker](https://docs.docker.com/get-docker/)  and click on the **Docker Desktop for Windows**, then click on the **Docker Desktop for Windows** button link.

Run the **Docker Desktop Installer.exe** file from Downloads.

After installation when you open docker you’ll be prompted to download Windows Subsystem for Linux (WSL) in a pop up window just click on the link to proceed with installation.


This will redirect you to the site where you’ll click on **WSL2 Linux kernel update package for x64 machines**.

Run the `wsl_update_x64.msi` file from Downloads.

Open PowerShell and run this command to set WSL 2 as the default version when installing a new Linux distribution:


```bash
wsl --set-default-version 2
```
Install Linux distribution of your choice from Microsoft store. Ubuntu 20.04 LTS was used for this setup.

You will then need to create a user account and password for your new Linux distribution.

Click Enter when you get the `Installing, this may take a few minutes…` message. 

Restart PC once done setting up Ubuntu.

You can also watch [this YouTube video](https://www.youtube.com/watch?v=lIkxbE_We1I) to assist in installation Installing Docker on Windows 10.

## Dockerfile

In the root directory of your spring project, create a new file named `Dockerfile`. 

**Note:** The filename does not have an extension, has to be exactly spelt and is case-sensitive.


