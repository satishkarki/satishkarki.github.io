---
layout: post
title: "Docker: Top-Down Overview"
date: 2026-08-04
categories: DOCKER
tags: contianer vm linux
image:
    path: assets/img/post/docker/docker-getting-started.png
---

> It works on my machine !!!

I am using Fedora VM so I am following [installtion on Linux](https://docs.docker.com/desktop/setup/install/linux/) guide for Docker Desktop.

Under general requirement, it says Docker desktop depends on KVM virtualization and QEMU. What does it mean?

Don't be confused. Docker itself doesn't normally depend on KVM or QEMU. Docker engine runs directly on Linux. Docker containers share the host’s Linux kernel. Docker uses Linux features called namespaces to keep containers separated and control groups, or cgroups, to limit CPU and memory use. It does not need to create a separate virtual computer for every container.

On the other hand, Docker Desktop creates a small Linux virtual machine. That VM uses:
* KVM to run efficiently on the real CPU.
* QEMU as part of the virtual-machine environment.
* Docker Engine inside the VM.
* Containers inside that Docker Engine.

Lets briefly tocuh on KVM and QEMU before diving into Docker Desktop installtion. 
QEMU as the name suggest- Quick Emulator creates virtual machine model- motherboard, firmware, disks, network cards, display, USB ect. It can even emulate the CPU. But the software emulated CPU can be slow- thats where the KVM comes in. Kernal-based Virtual Machine turns Linux kernel into a hardware assisted hypervisor.  It lets normal guest CPU instrcutions run directly on the host CPU. 

![KVM QEMU](assets/img/post/docker/kvm-qemu.png)



## Installation

**Installing using rpm repository**

1. [Docker's package repository](https://docs.docker.com/engine/install/fedora/#set-up-the-repository)

   ```bash
   # Set up the repository
   sudo dnf config-manager addrepo --from-repofile https://download.docker.com/linux/fedora/docker-ce.repo
   ```
2. Download the latest [RPM package](https://desktop.docker.com/linux/main/amd64/docker-desktop-x86_64.rpm?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64). 

3. Install the pacakge with dnf
   ```bash
   sudo dnf install ./docker-desktop-x86_64.rpm
   ```
Unfortunately, the above process didn't work for me. I am using M3 mac, runing VMware hypervisor on it and running Fedora inside it, now I am tryring to install Decoker desktop (another VM) with in Fedora (another VM). 
```bash
M3 Mac
└── VMware Fusion VM
    └── Fedora
        └── Docker Desktop VM
            └── Containers
```


This is virtualization inside virtualization. Broadcom states that nested hypervisors are generally unsupported in Fusion and that Apple-silicon Fusion hosts do not currently provide nested virtualization.

Instead, lets roll with docker engine, who cares about the docker GUI, I am a command line hero, yeah- lets do it.  Only if I knew how to get out of VIM :). Here is how it will look now.

```bash
M3 Mac
└── VMware Fusion
    └── Fedora ARM64
        └── Docker Engine
            ├── Container A
            └── Container B
```

I am going to put a hold on this thought for now. Let's pivot to installing Docker Desktop for MacBook. We will revisit the docker engine after gaining familiarity with the Docker Desktop.

### Installation on MacBook

Download link [Apple Silicon](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-arm64)


**Running first container**
```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```
![First Container](assets/img/post/docker/first-container.png)

And here is how it looks:
```bash
http://localhost:8080
```

![Congrats](assets/img/post/docker/congrats.png)

> What is happening behind the scene? 

* docker run — Creates and starts a new container.
* -d — Runs the container in detached mode, meaning it stays in the background.
* -p 8080:80 — Maps port 8080 on your computer to port 80 inside the container.
* docker/welcome-to-docker — The Docker image used to create the container.

Useful follow-up commands:
```bash
docker ps # See the running container
docker logs <container-id> # View its logs
docker stop <container-id> # Stop Container
docker rm <container-id> # Remove the stopped Container
docker run -d --name welcome-app -p 8080:80 docker/welcome-to-docker # You can also give it a friendly name

docker stop welcome-app # manage it using the name
docker rm welcome-app # manage it using the name

docker run -d -p 8081:80 docker/welcome-to-docker # If port 8080 is already being used, change the first port number
```

```bash
docker start <container-id> # To start stopped container
docker rename <container-id> <new-name> # To rename already running container
```

```bash
docker images # To see locally downlaoded images
docker image inspect docker/welcome-to-docker # To see the full registry and repository information
```

## Develop with Containers

Resource [link](https://docs.docker.com/get-started/introduction/develop-with-containers/)

```bash
git clone https://github.com/docker/getting-started-todo-app # Downlaod the project
cd getting-started-todo-app 
```

```bash
docker compose watch

docker compose stop # to stop
```
Wow, what just happened? I am blown away. The things going on behind the scene is insane, I will definitely revisit it later but for now let's cut the noise and highlight key things that happened.

Lets start with the project file. Here is the snippet:

![code file](assets/img/post/docker/code-file.png)

And here is the snippet from terminal

![terminal-command](assets/img/post/docker/terminal-output.png)

>What is happening?
* It appears, Docker reads compose.yaml file. That file describes the complete application environment- not just one container.
* Docker download these images from Docker Hub when they are not already available locally (as in terminal snippet above)
* Application images are built locally - The frontend and backend are built from the project’s Dockerfile (Confusing, I will revisit this later)
* Compose creates a private Docker network so containers can communicate using their service names. Meaning- In the below screenshot, we can see the app is made up of five containers and they need to communicate with each other.
![multiple-container](assets/img/post/docker/multi-container.png)
 

```bash
docker compose down # Remove the container and network

docker compose down --volumes # To explicitly remove the volumes for e.g MySQL
```
![docker-down](assets/img/post/docker/docker-down.png)

Now that we are a little bit familiar with the development process. We will get our feet wet by moving on to Build and push process.

## Build and push your first image

Before proceeding, let's clarify the confusion between container and image.
```shell
Dockerfile
    |
    | docker build
    v
Docker Image
    |
    | docker run
    v
Container
```
```shell
docker run -d -p 8080:80 docker/welcome-to-docker
```
Here:

* docker/welcome-to-docker = image
* running welcome website = container

> How do you define container? 

A running, isolated environment that contains an application and everything the application needs to run.
```bash
Container has
├── Application code
├── Runtime
├── Libraries
├── Configuration
├── Internal filesystem
└── Startup process
```
Let's get back to the topic:

If you already don't have the account for Docker, please create one. I am using the same account I used for Docker hub and it is working fine.

Here is the link to [Docker Home](https://app.docker.com)

### Creating an image repository
![docker-repo](assets/img/post/docker/docker-repo.png)

Here is how my repo looks like fater signing in and creating the repo.
* Go to Docker Hub.
* Select Create repository.
* On the Create repository page, enter the following information:
    * Repository name - getting-started-todo-app
    * Short description - feel free to enter a description if you'd like
    * Visibility - select Public to allow others to pull your customized to-do app

### Build and Push the image using CLI
```bash
git clone https://github.com/docker/getting-started-todo-app

cd getting-started-todo-app
```
```bash
docker build -t <DOCKER_USERNAME>/getting-started-todo-app . # t stands for tag, implicitly "latest" tag is attached 

# Lets say I want to version it then the command will look something like this
docker build -t satishkarki/getting-started-todo-app:v1 .
```
```bash
docker image ls # To verify the image exists locally

docker push <DOCKER_USERNAME>/getting-started-todo-app
```
The command here looks similar to how we work with the git. I love it. Kudos to who ever thought of it.

> One thing to notice here is that this example repository already includes docker file. It is a text-based script that provides the instruction set on how to build the image.

![docker-push](assets/img/post/docker/docker-push.png)

> Build and push process using the VS Code Extension is [here](https://docs.docker.com/get-started/introduction/build-and-push-first-image/) 


Now that we have gone through the quick rundown of how the docker works and how it helps solve the problem of "It works on my machine". Next we will look into the nitty gritty of the technical details including the docker file and compose.yml

Stay tuned!!!