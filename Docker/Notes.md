Absolutely. Based on the Docker commands you practiced in the terminal, here is the **practical “when do I use this command?” cheat sheet**, plus the important concepts you should know for backend development. Your practice covered the core Docker lifecycle: pull → run → inspect → stop → remove, plus images, ports, environment variables, logs, and `exec`. 

# 🐳 Docker Practical Summary — When to Use Which Command

## 1. 🔍 Check Docker

### `docker --version`

**When:** You want to check whether Docker is installed.

```bash
docker --version
```

You successfully verified Docker 29.7.2. 

### `docker info`

**When:** You want detailed information about the Docker installation, daemon, containers, images, storage, etc.

```bash
docker info
```

### `docker version`

**When:** You want detailed **client + server/daemon** version information.

```bash
docker version
```

---

# 🖼️ 2. Docker Images

Think:

> **Image = Blueprint**
> **Container = Running instance of that blueprint**

### `docker images`

**When:** You want to see images already downloaded/built on your machine.

```bash
docker images
```

You used this to see `ubuntu`, `mysql`, `backend`, `server`, etc. 

### `docker pull`

**When:** You want to download an image from Docker Hub/registry.

```bash
docker pull ubuntu
docker pull mysql
docker pull mysql:8.0
```

### Image tags

```bash
mysql
mysql:8.0
```

Here:

* `mysql` → image
* `8.0` → tag/version
* If you don't specify a tag, Docker generally uses `latest`.

You practiced both `mysql` and `mysql:8.0`. 

---

# 🚀 3. Creating & Running Containers

## ⭐ `docker run`

This is one of the **most important Docker commands**.

**When:** You want to create a new container from an image and start it.

```bash
docker run ubuntu
```

Interactive container:

```bash
docker run -it ubuntu
```

You used:

```bash
docker run -it ubuntu
```

and entered the Ubuntu container. 

### `-it`

Means:

* `-i` → interactive
* `-t` → terminal/TTY

So:

```bash
docker run -it ubuntu
```

means:

> Start Ubuntu and give me an interactive terminal.

---

# 🏃 4. Run Container in Background

### `-d`

```bash
docker run -d mysql
```

**When:** You want the container to run in the background.

For example, databases usually run in detached mode:

```bash
docker run -d mysql
```

You used:

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
```



---

# 🏷️ 5. Give Container a Name

### `--name`

```bash
docker run -d --name mysql-db mysql
```

**When:** You want an easy-to-remember container name.

Instead of:

```text
silly_wozniak
```

you get:

```text
mysql-db
```

Then you can use:

```bash
docker stop mysql-db
docker start mysql-db
docker logs mysql-db
```

Much easier.

---

# 📋 6. List Containers

## ⭐ `docker ps`

**When:** See currently **running containers**.

```bash
docker ps
```

## ⭐ `docker ps -a`

**When:** See **all containers**, including stopped containers.

```bash
docker ps -a
```

You saw stopped containers such as:

```text
Exited (137)
Exited (255)
```



### Remember

```text
docker ps       → running
docker ps -a    → all
```

---

# ▶️ 7. Start a Stopped Container

### `docker start`

**When:** Container already exists but is stopped.

```bash
docker start container_name
```

Example:

```bash
docker start mysql-db
```

You used:

```bash
docker start 3807513ebbdd
```

and the Ubuntu container became `Up`. 

### Important difference

```bash
docker run ubuntu
```

➡️ creates a **new container**

```bash
docker start existing-container
```

➡️ starts an **existing container**

🔥 This distinction is very important.

---

# 🛑 8. Stop a Container

### `docker stop`

```bash
docker stop mysql-db
```

**When:** Gracefully stop a running container.

You used:

```bash
docker stop mysql-older
docker stop mysql-latest
```



---

# 🔄 9. Restart a Container

### `docker restart`

```bash
docker restart mysql-db
```

**When:** Stop + start the same container.

Useful when your application/database needs restarting.

---

# 🗑️ 10. Remove Container

### `docker rm`

```bash
docker rm mysql-db
```

**When:** You don't need the container anymore.

Usually:

```text
stop → remove
```

Example:

```bash
docker stop mysql-db
docker rm mysql-db
```

You practiced exactly this workflow. 

### Important ⚠️

Removing a container does **not necessarily remove its image**.

---

# 🧹 11. Remove Image

### `docker rmi`

```bash
docker rmi mysql
```

**When:** You want to delete an image from your machine.

But if a container is still referencing that image, Docker may refuse.

You experienced this:

```text
conflict: unable to delete mysql:latest
```

because a container was still using it. 

So remember:

```text
Container depends on Image

Remove container
       ↓
Then remove image
```

---

# 🌐 12. Port Mapping

## ⭐⭐⭐ `-p`

One of the most important concepts for backend developers.

```bash
docker run -d -p 8080:3306 mysql
```

Meaning:

```text
HOST PORT : CONTAINER PORT
     ↓            ↓
   8080         3306
```

So:

```text
localhost:8080
       ↓
container:3306
```

You successfully created this mapping. 

### Example: Node.js

Suppose your Node application listens inside Docker on:

```text
3000
```

You can expose it:

```bash
docker run -p 5000:3000 my-backend
```

Then:

```text
Browser
   ↓
localhost:5000
   ↓
Docker container:3000
   ↓
