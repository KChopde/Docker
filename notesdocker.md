# Docker – Complete Notes for Developers & Interviews

## 1. Introduction
Docker is a **containerization platform** that packages an application and its dependencies into a standardized unit called a **container**.  
Containers ensure applications run consistently on any environment, solving dependency and configuration issues.

---

## 2. Why Docker?
- Eliminates environment inconsistency  
- Lightweight alternative to VMs  
- Faster deployments  
- Easy rollback & scaling  
- Perfect for microservices  
- Integrates smoothly with CI/CD  

---

## 3. Core Concepts

### 3.1 Docker Image
A **read-only template** used to create containers.  
Characteristics:
- Built from a Dockerfile  
- Made of layers (OverlayFS)  
- Immutable & portable  

### 3.2 Docker Container
A **running instance** of an image.  
Properties:
- Has its own filesystem, network, processes  
- Isolated via Linux kernel features  
- Lightweight (no full OS inside)  

### 3.3 Dockerfile
A text file defining steps to build an image.

Example:
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "app.py"]
```

### 3.4 Docker Registry
Stores images.

Examples:
- Docker Hub  
- AWS ECR  
- GitHub Container Registry  

---

## 4. Docker Architecture

```
+-------------------+
|   Docker Client   |
|   (docker CLI)    |
+---------+---------+
          |
          v
+-------------------+
|   Docker Daemon   |
|     (dockerd)     |
+---------+---------+
          |
          v
+-------------------------------+
|     containerd runtime        |
+-------------------------------+
          |
          v
+-------------------+
|       runc        |
|  (OCI runtime)    |
+-------------------+
```

### Components
- **Docker Client** – You run commands here  
- **Docker Daemon** – Manages containers & images  
- **containerd** – Manages container lifecycle  
- **runc** – Creates containers using Linux kernel  

---

## 5. How Docker Provides Isolation

### 5.1 Namespaces (Isolation)
Each container gets its own:
- Process IDs (PID)  
- Network interface  
- Mount points  
- Hostname  
- User IDs  

### 5.2 cgroups (Resource Limits)
Controls:
- CPU  
- Memory  
- I/O  
- Number of processes  

### 5.3 Union File System (OverlayFS)
Allows:
- Layered images  
- Fast caching  
- Reduced image size  

---

## 6. Docker Networking

### Network Types
- **bridge** → default, isolated container network  
- **host** → share host network stack  
- **none** → no network  
- **overlay** → multi-host networking (Swarm, K8s)  

### Port Mapping
Why Port Mapping is Needed?

Because containers are isolated → their ports are NOT accessible from outside unless you expose/map them.
```
docker run -p 8000:8000 app
```
Host:container

---

## 7. Docker Volumes (Persistent Storage)

Why volumes:
- Containers are ephemeral  
- Data needs to persist  

Types:
- **Named volumes**
- **Bind mounts**

Example:
```
docker run -v data:/var/lib/mysql mysql
```

---

## 8. Docker Compose

Used for **multi-container applications**.

Example:
```yaml
services:
  backend:
    build: .
    ports:
      - "8000:8000"

  mongodb:
    image: mongo
    volumes:
      - data:/data/db

volumes:
  data:
```

---

## 9. Docker vs Virtual Machines

| Feature | Docker | VM |
|--------|--------|-----|
| Boot Time | Seconds | Minutes |
| Size | MBs | GBs |
| Isolation | Process-level | Full OS |
| Performance | Near-native | Heavy |
| Best For | Microservices | Full OS needs |

---

## 10. Security Concepts
- User namespaces  
- Seccomp profiles  
- Dropping capabilities  
- Read-only root filesystem  
- Multi-stage builds for smaller images  

---

## 11. Docker Workflow (End-to-End)

1. Write application  
2. Create Dockerfile  
3. Build image  
   ```
   docker build -t myapp .
   ```
4. Run container  
   ```
   docker run -p 8000:8000 myapp
   ```
5. Push to registry  
6. Deploy with Docker, Compose, or Kubernetes  

---

## 12. Common Commands (Cheat Sheet)
[Find all other commands here](https://docs.docker.com/reference/cli/docker/)

### Image Commands
```
docker build -t name .
docker images
docker rmi image_id
```

### Container Commands
```
docker run -p 8000:8000 image
docker ps
docker ps -a
docker stop id
docker rm id
```

### Exec into container
```
docker exec -it container_id bash
```

### Volumes
```
docker volume ls
docker volume create name
```

---

## 13. Real World Use Cases
- Microservices  
- Machine learning deployments  
- Dev environment isolation  
- CI/CD pipelines  
- Cloud-native applications  
- Data engineering pipelines  
- API & backend deployments  

---

## 14. Quick Summary (Revision)

- **Images** = blueprint  
- **Containers** = runtime  
- **Dockerfile** = build steps  
- **Volumes** = persistent data  
- **Networks** = container communication  
- **Namespaces + cgroups** = isolation  
- **OverlayFS** = layered images  
- **Compose** = multi-container setups  

---
