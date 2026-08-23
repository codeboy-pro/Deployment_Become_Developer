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

- `mysql` → image
- `8.0` → tag/version
- If you don't specify a tag, Docker generally uses `latest`.

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

- `-i` → interactive
- `-t` → terminal/TTY

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

- What is happening?
- Did the application start?
- Is there an error?
- Why did it crash?
- Is MySQL ready?

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

- IP address
- Network
- Mounts
- Environment variables
- Ports
- Image
- Configuration

Very useful for debugging.

---

# 📊 17. Monitor Containers

### `docker stats`

```bash
docker stats
```

Shows:

- CPU usage
- Memory usage
- Network
- Block I/O

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

1830 docker --version
1831 docker
1832 clear
1833 docker pull hello-world
1834 docker images
1835 docker run hello-world
1836 docker pull ubuntu
1837 docker run -it ubuntu
1838 docker ps -a
1839 docker ps
1840 docker start 3807513ebbdd
1841 docker ps
1842 docker stop gallant_gagarin
1843 docker ps
1844 docker images
1845 docker rmi 5dd0d3e6e255
1846 docker ps
1847 docker ps -a
1848 docker rm 61ab5b110885
1849 docker images
1850 docker rmi 5dd0d3e6e255
1851 docker images
1852 docker ps -1
1853 docker ps -a
1854 docker rm 3807513ebbdd
1855 docker images
1856 docker rmi 2260313b31c8
1857 docker images
1858 docker ps -a
1859 docker pull mysql
1860 docker pull mysql:8.0
1861 docker images
1862 docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
1863 docker ps
1864 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older mysql:8.0
1865 docker images
1866 docker ps
1867 docker stop a091cafcdab5
1868 docker ps
1869 docker images
1870 docker rmi mysql
1871 docker rm mysql
1872 docker rm a091cafcdab5
1873 docker images
1874 docker rmi mysql
1875 docker images
1876 docker ps -a
1877 docker ps
1878 docker pull mysql
1879 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-latest mysql
1880 docker ps
1881 docker stop mysql-older
1882 docker stop mysql-latest
1883 docker rm mysql-older
1884 docker rm mysql-latest
1885 docker ps -a
1886 docker images
1887 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-latest -p8080:3306 mysql
1888 docker ps
1889 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older -p8080:3306 mysql:8.0
1890 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older -p5000:3306 mysql:8.0
1891 docker ps
1892 docker stop mysql-older
1893 docker rm mysql-older
1894 docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older -p3000:3306 mysql:8.0
1895 docker ps
1896 docker logs mysql-latest
1897 docker ps
1898 docker exec -it mysql-latest /bin/bash
1899 clear
1900 cd ..
1901 git status
1902 git add .
1903 git commit -m "add testapp"
1904 git push
1905 cd /media/guddu/NewVolume/Deployment_Become_Developer
1906 git rm --cached Docker/docker-testapp
1907 rm -rf Docker/docker-testapp/.git
1908 git add Docker/docker-testapp
1909 git commit -m "Add Docker test app"
1910 git push origin main
1911 git clone https://github.com/shradha-khapra/docker-testapp.git
1912 git status
1913 node server.js
1914 docker ps
1915 docker network ls
1916 docker network mongodb-network
1917 docker network create mongodb-network
1918 docker network ls
1919 docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1920 docker ps
1921 docker stop 96fd84347f5f
1922 docker stop a30a3b231a20
1923 sudo lsof -i :27017
1924 sudo systemctl stop mongod
1925 sudo lsof -i :27017
1926 docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1927 docker ps -a
1928 docker rm e405776487c4
1929 docker images
1930 docker rmi mongo
1931 docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1932 docker run mongo
1933 docker start mongo
1934 clear
1935 docker ps
1936 docker network ls
1937 docker rm mongo
1938 docker run -d -p 27017:27017 --name mongo --network mongodb-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1939 docker ps
1940 docker run -d -p8081:8081 --name mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URI="mongodb://admin:qwerty@mongo:27017" mongo-express
1941 docker ps
1942 docker stop 431b5b97edb3
1943 docker start 431b5b97edb3
1944 docker ps
1945 docker stop 431b5b97edb3
1946 docker ps -a
1947 docker stop 431b5b97edb3
1948 docker rm 431b5b97edb3
1949 docker images
1950 docker run mongo-express
1951 docker rmi mongo-express
1952 docker ps
1953 docker ps -a
1954 docker rm mongo-express
1955 docker images
1956 docker rmi mongo-express
1957 docker rmi 1b23d7976f02
1958 docker ps
1959 docker rm e85153ff221f
1960 docker rmi mongo-express
1961 docker ps
1962 docker run -d -p8081:8081 --name mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URI="mongodb://admin:qwerty@mongo:27017" mongo-express
1963 docker ps
1964 docker start mongo-express
1965 docker ps
1966 docker ps -a
1967 docker stop mongo-network
1968 docker rm mongo-network
1969 docker run -d -p 8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URI="mongodb://admin:qwerty@mongo:27017" mongo-express
1970 docker ps
1971 docker ps -a
1972 docker ps
1973 docker run -d -p 8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URI="mongodb://admin:qwerty@mongo:27017" mongo-express
1974 docker network ls
1975 clear
1976 docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1977 docker network ls
1978 docker network create mongo-network
1979 docker network ls
1980 docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo
1981 docker ps
1982 docker run -d -p 8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URI="mongodb://admin:qwerty@mongo:27017" mongo-express
1983 docker ps
1984 docker ps -a
1985 docker logs mongo-express
1986 docker rm mongo-express
1987 docker run -d -p 8081:8081 --name mongo-express --network mongo-network -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017/?authSource=admin" mongo-express
1988 docker ps
1989 curl http://localhost:8081
1990 clear
1991 docker ps -a
1992 docker stop mongo
1993 docker stop mongo-express
1994 code .
1995 docker
1996 docker -v
1997 docker ps -a
1998 docker start cbc87ac0ac63
1999 docker start 4a75c26f2a7f
2000 sudo lsof -i :27017
2001 sudo systemctl stop mongod
2002 sudo lsof -i :27017
2003 docker start cbc87ac0ac63
2004 docker start 4a75c26f2a7f
2005 clear
2006 cd getUsers
2007 cd /media/guddu/NewVolume/Deployment_Become_Developer/Docker/docker-testapp
2008 docker compose -f mongodb.yaml up -d
2009 docker network ls
2010 docker compose -f mongodb.yaml down
2011 docker network ls
2012 docker network rm mongodb-network
2013 docker network rm mongo-network
2014 docker network ls
2015 docker login
2016 docker logout
2017 docker login
2018 docker images
2019 docker push pradip123562m/testapp
2020 clear
2021 cd ..
2022 cd docker-testapp
2023 docker run -it -v /media/guddu/NewVolume/Deployment_Become_Developer/Docker/data:/test/data ubuntu
2024 docker run -it -v /home/guddu/Documents/data:/test/data ubuntu
2025 docker ps
2026 docker stop 7a3709b573ac
2027 docker ps
2028 docker ps -a
2029 docker start db5cfec4d06a
2030 docker exec -it db5cfec4d06a /bin/bash
2031 clear
2032 docker stop db5cfec4d06a
2033 docker rm db5cfec4d06a
2034 docker compose -f mongodb.yaml up
2035 docker compose -f mongodb.yaml down
2036 docker compose -f mongodb.yaml up
2037 docker volume ls
2038 docker volume create MyVolume
2039 docker volume ls
2040 docker volume rm MyVolume
2041 docker volume prune
2042 clear
2043 docker network ls
2044 docker network create myNetwork
2045 docker network ls
2046 history
guddu@g-IdeaPad-Slim-3-14IAH8:/media/guddu/NewVolume/Deployment_Become_Developer/Docker/docker-testapp$ ch
ch: command not found
guddu@g-IdeaPad-Slim-3-14IAH8:/media/guddu/NewVolume/Deployment_Become_Developer/Docker/docker-testapp$
