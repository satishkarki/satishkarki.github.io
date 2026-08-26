---
layout: post
title: "Docker: Core Concepts"
date: 2026-08-24
categories: DOCKER
tags: contianer images volumes
image:
    path: assets/img/post/docker-core-concepts/Docker-Core-Thumbnail.png
---
After having a good look at the top-down overview of how docker works. Let's dive into the core concept by following the official [docker workshop](https://docs.docker.com/get-started/workshop/)
* Docker: The tool/platform that manages everything
* Image: The packaged blueprint/template for an application
* Container: an instance created from that image, usually where the application actually runs

```bash
Dockerfile
    |
    | docker build
    v
  IMAGE
    |
    | docker run
    v
 CONTAINER
 ```

## Two important principles of images
* Images are immutable. Once an image is created, it can't be modified. You can only make a new image or add changes on top of it.
* Containers are composed of layers. Each layer represents a set of file system changes that add, remove, or modify files.

```bash
                IMAGE

        ┌─────────────────┐
        │ Layer 4: app.py │
        ├─────────────────┤
        │ Layer 3: Flask  │
        ├─────────────────┤
        │ Layer 2: Python │
        ├─────────────────┤
        │ Layer 1: Ubuntu │
        └─────────────────┘
                ↑
            all read-only
            all immutable


            docker run
                ↓


             CONTAINER

        ┌─────────────────┐
        │ Writable layer  │ ← container changes
        ├─────────────────┤
        │ Layer 4         │
        ├─────────────────┤
        │ Layer 3         │
        ├─────────────────┤
        │ Layer 2         │
        ├─────────────────┤
        │ Layer 1         │
        └─────────────────┘
```
> Docker doesn't edit an existing image. It builds new filesystem changes as layers, while containers get their own writable layer on top of the image.

## Registry 
An image registry is a place where Docker images are stored and distributed.

