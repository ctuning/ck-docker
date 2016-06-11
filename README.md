Automating creation and execution of Docker images via CK
=========================================================

Status
======
beta, relatively stable

Prerequisites
=============

## Linux
### Install Docker

To install Docker, please refer to the official 
[Docker installation instructions](https://docs.docker.com/engine/installation/). 
Please make sure that you can run the "hello-world" example:
```
$ docker run hello-world

Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```
**NB:** To run Docker without `sudo` on Linux, create a `docker` user group 
(e.g. see instructions for Ubuntu [here](https://docs.docker.com/engine/installation/linux/ubuntulinux/#create-a-docker-group).

## Windows

### Install Oracle VirtualBox

Download and install it from https://www.virtualbox.org/wiki/Downloads

Note, that if you plan to use devices connected via USB
(for example, access Android devices via ADB), you also need 
to install VirtualBox Extension Pack (you can download it from above page).

### Install Docker

Follow these guidelines:
* https://docs.docker.com/engine/installation/windows

Note that if you plan to use USB devices, you need
to update "default" VirtualBox image (created by Docker
after installation) and enable USB (USB2 or USB3).

### Obtain Docker IP
You can obtain internal IP of the docker machine
(required for CK web services) using the following command:
```
 $ docker-machine ip
```

Authors
=======

* Anton Lokhmotov, dividiti (UK)
* Grigori Fursin, cTuning foundaton (France)

License
=======
* BSD, 3-clause

Installation
============
```
 $ ck pull repo:ck-docker
```

Usage
=====

### List existing Docker images in CK:

```
 $ ck list docker
```

### Build a given image locally (for example, to read CK-based interactive article):

```
 $ ck build docker:ck-ubuntu-16.04-interactive-report
```

### Run Docker image (for example, interactive paper)

```
 $ ck run docker:ck-ubuntu-16.04-interactive-report
```

Now, you should be able to view interactive article via browser:

```
 $ firefox http://localhost:3344/web?wcid=1e348bd6ab43ce8a:b0779e2a64c22907
```

You can also start a CK dashboard simply via:
```
 $ firefox http://localhost:3344
```

### Participate in GCC crowd-tuning:

```
 $ ck build docker:ck-ubuntu-16.04-crowdtune-gcc
 $ ck run docker:ck-ubuntu-16.04-crowdtune-gcc
```

### Use CK as a remote web service

```
 $ ck build docker:ck-ubuntu-16.04
 $ ck run docker:ck-ubuntu-16.04
```

### Customize CK server host and ports:

```
 $ export WFE_HOST=123.456.0.78 WFE_PORT=9999 CK_PORT=3344
 $ ck run docker:ck-ubuntu-16.04 --cmd=" -p ${WFE_PORT}:${CK_PORT} --env WFE_HOST=${WFE_HOST} --env WFE_PORT=${WFE_PORT} --env CK_PORT=${CK_PORT} --env CK_PORT=${CK_PORT}"

Starting CK web service on 172.17.0.2:3344 (configured for access at 123.456.0.78:9999) ...
```

### Create your own CK-based Docker image

Select the most close Docker image in CK and copy it to a new CK entry:

```
 $ ck cp docker:ck-ubuntu-16.04-interactive-report :my-new-image
```

Find its path:

```
 $ ck find docker:my-new-image
```

Edit Dockerfile and .cm/meta.json in this directory.

When ready, build your image

```
 $ ck build docker:my-new-image
```

and then run it

```
 $ ck run docker:my-new-image
```

That's all!


Misc notes
==========

If you want to access devices connected via USB from Docker image, 
you need to run Docker as following:

```
 $ docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb ...
```
