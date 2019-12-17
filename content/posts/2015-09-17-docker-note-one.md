---
title: "Docker Note One"
date: "2015-09-17"
template: "post"
draft: false
slug: "docker-note-one"
category: "docker"
tags:
  - "docker"
description: ""
socialImage: ""
---

### images & containers

- A container is a stripped-to-basics version of a Linux operating system.
- An image is software you load into a container.

### Dockerfile

- The `FROM` keyword tells Docker which image your image is based on.
- Build an image from your Dockerfile
- `docker build -t docker-whale .`
- `docker run docker-whale`

### Tag, push, and pull your image

- `docker tag 7d9495d03763 anggao/docker-whale:latest`
- `docker login to log into the Docker Hub`
- `docker push anggao/docker-whale`

### Remove images

- `docker rmi -f 7d9495d03763`
- `docker rmi -f docker-whale`

### Pull images

- `docker pull anggao/docker-whale`

### install Docker using Docker Toolbox

- Docker Machine for running the docker-machine binary
- Docker Engine for running the docker binary
- Docker Compose for running the docker-compose binary
- Kitematic, the Docker GUI

- You use docker-machine to create and attach to a virtual machine (VM).

- In networking, localhost means your computer.
- The Docker host is the computer(system) on which the containers run.
- In an OS X installation, the docker daemon is running inside a Linux VM called default
- The VM runs completely from RAM, is a small ~24MB download, and boots in approximately 5s

### docker-machine

- Create a new Docker VM.
- `docker-machine create --driver virtualbox default`
- The command also creates a machine configuration in the `~/.docker/machine/machines/default`
- List your available machines.
  - `docker-machine ls`
- Get the environment commands for your new VM.
  - `docker-machine env default`
- To remove a VM
  - `docker-machine rm <machine-name>`

### Access container ports

- Start an NGINX container on the `DOCKER_HOST`
  - `docker run -d -P --name web nginx`
  - `-d` flag keeps the container running in the background
  - The `-P` flag publishes exposed ports from the container to your local host; this lets you access them from your Mac
- To stop and then remove your running nginx container, do the following:
  - `docker stop web`
  - `docker rm web`

### Mount a volume on the container

- When you start a container it automatically shares your `/Users/username` directory with the VM.
- You can use this share point to mount directories onto your container.
- `docker run -d -P -v $HOME/site:/usr/share/nginx/html --name mysite nginx`
- Get the mysite container’s port.
- `docker port mysite`
