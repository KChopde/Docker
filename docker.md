# Docker Notes – `docker.md`

---

## 1. What is Docker?

- **Docker** is a platform to **build, ship, and run** applications in **containers**.
- **Container** = lightweight, isolated environment sharing the **host OS kernel**, but with its own filesystem, processes, and network namespace.
- Compared to VMs:
  - VMs: full OS per VM (hypervisor overhead).
  - Containers: share OS kernel → **faster start**, **lighter** (MBs vs GBs).

**Key benefits**
- Consistency across environments (“works on my machine” → gone).
- Fast startup & shutdown.
- Easy scaling and deployment.

---

## 2. Core Components & Architecture

- **Docker Client (`docker` CLI)**  
  Sends commands to the Docker daemon.

- **Docker Daemon (`dockerd`)**  
  Runs on host, manages images, containers, networks, volumes.

- **Docker Image**
  - Immutable **template** (filesystem + metadata) used to create containers.
  - Built from `Dockerfile` in **layers**.

- **Docker Container**
  - **Running instance** of an image (plus runtime state).

- **Docker Registry**
  - Stores images (e.g. **Docker Hub**, private registries).
  - `docker pull`, `docker push`.

---

## 3. Important Docker Concepts

### 3.1 Images & Layers

- Image = stack of **read-only layers**.
- Each line in `Dockerfile` → new **layer**.
- Final container = image layers + **thin read-write layer**.
- Benefit: **layer caching** → faster builds and smaller storage.

### 3.2 Containers

- Ephemeral by default:
  - If container is deleted, data is lost unless stored in **volumes** or external storage.
- Container lifecycle:
  - `create` → `start` → `running` → `stopped` → `removed`.

---

## 4. Dockerfile – Core Instructions

**Basic structure:**

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
