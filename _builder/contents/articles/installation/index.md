---
title: Installation
template: article.jade
---

# Overview
In this Tutorial we will explain how to set up your own instance of DIVAServices. This Tutorial is split into three main parts:

 - [Setting up a Docker instance](#setting-up-a-docker-instance)
 - [Setting up the File Server](#setting-up-the-file-server)
 - [Setting up DIVAServices](#setting-up-divaservices))
 
## Setting up a Docker instance
DIVAServices requires a [Docker](https://docker.io) instance running on a server. This can be the same server or a different one. We suggest to use a clean Ubuntu LTS operating system (currently 16.04).

For simplicity we refer here to the installation documentation for Docker itself:
 - [Install Docker on Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

Additionally the Docker Remote API needs to be enabled:
 - [Enable Docker Remote API](https://docs.docker.com/install/linux/linux-postinstall/#configure-where-the-docker-daemon-listens-for-connections)
    - Set it do listen to `0.0.0.0:2375` as shown in the Tutorial

We suggest to additionally perform the following steps:
- [Manage Docker as non-root user](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)
- [Configure Docker to start at boot](https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot)

### Additional Tool
Additionally the server running Docker needs to have the following tools installed
- [cwltool](https://github.com/common-workflow-language/cwltool) the executer for the Common Workflow Language
    - Installation instructions are provided on the Github Page
- [OpenSSH](http://ubuntuhandbook.org/index.php/2016/04/enable-ssh-ubuntu-16-04-lts/) with login enabled for the account running docker
    - **Note** DIVAServices currently uses SSH with simple username/password authentication. This is not high security and it is therefore recommended that the Docker server does not have direct exposure to the internet.

## Setting up the File Server
DIVAServices needs access to a location in which it can store its files. This can be on the same server where DIVAServices is installed, or on a separate server.

In order to share data between the different instances we suggest setting up a [Network File System](https://help.ubuntu.com/community/SettingUpNFSHowTo) to which all different involved machines have access to.

## Setting up DIVAServices
DIVAServices can be set up in two different ways:
 - as a Docker container
 - running locally as a regular application using Node.JS

We suggest to run it as a Docker container as it is simpler to install and to maintain.

### Install DIVAServices using docker-compose

#### Downloading the DIVAServices sources
The first step for the insallation is to clone the github repository:

```bash
git clone https://github.com/lunactic/DIVAServices.git
```
**Note** If you want to run the latest updates check out the `development` branch (this can be unstable)

#### Build the Docker image
The docker image can be built with [docker-compose](https://docs.docker.com/compose/install/).

In the `docker-compose.yaml` file you need to change the mountpoints for the volumes:
``` YAML
version: '3.3'
services:
  dev_web:
    env_file:
     .env_dev
    build: .
    command: yarn run start
    ports:
     - "8080:8080"
    volumes:
     - .:/code
     - PATH_TO_YOUR_DATA_LOCATION:/data
```

And then you can build the Docker Image using
```bash
docker-compose -f docker-compose.yaml build
```