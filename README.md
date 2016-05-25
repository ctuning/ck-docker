# Collective Knowledge repository for Docker images

## Install Docker

To install Docker, please refer to the official [Docker installation instructions](https://docs.docker.com/engine/installation/). Please make sure that you can run the "hello-world" example:
```
$ docker run hello-world

Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```
**NB:** To run Docker without `sudo` on Linux, create a `docker` user group (e.g. see instructions for Ubuntu [here](https://docs.docker.com/engine/installation/linux/ubuntulinux/#create-a-docker-group).


## Build a CK Docker image
```
$ docker build -t ctuning/ck-ubuntu-16.04 docker/ubuntu-16.04
```

## Run a CK Docker image
```
$ docker run -it ctuning/ck-ubuntu-16.04
```
