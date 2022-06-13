# Dockerfile for Micro-GW Base Image #

This section defines the step-by-step instructions to build an [Ubuntu](https://hub.docker.com/_/ubuntu/) Linux based Docker image for Micro-GW base image.

## Prerequisites

* [Docker](https://www.docker.com/get-docker) v17.09.0 or above

## How to build an image

1. Navigate to `/dockerfiles/ubuntu/mg` directory.
2. Build the Docker image using the following command.

```docker build --no-cache=true -t wso2/wso2micro-gw:3.2.4-ubuntu .```
   
> By default, the Docker image will prepackage the General Availability (GA) release version of the relevant WSO2 product.

## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
