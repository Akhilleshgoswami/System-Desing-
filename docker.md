
---

# 🐳 Docker Complete Guide

---

## 🔧 Docker Core Concepts

### 🖥 Docker Daemon
- Main engine of Docker (runs in background)
- Responsible for:
  - Running containers
  - Building images
  - Pulling images
- Acts as the **brain of Docker** that manages containers, images, networking, and storage.

### 💻 Docker CLI
- Command-line tool (`docker`) used to send commands to Docker Daemon
- Example:
```bash
docker run nginx
````

### 🖼 Docker Desktop

* GUI tool (for Windows/Mac)
* Helps you monitor:

  * Running containers
  * Available images
  * Resource usage
* Only a **visual interface**, not the core engine

---

## 📦 Images vs Containers

### 🖼 Docker Image

* Read-only template for containers
* Contains:

  * Operating system (lightweight)
  * Dependencies
  * Application code
* Think of it as a **blueprint**.

### 🐳 Docker Container

* Running instance of an image
* Isolated environment that executes application
* Think of it as a **live machine** created from the image.

### 🔗 Relationship

```
Image → Container (run)
```

---

## ⚙️ Basic Docker Commands

### ▶️ Run Container

```bash
docker run -it <image_name>
```

* Creates and starts a container
* `-it` → interactive terminal mode

### 📋 List Running Containers

```bash
docker container ls
```

* Shows **only running containers**
* Displays: Container ID, Image, Status, Ports

### 📋 List All Containers

```bash
docker container ls -a
```

* Shows **all containers** including stopped/exited

### ▶️ Start Container

```bash
docker start <container_name>
```

* Starts a **stopped container** without creating a new one

### ⏹ Stop Container

```bash
docker stop <container_name>
```

* Gracefully stops a running container

### 💻 Execute Command in Container

```bash
docker exec <container_name> ls
```

* Runs a command inside the container (non-interactive)

### 🖥 Interactive Shell Inside Container

```bash
docker exec -it <container_name> bash
```

* Opens terminal inside container for interactive commands

---

## 📦 Docker Images

### 📋 List Images

```bash
docker image ls
```

* Shows all locally available images
* Displays: Repository, Tag, Image ID, Size

---

## 🌐 Port Mapping

### ❌ Problem

* Node.js app runs inside container on port 8000
* Not accessible from host machine

```
http://localhost:8000 → ❌
```

* Because container ports are isolated

### ✅ Solution

```bash
docker run -it -p <host_port>:<container_port> <image_name>
```

### Example

```bash
docker run -it -p 1025:8000 node-app
```

* Container port → 8000
* Host port → 1025

Access via browser:

```
http://localhost:1025
```

### Explanation

* `-p <host_port>:<container_port>` → maps container’s internal port to host machine port

---

## 🔑 Environment Variables

### Purpose

* Pass configuration data into container:

  * API keys
  * DB URLs
  * Environment configs

### Command

```bash
docker run -it -p 3000:3000 -e KEY=value -e NODE_ENV=production <image_name>
```

### Example

```bash
docker run -it -p 3000:3000 -e PORT=3000 -e DB_URL=mongodb://localhost:27017 app_image
```

* Inside app:

```js
process.env.PORT
process.env.DB_URL
```

---

## 📄 Dockerfile

### Definition

* Text file containing instructions to build a Docker image
* Not a command, but a **recipe for creating images**

### Example Dockerfile (Node.js)

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "index.js"]
```

### Explanation

* `FROM node:18` → Base image with Node.js
* `WORKDIR /app` → Working directory in container
* `COPY package*.json ./` → Copy dependencies for caching
* `RUN npm install` → Install project dependencies
* `COPY . .` → Copy remaining project files
* `EXPOSE 3000` → Expose port 3000
* `CMD ["node", "index.js"]` → Command to run app

---

## 🏗 Build Docker Image

```bash
docker build -t node-v1 .
```

* `-t node-v1` → name/tag of image
* `.` → current directory (Dockerfile location)

---

## ▶️ Run Docker Image

```bash
docker run -it -p 3000:3000 node-v1
```

* Access app via:

```
http://localhost:3000
```

---
How Docker Caching Works
Docker builds images layer by layer
Each command in the Dockerfile creates a layer
If a layer hasn’t changed, Docker reuses the cached version instead of rebuilding
This speeds up builds significantly, especially for dependencies
Example
# Layer 1: Base image
FROM node:18

# Layer 2: Copy dependencies
COPY package*.json ./

# Layer 3: Install dependencies
RUN npm install

# Layer 4: Copy project files
COPY . .
If only project files change (Layer 4), Docker reuses cache for Layers 1–3
Benefit: npm install is not repeated unnecessarily
Best Practices for Caching
Copy package.json separately before copying full code
Run dependency install before copying application files
Keep frequently changing code at the bottom of Dockerfile
Use .dockerignore to exclude unnecessary files from build

## 🧠 Summary (Interview-ready)

* **Docker Daemon** → core engine running containers
* **CLI** → command-line tool interacting with daemon
* **Docker Desktop** → GUI interface to monitor containers and images
* **Image** → template/blueprint
* **Container** → running instance
* **Port Mapping** → expose container ports to host
* **Environment Variables** → pass configuration inside container
* **Dockerfile** → instructions to build image
* **docker build** → create image from Dockerfile
* **docker run** → start container from image

```

