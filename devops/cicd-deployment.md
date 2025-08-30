# 🔄 CI/CD & Deployment - Complete Knowledge Map

## 🧠 Mindmap Overview
```
CI/CD & Deployment
├── 🔄 Continuous Integration
│   ├── Code Building → Compile, test, package
│   ├── Automated Testing → Unit, integration, e2e tests
│   ├── Code Quality → Linting, formatting, security scans
│   └── Early Feedback → Quick error detection
├── 🚀 Continuous Delivery
│   ├── Automated Deployment → Staging, production
│   ├── Environment Management → Dev, staging, prod
│   ├── Configuration Management → Environment variables
│   └── Rollback Strategies → Quick failure recovery
├── 🐳 Containerization
│   ├── Docker → Application packaging
│   ├── Image Management → Registry, versioning
│   ├── Multi-stage Builds → Optimized images
│   └── Security Scanning → Vulnerability detection
├── ☁️ Cloud Deployment
│   ├── Kubernetes → Container orchestration
│   ├── Serverless → Function-based deployment
│   ├── Infrastructure as Code → Terraform, CloudFormation
│   └── Auto-scaling → Dynamic resource management
└── 📊 Monitoring & Observability
    ├── Logging → Centralized log collection
    ├── Metrics → Performance monitoring
    ├── Tracing → Request flow tracking
    └── Alerting → Proactive issue detection
```

## 📋 Table of Contents
- [Continuous Integration](#continuous-integration)
- [Continuous Delivery](#continuous-delivery)
- [Containerization](#containerization)
- [Cloud Deployment](#cloud-deployment)
- [Monitoring & Observability](#monitoring--observability)
- [Common Interview Questions](#common-interview-questions)

## ❓ Common Interview Questions

### 1. CI/CD Pipeline
**Q: Giải thích CI/CD pipeline và các stages?**
**A:** CI (Continuous Integration): automated build, test, code quality. CD (Continuous Delivery/Deployment): automated deployment. Stages: build → test → analyze → deploy → monitor. Automated từ code commit đến production.

### 2. Deployment Strategies
**Q: Blue-Green vs Canary deployment - ưu nhược điểm?**
**A:** Blue-Green: zero-downtime, instant rollback, simple traffic switching. Canary: gradual rollout, risk mitigation, performance monitoring. Blue-Green tốt cho simple apps, Canary tốt cho complex systems.

### 3. CI/CD Tools
**Q: GitHub Actions vs Jenkins - khi nào sử dụng?**
**A:** GitHub Actions: integrated với GitHub, simple workflows, cloud-based. Jenkins: self-hosted, extensive plugins, complex pipelines, enterprise features. GitHub Actions cho simple projects, Jenkins cho complex enterprise needs.

### 4. Deployment Automation
**Q: Cách implement automated deployment?**
**A:** Use infrastructure as code (Terraform, CloudFormation), container orchestration (Kubernetes), automated testing, health checks, rollback mechanisms, monitoring và alerting, feature flags.

### 5. Monitoring & Observability
**Q: Cách monitor production deployments?**
**A:** Application performance monitoring (APM), centralized logging, metrics collection, distributed tracing, real-time alerting, health checks, user experience monitoring, business metrics tracking. 