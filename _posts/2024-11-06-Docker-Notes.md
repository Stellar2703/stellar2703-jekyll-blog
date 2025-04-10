---
title: "Docker Notes"
date: 202-02-12 00:00:00 +0800
categories: [DevOps]
image: assets/images/imagesdocker.jpg
tags: [Docker notes]
---  
IMAGE : A single file with all the deployments and configurations required to run a program is called an image
It is a file system snapshot

CONTAINER : A container is an instance of an image
A container is a program that has its own hardware resources and memory, networking and storage

KERNEL : Is an intermediate program that governs access between the programs and the hardware resources

NAMESPACES : the process of segmenting the hardware and software resources based on the resource


**Process of creating a Dockerfile**

```
# Use an existing image as a base
FROM alpine

# Downlaod and install a dependency
RUN apk add --update redis

# Tell the image what to do when it starts as a container
CMD ["redis-server"]
```

- alpine is used as an infrastructure or architecture to the empty image of the container that is build
- apk is the package manager on alpine 
- for every step a new container is created and the snapshot of the container is created and the container gets deleted. then a new container is created for the next step with he FS snapshot of the old container.
- if you try to run docker build again then if the some the steps are same then it does not start from the first rather it uses the cache of the previous build file and continues.


Need for Docker-Compose
- if you want to run multi container application on docker then you should setup docker networking using a handful of docker cli commands which also should be run every time you restart the container.
- Docker-compose is a separate cli that gets installed already.
- Used to start up multiple Docker containers at the same time
- Automates some of the long-winded arguments we were passing to 'docker run' like -p sh etc