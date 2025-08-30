# 🐳 Docker & Containerization - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Docker & Containerization
├── 🐳 Docker Fundamentals
│   ├── Containers → Lightweight, isolated environments
│   ├── Images → Immutable templates, layers
│   ├── Dockerfile → Image build instructions
│   └── Registry → Image storage, distribution
├── 🏗️ Container Architecture
│   ├── Namespaces → Process isolation
│   ├── Cgroups → Resource management
│   ├── Union File System → Layered storage
│   └── Container Runtime → Container execution engine
├── 🔄 Container Lifecycle
│   ├── Build → Create images from Dockerfile
│   ├── Run → Start containers from images
│   ├── Stop → Gracefully stop containers
│   └── Remove → Clean up containers and images
├── 🚀 Container Orchestration
│   ├── Docker Compose → Multi-container applications
│   ├── Kubernetes → Production orchestration
│   ├── Docker Swarm → Docker-native orchestration
│   └── Service Discovery → Container communication
└── 🛡️ Security & Best Practices
    ├── Image Security → Vulnerability scanning
    ├── Runtime Security → Container isolation
    ├── Resource Limits → Memory, CPU constraints
    └── Multi-stage Builds → Optimized images
```

## 📋 Table of Contents
- [Docker Fundamentals](#docker-fundamentals)
- [Container Architecture](#container-architecture)
- [Container Lifecycle](#container-lifecycle)
- [Container Orchestration](#container-orchestration)
- [Security & Best Practices](#security--best-practices)
- [Common Interview Questions](#common-interview-questions)

## ❓ Common Interview Questions

### 1. Container Fundamentals
**Q: Container vs Virtual Machine - sự khác biệt?**
**A:** Containers share host OS kernel, lightweight, fast startup, less isolation. VMs có full OS, better isolation, slower startup, more resource usage. Containers tốt hơn cho microservices, VMs tốt hơn cho legacy apps.

### 2. Docker Architecture
**Q: Giải thích Docker architecture và cách hoạt động?**
**A:** Docker daemon (background service) quản lý containers, images, networks. Client giao tiếp với daemon qua API. Registry lưu trữ images. Containers chạy từ images với isolated namespaces và cgroups.

### 3. Dockerfile Optimization
**Q: Cách optimize Dockerfile và reduce image size?**
**A:** Use multi-stage builds, minimal base images (Alpine), combine RUN commands, cleanup package managers, use .dockerignore, optimize layer caching, remove unnecessary files.

### 4. Container Security
**Q: Security best practices cho Docker containers?**
**A:** Run as non-root user, scan images for vulnerabilities, use minimal base images, limit container capabilities, implement resource limits, secure network policies, regular updates.

### 5. Container Orchestration
**Q: Docker Compose vs Kubernetes - khi nào sử dụng?**
**A:** Docker Compose cho development, simple deployments, single host. Kubernetes cho production, complex orchestration, multi-host, scaling, service discovery, advanced networking. 