# 🐳 Docker & Containerization — Interview Questions & Answers

A comprehensive, production-oriented interview guide covering Docker fundamentals, container internals, Dockerfiles, multi-stage builds, volumes, networking, Docker Compose, and Node.js deployment strategies.

---

## 📋 Table of Contents

1. [What Is Docker and Why Is It Used?](#1-what-is-docker-and-why-is-it-used)
2. [Difference Between a Virtual Machine (VM) and a Docker Container](#2-difference-between-a-virtual-machine-vm-and-a-docker-container)
3. [What Is a Docker Image?](#3-what-is-a-docker-image)
4. [What Is a Docker Container?](#4-what-is-a-docker-container)
5. [Difference Between a Docker Image and a Docker Container](#5-difference-between-a-docker-image-and-a-docker-container)
6. [What Is a Dockerfile and How Does It Work?](#6-what-is-a-dockerfile-and-how-does-it-work)
7. [Difference Between CMD and ENTRYPOINT in a Dockerfile](#7-difference-between-cmd-and-entrypoint-in-a-dockerfile)
8. [What Is Docker Compose and When Do You Use It?](#8-what-is-docker-compose-and-when-do-you-use-it)
9. [What Is a Volume in Docker and Why Is It Used?](#9-what-is-a-volume-in-docker-and-why-is-it-used)
10. [Why Are Volumes Critical for Databases (MySQL, PostgreSQL, MongoDB)?](#10-why-are-volumes-critical-for-databases-mysql-postgresql-mongodb)
11. [What Is Port Mapping and How Does It Work?](#11-what-is-port-mapping-and-how-does-it-work)
12. [Difference Between COPY and ADD in a Dockerfile](#12-difference-between-copy-and-add-in-a-dockerfile)
13. [What Is .dockerignore and Why Is It Important?](#13-what-is-dockerignore-and-why-is-it-important)
14. [How Do You Check Running and Stopped Containers?](#14-how-do-you-check-running-and-stopped-containers)
15. [How Do You Access the Shell of a Running Container?](#15-how-do-you-access-the-shell-of-a-running-container)
16. [How Do You View and Stream Container Logs?](#16-how-do-you-view-and-stream-container-logs)
17. [How Do You Stop, Kill, and Remove a Container?](#17-how-do-you-stop-kill-and-remove-a-container)
18. [How Do You Remove Docker Images and Free Disk Space?](#18-how-do-you-remove-docker-images-and-free-disk-space)
19. [What Is a Docker Network and What Are the Network Drivers?](#19-what-is-a-docker-network-and-what-are-the-network-drivers)
20. [How Do Containers Communicate with Each Other on the Same Network?](#20-how-do-containers-communicate-with-each-other-on-the-same-network)
21. [What Is a Multi-Stage Build and Why Is It Used?](#21-what-is-a-multi-stage-build-and-why-is-it-used)
22. [How Would You Dockerize a Production-Ready Node.js Application?](#22-how-would-you-dockerize-a-production-ready-nodejs-application)
23. [What Happens to Data If a Docker Container Crashes?](#23-what-happens-to-data-if-a-docker-container-crashes)
24. [Why Should Local node_modules Never Be Copied into a Docker Image?](#24-why-should-local-node_modules-never-be-copied-into-a-docker-image)
25. [Why Use Alpine Base Images Instead of Standard Images?](#25-why-use-alpine-base-images-instead-of-standard-images)
26. [How Would You Deploy a Node.js and MySQL Application Using Docker Compose?](#26-how-would-you-deploy-a-nodejs-and-mysql-application-using-docker-compose)

---

## 1. What Is Docker and Why Is It Used?

### 💡 Answer

**Docker** is an open-source containerization platform that packages an application and all its required dependencies (code, runtime, system libraries, environment variables, configuration) into an isolated, standardized unit called a **container**.

**The Problem Docker Solves:**
In traditional software deployment, developers face the classic issue: *"It works on my local machine, but fails on staging or production!"* This happens due to differences in OS versions, Node.js runtimes, missing global packages, or mismatched system libraries.

Docker eliminates this problem entirely by ensuring the application runs inside an identical execution environment across:
- Developer local machines (Mac, Windows, Linux)
- Staging servers
- Cloud virtual machines (AWS EC2, Azure VMs, GCP)
- Production Kubernetes / Docker Swarm clusters

**Key Benefits:**
- **Consistency:** Guarantees uniform environment from dev to production.
- **Isolation:** Each container runs in its own process space without interfering with other containers.
- **Portability:** Build once, run anywhere Docker is installed.
- **Fast Startup & Scalability:** Containers spin up in milliseconds compared to traditional virtual machines.

### 🎯 Quick Interview Answer

Docker is an open-source containerization platform that bundles an application and all its dependencies into an isolated container, guaranteeing consistent behavior across development, testing, and production environments.

---

## 2. Difference Between a Virtual Machine (VM) and a Docker Container

### 💡 Answer

The fundamental distinction lies in **virtualization level**: Virtual Machines virtualize at the **hardware level**, whereas Docker containers virtualize at the **operating system level**.

---

**Architecture Comparison:**

```text
[ Virtual Machine Architecture ]          [ Docker Container Architecture ]
┌─────────────────────────────┐           ┌─────────────┬─────────────────┐
│ App 1 (Node.js) │ App 2     │           │ App 1 (Node)│ App 2 (React)   │
├─────────────────┼───────────┤           ├─────────────┼─────────────────┤
│ Guest OS        │ Guest OS  │           │ Bins / Libs │ Bins / Libs     │
├─────────────────┴───────────┤           ├─────────────┴─────────────────┤
│ Hypervisor (VMware / KVM)   │           │ Docker Engine / Containerd    │
├─────────────────────────────┤           ├───────────────────────────────┤
│ Host OS (Linux / Windows)   │           │ Host OS (Linux Kernel)        │
├─────────────────────────────┤           ├───────────────────────────────┤
│ Physical Hardware / Server  │           │ Physical Hardware / Server    │
└─────────────────────────────┘           └───────────────────────────────┘
```

---

**Comparison Table:**

| Feature | Virtual Machine (VM) | Docker Container |
| :--- | :--- | :--- |
| **Virtualization** | Hardware level (via Hypervisor) | OS level (via Linux Namespaces & cgroups) |
| **Operating System** | Full independent **Guest OS** per VM | Shares the **Host OS kernel** |
| **Size on Disk** | Heavy (Gigabytes: 10GB–50GB+) | Lightweight (Megabytes: 50MB–500MB) |
| **Startup Time** | Slow (Minutes to boot Guest OS) | Instant (Milliseconds to seconds) |
| **Memory / CPU Overhead** | High (Allocates dedicated RAM/CPU) | Minimal (Shares resources dynamically) |
| **Isolation Level** | Strong hardware isolation | Process-level isolation |

### 🎯 Quick Interview Answer

Virtual Machines virtualize the hardware and require a complete guest OS per VM, making them heavy and slow to boot. Docker containers share the host OS kernel and isolate processes using Linux namespaces and cgroups, making them lightweight and able to start instantly.

---

## 3. What Is a Docker Image?

### 💡 Answer

A **Docker Image** is an immutable, read-only template that contains the application source code, runtime, system libraries, configuration files, and dependencies needed to run an application.

---

**Key Characteristics:**
- **Immutable & Read-Only:** Once built, an image cannot be changed. If code changes, a new image layer is created.
- **Layered File System (UnionFS):** Every command in a `Dockerfile` (e.g., `RUN`, `COPY`) creates a cached, read-only layer. Unchanged layers are reused across builds for near-instant build times.
- **Blueprints:** Think of an image as a compiled executable, a blueprint, or a class in OOP.

**Example Command:**
```bash
# Pulls the official Node.js 20 LTS Alpine image from Docker Hub
docker pull node:20-alpine
```

### 🎯 Quick Interview Answer

A Docker image is a read-only, layered blueprint that bundles the application code, runtime, dependencies, and system tools required to instantiate a container.

---

## 4. What Is a Docker Container?

### 💡 Answer

A **Docker Container** is a live, running instance of a Docker Image. 

When you run a container from an image, Docker adds a thin **read-write layer** (the container layer) on top of the underlying read-only image layers. Any file modifications, temporary caches, or logs created during execution are stored in this read-write layer.

---

**Example Command:**
```bash
# Run a Node.js 20 container in detached (-d) mode
docker run -d --name my-node-app -p 3000:3000 node:20-alpine
```

**Container Lifecycle States:**
`Created` $\rightarrow$ `Running` $\rightarrow$ `Paused / Stopped` $\rightarrow$ `Exited` $\rightarrow$ `Removed`

### 🎯 Quick Interview Answer

A Docker container is a running, executable instance of an image that executes as an isolated process on the host system with its own writable filesystem layer and network namespace.

---

## 5. Difference Between a Docker Image and a Docker Container

### 💡 Answer

The relationship between an image and a container is identical to the relationship between a **Class and an Object** in Object-Oriented Programming:

---

**Comparison Table:**

| Feature | Docker Image | Docker Container |
| :--- | :--- | :--- |
| **Analogy** | Blueprint / Recipe / Class | House / Dish / Object Instance |
| **State** | Static, read-only template | Dynamic, living process in execution |
| **Mutability** | 100% Immutable (cannot be edited) | Writable (top layer permits file changes) |
| **Storage** | Stored on disk or in Docker Registry (Docker Hub, AWS ECR) | Stored in system RAM and host process table |
| **Multiplicity** | Built once | Spawns multiple independent containers |

---

**Practical Example:**
```bash
# One single image (nginx:alpine) can spawn multiple independent running containers
docker run -d --name web-server-1 -p 8081:80 nginx:alpine
docker run -d --name web-server-2 -p 8082:80 nginx:alpine
```

### 🎯 Quick Interview Answer

A Docker image is a static, read-only blueprint saved on disk; a container is an active, writable running process instantiated from that image.

---

## 6. What Is a Dockerfile and How Does It Work?

### 💡 Answer

A **Dockerfile** is a plain text file containing a sequential list of declarative instructions and arguments that the Docker engine executes to automatically build a custom Docker image.

---

**Core Instructions:**
- `FROM`: Specifies the base image (e.g., `FROM node:20-alpine`).
- `WORKDIR`: Sets the working directory inside the container for subsequent commands.
- `COPY`: Copies files/folders from the host machine into the container.
- `RUN`: Executes commands inside the container during image build time (e.g., `RUN npm install`).
- `EXPOSE`: Documents the network port the container listens on at runtime.
- `ENV`: Sets environment variables.
- `CMD`: Specifies the default command to execute when the container starts.

---

**Example Node.js Dockerfile:**

```dockerfile
# 1. Base image
FROM node:20-alpine

# 2. Set directory inside container
WORKDIR /app

# 3. Copy dependency manifests first (to leverage layer caching)
COPY package*.json ./

# 4. Install production dependencies
RUN npm install --only=production

# 5. Copy the rest of the application code
COPY . .

# 6. Inform Docker of the listening port
EXPOSE 3000

# 7. Start the application
CMD ["npm", "start"]
```

**Build Command:**
```bash
docker build -t my-node-service:1.0 .
```

### 🎯 Quick Interview Answer

A Dockerfile is a step-by-step configuration script containing commands (`FROM`, `WORKDIR`, `COPY`, `RUN`, `CMD`) that the Docker engine uses to build a reproducible container image.

---

## 7. Difference Between CMD and ENTRYPOINT in a Dockerfile

### 💡 Answer

Both `CMD` and `ENTRYPOINT` specify the command executed when a container starts, but they behave differently when arguments are passed via the command line (`docker run`):

---

**1. CMD (Default Command / Overridable):**
- Sets a default command that **can be completely replaced** by parameters supplied during `docker run`.
- Example in Dockerfile:
  ```dockerfile
  CMD ["npm", "start"]
  ```
- If you run: `docker run my-image node app.js`, the default `npm start` is completely ignored and replaced with `node app.js`.

---

**2. ENTRYPOINT (Fixed Executable / Immutable):**
- Configures the container to run as a fixed executable. Command-line parameters passed to `docker run` are **appended as arguments** rather than replacing the command.
- Example in Dockerfile:
  ```dockerfile
  ENTRYPOINT ["node"]
  CMD ["server.js"]
  ```
- If you run: `docker run my-image app.js`, the command executed is `node app.js` (`CMD` was overridden as an argument, while `ENTRYPOINT` remained fixed).

---

**Comparison Table:**

| Feature | CMD | ENTRYPOINT |
| :--- | :--- | :--- |
| **Purpose** | Provides default command or default arguments | Configures an unchangeable base executable |
| **CLI Override** | Fully replaced by `docker run <image> <command>` | Appends CLI parameters as arguments to entrypoint |
| **Best Practice** | General application servers | CLI utility tools, database engines |

### 🎯 Quick Interview Answer

`ENTRYPOINT` sets the fixed executable that will always run when the container starts, whereas `CMD` provides default arguments that can easily be overridden from the `docker run` command line.

---

## 8. What Is Docker Compose and When Do You Use It?

### 💡 Answer

**Docker Compose** is an orchestration tool used to define, configure, and manage **multi-container Docker applications** using a single YAML configuration file, typically named `docker-compose.yml`.

---

**Why It Is Used:**
In modern microservices or full-stack applications, your system consists of multiple interdependent services (e.g., a React frontend, a Node.js backend, a MySQL database, and a Redis cache). Manually starting each container with `docker run`, configuring network bridges, and passing port flags is slow, error-prone, and unmaintainable.

Docker Compose allows you to launch the entire stack with a single command:
```bash
docker compose up -d
```

---

**Example `docker-compose.yml` (Node.js + MySQL):**

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=mysql_db
      - DB_USER=root
      - DB_PASSWORD=secret
    depends_on:
      - mysql_db

  mysql_db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: my_database
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

### 🎯 Quick Interview Answer

Docker Compose is a tool that allows developers to define, configure, and run multi-container applications (e.g., Node.js + MySQL + Redis) through a single declarative `docker-compose.yml` file using one command (`docker compose up`).

---

## 9. What Is a Volume in Docker and Why Is It Used?

### 💡 Answer

A **Docker Volume** is a dedicated storage directory on the host machine filesystem, fully managed by Docker, used to **persist data generated by and used by running containers**.

---

**The Problem of Ephemeral Containers:**
By default, all files created inside a container are stored in its temporary read-write layer. When the container is deleted (`docker rm`), that read-write layer is permanently destroyed, causing total data loss.

**How Volumes Solve This:**
Volumes exist outside the container lifecycle on the host machine (`/var/lib/docker/volumes/` on Linux). When a volume is mounted to a container directory:
- Data written by the container is stored safely on the host.
- If the container crashes or is deleted, the data remains 100% intact.
- A new container can immediately attach to the same volume and resume operations.

---

**Commands:**
```bash
# 1. Create a named volume
docker volume create my_data_volume

# 2. Mount volume to a container directory (-v <volume_name>:<container_path>)
docker run -d -v my_data_volume:/app/data --name app-service my-app
```

### 🎯 Quick Interview Answer

A Docker Volume is a persistent storage mechanism managed by Docker on the host filesystem that survives container restarts, updates, and removals, ensuring critical application data is never lost.

---

## 10. Why Are Volumes Critical for Databases (MySQL, PostgreSQL, MongoDB)?

### 💡 Answer

Database engines like MySQL, PostgreSQL, and MongoDB continuously write transactional data, indexes, and logs to the container's internal filesystem:
- **MySQL:** `/var/lib/mysql`
- **PostgreSQL:** `/var/lib/postgresql/data`
- **MongoDB:** `/data/db`

---

**Why Volumes Are Mandatory for DB Containers:**
1. **Container Upgrades:** When upgrading MySQL from version `8.0` to `8.4`, you remove the old container and spin up the new one. With a volume, the new container immediately reads all existing tables without downtime or data migration.
2. **Crash Resilience:** If the database process runs out of memory (OOM) and the container is killed, all tables, users, and transactions remain saved in the volume.
3. **Backup Management:** Backups can be taken directly from the host volume directory without locking database processes.

```yaml
# Mounting persistent volume in Docker Compose for MySQL
services:
  db:
    image: mysql:8.0
    volumes:
      - mysql_storage:/var/lib/mysql

volumes:
  mysql_storage:
```

### 🎯 Quick Interview Answer

Database containers must use volumes because container filesystems are ephemeral. Mounting database directories to persistent volumes guarantees that transactions and table data survive container restarts, crashes, and version upgrades.

---

## 11. What Is Port Mapping and How Does It Work?

### 💡 Answer

Each Docker container runs in an isolated network namespace with its own internal, private IP address. By default, external traffic and localhost cannot reach services running inside the container.

**Port Mapping** (also called Port Forwarding or Publishing) binds a port on the **Host Machine** to a port inside the **Container**.

---

**Syntax:**
```bash
docker run -p <HostPort>:<ContainerPort> <image_name>
```

**Real Example:**
```bash
docker run -d -p 8080:3000 --name node-app my-node-image
```

**Traffic Flow:**
```text
Browser Request: http://localhost:8080
       │
       ▼
Host Machine Port 8080
       │
       ▼ (Docker NAT Bridge Routing)
Container Private Port 3000 (Node.js Express Server)
```

> **Note on EXPOSE:** The `EXPOSE 3000` instruction in a `Dockerfile` serves only as documentation for developers. It does **not** publish the port to the host. You must explicitly pass `-p` (or use `ports:` in Compose) to make it accessible.

### 🎯 Quick Interview Answer

Port mapping binds a port on the host machine to an internal port inside the container (`-p host:container`), routing incoming network traffic into the container's isolated network space.

---

## 12. Difference Between COPY and ADD in a Dockerfile

### 💡 Answer

Both `COPY` and `ADD` are instructions used to transfer files and directories from the host machine into the container's filesystem during image build, but `ADD` includes additional automated features:

---

**Comparison Table:**

| Feature | COPY | ADD |
| :--- | :--- | :--- |
| **Core Function** | Copies local files/folders from build context to image | Copies local files, fetches remote URLs, and auto-extracts tar archives |
| **Remote URL Support** | ❌ No | ✅ Yes (`ADD https://example.com/file.tar.gz /tmp/`) |
| **Archive Auto-Extraction** | ❌ No (Copies `.tar.gz` as a compressed file) | ✅ Yes (Automatically unpacks compressed `.tar.gz` files into destination) |
| **Predictability & Security** | High (Does exactly what you expect) | Lower (May introduce unexpected remote files or unpack archives unintentionally) |
| **Official Best Practice** | **Recommended for almost all use cases** | Only recommended when auto-extracting local tarballs |

**Code Example:**
```dockerfile
# Best Practice: Use COPY for standard application files
COPY package*.json ./
COPY . .

# Only use ADD when unpacking a tarball directly:
ADD app-bundle.tar.gz /app/
```

### 🎯 Quick Interview Answer

`COPY` is the standard instruction for copying local files into an image; `ADD` offers advanced capabilities like downloading from remote URLs and automatically unpacking tar archives, though `COPY` is preferred for clarity and security.

---

## 13. What Is .dockerignore and Why Is It Important?

### 💡 Answer

The **`.dockerignore`** file works exactly like `.gitignore`. It defines a list of files and directories that the Docker CLI should exclude when sending the **Build Context** to the Docker daemon during `docker build`.

---

**Standard `.dockerignore` for Node.js:**
```text
node_modules
npm-debug.log
.git
.gitignore
.env
.env.*
dist
Dockerfile
README.md
coverage
```

---

**Why It Is Crucial:**
1. **Faster Build Times:** Prevents uploading hundreds of megabytes of local `node_modules` or `.git` history into the Docker daemon daemon memory.
2. **Smaller Final Image:** Prevents bloating image layers with temporary files and logs.
3. **Security (Prevents Secret Leaks):** Ensures sensitive local secrets (`.env` files containing DB passwords or API keys) are not baked into public image layers.
4. **Avoids Architecture Incompatibilities:** Ensures local macOS/Windows compiled C++ binaries are not copied into Linux containers.

### 🎯 Quick Interview Answer

`.dockerignore` prevents unnecessary and sensitive files (like `node_modules`, `.git`, and `.env`) from being uploaded to the Docker build context, resulting in faster builds, smaller images, and enhanced security.

---

## 14. How Do You Check Running and Stopped Containers?

### 💡 Answer

Docker provides the `ps` command to inspect container runtime states:

---

**1. View Only Currently Running Containers:**
```bash
docker ps
```
*Displays:* Container ID, Image, Command, Created Time, Status (Up), Port Mappings, and Name.

---

**2. View All Containers (Running, Stopped, Exited):**
```bash
docker ps -a
```
*Use case:* Helps identify containers that failed on startup, crashed with exit code 1, or were stopped.

---

**3. View Only Container IDs (Useful for Scripting):**
```bash
# Get IDs of running containers
docker ps -q

# Stop all running containers at once:
docker stop $(docker ps -q)
```

### 🎯 Quick Interview Answer

Use `docker ps` to display all currently active and running containers, and use `docker ps -a` to view all containers, including stopped, exited, or failed instances.

---

## 15. How Do You Access the Shell of a Running Container?

### 💡 Answer

To open an interactive shell inside an already running container for debugging, inspecting files, or checking logs, use the **`docker exec`** command with the `-it` flags:

---

**Command:**
```bash
# For Alpine-based containers (uses sh):
docker exec -it <container_name_or_id> sh

# For Ubuntu/Debian/Standard containers (uses bash):
docker exec -it <container_name_or_id> bash
```

**Flag Breakdown:**
- `-i` (**interactive**): Keeps STDIN open so you can type commands.
- `-t` (**pseudo-TTY**): Allocates a terminal prompt for formatting and shell interaction.

**Difference: `docker run` vs `docker exec`:**
- `docker run`: Creates and starts a **brand-new** container from an image.
- `docker exec`: Runs a new command or opens a shell inside an **already running** container.

### 🎯 Quick Interview Answer

Run `docker exec -it <container_id> sh` (or `bash`) to attach an interactive terminal session inside a running container for real-time debugging and configuration inspection.

---

## 16. How Do You View and Stream Container Logs?

### 💡 Answer

Docker captures all messages written to Standard Output (`stdout`) and Standard Error (`stderr`) and exposes them via the `docker logs` command:

---

**1. Print Recent Logs:**
```bash
docker logs <container_name_or_id>
```

**2. Follow / Stream Logs in Real-Time (`-f` flag):**
```bash
docker logs -f <container_name_or_id>
```
*Equivalent to Linux `tail -f`. Press `Ctrl + C` to exit.*

**3. View the Last N Lines:**
```bash
docker logs --tail 100 -f <container_name_or_id>
```

**4. View Logs with Timestamps:**
```bash
docker logs -t <container_name_or_id>
```

> **Production Tip:** In Node.js applications, always use `console.log()` or logging libraries like Winston that stream to `process.stdout`. Avoid writing logs exclusively to local files inside the container.

### 🎯 Quick Interview Answer

Use `docker logs <container_id>` to view terminal output, and append the `-f` flag (`docker logs -f <container_id>`) to stream live logs in real time.

---

## 17. How Do You Stop, Kill, and Remove a Container?

### 💡 Answer

Container termination and cleanup involve distinct commands depending on whether a graceful shutdown or immediate termination is required:

---

**1. Gracefully Stop a Container (`SIGTERM`):**
```bash
docker stop <container_id>
```
- Sends `SIGTERM` to the main process inside the container.
- Gives the application a 10-second grace period to close database connections, complete active HTTP requests, and exit cleanly.
- If it does not stop in 10 seconds, Docker issues `SIGKILL`.

---

**2. Immediately Kill a Container (`SIGKILL`):**
```bash
docker kill <container_id>
```
- Instantly halts the container without waiting for clean shutdown.

---

**3. Remove a Stopped Container:**
```bash
docker rm <container_id>
```

**4. Force Stop and Remove a Running Container:**
```bash
docker rm -f <container_id>
```

**5. Clean Up All Stopped Containers at Once:**
```bash
docker container prune
```

### 🎯 Quick Interview Answer

Use `docker stop` to gracefully terminate a container using `SIGTERM`, and `docker rm` to remove it from disk. Use `docker container prune` to clean up all stopped containers in bulk.

---

## 18. How Do You Remove Docker Images and Free Disk Space?

### 💡 Answer

Over time, unused images, intermediate build caches, and dangling images consume significant disk space:

---

**1. Remove a Specific Image:**
```bash
docker rmi <image_id_or_name>
```
*(Fails if any active or stopped container is still referencing the image).*

**2. Force Remove an Image:**
```bash
docker rmi -f <image_id>
```

**3. Remove Dangling Images (Untagged `<none>` images):**
```bash
docker image prune
```

**4. The Complete Nuclear Cleanup (System Prune):**
```bash
# Removes all stopped containers, unused networks, and dangling images
docker system prune

# Comprehensive cleanup (includes all unused images and build caches):
docker system prune -a --volumes
```

### 🎯 Quick Interview Answer

Delete a specific image using `docker rmi <image_id>`. To reclaim disk space, run `docker image prune` to remove untagged layers, or `docker system prune -a` to clean up all unused containers, images, and networks.

---

## 19. What Is a Docker Network and What Are the Network Drivers?

### 💡 Answer

A **Docker Network** provides an isolated communication layer enabling containers to discover and securely communicate with each other, the host system, and external networks.

---

**Core Network Drivers:**

| Network Driver | Description | Common Use Case |
| :--- | :--- | :--- |
| **bridge** | Default network driver on a single host. Creates a private virtual network; containers get internal IPs (e.g., `172.17.0.x`). | Standard multi-container apps on a single server |
| **host** | Removes network isolation between container and host. The container binds directly to host network interfaces (no `-p` needed). | High-performance apps requiring lowest possible latency |
| **overlay** | Multi-host network allowing containers across different physical/virtual servers to communicate securely. | Docker Swarm & multi-node clusters |
| **none** | Disables all networking interfaces except loopback. Complete isolation. | High-security offline batch jobs / cryptographic tasks |

---

**Commands:**
```bash
# Create a custom bridge network
docker network create my-app-network

# Run container on custom network
docker run -d --network my-app-network --name backend-api my-node-image
```

### 🎯 Quick Interview Answer

Docker networks manage inter-container traffic. The most common drivers are `bridge` (default isolated network on a single host), `host` (shares host network stack for zero overhead), and `overlay` (multi-host swarm routing).

---

## 20. How Do Containers Communicate with Each Other on the Same Network?

### 💡 Answer

Containers attached to the **same user-defined network** communicate seamlessly using **Automatic DNS Service Discovery**.

---

**How Embedded DNS Works:**
1. When you create a custom bridge network or use Docker Compose, Docker runs an internal DNS server at `127.0.0.11`.
2. This DNS server maps each **container name** or **service name** directly to its internal container IP.
3. Applications do not need hardcoded IP addresses (which change upon container restarts).

---

**Practical Example in Docker Compose:**

```yaml
version: '3.8'

services:
  web_api:
    build: .
    ports:
      - "3000:3000"
    networks:
      - internal_net

  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: shop_db
    networks:
      - internal_net

networks:
  internal_net:
    driver: bridge
```

**Inside Node.js Express Code:**
```javascript
// Connect directly using the service name 'database' as the hostname!
const mysql = require('mysql2');

const connection = mysql.createConnection({
  host: 'database', // Handled automatically by Docker DNS
  user: 'root',
  password: 'rootpassword',
  database: 'shop_db',
  port: 3306
});
```

> **Important Difference:** The default default bridge network (`bridge`) does **not** support DNS resolution by container name; only **custom user-defined networks** and Docker Compose networks enable automatic DNS.

### 🎯 Quick Interview Answer

Containers on the same user-defined network communicate using Docker's embedded DNS server, which resolves container or Compose service names directly into internal IP addresses without hardcoding IPs.

---

## 21. What Is a Multi-Stage Build and Why Is It Used?

### 💡 Answer

A **Multi-Stage Build** is a Dockerfile optimization technique where multiple `FROM` instructions are used in a single file to separate the **build/compilation environment** from the **final runtime environment**.

---

**The Problem with Single-Stage Builds:**
To build a modern Node.js/TypeScript or React application, you need compilers (`tsc`), development dependencies (`devDependencies`), testing frameworks, and build tools (`webpack`, `vite`). If you ship all of this in your production image, your image size swells to **800MB–1.5GB**, making deployments slow and increasing security vulnerabilities.

**The Multi-Stage Solution:**
- **Stage 1 (Builder):** Uses a complete environment to install all dependencies and compile code.
- **Stage 2 (Production Runtime):** Starts fresh from a minimal Alpine base image and copies **only** the compiled output (`dist/`) and production dependencies (`npm ci --only=production`).

---

**Example Multi-Stage Dockerfile:**

```dockerfile
# ==========================================
# STAGE 1: Build & Compile TypeScript
# ==========================================
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# ==========================================
# STAGE 2: Lightweight Production Runtime
# ==========================================
FROM node:20-alpine AS runner

WORKDIR /app
ENV NODE_ENV=production

COPY package*.json ./
RUN npm ci --only=production

# Copy only the compiled JS from builder stage
COPY --from=builder /app/dist ./dist

USER node
EXPOSE 3000

CMD ["node", "dist/server.js"]
```

**Benefits:**
- **Drastic Size Reduction:** Reduces images from ~1GB down to ~80MB–120MB.
- **Security:** Excludes source code, build tools, git, and devDependencies from production.

### 🎯 Quick Interview Answer

A multi-stage build uses multiple `FROM` instructions in a single Dockerfile to separate build-time dependencies from the runtime environment, resulting in dramatically smaller and more secure production images.

---

## 22. How Would You Dockerize a Production-Ready Node.js Application?

### 💡 Answer

A production-ready Docker setup requires security best practices, layer caching, non-root user execution, and graceful process management:

---

**Production Dockerfile:**

```dockerfile
# 1. Use a specific LTS Alpine image
FROM node:20-alpine

# 2. Set working directory
WORKDIR /usr/src/app

# 3. Set production environment
ENV NODE_ENV=production

# 4. Copy dependency manifests first to leverage Docker layer caching
COPY package*.json ./

# 5. Clean, reproducible install of production dependencies only
RUN npm ci --only=production && npm cache clean --force

# 6. Copy remaining application files
COPY . .

# 7. Run as non-root user for security (Alpine Node image provides 'node' user)
USER node

# 8. Expose the port
EXPOSE 3000

# 9. Execute with node directly (NOT npm start) to ensure proper SIGTERM signal handling
CMD ["node", "server.js"]
```

---

**Essential `.dockerignore`:**
```text
node_modules
npm-debug.log
.git
.env
.env.*
Dockerfile
.dockerignore
```

**Build and Run Commands:**
```bash
# Build the production image
docker build -t my-node-app:1.0 .

# Run in background with memory limit and restart policy
docker run -d \
  --name production-node-api \
  -p 3000:3000 \
  --memory="512m" \
  --restart=unless-stopped \
  my-node-app:1.0
```

> **Why avoid `CMD ["npm", "start"]` in production?**  
> `npm` does not forward Linux OS signals (`SIGTERM`, `SIGINT`) to the child Node.js process. When Docker stops the container, Node never receives the signal, preventing graceful connection closures and forcing a hard kill after 10 seconds. Using `node server.js` fixes this.

### 🎯 Quick Interview Answer

To Dockerize a production Node.js app: use an Alpine base image, copy `package.json` first for layer caching, run `npm ci --only=production`, switch to a non-root `node` user, and execute directly with `CMD ["node", "server.js"]` for proper OS signal handling.

---

## 23. What Happens to Data If a Docker Container Crashes?

### 💡 Answer

What happens to data upon a container crash depends entirely on **where the data is written**:

---

- **Case 1: Data stored in the container's writable layer (NO Volume mounted):**
  - If the container crashes and enters the `Exited` state, the data **is still preserved** in the container's layer on disk.
  - If you restart it (`docker restart <id>`), the data remains accessible.
  - **However:** As soon as the container is removed (`docker rm <id>` or recreated via a new deployment/Compose), the writable layer is permanently deleted and all data is **lost forever**.

- **Case 2: Data stored in a mounted Docker Volume or Bind Mount:**
  - The data resides directly on the host machine filesystem outside the container.
  - When the container crashes, stops, or is completely deleted with `docker rm -f`, **100% of the data remains safe on disk**.
  - A newly spawned container can immediately mount the same volume and continue operating without loss.

### 🎯 Quick Interview Answer

Data written to a container's internal writable layer survives a crash but is permanently destroyed if the container is removed; data stored in a mounted volume remains safe on the host filesystem regardless of container crashes or deletions.

---

## 24. Why Should Local node_modules Never Be Copied into a Docker Image?

### 💡 Answer

Developers must always add `node_modules` to `.dockerignore` and run `npm install` or `npm ci` inside the Docker image during the build process, due to the following critical reasons:

---

1. **OS & Architecture Incompatibilities (Native C++ Addons):**
   - Packages like `bcrypt`, `sharp`, `sqlite3`, or `node-sass` compile C/C++ bindings against the host OS kernel and C libraries (e.g., macOS Darwin, Windows, or glibc Linux).
   - If you copy local macOS or Windows `node_modules` into a Linux Alpine container (`musl libc`), the application will crash immediately on startup with `Error: Cannot find module` or binary mismatch errors.
2. **Slow Builds & Context Bloat:**
   - A local `node_modules` folder can easily contain 40,000+ files and 500MB+ of data. Transferring this across the Docker build context adds minutes to every single build.
3. **Dirty / Inconsistent Dependencies:**
   - Local environments often contain test packages, outdated peer dependencies, or development packages that should never be present in a clean production build.

### 🎯 Quick Interview Answer

Local `node_modules` must never be copied into an image because native C++ binaries compiled on macOS or Windows will fail on Linux containers. Adding `node_modules` to `.dockerignore` and running `npm ci` inside the Dockerfile guarantees clean, platform-compatible binaries.

---

## 25. Why Use Alpine Base Images Instead of Standard Images?

### 💡 Answer

In production Docker environments, using minimal distributions like **Alpine Linux** (`node:20-alpine`) instead of standard distributions (`node:20` based on Debian) offers two immense advantages:

---

**1. Massive Reduction in Image Size:**
- Standard `node:20` image: **~1.1 GB**
- Alpine-based `node:20-alpine` image: **~180 MB**
- *Result:* Over **80% reduction** in storage, enabling 5x faster CI/CD build uploads and cloud deployment downloads.

---

**2. Drastically Reduced Attack Surface & Security Vulnerabilities:**
- Standard Linux distributions bundle hundreds of utility binaries (e.g., `curl`, `python`, `perl`, `sed`, `git`, package managers). Every installed binary represents a potential CVE (Common Vulnerabilities and Exposures) security exploit.
- Alpine is stripped down to the bare minimum required to run the OS and Node.js runtime, drastically minimizing security attack vectors.

---

**Comparison Table:**

| Metric | `node:20` (Debian-based) | `node:20-alpine` (Alpine-based) |
| :--- | :--- | :--- |
| **Base Image Size** | ~1.1 GB | ~180 MB |
| **C Library** | `glibc` | `musl libc` |
| **Security Vulnerabilities (CVEs)** | High (Includes hundreds of OS packages) | Extremely Low (Minimal surface) |
| **CI/CD Pull/Push Speed** | Slow | Blazing Fast |

> **Trade-off to remember:** Because Alpine uses `musl libc` instead of `glibc`, compiling native packages (like `node-gyp`) occasionally requires adding `python3`, `make`, and `g++` in the Dockerfile build stage.

### 🎯 Quick Interview Answer

Alpine base images reduce image sizes by over 80% (from ~1.1GB down to ~180MB) and dramatically shrink the security attack surface by excluding unnecessary Linux binaries and packages.

---

## 26. How Would You Deploy a Node.js and MySQL Application Using Docker Compose?

### 💡 Answer

This is one of the most frequently asked practical architecture questions in senior full-stack and backend interviews (TCS, Infosys, Accenture, Capgemini, Cognizant, and product companies).

---

**1. Project Structure:**
```text
my-fullstack-app/
├── Dockerfile
├── .dockerignore
├── docker-compose.yml
├── package.json
└── server.js
```

---

**2. The Complete `docker-compose.yml`:**

```yaml
version: '3.8'

services:
  # ---------------------------------------------
  # Node.js API Service
  # ---------------------------------------------
  api:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: node_backend_app
    restart: always
    ports:
      - "3000:3000"
    environment:
      - PORT=3000
      - DB_HOST=mysql_db
      - DB_USER=root
      - DB_PASSWORD=dbsecret
      - DB_NAME=company_db
    depends_on:
      mysql_db:
        condition: service_healthy
    networks:
      - app_network

  # ---------------------------------------------
  # MySQL Database Service
  # ---------------------------------------------
  mysql_db:
    image: mysql:8.0
    container_name: mysql_database
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: dbsecret
      MYSQL_DATABASE: company_db
    ports:
      - "3306:3306"
    volumes:
      - mysql_storage:/var/lib/mysql
    networks:
      - app_network
    # Healthcheck ensures Node.js doesn't start before MySQL is ready to accept connections
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root", "-pdbsecret"]
      interval: 10s
      timeout: 5s
      retries: 5

# ---------------------------------------------
# Persistent Volumes & Custom Bridge Networks
# ---------------------------------------------
volumes:
  mysql_storage:
    driver: local

networks:
  app_network:
    driver: bridge
```

---

**3. Execution Workflow:**

```bash
# Step 1: Build images and start all containers in detached mode
docker compose up --build -d

# Step 2: Check status of running services
docker compose ps

# Step 3: Stream live logs from Node.js service
docker compose logs -f api

# Step 4: Stop and tear down all containers (volumes remain safe)
docker compose down
```

### 🎯 Quick Interview Answer

To deploy a Node.js + MySQL stack: configure a `docker-compose.yml` file defining both services, attach a persistent named volume to `/var/lib/mysql` for data safety, link them through a shared bridge network, use MySQL's service name as the database host, and enforce a healthcheck so Node.js connects only after MySQL is ready.
