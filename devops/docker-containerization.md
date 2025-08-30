# 🐳 Docker & Containerization - Mindmap

## 🧠 Mindmap Structure
```
Docker & Containerization
├── 🏗️ Container Fundamentals
│   ├── Containerization
│   │   ├── Process isolation → Isolated processes, namespaces
│   │   ├── Resource management → CPU, memory, disk limits
│   │   ├── Lightweight virtualization → OS-level virtualization
│   │   ├── Portability → Run anywhere Docker is available
│   │   └── Efficiency → Shared kernel, minimal overhead
│   ├── Docker Engine
│   │   ├── Runtime environment → Container execution engine
│   │   ├── Daemon process → Background service, container management
│   │   ├── Client-server architecture → CLI client, daemon communication
│   │   ├── API interface → RESTful API for container operations
│   │   └── Plugin system → Extensible functionality
│   ├── Images vs Containers
│   │   ├── Images → Immutable templates, read-only layers
│   │   ├── Containers → Running instances from images
│   │   ├── Layer-based storage → Union file system, copy-on-write
│   │   ├── Image layers → Base image, application layers
│   │   └── Container lifecycle → Create, start, stop, remove
│   ├── Namespaces
│   │   ├── Process isolation → PID namespace, process isolation
│   │   ├── Network isolation → Network namespace, isolated networking
│   │   ├── Mount isolation → Mount namespace, file system isolation
│   │   ├── User isolation → User namespace, user ID mapping
│   │   └── UTS isolation → Hostname isolation
│   └── Cgroups
│       ├── Resource limits → CPU, memory, disk I/O limits
│       ├── Network bandwidth → Network traffic control
│       ├── Process groups → Group resource allocation
│       ├── Resource monitoring → Usage tracking, statistics
│       └── Resource enforcement → Hard limits, throttling
├── 🏛️ Docker Architecture
│   ├── Daemon
│   │   ├── Background service → Container management service
│   │   ├── Container management → Create, start, stop containers
│   │   ├── Image building → Dockerfile processing, layer creation
│   │   ├── Resource management → Memory, CPU allocation
│   │   └── API server → HTTP API for client communication
│   ├── Client
│   │   ├── Command line interface → Docker CLI commands
│   │   ├── API communication → HTTP requests to daemon
│   │   ├── User interactions → Command execution, feedback
│   │   ├── Configuration → Client settings, preferences
│   │   └── Scripting → Automation, CI/CD integration
│   ├── Registry
│   │   ├── Image storage → Centralized image repository
│   │   ├── Docker Hub → Public registry, official images
│   │   ├── Private registries → Enterprise, security requirements
│   │   ├── Image distribution → Pull, push, tag operations
│   │   └── Access control → Authentication, authorization
│   ├── Layers
│   │   ├── Union file system → Layered file system
│   │   ├── Copy-on-write → Efficient storage, layer sharing
│   │   ├── Image optimization → Minimal layer count
│   │   ├── Storage efficiency → Shared layers between images
│   │   └── Layer caching → Build optimization, faster builds
│   └── Build Context
│       ├── Dockerfile location → Build context definition
│       ├── Build context size → Minimize context size
│       ├── .dockerignore → Exclude unnecessary files
│       ├── Context optimization → Efficient build process
│       └── Build performance → Faster builds, smaller images
├── 📝 Dockerfile Best Practices
│   ├── Multi-stage Builds
│   │   ├── Separate build stages → Build vs runtime separation
│   │   ├── Reduce image size → Final image optimization
│   │   ├── Security improvement → Remove build tools from final image
│   │   ├── Production-ready images → Minimal runtime images
│   │   └── Build optimization → Efficient build process
│   ├── Layer Caching
│   │   ├── Optimize build time → Leverage Docker layer caching
│   │   ├── Minimize layer changes → Stable base layers
│   │   ├── Efficient caching → Maximize cache hits
│   │   ├── Build order → Dependencies first, application last
│   │   └── Cache invalidation → Understand cache behavior
│   ├── Security
│   │   ├── Non-root users → Security best practices
│   │   ├── Minimal base images → Reduce attack surface
│   │   ├── Vulnerability scanning → Regular security audits
│   │   ├── Image signing → Digital signature verification
│   │   └── Security policies → Security requirements enforcement
│   ├── Dependencies
│   │   ├── Install only necessary packages → Minimal package installation
│   │   ├── Cleanup package managers → Remove package caches
│   │   ├── Version pinning → Specific package versions
│   │   ├── Dependency optimization → Minimal dependency tree
│   │   └── Security updates → Regular security patches
│   └── Environment
│       ├── Environment variables → Configuration management
│       ├── Configuration management → Runtime configuration
│       ├── Secrets handling → Secure secret management
│       ├── Working directory → Application directory setup
│       └── User permissions → Proper file permissions
├── 🚀 Container Orchestration
│   ├── Docker Compose
│   │   ├── Multi-container applications → Service definition
│   │   ├── Service definition → YAML configuration
│   │   ├── Networking → Inter-service communication
│   │   ├── Volume management → Persistent data storage
│   │   └── Development environment → Local development setup
│   ├── Kubernetes
│   │   ├── Container orchestration → Production container management
│   │   ├── Scaling → Horizontal, vertical scaling
│   │   ├── Service discovery → Load balancing, DNS resolution
│   │   ├── Load balancing → Traffic distribution
│   │   ├── Health checks → Service health monitoring
│   │   └── Rolling updates → Zero-downtime deployments
│   ├── Service Discovery
│   │   ├── Container communication → Inter-service networking
│   │   ├── DNS resolution → Service name resolution
│   │   ├── Service mesh → Advanced networking features
│   │   ├── Load balancing → Traffic distribution
│   │   └── Health monitoring → Service availability
│   ├── Networking
│   │   ├── Bridge networks → Default network mode
│   │   ├── Overlay networks → Multi-host networking
│   │   ├── Port mapping → Host port to container port
│   │   ├── Network policies → Security, traffic control
│   │   └── Service mesh → Advanced networking features
│   └── Storage
│       ├── Volumes → Persistent data storage
│       ├── Bind mounts → Host directory mounting
│       ├── Persistent storage → Data persistence across containers
│       ├── Storage drivers → Different storage backends
│       └── Data management → Backup, restore, migration
├── 📊 Container Performance
│   ├── Image Optimization
│   │   ├── Minimal base images → Alpine Linux, distroless
│   │   ├── Layer reduction → Combine RUN commands
│   │   ├── Multi-stage builds → Separate build and runtime
│   │   ├── Size optimization → Reduce final image size
│   │   └── Build optimization → Faster build times
│   ├── Resource Limits
│   │   ├── CPU limits → CPU allocation, throttling
│   │   ├── Memory limits → Memory allocation, OOM handling
│   │   ├── Disk I/O limits → Storage performance control
│   │   ├── Network limits → Bandwidth control
│   │   └── Resource monitoring → Usage tracking, alerts
│   ├── Security Scanning
│   │   ├── Vulnerability assessment → Security vulnerability scanning
│   │   ├── Image scanning → Container image security
│   │   ├── Security policies → Security requirement enforcement
│   │   ├── Compliance checking → Security compliance validation
│   │   └── Regular audits → Continuous security monitoring
│   ├── Registry Management
│   │   ├── Image tagging → Version management, semantic versioning
│   │   ├── Versioning → Image version control
│   │   ├── Cleanup policies → Old image removal
│   │   ├── Access control → Authentication, authorization
│   │   └── Storage optimization → Efficient storage usage
│   └── CI/CD Integration
│       ├── Automated builds → Build automation
│       ├── Testing → Automated testing in containers
│       ├── Deployment → Automated deployment
│       ├── Image promotion → Staging to production
│       └── Rollback → Quick rollback capabilities
└── 🛠️ Docker Tools & Ecosystem
    ├── CLI Tools
    │   ├── Docker commands → Container management commands
    │   ├── docker-compose → Multi-container orchestration
    │   ├── docker-machine → Machine management
    │   ├── docker-swarm → Swarm mode orchestration
    │   └── Command automation → Scripting, automation
    ├── Desktop Applications
    │   ├── Docker Desktop → GUI management interface
    │   ├── Container monitoring → Real-time container status
    │   ├── Image management → Image browsing, management
    │   ├── Volume management → Data volume management
    │   └── Network management → Network configuration
    ├── Registry Tools
    │   ├── Harbor → Enterprise registry solution
    │   ├── Nexus → Artifact repository management
    │   ├── AWS ECR → Amazon container registry
    │   ├── Google Container Registry → GCP container registry
    │   └── Registry management → Image management, security
    ├── Monitoring
    │   ├── Container metrics → Performance monitoring
│   ├── Resource usage → CPU, memory, disk monitoring
│   ├── Performance monitoring → Response time, throughput
│   ├── Health monitoring → Container health status
│   └── Alerting → Performance threshold alerts
    └── Development Tools
        ├── Development environments → Local development setup
        ├── Debugging → Container debugging tools
        ├── Hot reloading → Development workflow optimization
        ├── IDE integration → VS Code, IntelliJ integration
        └── Development workflows → Optimized development process
```

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