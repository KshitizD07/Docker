# 🐳 Complete Docker & Containerization Guide

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.kernel.org/)
[![DevOps](https://img.shields.io/badge/DevOps-Ready-007ACC?style=for-the-badge&logo=azuredevops&logoColor=white)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

A structured, comprehensive, and production-grade guide covering everything from Docker fundamentals and virtualization internals to advanced Dockerfile optimization, storage drivers, networking segmentation, secrets management, and multi-tier architectures.

---

## 📚 Repository Modules & Roadmap

This repository is organized into targeted modules designed to take you from foundational concepts to production-level DevOps engineering:

```mermaid
flowchart LR
    M1["1. Core Concepts<br/>(aboutdocker.md)"]
    M2["2. Essential CLI<br/>(basic_commands.md)"]
    M3["3. Images & Dockerfiles<br/>(dockerfile.md)"]
    M4["4. Networking & Volumes<br/>(dockerNetworking.md)"]
    M5["5. Advanced & Multi-Tier<br/>(docker_misc_1.md)"]

    M1 ==> M2 ==> M3 ==> M4 ==> M5
```

| Module | Document | Core Topics Covered |
| :--- | :--- | :--- |
| **01. Foundations** | [📘 `aboutdocker.md`](./aboutdocker.md) | Virtualization vs. Containerization, Hypervisors, Docker Client-Daemon Architecture (`dockerd`, `containerd`, `runC`), Image Registries, Image vs Container comparison. |
| **02. Command Line** | [💻 `basic_commands.md`](./basic_commands.md) | Image lifecycle (`pull`, `images`, `rmi`), Container lifecycle (`create`, `start`, `stop`, `run`, `exec`, `rm`), debugging (`logs`, `inspect`, `prune`). |
| **03. Image Building** | [🛠️ `dockerfile.md`](./dockerfile.md) | Dockerfile instructions (`FROM`, `RUN`, `CMD`, `WORKDIR`, `EXPOSE`, `ENV`), Image layer caching, UnionFS, Build-time vs Runtime phases. |
| **04. Network & Storage** | [🌐 `dockerNetworking.md`](./dockerNetworking.md) | Docker software bridges (`docker0`), network segmentation, persistent named volumes, storage drivers, and data isolation. |
| **05. Advanced DevOps** | [🚀 `docker_misc_1.md`](./docker_misc_1.md) | Container resource inspection (`lscpu`, `df -h`, `docker stats`), `-i` vs `-t` flags, batch cleanup scripts, single-file vs directory bind mounts, port mapping, secrets management (MySQL case study), and multi-tier production architecture. |

---

## 🏛️ High-Level Docker Architecture

Docker utilizes a client-server architecture. The **Docker Client** speaks to the **Docker Daemon (`dockerd`)**, which handles the heavy lifting of building, running, and distributing your containers.

```mermaid
flowchart TD
    subgraph ClientLayer ["1. Client (CLI / UI)"]
        CLI["docker run / build / pull / exec"]
    end

    subgraph HostLayer ["2. Docker Host (Daemon & Runtimes)"]
        Daemon["Docker Daemon (dockerd)"]
        Containerd["containerd (High-Level Runtime)"]
        RunC["runC (OCI Low-Level Runtime)"]
        
        Daemon -->|gRPC| Containerd
        Containerd -->|Spawns| RunC
        
        subgraph ContainersZone ["Isolated Containers (Namespaces & Cgroups)"]
            C1["Container: Web / Nginx"]
            C2["Container: API / Node.js"]
            C3["Container: DB / PostgreSQL"]
        end
        
        RunC --> C1 & C2 & C3
    end

    subgraph RegistryLayer ["3. Docker Registry"]
        DockerHub[("Docker Hub / AWS ECR / GCP Artifact Registry")]
    end

    CLI -->|REST API over Unix Socket / TCP| Daemon
    Daemon <==>|Pushes / Pulls Images| DockerHub
```

---

## ⚡ Quick Reference Cheat Sheet

### 1. Managing Containers
```bash
# Run a detached container with custom name and port forwarding
docker run -d --name my-web -p 8080:80 nginx:alpine

# Execute an interactive shell inside a running container
docker exec -it my-web /bin/sh

# View container logs in real time
docker logs -f my-web

# List all containers with resource consumption
docker stats
```

### 2. Managing Images & Builds
```bash
# Build an image with a specific tag using current directory context
docker build -t my-app:1.0.0 .

# List local images
docker images

# Remove unused/dangling images
docker image prune -a
```

### 3. Networks & Volumes
```bash
# Create a dedicated isolated network
docker network create app-network

# Create a persistent named volume
docker volume create app-db-data

# Run container attached to custom network and volume
docker run -d \
  --name postgres-db \
  --network app-network \
  -v app-db-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=mysecret \
  postgres:15-alpine
```

### 4. Bulk Cleanup (Dev & CI/CD)
```bash
# Stop and force-remove all containers
docker rm -f $(docker ps -a -q)

# Nuclear cleanup (stopped containers, unused networks, dangling images, cache)
docker system prune -af --volumes
```

---

## 🛡️ Production & DevOps Best Practices

> [!TIP]
> **Layer Caching Optimization**  
> Order your `Dockerfile` instructions from least frequently changed to most frequently changed. Always copy package dependency files (e.g. `package.json`, `requirements.txt`, `go.mod`) and install dependencies *before* copying the remaining application source code.

> [!IMPORTANT]
> **Least Privilege & Security**  
> Avoid running containers as `root`. Use `USER nonroot` or create a dedicated application user inside your `Dockerfile`.

> [!WARNING]
> **Secrets Management**  
> Never bake API keys, private certificates, or database passwords into images (`ENV` or `ARG`) or pass them as plaintext CLI arguments. Use Docker Secrets, mounted files (`_FILE` convention), or secret management engines (e.g. AWS Secrets Manager, HashiCorp Vault).

---

## 🤝 Contributing & License

Contributions, improvements, and real-world DevOps examples are welcome!
This project is open-source and available under the [MIT License](LICENSE).