Node.js
```

### ⚠️ Common mistake

You tried:

```bash
-p 8080:3306
```

for two containers.

The second container failed because:

```text
8080 is already allocated
```



So:

> **One host port cannot normally be bound by two containers at the same time.**

---

# 🔐 13. Environment Variables

## ⭐ `-e`

```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
```

**When:** Pass configuration into the container.

Example:

```bash
-e NODE_ENV=production
-e PORT=3000
-e DATABASE_URL=...
```

You used `MYSQL_ROOT_PASSWORD` when starting MySQL. 

Inside the container:

```bash
env
```

can show environment variables.

You verified the MySQL password variable using `env`. 

---

# 📜 14. Check Container Logs

## ⭐⭐⭐ `docker logs`

**When:** Your application/container is running but you want to know:

* What is happening?
* Did the application start?
* Is there an error?
* Why did it crash?
* Is MySQL ready?

```bash
docker logs mysql-latest
```

You used this to inspect MySQL startup logs. 

### Follow logs live

```bash
docker logs -f mysql-latest
```

`-f` = follow.

🔥 Very useful when debugging backend applications.

---

# 💻 15. Enter a Running Container

## ⭐⭐⭐ `docker exec`

```bash
docker exec -it mysql-latest /bin/bash
```

**When:** You want to execute commands **inside an already running container**.

You used exactly this. 

For example:

```bash
docker exec -it mysql-latest bash
```

Then you're inside the container.

---

# 🔎 16. Inspect Container

### `docker inspect`

```bash
docker inspect mysql-latest
```

**When:** You need detailed technical information:

* IP address
* Network
* Mounts
* Environment variables
* Ports
* Image
* Configuration

Very useful for debugging.

---

# 📊 17. Monitor Containers

### `docker stats`

```bash
docker stats
```

Shows:

* CPU usage
* Memory usage
* Network
* Block I/O

Useful when a container is consuming too many resources.

---

# 🧠 18. Important Docker Concepts

These are more important than memorizing 50 commands.

### ⭐ Image

A read-only template used to create containers.

```text
Node Image
     ↓
Container
```

### ⭐ Container

A running/stopped instance of an image.

```text
Image → Container
```

### ⭐ Dockerfile

Instructions for creating an image.

Example:

```dockerfile
FROM node:22
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

Then:

```bash
docker build -t my-backend .
```

---

# 🧱 19. Docker Image Layers

You should understand this concept well.

A Docker image consists of multiple layers:

```text
Application Layer
        ↓
Dependencies Layer
        ↓
System Packages Layer
        ↓
Base Image
```

Docker can reuse unchanged layers.

That's why Docker builds can become much faster after the first build.

---

# 💾 20. Volumes ⭐⭐⭐

Very important for databases.

Problem:

```text
Container
   ↓
Container deleted
   ↓
Data can disappear
```

Volumes solve this.

```text
Docker Volume
      ↓
Container
      ↓
Database data
```

Typical command:

```bash
docker volume create mysql-data
```

Then:

```bash
docker run -d \
  --name mysql \
  -v mysql-data:/var/lib/mysql \
  mysql
```

### Key idea

> **Containers are disposable; important data should be persistent.**

---

# 🌐 21. Docker Networks ⭐⭐⭐

Very important when you have:

```text
Node.js
   ↓
MongoDB
   ↓
Redis
```

Instead of communicating through `localhost`, containers can communicate through a Docker network.

```bash
docker network create my-network
```

Then:

```bash
docker run -d --network my-network --name backend ...
docker run -d --network my-network --name db ...
```

Backend can communicate with:

```text
db
```

rather than:

```text
localhost
```

This becomes **extremely important with Docker Compose and microservices**.

---

# 🧩 22. Docker Compose ⭐⭐⭐⭐⭐

Once your application has multiple services, don't manually run 10 commands.

Example:

```text
        Docker Compose
             │
     ┌───────┼────────┐
     ↓       ↓        ↓
   Node     MySQL    Redis
```

You define everything in:

```text
compose.yaml
```

Then:

```bash
docker compose up
```

Stop/remove:

```bash
docker compose down
```

Background:

```bash
docker compose up -d
```

Check:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs
```

For your backend/microservices path, **Docker Compose is one of the most important things to learn after basic Docker.**

---

# 🔥 Your Docker Command Map

Save this mentally:

```text
CHECK
│
├── docker --version
├── docker version
└── docker info

IMAGE
│
├── docker pull
├── docker images
├── docker build
└── docker rmi

CONTAINER
│
├── docker run
├── docker ps
├── docker ps -a
├── docker start
├── docker stop
├── docker restart
└── docker rm

DEBUG
│
├── docker logs
├── docker exec
├── docker inspect
└── docker stats

CONFIGURATION
│
├── -p       → Port
├── -e       → Environment variable
├── --name   → Container name
└── -v       → Volume

NETWORK
│
├── docker network ls
├── docker network create
└── docker network inspect

STORAGE
│
├── docker volume ls
├── docker volume create
└── docker volume inspect

MULTI-CONTAINER
│
├── docker compose up
├── docker compose down
├── docker compose ps
└── docker compose logs

REGISTRY
│
├── docker login
├── docker tag
├── docker push
└── docker pull
```

## 🎯 If you're learning Docker for Backend/SDE

Don't try to memorize everything. Master these **first**:

**`docker pull` → `docker images` → `docker run` → `docker ps` → `docker ps -a` → `docker start` → `docker stop` → `docker rm` → `docker rmi` → `docker logs` → `docker exec` → `docker inspect` → `-p` → `-e` → `-v` → Dockerfile → `docker build` → Docker Compose → Networks → Volumes.**

And the **5 concepts you absolutely should be able to explain in an interview** are:

1. **Image vs Container**
2. **Dockerfile and Docker build process**
3. **Port mapping (`host:container`)**
4. **Volumes and data persistence**
5. **Docker networking + Docker Compose**

Those five are the real meat of Docker for a backend developer.
