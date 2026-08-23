# 🐳 Docker Zero to Hero

## Complete Practical & Interview-Oriented Notes

> **Goal:** Go from “I don't know Docker” → “I can Dockerize a backend application, run MongoDB with Compose, manage containers, troubleshoot problems, use volumes/networks, and explain Docker in an interview.”

---

# TABLE OF CONTENTS

1. What is Docker?
2. Why Docker?
3. Docker vs Virtual Machine
4. Docker Architecture
5. Important Docker Terminology
6. Installing & Verifying Docker
7. Docker Images
8. Docker Containers
9. The `docker run` Command
10. Container Lifecycle
11. Port Mapping
12. Environment Variables
13. Docker Logs & Troubleshooting
14. `docker exec`
15. `docker inspect`
16. Docker Image Layers
17. Dockerfile
18. Building Your Own Image
19. Dockerizing a Node.js Application
20. Docker Hub & Image Publishing
21. Docker Volumes
22. Bind Mounts
23. Docker Networks
24. Container-to-Container Communication
25. Docker Compose
26. MongoDB + Mongo Express with Compose
27. Understanding Your `mongodb.yaml`
28. Developing with Docker
29. Common Docker Errors
30. Docker Cleanup Commands
31. Complete Command Cheat Sheet
32. Real Backend Workflow
33. Interview Questions
34. Docker Mental Model
35. Zero-to-Hero Learning Path

---

# 1. WHAT IS DOCKER?

## Simple Definition

**Docker is a platform that allows us to package an application together with its dependencies and run it in an isolated environment called a container.**

Instead of saying:

> “It works on my computer.”

Docker tries to make the environment reproducible.

For example, suppose your Node.js application needs:

```text
Node.js 22
MongoDB
npm packages
Environment variables
Specific system dependencies
```

You can package the application environment using Docker.

Then another developer or server can run essentially the same containerized application.

---

# 2. THE PROBLEM DOCKER SOLVES

Imagine you build a Node.js application.

Your computer:

```text
Ubuntu
Node 22
MongoDB 8
npm packages
Environment variables
```

Your friend's computer:

```text
Windows
Node 20
MongoDB 7
Different npm versions
Different configuration
```

Your application may work on your computer but fail on theirs.

Typical problems:

```text
"It works on my machine!"
```

Docker reduces these environment differences by packaging the application environment.

Conceptually:

```text
Application
     +
Dependencies
     +
Configuration
     ↓
 Docker Image
     ↓
 Docker Container
```

---

# 3. WHY DO WE USE DOCKER?

## Major benefits

### 1. Consistency

The application runs in a predictable environment.

### 2. Isolation

Applications can run separately.

Example:

```text
Container 1 → Node.js
Container 2 → MongoDB
Container 3 → Redis
```

### 3. Portability

The same image can run on:

```text
Developer laptop
      ↓
Testing server
      ↓
Cloud server
```

### 4. Fast startup

Containers generally start much faster than full virtual machines.

### 5. Easy deployment

You can build an image and deploy that image.

### 6. Easy development

You don't necessarily need to install every dependency directly on your host machine.

---

# 4. DOCKER VS VIRTUAL MACHINE

This is an important interview topic.

## Virtual Machine

A VM virtualizes hardware and runs a complete guest operating system.

```text
Physical Machine
       ↓
   Hypervisor
       ↓
 ┌───────────────┐
 │ VM            │
 │ Guest OS      │
 │ Application   │
 └───────────────┘
```

Multiple VMs can have separate operating systems.

---

# Docker

Docker containers share the host operating system kernel while isolating applications/processes.

```text
Physical Machine
       ↓
    Host OS
       ↓
 Docker Engine
       ↓
 ┌───────┬───────┬───────┐
 │ App A │ App B │ App C │
 │Container│Container│
 └───────┴───────┴───────┘
```

## Main difference

| Docker Container             | Virtual Machine                |
| ---------------------------- | ------------------------------ |
| Shares host kernel           | Has guest OS                   |
| Lightweight                  | Heavier                        |
| Starts quickly               | Usually slower                 |
| Uses fewer resources         | Uses more resources            |
| Application isolation        | Full OS isolation              |
| Images are generally smaller | VM images are generally larger |

### Important

Docker is **not simply a lightweight VM**.

The underlying isolation mechanism is different.

---

# 5. DOCKER ARCHITECTURE

The major components are:

```text
Docker Client
     ↓
Docker Daemon
     ↓
Docker Objects
 ┌────┬───────┬────────┐
 ↓    ↓       ↓
Images Containers Networks
             ↓
           Volumes
```

## Docker Client

The CLI you interact with:

```bash
docker ps
docker run
docker build
docker images
```

## Docker Daemon

The Docker daemon performs the actual container/image management.

## Docker Registry

A registry stores Docker images.

The most famous public registry is:

```text
Docker Hub
```

---

# 6. IMPORTANT DOCKER TERMINOLOGY

You must understand these words before learning commands.

---

## Image

An **image is a blueprint/template used to create containers.**

Example:

```text
ubuntu image
mysql image
mongo image
node image
```

Think:

```text
Image = Blueprint
```

---

## Container

A container is an instance created from an image.

```text
Image
  ↓
Container
```

Example:

```bash
docker run ubuntu
```

Docker uses the Ubuntu image to create a container.

Think:

```text
Image = Class
Container = Object
```

This analogy isn't technically perfect, but it is useful for remembering the relationship.

---

## Dockerfile

A Dockerfile contains instructions for building an image.

Example:

```dockerfile
FROM node
WORKDIR /app
COPY . .
CMD ["node", "server.js"]
```

---

## Registry

A registry stores images.

Example:

```text
Docker Hub
```

---

## Volume

A volume provides persistent storage for containers.

Useful for:

```text
MongoDB
MySQL
PostgreSQL
Application data
```

---

## Network

A Docker network allows containers to communicate with each other.

Example:

```text
Node.js
   ↓
Docker Network
   ↓
MongoDB
```

---

## Compose

Docker Compose allows you to define and run multiple services using a YAML file.

Example:

```text
Node.js
MongoDB
Redis
Mongo Express
```

can all be defined in one file.

---

# 7. CHECK WHETHER DOCKER IS INSTALLED

## Check version

```bash
docker --version
```

Example:

```text
Docker version 29.x.x
```

Your command history shows that you used:

```bash
docker --version
```

and also:

```bash
docker -v
```

Both are commonly used to quickly check the installed Docker version.

---

## Detailed Docker information

```bash
docker info
```

Shows information about:

* Docker engine
* Containers
* Images
* Storage
* Resources
* Runtime
* Docker configuration

---

## Client and server version

```bash
docker version
```

This gives detailed Docker client/daemon information.

---

# 8. DOCKER IMAGES

Think:

```text
IMAGE
 ↓
Blueprint
 ↓
Used to create containers
```

---

## List images

```bash
docker images
```

or:

```bash
docker image ls
```

Your practice included:

```bash
docker images
```

---

## Pull an image

```bash
docker pull ubuntu
```

This downloads an image from a registry.

Examples:

```bash
docker pull ubuntu
docker pull mysql
docker pull mongo
docker pull mysql:8.0
```

Your practice included both:

```bash
docker pull mysql
docker pull mysql:8.0
```

---

