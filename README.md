Automating creation and execution of Docker images via CK
=========================================================

[![logo](https://github.com/ctuning/ck-guide-images/blob/master/logo-powered-by-ck.png)](http://cKnowledge.org)
[![logo](https://github.com/ctuning/ck-guide-images/blob/master/logo-validated-by-the-community-simple.png)](http://cTuning.org)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

This repository helps you automate various Docker tasks
including "build", "run" and "push" via CK. Useful
to build and deploy CK-powered Docker images 
for example to crowdsource autotuning and other experiments,
or share your projects for [Artifact Evaluation](http://cTuning.org/ae)
at conferences. You can find several such Docker images prepared 
via CK [here](https://hub.docker.com/u/ctuning).
If you have questions and comments, feel free to get
in touch with [the community](https://groups.google.com/forum/#!forum/collective-knowledge the community)!

Status
======
Stable repository

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
* Grigori Fursin, cTuning foundation (France)

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

### List existing Docker images in the CK:

```
 $ ck list docker
```

### Run a given Docker image:

```
 $ ck run docker:ck
```

This command will download Docker image with Ubuntu and CK installed,
and will start an interactive bash session.

If you need sudo to run docker on your machine, use flag '--sudo' as following:
```
 $ ck run docker:ck --sudo
```

If a given image was not shared with Docker Hub, 
you should build it locally.

### Build a given image locally 

```
 $ ck build docker:ck
```

If you need sudo to run docker, use flag '--sudo' as following:
```
 $ ck build docker:ck --sudo
```

If build succeeded, you can run this image via
```
 $ ck run docker:ck
```

Examples
========

## Use CK web server (JSON API)

We have shared CK-based web server via Docker Hub. You can run it as following:

```
 $ ck run docker:ck-web-server

 Starting CK web service on 0.0.0.0:3344 (configured for access at 127.0.0.1:3344) ...
```

On Linux you should be able to access it via any browser as following:
```
 $ firefox http://localhost:3344

```

On Windows Docker will use a different IP such as 192.168.99.100 .
In such case you should use it to browse CK repository, i.e.
```
 $ firefox http://192.168.99.100:3344
```

Alternatively, you can ask CK to automatically detect this IP and start web server as following:
```
 $ ck run docker:ck-web-server --browser
```

If you plan to use it externally or in a workgroup, you will need to set
external IP and port as following:

```
 $ export WFE_HOST=123.456.0.78 WFE_PORT=9999 CK_PORT=3344
 $ ck run docker:ck-ubuntu-16.04 --cmd=" -p ${WFE_PORT}:${CK_PORT} --env WFE_HOST=${WFE_HOST} --env WFE_PORT=${WFE_PORT} --env CK_PORT=${CK_PORT} --env CK_PORT=${CK_PORT}"

Starting CK web service on 172.17.0.2:3344 (configured for access at 123.456.0.78:9999) ...
```

Note, that WFE_HOST is external IP and WFE_PORT is external port 
which CK web front end (dashboard) will use when preparing links 
in html pages. CK_HOST is internal host (can be left blank)
and CK_PORT is internal port.

Finally, if you do not have CK installed, you can run Docker image 
with the CK web server directly:
```
 $ docker run --rm -p 3344:3344 ctuning/ck-web-server
```

and you can customize it as following:
```
 $ docker run --rm --env CK_HOST=0.0.0.0 --env WFE_HOST=localhost --env CK_PORT=3344 --env WFE_PORT=3344 -p 3344:3344 ctuning/ck-web-server
```

## Browse interactive article and reproduce experiments

We converted parts of our following papers and their results into CK-based interactive articles
(to let the community reproduce our results and build upon them):
* http://arxiv.org/abs/1506.06256 (CPC'15)
* https://hal.inria.fr/hal-01054763 (JSP'14)
* http://bit.ly/ck-date16 (DATE'16)
* http://bit.ly/ck-multiprog16 (MULTIPROG'16)

You can download related Docker image and browse it as following:
```
 $ ck run docker:ck-interactive-article --browser
```

Alternatively, you can run it manually via docker:
```
 $ docker run --rm ctuning/ck-interactive-article
 $ firefox http://localhost:3344/web?wcid=1e348bd6ab43ce8a:b0779e2a64c22907
```

On Windows, you will need to change localhost to IP reported via 'docker-machine ip'
as described in the previous sub-section.

You can replay experiments from above papers (by copy/pasting replay CMD
from the web dashboard) on your machine  by running above Docker image 
in the interactive mode:

```
 $ ck run docker:ck-interactive-article-cmd
 $ ck replay experiment ...
```

You can build this Docker image locally via

```
 $ ck build docker:ck-interactive-article
```

If you need sudo to run docker, use flag '--sudo' as following:
```
 $ ck build docker:ck-interactive-article --sudo
```

## Participate in GCC crowd-tuning

You can participate in GCC crowd-tuning (i.e. collaboratively
tuning optimization heuristic of GCC and sharing experimental results
via public repository http://cknowledge.org/repo as following:

```
 $ ck run docker:ck-crowdtune-gcc

or

 $ docker run ctuning/ck-crowdtune-gcc
```

However, we suggest to use CK natively. In such, case you will
be able to take advantage of your latest environment and GCC compiler
as following:

```
 $ ck pull repo:ck-crowdtuning
 $ ck crowdtune program --gcc
```

## Participate in LLVM crowd-tuning

Similar to GCC crowd-tuning you can collaboratively tune LLVM optimization
heuristic on your machine via
```
 $ ck run docker:ck-crowdtune-llvm

  or

 $ docker run ctuning/ck-crowdtune-llvm
```

## Create your own CK-based Docker image

Select the most close Docker image in CK and copy it to a new CK entry:

```
 $ ck cp docker:ck-ubuntu-16.04-interactive-report :my-cool-image
```

Find its path:

```
 $ ck find docker:my-cool-image
```

Edit Dockerfile and .cm/meta.json in this directory.

When ready, build your image (and specify your own organization,
to avoid using 'ctuning' as default)

```
 $ ck build docker:my-cool-image --org=my-org
```

and then run it

```
 $ ck run docker:my-cool-image
```

You can also login to the Docker Hub and push your image 
to your account via
```
 $ ck login docker
 $ ck push docker:my-cool-image --org=my-org 
```

That's all!

Misc notes
==========

If you want to access devices connected via USB from Docker image, 
you need to run Docker as following:

```
 $ docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb ...
```

Feedback
========
* https://groups.google.com/forum/#!forum/collective-knowledge
