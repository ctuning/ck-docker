# Automating creation and execution of Docker images via Collectiv Knowledge Framework





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

To build a CK Docker image named `ctuning/ck-ubuntu-16.04` from a `Dockerfile` in `docker/ubuntu-16.04`, run:

```
$ docker build -t ctuning/ck-ubuntu-16.04 docker/ubuntu-16.04
```
from the directory containing this `README.md`.

## Run a CK Docker image

### Local web service

The CK web service can be run locally and accessed at `http://localhost:3344/` as follows:

```
$ docker run --rm -it ctuning/ck-ubuntu-16.04
```

### Remote web service

The CK web service can be run on a remote server (at an address `${WFE_HOST}` and with an opened port `${WFE_PORT}`) and accessed at `${WFE_HOST}:${WFE_PORT}` as follows:

```
$ export WFE_HOST=123.456.0.78 WFE_PORT=9999 CK_PORT=3344
$ docker run --rm -it -p ${WFE_PORT}:${CK_PORT} \
  --env WFE_HOST=${WFE_HOST} --env WFE_PORT=${WFE_PORT} --env CK_PORT=${CK_PORT} \
  ctuning/ck-ubuntu-16.04
Starting CK web service on 172.17.0.2:3344 (configured for access at 123.456.0.78:9999) ...
```

**NB:** The `${CK_PORT}` variable must be defined but its value is inconsequential.