# 9. IMAGE TAGS

Consider:

```bash
mysql:8.0
```

Here:

```text
mysql → image name
8.0   → tag
```

You can have:

```text
mysql:8.0
mysql:8.4
```

The tag identifies a particular image variant/version.

If no tag is supplied, Docker commonly uses:

```text
latest
```

but `latest` should not be interpreted as “always the newest possible version” in every registry workflow.

---

# 10. SEARCH FOR AN IMAGE

```bash
docker search mysql
```

This searches Docker Hub for images matching the term.

---

# 11. RUNNING A CONTAINER

## The most important command:

```bash
docker run IMAGE
```

Example:

```bash
docker run ubuntu
```

What happens?

Conceptually:

```text
docker run
    ↓
Find image locally
    ↓
If unavailable → pull image
    ↓
Create container
    ↓
Start container
```

This is why:

```bash
docker run ubuntu
```

can download Ubuntu if it isn't already present locally.

---

# 12. `docker run` DOES TWO IMPORTANT THINGS

Remember:

```bash
docker run
```

means:

```text
CREATE + START
```

It does **not** mean:

```text
start an existing stopped container
```

For that, use:

```bash
docker start
```

### Difference

```bash
docker run ubuntu
```

→ Creates a new container.

```bash
docker start my-container
```

→ Starts an existing container.

🔥 Very important interview concept.

---

# 13. INTERACTIVE CONTAINER

```bash
docker run -it ubuntu
```

`-i`:

```text
interactive
```

`-t`:

```text
allocate terminal/TTY
```

So:

```bash
docker run -it ubuntu
```

means:

> Create and start an Ubuntu container and attach an interactive terminal.

You practiced this exact workflow.

---

# 14. RUN IN BACKGROUND

Use:

```bash
-d
```

Example:

```bash
docker run -d mysql
```

`-d` means:

```text
detached mode
```

The container runs in the background.

Typical use:

```text
Database
Backend API
Redis
Nginx
```

---

# 15. GIVE A CONTAINER A NAME

Use:

```bash
--name
```

Example:

```bash
docker run -d --name mysql-db mysql
```

Instead of Docker generating a random name, you can use:

```text
mysql-db
```

Then:

```bash
docker logs mysql-db
docker stop mysql-db
docker start mysql-db
docker inspect mysql-db
```

This is much easier than using container IDs.

---

# 16. LIST CONTAINERS

## Running containers

```bash
docker ps
```

Only currently running containers are shown.

---

## All containers

```bash
docker ps -a
```

Shows:

```text
running
+
stopped
+
exited
```

Remember:

```text
docker ps     → running
docker ps -a  → all
```

---

# 17. CONTAINER LIFECYCLE

A useful mental model:

```text
               docker run
                   ↓
               CREATED
                   ↓
                RUNNING
                ↙     ↘
          docker stop  crash
                ↓        ↓
              STOPPED / EXITED
                   ↓
              docker start
                   ↓
                RUNNING
                   ↓
               docker rm
                   ↓
                REMOVED
```

---

# 18. START A STOPPED CONTAINER

```bash
docker start container_name
```

Example:

```bash
docker start mysql-db
```

A container already exists; you're simply starting it again.

---

# 19. STOP A CONTAINER

```bash
docker stop mysql-db
```

This asks the container to stop gracefully.

---

# 20. RESTART

```bash
docker restart mysql-db
```

Conceptually:

```text
STOP
 ↓
START
```

---

# 21. REMOVE A CONTAINER

```bash
docker rm mysql-db
```

Usually:

```bash
docker stop mysql-db
docker rm mysql-db
```

However, a stopped container can be removed directly.

---

# 22. FORCE REMOVE

Sometimes you may use:

```bash
docker rm -f mysql-db
```

This forcefully removes a running/stopped container.

Use it carefully.

---

# 23. REMOVE AN IMAGE

```bash
docker rmi mysql
```

or:

```bash
docker image rm mysql
```

If a container still references an image, Docker may refuse to remove it.

Your command history demonstrated this type of conflict.

Mental model:

```text
Image
 ↓
Container
```

If the container still depends on that image:

```text
Remove image ❌
```

Remove the container first:

```text
Container → remove
      ↓
Image → remove
```

---

# 24. CLEAN UNUSED IMAGES

```bash
docker image prune
```

Removes unused/dangling images according to Docker's pruning rules.

Be careful with cleanup commands on machines containing important images.

---

# 25. PORT MAPPING

This is one of the most important concepts for backend development.

Suppose MongoDB inside the container listens on:

```text
27017
```

You can map it to host port:

```text
27017
```

using:

```bash
docker run -p 27017:27017 mongo
```

The format is:

```text
-p HOST_PORT:CONTAINER_PORT
```

---

# 26. UNDERSTAND `-p` PROPERLY

Example:

```bash
docker run -d -p 8080:3306 mysql
```

means:

```text
HOST                    CONTAINER

localhost:8080
       │
       ▼
   Docker port 3306
       │
       ▼
     MySQL
```

The first number:

```text
8080
```

is the host port.

The second:

```text
3306
```

is the container port.

Remember:

```text
-p HOST:CONTAINER
```

---

# 27. NODE.JS PORT EXAMPLE

Suppose your Node.js server listens inside the container on:

```text
3000
```

You can run:

```bash
docker run -d -p 5000:3000 my-backend
```

Then:

```text
Browser
   ↓
localhost:5000
   ↓
Docker host port 5000
   ↓
Container port 3000
   ↓
Node.js server
```

---

# 28. HOST PORT CONFLICT

Suppose you run:

```bash
docker run -p 8080:3306 mysql
```

Then another container:

```bash
docker run -p 8080:3306 mysql
```

The second one normally fails because:

```text
Host port 8080
```

is already occupied.

You experienced exactly this situation in your command history.

Use another host port:

```bash
docker run -p 8081:3306 mysql
```

or:

```bash
docker run -p 5000:3306 mysql
```

---

# 29. IMPORTANT: CONTAINER PORTS CAN BE THE SAME

This is allowed:

```text
Container A → 3306
Container B → 3306
```

because each container has its own network namespace.

But this is usually not allowed simultaneously:

```text
Host:8080 → Container A:3306
Host:8080 → Container B:3306
```

because the host port conflicts.

---

# 30. ENVIRONMENT VARIABLES

Use:

```bash
-e KEY=value
```

Example:

```bash
docker run -d \
  -e MYSQL_ROOT_PASSWORD=secret \
  mysql
```

Environment variables allow configuration to be passed into the container.

Examples:

```bash
-e NODE_ENV=production
-e PORT=3000
-e DB_HOST=mongo
```

---

# 31. MYSQL EXAMPLE

You practiced:

```bash
docker run -d \
  -e MYSQL_ROOT_PASSWORD=secret \
  --name mysql-latest \
  mysql
```

The container receives:

```text
MYSQL_ROOT_PASSWORD=secret
```

inside its environment.

You can inspect environment variables using:

```bash
docker exec -it mysql-latest env
```

---

# 32. LOGS

One of your most important debugging commands:

```bash
docker logs container_name
```

Example:

```bash
docker logs mysql-latest
```

Use logs when:

* Container exits immediately
* Application crashes
* Database doesn't start
* Configuration is wrong
* Connection fails
* Server doesn't respond