Think of it like an app store for Docker images.
[Docker Hub](https://hub.docker.com/) is a public registry that anyone can use and is the default registry.



> Note : A registry is a centralized location that stores and manages container images, whereas a repository is a collection of related container images within a registry. 

```bash
Docker Hub                ← Registry

nginx                     ← Repository
├── nginx:1.27
├── nginx:1.28
└── nginx:latest

redis                     ← Repository
├── redis:7
├── redis:8
└── redis:latest
```

## Docker Compose

`Dockerfile` describes how to build an image and `compose.yaml` describes how to run one or more containers together.
```bash
Dockerfile
    |
    | docker build
    v
  IMAGE
    |
    |
    +-------------------+
                        |
                  compose.yaml
                        |
                        | docker compose up
                        v
                 CONTAINER(S)
```
Conceptually :

```bash
docker compose up -d --build
```

```bash
Dockerfile
    |
    | --build
    v
new/rebuilt image
    |
    | up
    v
container
    |
    | -d
    v
runs in background
```
## Docker Workshop

I am following the [Docker Workshop](https://docs.docker.com/get-started/workshop/) guide.


## Part 1: [Containerize an app](https://docs.docker.com/get-started/workshop/02_our_app/)

### Step A. Clone the repo
```bash
git clone https://github.com/docker/getting-started-app.git
```

### Step B. Creating the dockerfile
```dockerfile
# syntax=docker/dockerfile:1

FROM node:24-alpine
WORKDIR /app
COPY . .
RUN npm install --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000
```
This Dockerfile does the following:

* Uses node:24-alpine as the base image, a lightweight Linux image with Node.js pre-installed
* Sets /app as the working directory
* Copies source code into the image
* Installs the necessary dependencies
* Specifies the command to start the application
* Documents that the app listens on port 3000

```bash
Your final image
┌───────────────────────┐
│ Your application      │
├───────────────────────┤
│ npm dependencies      │
├───────────────────────┤
│ Node.js               │
├───────────────────────┤
│ Alpine Linux          │
└───────────────────────┘
```

### Step C. Build the image
```bash
docker build -t getting-started .
```
```bash
docker build -t getting-started .
│      │     │        │         │
│      │     │        │         └─ build context: current directory
│      │     │        └───────── image name
│      │     └────────────────── tag/name option
│      └──────────────────────── build an image
└─────────────────────────────── Docker CLI
```

### Step D. Start an app container
```bash
docker run -d -p 127.0.0.1:3000:3000 getting-started
```
```bash
docker run -d -p 127.0.0.1:3000:3000 getting-started
│      │   │  │         │    │       │
│      │   │  │         │    │       └─ image
│      │   │  │         │    └──────── container port
│      │   │  │         └───────────── host port
│      │   │  └─────────────────────── host IP
│      │   └────────────────────────── publish a port
│      └────────────────────────────── detached/background
└───────────────────────────────────── Docker
```

Let's say I want to upload this image to my docker hub (repo). Here is how I will do it:
```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```
***Example***
```bash
docker tag getting-started:latest username/getting-started:v1.0
docker push username/getting-started:v1.0
```

![docker-push](assets/img/post/docker-core-concepts/docker-push.png)

### Step E. Stop an app container
```bash
docker stop <container id> # This will stop the container only
docker rm <contianer id> # Remvoe the container

# To find the running contianers we can use 
docker ps # Shows running containers
docker ps -a # Shows running and stopped containers as well
```

### Step F. What if I remove the image file 
```bash
docker rmi getting-started
#OR
docker image rm getting-started
```
One thing to note is that, the following command will not delete the remove repo
```bash
docker rmi username/getting-started:v1.0
```
If we need to pull the image again then we can use 
```bash
docker pull username/getting-started
```

Here, after removing the local image and pulling it from repo, I noticed the image is name as `username/getting-started:v1.0` instead of `getting-started:latest`.  No worries, we can create a local alias.

```bash
docker tag username/getting-started:v1.0 getting-started:v1.0
```
![docker-alias](assets/img/post/docker-core-concepts/docker-alias.png)

On the above screenshot, as we can see the alias points to same docker image ID.

I will stop here and let's move on to next topic. Hopefully, I am expecting `Part 2` will cover the images immutable feature and also image versioning. That would be cool. Let's dig in.

## Part 2 : [Update the application](https://docs.docker.com/get-started/workshop/03_updating_app/)


Looks like this cover the `Steps E` from `Part 1`.
### Step A. Update the source code
```js
- <p className="text-center">No items yet! Add one above!</p>
+ <p className="text-center">You have no todo items yet! Add one above!</p>
```
### Step B. Build the updated version

```bash
docker run -dp 127.0.0.1:3000:3000 getting-started:v2.0
```

![docker-rebuild](assets/img/post/docker-core-concepts/docker-rebuild.png)

Here as you can see on the screenshot, it didn't throw any error as I already stopped and removed the container created by image `getting-started:v1.0`. What if it was running? - I will be getting `Bind for 127.0.0.1:3000 failed:` because the old container is already using the host's port 3000 and only one process on the machine(containers included) can listen to specific port.

Let's try the other way around. Let's spin on the container from v1.0 while v2.0 is still running to test the theory.

Tadaaa!!!

![port-duplication](assets/img/post/docker-core-concepts/same-port.png)

I am more surprised with the suggestion `docker ai "help me fix this container error"`. Are we gonna be so dependent on AI that we eventually have to ask GPT-Hey how to wipe my ass? Where is human intelligence going collectively? Is this what singularity looks like? AI improving day by day and human intelligence degrading second by second? I guess so. Ironically, I had to ask the GPT- ` docker ai` is Docker's new feature?  And yes, it is docker's built-in AI assistant `Gordon`. Hey `Gordon` I hope you don't mind my little rant. Anyway, lets move on, getting side-tracked here.

>Note: It seems the container didn't start, but it was created. So it will be wise to use a different port if you want to run it or if not remove it.

### Step C. If I want to push v2.0 to my repo

```bash
docker tag getting-started:v2.0 username/getting-started:v2.0
docker push username/getting-started:v2.0
```
![push-v2](assets/img/post/docker-core-concepts/docker-push-v2.png)

Next we will look into sharing the application.

## Part 3 : [Share the application](https://docs.docker.com/get-started/workshop/04_sharing_app/)

Looks like this part covers the docker registry - Docker hub and repository. 

Curiosity got the best of me. Just to recap, we already set this up in Part 1 Section D.

```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```
Here is how my docker hub repo looks like. I am getting the gist of versioning as well.
![docker-hub](assets/img/post/docker-core-concepts/docker-hub.png)

## Part 4 : [Persist the DB](https://docs.docker.com/get-started/workshop/05_persisting_data/)

Let's look at container's filesystem first. When a container runs, it uses the various layers from an image for its filesystem. Each container also gets its own "scratch space" to create/update/remove files. Any changes won't be seen in another container, even if they're using the same image.

![Container nature](assets/img/post/docker-core-concepts/docker-db.png)

On the above screenshot, we ran a new Alpine container and created a `greeting.txt` file, with `-rm` we removed the container when file creation is finished.
```bash
docker run --rm alpine touch greeting.txt
```
Now if we try to check the stat of the `greeting.txt` after spinning on another container using the same image we get the error- cannot find the `greeting.txt` file.

```bash
docker run --rm alpine stat greeting.txt
```
### Container Volume
It provides the ability to connect specific filesystem paths of the container back to the host machine. If you mount a directory in the container, changes in that directory are also seen on the host machine. If you mount that same directory across container restarts, you'd see the same files.

### Step A. Create a volume and start the container

Let's look at the commands and let's try to understand what's going on.
```bash
docker volume create todo-db
```
It simply creates a [volume](https://docs.docker.com/engine/storage/volumes/). Let's check
```bash
docker volume ls
```

![docker volume](assets/img/post/docker-core-concepts/docker-volume-ls.png)

```bash
docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
```
Let's break it down.
```bash
docker run
│
├── -d
│     run in the background
│
├── -p 127.0.0.1:3000:3000
│     connect your computer's port 3000
│     to the container's port 3000
│
├── --mount
│     attach some storage
│
│   ├── type=volume
│   │     use a Docker volume
│   │
│   ├── src=todo-db
│   │     use the volume named "todo-db"
│   │
│   └── target=/etc/todos
│         make that volume available here
│         inside the container
│
└── getting-started
      image used to create the container
```
I am more interested in target `/etc/todos`. Why this path? The answer is in source code. 

![Sourcecode](assets/img/post/docker-core-concepts/source-code.png)

Look at the highlighted line of code - up until now, container would get its own "scratch space" to create/update/remove files - which in this case is `/etc/todos/todo.db` and once container is stopped and remove, it would go away.

Now with volume, here is what's happening:
```bash
Container                         Volume: todo-db
┌────────────────────────┐       ┌─────────────────────┐
│                        │       │                     │
│ /etc/todos ─────────────────►  │ todo.db             │
│      │                 │       │                     │
│      └── todo.db       │       └─────────────────────┘
│                        │
└────────────────────────┘
```
### Step B. Verify the data persist
* Add items to the todo list app http://127.0.0.1:3000/
* Remvoe the container
    ```bash
    docker rm -f <id>
    ```
* Start a new container 
    ```bash
    docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
    ```
* Yes, my todo list is still there.

### Step C. Dive into the volume
Where is Docker storing my data when I use a volume?
```bash
docker volume inspect todo-db
```
***Output***
```json
[
    {
        "CreatedAt": "2026-08-25T19:43:52Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": null,
        "Scope": "local"
    }
]
```
The `Mountpoint` is the actual location of the data on the disk.
```bash
Container sees:                 Docker stores:

/etc/todos                     /var/lib/docker/volumes/
       │                        todo-db/_data
       │                              ▲
       └──────────────────────────────┘
             mounted volume
```
To see mount point only:
```bash
docker volume inspect --format '{{ .Mountpoint }}' todo-db
```
## Part 5: [Use bind mounts](https://docs.docker.com/get-started/workshop/06_bind_mounts/)












