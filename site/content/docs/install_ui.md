---
title: "SNAS UI Installation"
date: 2017-01-04T15:04:10.000Z
---

SNAS User Interface (UI) Docker Install Steps
==============================================

<!--more-->

Before installing the UI container make sure you are running the latest db_rest, MySQL schema, and openbmpd.  Get the latest at [github.com/openbmp/docker](http://github.com/openbmp/docker).

The UI container includes:

* **nginx** - Web Server
* **ui** - SNAS UI

> #### NOTE
> **Link-State SPF** stored procedures are **not** included.  Request them from *tievens@cisco.com*
> if you plan to use link-state with path visualizations.

### Recommended Current Linux Distributions

  1. Ubuntu 16.04/Xenial
  1. CentOS 7/RHEL 7


### **Installation Steps**

- - -

### 1) Install docker

Docker host should be **Linux x86_64**.   Follow the [Docker Instructions](https://docs.docker.com/installation/) to install docker.  

### 2) Download the docker image

    docker pull openbmp/ui

### 3) Run the docker container

#### Memory Requirements: 
Tomcat is configured to expect at least **1G** of RAM.

#### Environment Variables: 
Below table lists the environment variables that can be used with ``docker -e <name=value>``


NAME | Value | Details
:---- | ----- |:-------
**OPENBMP\_API\_HOST** | host or ip | Hostname or IP address of OpenBMP API service. The default is **localhost**
**OPENBMP\_API\_PORT** | port | Port for the OpenBMP API service. The default is **8001**

You only need to set the OPENBMP\_API\_HOST if you have installed the MySQL consumer or AIO container on a different host/machine.

#### Run
     docker run -d --name=openbmp_ui \
         -e OPENBMP_API_HOST=localhost \
         -p 8000:8000 \
         openbmp/ui

#### Login

The default login is defined/set in the database in AIO or MySQL consumer container.   The default is as follows:

> Username: **openbmp**
> Password: **CiscoRA**

You should be able to login to the UI by accessing [http://HOST:8000/](http://HOST:8000)

- - -

### **Troubleshooting and Monitoring**
You can use standard docker exec commands to monitor the log files.  To monitor tomcat, use:  

```docker exec openbmp_ui tail -f /var/log/tomcat7/catalina.out```

Alternatively, it can be easier at times to navigate all the log files from within the container. You can connect to the docker container using:

```docker exec -it openbmp_ui bash```

You can monitor the docker container by getting the console logs. This is useful if the container exits due to invalid start or for another reason.

```docker logs openbmp_ui``` 

- - -

### **System Start/Restart Config (Ubuntu 16.04/Xenial)**
 By default, the containers will not start automatically on system boot/startup.  You can use the below example to instruct the container to start automatically.

You can read more at [Docker Admin Guide](https://docs.docker.com/engine/admin/start-containers-automatically/) on how to start containers automatically. 

 > #### IMPORTANT
 > The ```--name=openbmp_ui``` parameter given to the ```docker run``` command is used with the ```-a openbmp_ui``` parameter below to start the container by name instead of container ID.  You can use whatever name you want, but make sure to use the same name used in docker run.

     cat <<END > /etc/init/openbmp_ui.conf
     description "SNAS UI container"
     author "tievens@cisco.com"
     start on filesystem and started docker
     stop on runlevel [!2345]
     respawn
     script
       /usr/bin/docker start -a openbmp_ui
     end script
     END