---

# 33. FOLLOW LOGS

```bash
docker logs -f mysql-latest
```

`-f`:

```text
follow
```

It continuously displays new log output.

Useful for development.

---

# 34. EXECUTE COMMANDS INSIDE A CONTAINER

Use:

```bash
docker exec -it container_name bash
```

Example:

```bash
docker exec -it mysql-latest /bin/bash
```

Some lightweight images don't contain Bash.

Then use:

```bash
docker exec -it container_name sh
```

For example:

```bash
docker exec -it alpine sh
```

---

# 35. `docker exec` VS `docker run`

This distinction is important.

### `docker run`

Creates a **new container**.

```bash
docker run ubuntu
```

### `docker exec`

Runs a command inside an **already running container**.

```bash
docker exec -it ubuntu-container sh
```

Remember:

```text
run  → new container
exec → existing running container
```

---

# 36. INSPECT A CONTAINER

```bash
docker inspect container_name
```

Provides detailed JSON information.

Useful for investigating:

* Container configuration
* IP address
* Network
* Mounts
* Environment variables
* Port bindings
* Image
* Runtime configuration

Example:

```bash
docker inspect mongo
```

---

# 37. MONITOR RESOURCE USAGE

```bash
docker stats
```

Shows live statistics such as:

```text
CPU
Memory
Network I/O
Block I/O
```

Useful when:

```text
Container consuming too much RAM
Container consuming too much CPU
Performance investigation
```

---

# 38. DOCKER IMAGE LAYERS

A Docker image isn't one giant block.

It is composed of layers.

Conceptually:

```text
┌──────────────────────┐
│ Application changes  │
├──────────────────────┤
│ npm dependencies     │
├──────────────────────┤
│ System packages      │
├──────────────────────┤
│ Base image           │
└──────────────────────┘
```

Docker can reuse layers that haven't changed.

This is one reason Docker builds can become faster after the initial build.

---

# 39. WHY LAYERS MATTER

Suppose:

```dockerfile
FROM node
COPY package*.json ./
RUN npm install
COPY . .
```

If you only change:

```text
server.js
```

Docker may reuse the earlier layers.

But if you put:

```dockerfile
COPY . .
```

before dependency installation, frequent source changes can invalidate more cache.

Therefore Dockerfile instruction order matters.

---

# 40. DOCKERFILE

A Dockerfile is a text file containing instructions to build an image.

Example:

```dockerfile
FROM node

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

---

# 41. `FROM`

```dockerfile
FROM node
```

Defines the base image.

It tells Docker:

> Start building my image from this image.

Examples:

```dockerfile
FROM node
FROM node:22
FROM ubuntu
FROM python:3.12
```

---

# 42. `WORKDIR`

```dockerfile
WORKDIR /app
```

Sets the working directory inside the image/container.

After this:

```dockerfile
COPY . .
```

means copy the source into:

```text
/app
```

Using `WORKDIR` is generally cleaner than manually creating directories with `RUN mkdir`.

---

# 43. `COPY`

```dockerfile
COPY . .
```

Copies files from the Docker build context into the image.

Example:

```text
Host project
     ↓
docker build
     ↓
Image
```

---

# 44. `RUN`

```dockerfile
RUN npm install
```

Executes a command while the image is being built.

Important distinction:

```text
RUN
 ↓
build time
```

---

# 45. `CMD`

Example:

```dockerfile
CMD ["node", "server.js"]
```

Defines the default command used when a container starts.

Important:

```text
RUN → image build time

CMD → container startup
```

---

# 46. `ENV`

You used:

```dockerfile
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty
```

This sets environment variables in the image.

However, **don't put real production secrets/passwords into a Dockerfile** because they become part of the image metadata/history and are not a good secret-management strategy.

For real applications, prefer runtime environment configuration or proper secret management.

---

# 47. `EXPOSE`

Example:

```dockerfile
EXPOSE 3000
```

It documents that the application expects to listen on port 3000.

Important:

```text
EXPOSE 3000
```

does **not by itself publish the port to your host**.

You still need:

```bash
docker run -p 3000:3000 image
```

---

# 48. YOUR DOCKERFILE

Your uploaded Dockerfile is:

```dockerfile
FROM node

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty
    
RUN mkdir -p docker-testapp

COPY . /docker-testapp

CMD ["node", "/docker-testapp/server.js"]
```

Let's understand it.

---

## Line 1

```dockerfile
FROM node
```

Start from the Node.js base image.

---

## Lines 3–4

```dockerfile
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty
```

Creates environment variables.

Inside the container, these can be accessed through the environment.

---

## Line 6

```dockerfile
RUN mkdir -p docker-testapp
```

Creates a directory during image building.

Then:

```dockerfile
COPY . /docker-testapp
```

copies the build context into that directory.

---

## Final line

```dockerfile
CMD ["node", "/docker-testapp/server.js"]
```

When the container starts, Docker runs:

```bash
node /docker-testapp/server.js
```

---

# 49. BUILD YOUR IMAGE

Suppose your Dockerfile is in the current directory.

Run:

```bash
docker build -t my-backend .
```

Meaning:

```text
docker build
       ↓
Build image
       ↓
-t my-backend
       ↓
Give image a name
       ↓
.
       ↓
Use current directory as build context
```

---

# 50. IMAGE TAGGING

You can specify a version:

```bash
docker build -t my-backend:1.0 .
```

Then:

```bash
docker images
```

may show:

```text
my-backend   1.0
```

You can also use:

```bash
docker build -t my-backend:latest .
```

---

# 51. BUILD WITHOUT CACHE

The cheat sheet includes:

```bash
docker build -t my-backend:1.0 . --no-cache
```

The commonly used spelling is:

```bash
docker build --no-cache -t my-backend:1.0 .
```

This forces Docker to rebuild without using cached layers.

Useful when:

```text
Cache is causing confusing behavior
Dependencies need fresh installation
You want a completely fresh build
```

---

# 52. DOCKERIZING A NODE.JS APPLICATION

Suppose your project:

```text
docker-testapp/
│
├── package.json
├── package-lock.json
├── server.js
└── Dockerfile
```

A good starting Dockerfile:

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

Build:

```bash
docker build -t docker-testapp .
```

Run:

```bash
docker run -d \
  --name docker-testapp \
  -p 3000:3000 \
  docker-testapp
```

Check:

```bash
docker ps
```

Logs:

```bash
docker logs docker-testapp
```

---

# 53. THE COMPLETE APPLICATION FLOW

```text
Source Code
     ↓
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Container
     ↓
Application
```

This is one of the most important Docker workflows.

---

# 54. DOCKER HUB

Docker Hub is a public container image registry.

You can:

```text
Pull images
Push images
Share images
Version images
```

---

# 55. LOGIN

```bash
docker login
```

You practiced:

```bash
docker login
docker logout
```

After successful login, Docker can authenticate to registries.

---

# 56. TAG AN IMAGE FOR DOCKER HUB

Suppose your Docker Hub username is:

```text
pradip123562m
```

and your repository is:

```text
testapp
```

Tag:

```bash
docker tag my-backend pradip123562m/testapp:latest
```

General syntax:

```bash
docker tag SOURCE_IMAGE USERNAME/REPOSITORY:TAG
```

---

# 57. PUSH IMAGE

```bash
docker push pradip123562m/testapp:latest
```

Your command history shows that you practiced:

```bash
docker push pradip123562m/testapp
```

The image is uploaded to the registry repository.

---

# 58. PULL YOUR IMAGE

On another machine:

```bash
docker pull pradip123562m/testapp:latest
```

Then:

```bash
docker run pradip123562m/testapp:latest
```

This is a major deployment concept.

---

# 59. DOCKER VOLUMES

Containers are generally treated as disposable.

Suppose MongoDB stores data inside the container.

If you delete the container:

```text
MongoDB container
       ↓
