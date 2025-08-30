# �� CI/CD & Deployment - Mindmap

## 🧠 Mindmap Structure
```
CI/CD & Deployment
├── 🏗️ CI/CD Pipeline
│   ├── Continuous Integration
│   │   ├── Automated testing → Run tests on every commit
│   │   ├── Code quality checks → Linting, formatting, static analysis
│   │   ├── Build automation → Automated build process
│   │   ├── Early bug detection → Catch issues early in development
│   │   └── Integration testing → Test component interactions
│   ├── Continuous Delivery
│   │   ├── Automated deployment preparation → Ready for production
│   │   ├── Staging environment testing → Production-like testing
│   │   ├── Deployment automation → Automated deployment process
│   │   ├── Manual approval gates → Human oversight when needed
│   │   └── Rollback capability → Quick rollback if needed
│   ├── Continuous Deployment
│   │   ├── Automated production deployment → Deploy to production
│   │   ├── Zero-downtime releases → Continuous availability
│   │   ├── Feature flags → Gradual feature rollout
│   │   ├── Canary deployments → Risk mitigation strategies
│   │   └── Blue-green deployments → Instant rollback capability
│   ├── Pipeline Stages
│   │   ├── Build → Compile, package, create artifacts
│   │   ├── Test → Unit, integration, E2E testing
│   │   ├── Analyze → Code quality, security scanning
│   │   ├── Deploy → Deploy to target environments
│   │   └── Monitor → Performance monitoring, health checks
│   └── Triggers
│       ├── Code commits → Automatic pipeline execution
│       ├── Pull requests → Pre-merge validation
│       ├── Scheduled builds → Regular automated builds
│       ├── Manual triggers → Human-initiated builds
│       └── Webhook triggers → External system integration
├── 🛠️ CI/CD Tools
│   ├── GitHub Actions
│   │   ├── GitHub-native CI/CD → Integrated with GitHub
│   │   ├── Workflow automation → YAML-based workflows
│   │   ├── Matrix builds → Multiple environment testing
│   │   ├── Marketplace integration → Extensive plugin ecosystem
│   │   └── Free tier → Generous free usage limits
│   ├── Jenkins
│   │   ├── Self-hosted automation → On-premises CI/CD
│   │   ├── Pipeline as code → Jenkinsfile, declarative pipelines
│   │   ├── Extensive plugins → Large plugin ecosystem
│   │   ├── Distributed builds → Multi-node build execution
│   │   └── Enterprise features → Advanced security, scalability
│   ├── GitLab CI
│   │   ├── Integrated CI/CD → Native GitLab integration
│   │   ├── Pipeline visualization → Visual pipeline representation
│   │   ├── Container support → Docker integration
│   │   ├── Auto DevOps → Automated pipeline generation
│   │   └── Security scanning → Built-in security features
│   ├── CircleCI
│   │   ├── Cloud-based CI/CD → Hosted CI/CD service
│   │   ├── Parallel execution → Fast build execution
│   │   ├── Docker support → Native container support
│   │   ├── Orbs → Reusable pipeline components
│   │   └── Performance optimization → Fast build times
│   └── Azure DevOps
│       ├── Microsoft ecosystem → Azure integration
│       ├── Comprehensive toolchain → Full development lifecycle
│       ├── Enterprise features → Advanced enterprise capabilities
│       ├── Azure integration → Native cloud integration
│       └── Team collaboration → Built-in collaboration tools
├── 🔨 Build & Test Automation
│   ├── Build Tools
│   │   ├── Webpack → JavaScript module bundler
│   │   ├── Vite → Fast build tool, modern development
│   │   ├── Gradle → Java build automation
│   │   ├── Maven → Java project management
│   │   ├── npm scripts → Package.json build scripts
│   │   └── Build optimization → Faster builds, smaller artifacts
│   ├── Package Managers
│   │   ├── npm → Node.js package management
│   │   ├── yarn → Fast, reliable package management
│   │   ├── pip → Python package management
│   │   ├── Maven → Java dependency management
│   │   ├── NuGet → .NET package management
│   │   └── Dependency management → Version control, security
│   ├── Testing Automation
│   │   ├── Unit tests → Automated unit testing
│   │   ├── Integration tests → Component integration testing
│   │   ├── E2E tests → End-to-end user workflow testing
│   │   ├── Test reporting → Test results, coverage reports
│   │   └── Test parallelization → Parallel test execution
│   ├── Code Quality
│   │   ├── Linting → Code style, best practices
│   │   ├── Formatting → Consistent code formatting
│   │   ├── Static analysis → Code quality analysis
│   │   ├── Code coverage → Test coverage metrics
│   │   └── Quality gates → Minimum quality requirements
│   └── Security Scanning
│       ├── Dependency scanning → Vulnerability detection
│       ├── Code scanning → Security issue detection
│       ├── Container scanning → Image security analysis
│       ├── Infrastructure scanning → IaC security analysis
│       └── Compliance checking → Security compliance validation
├── 🚀 Deployment Strategies
│   ├── Blue-Green Deployment
│   │   ├── Zero-downtime deployment → Continuous availability
│   │   ├── Instant rollback → Quick rollback capability
│   │   ├── Traffic switching → Instant traffic redirection
│   │   ├── Environment duplication → Identical environments
│   │   └── Risk mitigation → Safe deployment strategy
│   ├── Canary Deployment
│   │   ├── Gradual rollout → Incremental user exposure
│   │   ├── Risk mitigation → Limited risk exposure
│   │   ├── Performance monitoring → Real-time performance tracking
│   │   ├── User segmentation → Controlled user groups
│   │   └── Rollback capability → Quick rollback if issues
│   ├── Rolling Deployment
│   │   ├── Incremental updates → Gradual service updates
│   │   ├── Minimal downtime → Reduced service interruption
│   │   ├── Health checks → Service health validation
│   │   ├── Load balancing → Traffic distribution
│   │   └── Update verification → Deployment validation
│   ├── Feature Flags
│   │   ├── Feature toggles → Enable/disable features
│   │   ├── A/B testing → User experience testing
│   │   ├── Gradual rollout → Controlled feature release
│   │   ├── Risk management → Feature risk control
│   │   └── User segmentation → Targeted feature delivery
│   └── Database Migrations
│       ├── Schema changes → Database structure updates
│       ├── Data migration → Data transformation, migration
│       ├── Rollback strategies → Migration rollback plans
│       ├── Zero-downtime → Continuous database availability
│       └── Migration testing → Pre-production validation
├── ☁️ Cloud Deployment
│   ├── AWS
│   │   ├── EC2 → Virtual machine instances
│   │   ├── Lambda → Serverless functions
│   │   ├── ECS → Container orchestration
│   │   ├── CloudFormation → Infrastructure as code
│   │   └── Deployment automation → AWS-native deployment tools
│   ├── Azure
│   │   ├── App Service → Managed web application hosting
│   │   ├── Functions → Serverless compute
│   │   ├── Container Instances → Container hosting
│   │   ├── ARM templates → Infrastructure as code
│   │   └── DevOps integration → Native CI/CD integration
│   ├── GCP
│   │   ├── Compute Engine → Virtual machine instances
│   │   ├── Cloud Functions → Serverless functions
│   │   ├── Cloud Run → Container hosting
│   │   ├── Terraform → Infrastructure as code
│   │   └── Cloud-native tools → GCP-specific deployment tools
│   ├── Serverless
│   │   ├── Function-as-a-Service → Event-driven compute
│   │   ├── Auto-scaling → Automatic resource scaling
│   │   ├── Pay-per-use → Usage-based pricing
│   │   ├── Event-driven → Trigger-based execution
│   │   └── Managed infrastructure → No server management
│   └── Container Orchestration
│       ├── Kubernetes → Container orchestration platform
│       ├── Docker Swarm → Docker-native orchestration
│       ├── ECS → AWS container orchestration
│       ├── Managed services → Cloud-managed orchestration
│       └── Self-hosted → On-premises orchestration
└── 📊 Monitoring & Observability
    ├── Application Monitoring
    │   ├── Performance metrics → Response time, throughput
    │   ├── Error tracking → Error rates, error types
    │   ├── User experience → Real user monitoring
    │   ├── Business metrics → User engagement, conversion
    │   └── Custom metrics → Application-specific metrics
    ├── Logging
    │   ├── Centralized logging → Central log aggregation
    │   ├── Log aggregation → Collect logs from all services
    │   ├── Log analysis → Search, filter, analyze logs
    │   ├── Structured logging → JSON, structured log format
    │   └── Log retention → Log storage, archival policies
    ├── Metrics
    │   ├── System metrics → CPU, memory, disk, network
    │   ├── Business metrics → User actions, business KPIs
    │   ├── Custom dashboards → Visual metric representation
    │   ├── Alerting → Performance threshold notifications
    │   └── Trend analysis → Performance over time
    ├── Tracing
    │   ├── Distributed tracing → Request flow tracking
    │   ├── Request flow → End-to-end request tracing
    │   ├── Performance bottlenecks → Identify slow components
    │   ├── Service dependencies → Service interaction mapping
    │   └── Trace analysis → Performance optimization insights
    └── Alerting
        ├── Real-time notifications → Immediate issue alerts
        ├── Escalation policies → Alert escalation procedures
        ├── Incident response → Automated incident handling
        ├── Alert management → Alert deduplication, grouping
        └── Communication channels → Slack, email, SMS alerts
```

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