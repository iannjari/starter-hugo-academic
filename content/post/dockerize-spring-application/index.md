---
title: Dockerizing a Spring Boot Application
date: 2022-04-07T00:00:00Z
draft: false
featured: false
authors:
  - admin
tags:
  - Spring
categories: null
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)"
  focal_point: ""
  placement: 2
  preview_only: true
  filename: featured.jpg
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

Build your project using `gradle clean build` or `mvn package` from the root of the project in a terminal or using the IDE of your choice.
This generates a JAR file in the 'build/libs/` folder for gradle or `/target/` for maven.

In the Dockerfile, define the JDK used to build and compile the project, in this case, JDK 17 as follows;
```bash
FROM openjdk:17
```

Next, we need to expose the container's port 8080:
```bash
EXPOSE 8080
```

We then need to define the location of the JAR file to be contained in the container, in this case, `demo-v1.jar` in the folder where it was generated when the project 
build was performed.
Provide the relative path from the project root, and a desired name of the resulting image JAR in the format:
```bash
ADD path_to_jarfile desired_name_of_jar_in_docker_image
```
In our case,

```bash
ADD build/libs/demo-v1.jar tushughuli-v1.jar
```
Finally, define the entrypoint to the docker image, which is a way of defining what will be run when the container is run itself.

Use this format:
```bash
ENTRYPOINT ["java","-jar","/desired_name_of_jar_in_docker_image"]

In our case,
```bash
ENTRYPOINT ["java","-jar","/tushughuli-v1.jar"]
```

## Building the docker image

The grammar of the command to build a docker image is as follows:

`docker image build [OPTIONS] PATH | URL | -`

To build an image, from the project root in a terminal, run:

`docker image build .`

**Note:** the `.` denotes that the Dockerfile is in the same folder the command is being run from.

Depending on the size of the project, this may take some time.

When complete, running `docker images` should show a list of local docker images including the last one.

## Conclusion
We have learnt how to install docker, create a Dockerfile and build a docker image from a Spring Boot Application.

In a subsequent guide, we'll learn how to save this image on Docker Hub, and deploy it to Google Kurberntes Engine (Cloud).