Delete container
       ↓
Container filesystem disappears
```

Important data should therefore be stored outside the disposable container layer.

That's where volumes and bind mounts come in.

---

# 60. NAMED VOLUME

Create:

```bash
docker volume create MyVolume
```

List:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect MyVolume
```

Remove:

```bash
docker volume rm MyVolume
```

Your command history includes all of these operations.

---

# 61. MOUNT A NAMED VOLUME

Example:

```bash
docker run -d \
  --name mongo \
  -v mongo-data:/data/db \
  mongo
```

Meaning:

```text
mongo-data
    ↓
/data/db inside container
```

MongoDB stores database files at its container data directory.

The volume keeps that data independent from the container lifecycle.

---

# 62. BIND MOUNT

A bind mount maps a specific host directory into the container.

Example from your practice:

```bash
docker run -it \
  -v /home/guddu/Documents/data:/test/data \
  ubuntu
```

Format:

```text
-v HOST_PATH:CONTAINER_PATH
```

So:

```text
Host:
/home/guddu/Documents/data

        ↓

Container:
/test/data
```

Changes can be visible from both sides.

---

# 63. VOLUME VS BIND MOUNT

| Named Volume                          | Bind Mount                               |
| ------------------------------------- | ---------------------------------------- |
| Managed by Docker                     | Uses explicit host path                  |
| Good for persistent app/database data | Good for development/source-code sharing |
| Docker controls storage location      | You control exact host location          |
| Example: `mongo-data:/data/db`        | Example: `/home/user/data:/data`         |

---

# 64. ANONYMOUS VOLUME

The cheat sheet also mentions:

```bash
docker run -v /some/container/path image
```

This creates an anonymous volume.

Anonymous volumes are useful in some image configurations, but named volumes are generally easier to manage because you can refer to them by name.

---

# 65. VOLUME CLEANUP

List:

```bash
docker volume ls
```

Remove unused volumes:

```bash
docker volume prune
```

⚠️ Be careful.

Unused volumes may still contain data you care about.

---

# 66. DOCKER NETWORKING

Docker networks allow containers to communicate.

List networks:

```bash
docker network ls
```

Create:

```bash
docker network create mongo-network
```

Remove:

```bash
docker network rm mongo-network
```

Remove unused networks:

```bash
docker network prune
```

---

# 67. WHY NETWORKS MATTER

Suppose you have:

```text
Backend
MongoDB
Redis
```

You can put them on the same Docker network:

```text
       mongo-network
   ┌────────┼─────────┐
   ↓        ↓         ↓
Backend   Mongo      Redis
```

Then containers can communicate using service/container names.

For example:

```text
mongodb://mongo:27017
```

instead of trying to use:

```text
localhost
```

---

# 68. IMPORTANT: `localhost` IN CONTAINERS

This causes huge confusion for beginners.

Suppose:

```text
Backend container
MongoDB container
```

Inside the backend container:

```text
localhost
```

means:

> The backend container itself.

It does **not** mean the MongoDB container.

So this is usually wrong for container-to-container communication:

```text
mongodb://localhost:27017
```

Instead, if the Mongo container is named:

```text
mongo
```

use:

```text
mongodb://mongo:27017
```

when both are connected to the appropriate Docker network.

---

# 69. YOUR MONGODB NETWORK PRACTICE

You created:

```bash
docker network create mongo-network
```

Then MongoDB:

```bash
docker run -d \
  -p 27017:27017 \
  --name mongo \
  --network mongo-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty \
  mongo
```

This gives:

```text
MongoDB container
      ↓
mongo-network
      ↓
Container name = mongo
```

Other containers on the same network can resolve:

```text
mongo
```

---

# 70. MONGO EXPRESS

Mongo Express is a web-based interface for MongoDB.

Your practice used:

```bash
docker run -d \
  -p 8081:8081 \
  --name mongo-express \
  --network mongo-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017/" \
  mongo-express
```

Conceptually:

```text
Browser
   ↓
localhost:8081
   ↓
Mongo Express
   ↓
Docker Network
   ↓
mongo:27017
   ↓
MongoDB
```

---

# 71. YOUR FIRST REAL MULTI-CONTAINER ARCHITECTURE

You effectively built:

```text
                    Host Machine
                         │
            ┌────────────┴────────────┐
            │                         │
       localhost:8081           localhost:27017
            │                         │
            ▼                         ▼
    ┌───────────────┐          ┌──────────────┐
    │ Mongo Express │          │    MongoDB   │
    │   Container   │─────────▶│   Container  │
    └───────────────┘          └──────────────┘
             \                     /
              \                   /
               └─ mongo-network ─┘
```

This is already a practical Docker architecture.

---

# 72. DOCKER COMPOSE

When you have multiple containers, manually running long commands becomes annoying.

Without Compose:

```bash
docker network create mongo-network

docker run ...

docker run ...

docker run ...
```

With Compose:

```bash
docker compose up
```

One YAML file defines the services.

---

# 73. COMPOSE FILE

A Compose file commonly looks like:

```yaml
services:
  backend:
    image: my-backend

  mongo:
    image: mongo

  redis:
    image: redis
```

Compose creates and manages the services together.

---

# 74. YOUR `mongodb.yaml`

Your uploaded YAML is:

```yaml
version: "3.8"

services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    volumes:
      - /home/guddu/Documents/data:/data/db

  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/
```

---

# 75. UNDERSTANDING THE YAML

## `services`

```yaml
services:
```

Defines the containers/services managed by Compose.

You have:

```text
mongo
mongo-express
```

---

# 76. MONGO SERVICE

```yaml
mongo:
  image: mongo
```

Compose uses the Mongo image.

Conceptually:

```text
mongo service
     ↓
mongo image
     ↓
Mongo container
```

---

# 77. MONGO PORT

```yaml
ports:
  - "27017:27017"
```

Means:

```text
Host 27017
     ↓
Container 27017
```

So your host can connect to MongoDB through:

```text
localhost:27017
```

---

# 78. MONGO ENVIRONMENT

```yaml
environment:
  MONGO_INITDB_ROOT_USERNAME: admin
  MONGO_INITDB_ROOT_PASSWORD: qwerty
```

These configure the MongoDB initialization.

Conceptually:

```text
username = admin
password = qwerty
```

For real applications, avoid committing real credentials directly into a public repository.

---

# 79. MONGO VOLUME

Your YAML contains:

```yaml
volumes:
  - /home/guddu/Documents/data:/data/db
```

This is a bind mount:

```text
Host:
/home/guddu/Documents/data

        ↓

Container:
/data/db
```

Therefore MongoDB data is stored in the host directory rather than only inside the container's writable layer.

---

# 80. MONGO EXPRESS SERVICE

```yaml
mongo-express:
  image: mongo-express
```

This creates another service from the Mongo Express image.

---

# 81. MONGO EXPRESS PORT

```yaml
ports:
  - "8081:8081"
```

Means:

```text
Host 8081
   ↓
Container 8081
```

You can access the web interface through:

```text
localhost:8081
```

---

# 82. MONGO EXPRESS DATABASE CONNECTION

Your YAML contains:

```yaml
ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/
```

The important part is:

```text
mongo
```

not:

```text
localhost
```

Why?

Because Compose services can communicate over the Compose network using their service names.

Therefore:

```text
mongo-express
      ↓
mongo
      ↓
MongoDB
```

---

# 83. COMPOSE COMMANDS

You practiced:

```bash
docker compose -f mongodb.yaml up
```

and:

```bash
docker compose -f mongodb.yaml up -d
```

The `-f` option specifies the Compose file.

---

## Start in foreground

```bash
docker compose -f mongodb.yaml up
```

Logs appear in the terminal.

---

## Start in background

```bash
docker compose -f mongodb.yaml up -d
```

`-d` means detached mode.

---

## Stop and remove Compose resources

```bash
docker compose -f mongodb.yaml down
```

This removes the Compose-created containers and associated resources such as the default network, subject to what resources are configured/retained.

---

## List Compose services

```bash
docker compose -f mongodb.yaml ps
```

---

## Compose logs

```bash
docker compose -f mongodb.yaml logs
```

For one service:

```bash
docker compose -f mongodb.yaml logs mongo
```

---

# 84. DOCKER COMPOSE NETWORKING

Compose normally creates a network for the application.

Conceptually:

```text
mongodb.yaml
     ↓
Compose Project
     ↓
Default Network
     ↓
┌───────────────┐
│               │
Mongo       Mongo Express
│               │
└───────────────┘
```

Services can communicate using their service names.

Therefore:

```text
mongo-express → mongo
```

works conceptually as DNS/service discovery.

---

# 85. WHY COMPOSE IS SO IMPORTANT

For backend development, you may have:

```text
Frontend
Backend
MongoDB
Redis
Nginx
Message Queue
```

Manually running every container is inconvenient.

Compose allows:

```bash
docker compose up -d
```

to bring the environment up.

And:

```bash
docker compose down
```

to bring it down.

This is one of the most useful Docker tools for local development.

---

# 86. DEVELOPING WITH DOCKER

There are two common development styles.

## Style 1: Build a new image after every code change

```text
Change code
   ↓
docker build
   ↓
docker run
```

This works but can be slow.

---

## Style 2: Use bind mounts

```text
Host source code
       ↓
Bind mount
       ↓
Container
```

For development, your local code can be mounted into the container.

This is useful with:

```text
Node.js
React
Python
etc.
```

and can be combined with development tools such as file watchers.

---

# 87. PRODUCTION VS DEVELOPMENT

## Development

Often use:

```text
Bind mounts
Hot reload
Debug logs
Development dependencies
```

## Production

Often prefer:

```text
Immutable image
Minimal image
No source-code bind mount
Runtime configuration
Proper secrets
Health checks
Resource controls
```

---

# 88. DOCKERFILE BUILD PROCESS

When you run:

```bash
docker build -t myapp .
```

think:

```text
Dockerfile
    +
Build Context
    ↓
Docker Builder
    ↓
Instructions executed
    ↓
Layers created
    ↓
Image
```

The final result is an image.

---

# 89. BUILD CONTEXT

The final:

```text
.
```

means:

> Use the current directory as the build context.

Therefore:

```bash
docker build -t myapp .
```

allows Dockerfile instructions such as:

```dockerfile
COPY . .
```

to access files from that build context.

---

# 90. `.dockerignore`

A very important practical topic.

Create:

```text
.dockerignore
```

Example:

```text
node_modules
.git
.env
npm-debug.log
Dockerfile
README.md
```

Why?

You don't want unnecessary files sent as build context.

For Node applications:

```text
node_modules
```

is especially important because dependencies can be installed inside the image.

---

# 91. DOCKER IMAGE SIZE

Smaller images are generally better because they can provide:

* Faster builds
* Faster transfers
* Smaller storage footprint
* Smaller deployment artifacts

Possible strategies:

```text
Use appropriate base images
Use multi-stage builds
Avoid unnecessary packages
Use .dockerignore
Clean package caches where appropriate
```

---

# 92. MULTI-STAGE BUILDS

For compiled applications, you can separate:

```text
Build environment
```

from:

```text
Runtime environment
```

Conceptually:

```dockerfile
FROM node AS builder

# Build application

FROM node

# Copy only required build output
```

The final image doesn't need every build-time dependency.

This is an advanced but valuable Docker concept.

---

# 93. CONTAINER EXIT CODES

If:

```bash
docker ps
```

doesn't show your container, check:

```bash
docker ps -a
```

You might see:

```text
Exited (1)
Exited (137)
```

An exited container means its main process has stopped.

Remember:

> A container exists because of its main process.

If the main process finishes or crashes, the container stops.

---

# 94. WHY `docker run ubuntu` MAY EXIT IMMEDIATELY

Consider:

```bash
docker run ubuntu
```

Ubuntu's default process may finish immediately in a non-interactive context.

Then:

```bash
docker ps
```

may show nothing.

But:

```bash
docker ps -a
```

will show the exited container.

For interactive use:

```bash
docker run -it ubuntu
```

---

# 95. COMMON TROUBLESHOOTING FLOW

If your application isn't working:

### Step 1

```bash
docker ps
```

Is the container running?

### Step 2

```bash
docker ps -a
```

Did it exit?

### Step 3

```bash
docker logs container
```

Why did it exit?

### Step 4

```bash
docker inspect container
```

Check configuration/network/mounts.

### Step 5

```bash
docker exec -it container sh
```

If running, enter it and inspect the environment.

### Step 6

```bash
docker stats
```

Check resource usage if performance is the problem.

This workflow is extremely useful.

---

# 96. COMMON ERROR: PORT ALREADY ALLOCATED

Example:

```text
Bind for 0.0.0.0:8080 failed:
port is already allocated
```

Meaning:

```text
Another process/container is already using host port 8080.
```

Check containers:

```bash
docker ps
```

You can also investigate host processes with Linux tools.

Your command history used:

```bash
sudo lsof -i :27017
```

This is useful for finding what is using a host port.

---

# 97. YOUR MONGODB PORT CONFLICT

You used:

```bash
sudo lsof -i :27017
```

Then:

```bash
sudo systemctl stop mongod
```

This indicates that a MongoDB service on the host was occupying port `27017`.

Therefore Docker could not bind:

```text
host:27017
```

until the conflicting process was stopped.

This is an important real-world debugging lesson:

```text
Docker port problem
        ↓
Check Docker containers
        +
Check host processes
```

---

# 98. COMMON ERROR: CONTAINER NAME ALREADY IN USE

If you already have:

```text
mongo
```

and run:

```bash
docker run --name mongo ...
```

again, Docker can complain that the name is already assigned.

Check:

```bash
docker ps -a
```

Then either:

```bash
docker start mongo
```

or remove it:

```bash
docker rm mongo
```

if you no longer need it.

---

# 99. COMMON ERROR: IMAGE CANNOT BE REMOVED

If:

```bash
docker rmi mysql
```

fails because a container references it:

```text
Container
   ↓
references image
```

Remove the container first:

```bash
docker rm container
```

then:

```bash
docker rmi mysql
```

---

# 100. COMMON ERROR: CONTAINER EXITED

Use:

```bash
docker ps -a
```

then:

```bash
docker logs container_name
```

This is one of the first troubleshooting patterns you should memorize.

---

# 101. DOCKER CLEANUP

## Remove stopped container

```bash
docker rm container
```

## Remove unused images

```bash
docker image prune
```

## Remove unused volumes

```bash
docker volume prune
```

## Remove unused networks

```bash
docker network prune
```

Use cleanup commands carefully.

---

# 102. COMPLETE DOCKER COMMAND CHEAT SHEET

## Docker information

```bash
docker --version
docker -v
docker version
docker info
```

---

## Images

```bash
docker images
docker image ls
docker pull IMAGE
docker search IMAGE
docker rmi IMAGE
docker image prune
```

---

## Build

```bash
docker build -t IMAGE .
docker build -t IMAGE:TAG .
docker build --no-cache -t IMAGE .
```

---

## Containers

```bash
docker run IMAGE
docker run -it IMAGE
docker run -d IMAGE
docker run -d --name NAME IMAGE
docker ps
docker ps -a
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER
docker rm -f CONTAINER
```

---

## Ports

```bash
docker run -p HOST:CONTAINER IMAGE
```

Example:

```bash
docker run -d -p 3000:3000 my-backend
```

---

## Environment variables

```bash
docker run -e KEY=value IMAGE
```

Example:

```bash
docker run -d \
  -e NODE_ENV=production \
  my-backend
```

---

## Logs

```bash
docker logs CONTAINER
docker logs -f CONTAINER
```

---

## Execute commands

```bash
docker exec -it CONTAINER bash
docker exec -it CONTAINER sh
```

---

## Inspect

```bash
docker inspect CONTAINER
```

---

## Resources

```bash
docker stats
```

---

# 103. VOLUME COMMANDS

```bash
docker volume ls
docker volume create NAME
docker volume inspect NAME
docker volume rm NAME
docker volume prune
```

Mount:

```bash
docker run -v NAME:/container/path IMAGE
```

Bind mount:

```bash
docker run -v /host/path:/container/path IMAGE
```

---

# 104. NETWORK COMMANDS

```bash
docker network ls
docker network create NAME
docker network inspect NAME
docker network rm NAME
docker network prune
```

Run on network:

```bash
docker run --network NAME IMAGE
```

---

# 105. DOCKER HUB COMMANDS

```bash
docker login
docker logout
docker search IMAGE
docker tag SOURCE USER/REPO:TAG
docker push USER/REPO:TAG
docker pull USER/REPO:TAG
```

---

# 106. DOCKER COMPOSE COMMANDS

```bash
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
docker compose logs SERVICE
```

With your file:

```bash
docker compose -f mongodb.yaml up -d
```

Stop:

```bash
docker compose -f mongodb.yaml down
```

---

# 107. THE MOST IMPORTANT COMMAND DIFFERENCES

Memorize this table.

| Command          | Meaning                                |
| ---------------- | -------------------------------------- |
| `docker pull`    | Download image                         |
| `docker images`  | List images                            |
| `docker build`   | Build image                            |
| `docker run`     | Create + start new container           |
| `docker ps`      | Running containers                     |
| `docker ps -a`   | All containers                         |
| `docker start`   | Start existing container               |
| `docker stop`    | Stop container                         |
| `docker restart` | Restart container                      |
| `docker rm`      | Remove container                       |
| `docker rmi`     | Remove image                           |
| `docker logs`    | View logs                              |
| `docker exec`    | Execute command in running container   |
| `docker inspect` | Detailed configuration                 |
| `docker stats`   | Resource usage                         |
| `docker volume`  | Persistent storage                     |
| `docker network` | Container networking                   |
| `docker compose` | Multi-container application management |

---

# 108. THE BIG DOCKER MENTAL MODEL

If you remember only one diagram, remember this:

```text
                   DOCKER

                    IMAGE
                      │
               docker build/pull
                      │
                      ▼
                  CONTAINER
                      │
           ┌──────────┼──────────┐
           │          │          │
          Port       Env       Volume
           │          │          │
           ▼          ▼          ▼
        Network    Config      Data
                      │
                      ▼
                  Application
```

---

# 109. IMAGE → CONTAINER

The relationship is:

```text
Image
  │
  │ docker run
  ▼
Container
```

Multiple containers can be created from the same image:

```text
             Node Image
             /    |    \
            /     |     \
           ▼      ▼      ▼
       Backend1 Backend2 Backend3
```

---

# 110. CONTAINER → IMAGE

The normal workflow is:

```text
Write application
       ↓
Dockerfile
       ↓
docker build
       ↓
Image
       ↓
docker run
       ↓
Container
```

---

# 111. BACKEND DEVELOPMENT ARCHITECTURE

A realistic local backend setup may look like:

```text
                    Host
                     │
              Docker Compose
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
    Backend        MongoDB       Redis
   Container      Container     Container
       │             │             │
       └─────────────┼─────────────┘
                     │
                Docker Network
```

Data:

```text
MongoDB
   │
   ▼
Volume
   │
   ▼
Persistent Data
```

---

# 112. REAL-WORLD BACKEND WORKFLOW

Suppose you build:

```text
Node.js + Express
MongoDB
```

Your workflow becomes:

### Step 1 — Write backend

```text
server.js
package.json
```

### Step 2 — Create Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

### Step 3 — Build

```bash
docker build -t backend .
```

### Step 4 — Run

```bash
docker run -d \
  --name backend \
  -p 3000:3000 \
  backend
```

### Step 5 — Debug

```bash
docker ps
docker logs backend
```

### Step 6 — Add MongoDB

Use Compose.

### Step 7 — Add persistent volume

```text
MongoDB → Volume
```

### Step 8 — Connect containers through Docker network.

This is the foundation of Docker-based backend development.

---

# 113. DOCKER COMPOSE FULL EXAMPLE

A simple backend + MongoDB setup:

```yaml
services:

  backend:
    build: .
    ports:
      - "3000:3000"
    environment:
      MONGO_URL: mongodb://mongo:27017/mydb
    depends_on:
      - mongo

  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

Start:

```bash
docker compose up -d
```

Now:

```text
Backend
   ↓
mongodb://mongo:27017/mydb
   ↓
MongoDB
```

Notice:

```text
mongo
```

is used instead of:

```text
localhost
```

because the backend is talking to another container.

---

# 114. `depends_on`

Compose supports:

```yaml
depends_on:
  - mongo
```

This expresses a startup dependency.

However:

> `depends_on` does not automatically guarantee that the database is fully ready to accept connections.

For robust applications, use appropriate retry logic and/or health checks.

---

# 115. HEALTH CHECKS

A health check lets Docker/Compose determine whether a service is healthy.

Conceptually:

```text
Container running
       ≠
Application ready
```

For example, MongoDB's process could be running while the database is still initializing.

Health checks help distinguish:

```text
Running
```

from:

```text
Ready/Healthy
```

---

# 116. SECRETS AND SECURITY

Never casually put production credentials into:

```dockerfile
ENV PASSWORD=...
```

or public Compose files.

Your learning examples use:

```text
admin
qwerty
```

which is fine for local practice, but not for production.

For real projects:

```text
Environment variables
Secret managers
Docker secrets
Cloud secret services
```

should be considered.

---

# 117. DOCKER AND MICROSERVICES

Docker is commonly used to package individual services.

Example:

```text
                 Application
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
    Auth Service  User Service  Order Service
        │            │            │
        └────────────┼────────────┘
                     ▼
                  Network
```

Each service can have:

```text
Own image
Own container
Own configuration
Own scaling
```

Compose is particularly useful for running such multi-service environments locally.

---

# 118. WHAT YOU SHOULD NOT MEMORIZE

Don't try to memorize every Docker option.

Instead understand:

```text
Image
Container
Dockerfile
Build
Run
Port
Environment
Volume
Network
Compose
Registry
Logs
Debugging
```

Once these concepts are clear, commands become much easier.

---

# 119. FIVE THINGS YOU MUST EXPLAIN IN AN INTERVIEW

## 1. Image vs Container

Answer:

> An image is a read-only template used to create containers, while a container is a running or stopped instance created from an image.

---

## 2. Dockerfile

Answer:

> A Dockerfile contains instructions that Docker uses to build an image.

---

## 3. Port mapping

Answer:

> Docker port mapping connects a host port to a container port using the format `HOST:CONTAINER`.

Example:

```bash
-p 8080:3000
```

means host port 8080 forwards to container port 3000.

---

## 4. Volume

Answer:

> A volume provides persistent storage that is independent of a container's writable layer, which is particularly important for databases.

---

## 5. Docker Network

Answer:

> Docker networks allow containers to communicate with each other, commonly using container/service names as DNS names.

---

# 120. INTERVIEW QUESTIONS

## Beginner

### Q1. What is Docker?

Docker is a platform for packaging and running applications in isolated containers.

### Q2. What is a container?

An isolated runtime environment created from an image.

### Q3. What is an image?

A read-only template used to create containers.

### Q4. What is Docker Hub?

A registry for storing and distributing Docker images.

### Q5. What is Dockerfile?

A file containing image-build instructions.

---

# 121. INTERMEDIATE INTERVIEW QUESTIONS

### Q6. Difference between `docker run` and `docker start`?

```text
docker run
→ creates and starts a new container

docker start
→ starts an existing container
```

---

### Q7. Difference between `docker exec` and `docker run`?

```text
docker run
→ creates a new container

docker exec
→ runs a command inside an existing running container
```

---

### Q8. Why do we use `-p`?

To publish/map a container port to a host port.

---

### Q9. Why use volumes?

To persist data independently of the container lifecycle.

---

### Q10. Why use Docker networks?

To enable container-to-container communication.

---

# 122. ADVANCED INTERVIEW QUESTIONS

### Q11. What are Docker layers?

Images are constructed from layered filesystem changes. Docker can reuse unchanged layers during builds.

### Q12. Why are layers useful?

They enable caching and efficient image storage/building.

### Q13. Why can a container stop immediately?

Its main process may exit or crash.

### Q14. How do you debug an exited container?

```bash
docker ps -a
docker logs CONTAINER
```

### Q15. How do containers communicate?

Through Docker networks.

### Q16. Why isn't `localhost` normally used to connect to another container?

Because `localhost` inside a container refers to that container itself.

---

# 123. PRACTICAL DEBUGGING CHEAT SHEET

When something doesn't work:

```text
             Problem
                │
                ▼
          docker ps -a
                │
        ┌───────┴────────┐
        │                │
    Running?          Exited?
        │                │
        ▼                ▼
 docker logs        docker logs
        │
        ▼
 docker inspect
        │
        ▼
 check ports
        │
        ▼
 check network
        │
        ▼
 check environment
        │
        ▼
 docker exec
```

---

# 124. WHEN TO USE WHICH COMMAND

## “Is Docker installed?”

```bash
docker --version
```

## “What images do I have?”

```bash
docker images
```

## “Download MongoDB.”

```bash
docker pull mongo
```

## “Create and run MongoDB.”

```bash
docker run mongo
```

## “Run it in background.”

```bash
docker run -d mongo
```

## “Give it a name.”

```bash
docker run -d --name mongo mongo
```

## “Expose a port.”

```bash
docker run -d -p 27017:27017 mongo
```

## “Give configuration.”

```bash
docker run -e KEY=value image
```

## “See running containers.”

```bash
docker ps
```

## “See stopped containers.”

```bash
docker ps -a
```

## “Stop one.”

```bash
docker stop mongo
```

## “Start it again.”

```bash
docker start mongo
```

## “Remove it.”

```bash
docker rm mongo
```

## “See why it failed.”

```bash
docker logs mongo
```

## “Enter it.”

```bash
docker exec -it mongo bash
```

## “Inspect it.”

```bash
docker inspect mongo
```

## “Persist database data.”

```bash
docker volume create mongo-data
```

## “Connect containers.”

```bash
docker network create my-network
```

## “Run multiple services.”

```bash
docker compose up -d
```

---

# 125. YOUR DOCKER JOURNEY SO FAR

Based on your command history, you have already practiced:

```text
Docker installation/check
        ↓
Docker images
        ↓
docker pull
        ↓
docker run
        ↓
Interactive containers
        ↓
Container lifecycle
        ↓
MySQL containers
        ↓
Port mapping
        ↓
Environment variables
        ↓
Logs
        ↓
docker exec
        ↓
MongoDB
        ↓
Docker networks
        ↓
Mongo Express
        ↓
Docker Compose
        ↓
Docker Hub
        ↓
Bind mounts
        ↓
Docker volumes
```

That corresponds very closely to the learning sequence in your Docker syllabus.

---

# 126. YOUR ACTUAL COMMAND PATTERN

You practiced this sequence:

```bash
docker pull mysql
docker images
docker run -d \
  -e MYSQL_ROOT_PASSWORD=secret \
  --name mysql-latest \
  mysql

docker ps
docker logs mysql-latest
docker exec -it mysql-latest /bin/bash
```

The conceptual flow is:

```text
Pull image
   ↓
Create container
   ↓
Start container
   ↓
Check container
   ↓
Read logs
   ↓
Enter container
```

---

# 127. YOUR MONGODB + MONGO EXPRESS PATTERN

You practiced:

```bash
docker network create mongo-network
```

Then:

```bash
docker run -d \
  -p 27017:27017 \
  --name mongo \
  --network mongo-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty \
  mongo
```

Then Mongo Express:

```bash
docker run -d \
  -p 8081:8081 \
  --name mongo-express \
  --network mongo-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017/" \
  mongo-express
```

Conceptually:

```text
                 mongo-network
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
       Mongo Express          Mongo
         :8081                :27017
              │                 │
              └───────┬─────────┘
                      │
                 Container DNS
                      │
                    mongo
```

This is an excellent example for understanding Docker networking.

---

# 128. COMPOSE VERSION OF THE SAME IDEA

Instead of manually running both containers, you can describe them in:

```text
mongodb.yaml
```

Then:

```bash
docker compose -f mongodb.yaml up -d
```

This is the major advantage of Compose:

```text
Many Docker commands
       ↓
One configuration file
       ↓
docker compose up
```

---

# 129. DOCKER ZERO → HERO ROADMAP

Follow this order.

## LEVEL 1 — Fundamentals

Learn:

```text
What is Docker?
Image
Container
Registry
Docker Engine
Docker Hub
Docker vs VM
```

---

## LEVEL 2 — Commands

Master:

```bash
docker --version
docker pull
docker images
docker run
docker ps
docker ps -a
docker start
docker stop
docker restart
docker rm
docker rmi
```

---

## LEVEL 3 — Container Configuration

Master:

```bash
-d
-it
--name
-p
-e
```

---

## LEVEL 4 — Debugging

Master:

```bash
docker logs
docker logs -f
docker exec
docker inspect
docker stats
```

---

## LEVEL 5 — Dockerfile

Master:

```text
FROM
WORKDIR
COPY
RUN
ENV
EXPOSE
CMD
```

Then:

```bash
docker build
```

---

## LEVEL 6 — Images

Learn:

```text
Layers
Caching
Tags
Image size
.dockerignore
Docker Hub
```

---

## LEVEL 7 — Storage

Master:

```text
Volumes
Bind mounts
Anonymous volumes
Persistent database data
```

---

## LEVEL 8 — Networking

Master:

```text
Bridge networks
Container DNS
Service names
localhost vs container names
Port publishing
```

---

## LEVEL 9 — Compose

Master:

```yaml
services:
volumes:
networks:
ports:
environment:
depends_on:
```

Commands:

```bash
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
```

---

## LEVEL 10 — Backend Docker

Build:

```text
Node.js
+
Express
+
MongoDB
+
Redis
+
Docker Compose
```

---

# 130. FINAL MASTER MENTAL MODEL

Docker becomes much easier if you think in this sequence:

```text
                 SOURCE CODE
                      │
                      ▼
                  Dockerfile
                      │
                docker build
                      │
                      ▼
                   IMAGE
                      │
                 docker run
                      │
                      ▼
                 CONTAINER
                      │
        ┌─────────────┼──────────────┐
        │             │              │
        ▼             ▼              ▼
      PORT           ENV          VOLUME
        │             │              │
        └─────────────┼──────────────┘
                      ▼
                   NETWORK
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       Backend      Mongo        Redis
          │           │           │
          └───────────┼───────────┘
                      ▼
                Docker Compose
                      │
                      ▼
                 One command
             docker compose up
```

---

# 131. THE 20 COMMANDS TO MASTER FIRST

If you want the shortest high-value list, memorize these:

```bash
docker --version
docker info
docker images
docker pull
docker build
docker run
docker ps
docker ps -a
docker start
docker stop
docker restart
docker rm
docker rmi
docker logs
docker exec
docker inspect
docker stats
docker volume
docker network
docker compose
```

And these options:

```text
-d       → detached/background
-it      → interactive terminal
--name   → custom container name
-p       → port mapping
-e       → environment variable
-v       → volume/bind mount
--network → connect to network
```

---

# 132. THE GOLDEN RULES OF DOCKER

### Rule 1

```text
Image ≠ Container
```

Image is the blueprint.

Container is the running instance.

---

### Rule 2

```text
docker run ≠ docker start
```

```text
run   → new container
start → existing container
```

---

### Rule 3

```text
-p HOST:CONTAINER
```

Always remember the order.

---

### Rule 4

```text
localhost inside container
```

means that container itself.

---

### Rule 5

For container-to-container communication:

```text
Use Docker network + service/container name
```

Example:

```text
mongodb://mongo:27017
```

---

### Rule 6

```text
Container is disposable.
Important data should be persistent.
```

Use:

```text
Volume / Bind Mount
```

---

### Rule 7

When something crashes:

```bash
docker ps -a
docker logs CONTAINER
```

first.

---

### Rule 8

For multiple services:

```bash
docker compose up -d
```

---

### Rule 9

Don't put production secrets directly into Dockerfiles or public Compose files.

---

### Rule 10

Don't memorize commands blindly.

Understand:

```text
Image
→ Container
→ Port
→ Environment
→ Volume
→ Network
→ Compose
```

and the commands become intuitive.

---

# 133. FINAL DOCKER CHEAT SHEET

```text
🐳 DOCKER

IMAGE
docker pull IMAGE
docker images
docker build -t IMAGE .
docker rmi IMAGE

CONTAINER
docker run IMAGE
docker run -it IMAGE
docker run -d IMAGE
docker run -d --name NAME IMAGE

LIST
docker ps
docker ps -a

LIFECYCLE
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
docker rm CONTAINER

DEBUG
docker logs CONTAINER
docker logs -f CONTAINER
docker exec -it CONTAINER bash
docker exec -it CONTAINER sh
docker inspect CONTAINER
docker stats

CONFIG
-p HOST:CONTAINER
-e KEY=value
-v HOST:CONTAINER
--name NAME
--network NETWORK

IMAGE
docker build -t app .
docker tag app USER/REPO:TAG
docker push USER/REPO:TAG
docker pull USER/REPO:TAG

VOLUME
docker volume ls
docker volume create NAME
docker volume inspect NAME
docker volume rm NAME
docker volume prune

NETWORK
docker network ls
docker network create NAME
docker network inspect NAME
docker network rm NAME
docker network prune

COMPOSE
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
docker compose -f FILE up -d
docker compose -f FILE down
```

---

# 🎯 FINAL TAKEAWAY

You don't need to think of Docker as hundreds of commands.

Think of it as one connected system:

```text
               DOCKER
                  │
        ┌─────────┴─────────┐
        │                   │
      IMAGE              REGISTRY
        │                   │
        │                Docker Hub
        ▼
    CONTAINER
        │
   ┌────┼────┬────────┐
   │    │    │        │
  Port Env Volume   Network
                       │
              ┌────────┼────────┐
              ▼        ▼        ▼
           Backend   Mongo     Redis
              │        │        │
              └────────┼────────┘
                       │
                  COMPOSE
                       │
                       ▼
                Multi-service App
```

Once you understand this picture, Docker stops being a collection of commands and becomes a **system you can reason about**.

For your backend/SDE path, the highest-priority Docker topics are:

```text
⭐⭐⭐⭐⭐ Image vs Container
⭐⭐⭐⭐⭐ docker run
⭐⭐⭐⭐⭐ Port Mapping
⭐⭐⭐⭐⭐ Dockerfile
⭐⭐⭐⭐⭐ Image Layers & Caching
⭐⭐⭐⭐⭐ Volumes
⭐⭐⭐⭐⭐ Docker Networking
⭐⭐⭐⭐⭐ Docker Compose

⭐⭐⭐⭐  Environment Variables
⭐⭐⭐⭐  Logs & Debugging
⭐⭐⭐⭐  Docker Hub
⭐⭐⭐⭐  Bind Mounts

⭐⭐⭐    Docker vs VM
⭐⭐⭐    docker stats
⭐⭐⭐    Health Checks
⭐⭐⭐    Multi-stage Builds
```

Your uploaded syllabus specifically progresses through these same major areas: commands, layers, port binding, troubleshooting, VM comparison, development, Compose, Dockerizing applications, Docker Hub, volumes, and networks. Your command cheat sheet also covers the corresponding image, container, troubleshooting, registry, volume, and network commands.
