# ☁️ DevOps & Cloud - Complete Knowledge Map

## 🧠 Mindmap Overview
```
DevOps & Cloud
├── 🐳 Containerization
│   ├── Docker → Container engine, images, containers
│   ├── Docker Compose → Multi-container applications
│   ├── Container Registry → Docker Hub, ECR, GCR
│   └── Best Practices → Multi-stage builds, security
├── ☸️ Orchestration
│   ├── Kubernetes → Pods, services, deployments
│   ├── Helm → Package manager for K8s
│   ├── Service Mesh → Istio, Linkerd, traffic management
│   └── Monitoring → Prometheus, Grafana, logging
├── 🔄 CI/CD Pipelines
│   ├── Jenkins → Automation server, pipelines
│   ├── GitLab CI → GitLab integrated CI/CD
│   ├── GitHub Actions → GitHub integrated workflows
│   └── ArgoCD → GitOps, declarative deployments
├── ☁️ Cloud Platforms
│   ├── AWS → EC2, S3, Lambda, EKS
│   ├── Azure → VMs, Blob Storage, Functions, AKS
│   ├── GCP → Compute Engine, Cloud Storage, Cloud Functions, GKE
│   └── Multi-cloud → Hybrid cloud strategies
└── 🔧 Infrastructure as Code
    ├── Terraform → Infrastructure provisioning
    ├── Ansible → Configuration management
    ├── CloudFormation → AWS infrastructure
    └── Pulumi → Multi-language IaC
```

## 📋 Table of Contents
- [Containerization](#containerization)
- [Orchestration](#orchestration)
- [CI/CD Pipelines](#cicd-pipelines)
- [Cloud Platforms](#cloud-platforms)
- [Common Interview Questions](#common-interview-questions)

## 🐳 Containerization

### Docker Basics
```dockerfile
# Multi-stage Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```yaml
# Docker Compose
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    depends_on:
      - db
    networks:
      - app-network

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  postgres_data:
```

### Docker Best Practices
```dockerfile
# ✅ Good Dockerfile
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files first (for better caching)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production && npm cache clean --force

# Copy application code
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001
USER nodejs

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Expose port
EXPOSE 3000

# Start application
CMD ["npm", "start"]
```

## ☸️ Orchestration

### Kubernetes Basics
```yaml
# Pod Definition
apiVersion: v1
kind: Pod
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  containers:
  - name: web
    image: nginx:alpine
    ports:
    - containerPort: 80
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    livenessProbe:
      httpGet:
        path: /health
        port: 80
      initialDelaySeconds: 30
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /ready
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5

---
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web
        image: nginx:alpine
        ports:
        - containerPort: 80

---
# Service
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
spec:
  selector:
    app: web-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer

---
# Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-app-service
            port:
              number: 80
```

### Helm Charts
```yaml
# Chart.yaml
apiVersion: v2
name: myapp
description: A Helm chart for My Application
type: application
version: 0.1.0
appVersion: "1.0.0"

---
# values.yaml
replicaCount: 3

image:
  repository: nginx
  tag: "alpine"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi

---
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myapp.fullname" . }}
  labels:
    {{- include "myapp.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      {{- include "myapp.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      labels:
        {{- include "myapp.selectorLabels" . | nindent 8 }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

## 🔄 CI/CD Pipelines

### Jenkins Pipeline
```groovy
// Jenkinsfile
pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'myapp'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    publishTestResults testResultsPattern: 'test-results/*.xml'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image ${DOCKER_IMAGE}:${DOCKER_TAG}'
            }
        }
        
        stage('Push to Registry') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.example.com', 'registry-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push('latest')
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            when {
                branch 'main'
            }
            steps {
                script {
                    sh "kubectl set image deployment/myapp myapp=${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "kubectl rollout status deployment/myapp"
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

### GitLab CI
```yaml
# .gitlab-ci.yml
stages:
  - test
  - build
  - security
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: "/certs"

test:
  stage: test
  image: node:18-alpine
  script:
    - npm install
    - npm run test
    - npm run lint
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  only:
    - main
    - develop

security-scan:
  stage: security
  image: aquasec/trivy:latest
  script:
    - trivy image $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  allow_failure: true
  only:
    - main

deploy-staging:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl config use-context staging
    - kubectl set image deployment/myapp myapp=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/myapp
  environment:
    name: staging
    url: https://staging.myapp.com
  only:
    - develop

deploy-production:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl config use-context production
    - kubectl set image deployment/myapp myapp=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - kubectl rollout status deployment/myapp
  environment:
    name: production
    url: https://myapp.com
  when: manual
  only:
    - main
```

### GitHub Actions
```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Run linting
      run: npm run lint

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || github.ref == 'refs/heads/develop'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    
    - name: Log in to Container Registry
      uses: docker/login-action@v2
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    
    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v4
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/develop'
    environment: staging
    
    steps:
    - name: Deploy to staging
      run: |
        echo "Deploying to staging environment"
        # Add your deployment commands here

  deploy-production:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
    - name: Deploy to production
      run: |
        echo "Deploying to production environment"
        # Add your deployment commands here
```

## ☁️ Cloud Platforms

### AWS Services
```yaml
# AWS ECS Task Definition
{
  "family": "web-app",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "containerDefinitions": [
    {
      "name": "web-app",
      "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/web-app:latest",
      "portMappings": [
        {
          "containerPort": 3000,
          "protocol": "tcp"
        }
      ],
      "environment": [
        {
          "name": "NODE_ENV",
          "value": "production"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/web-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}

# AWS Lambda Function
import json
import boto3

def lambda_handler(event, context):
    # Process the event
    print("Event:", json.dumps(event))
    
    # Your business logic here
    response = {
        "statusCode": 200,
        "body": json.dumps({
            "message": "Hello from Lambda!",
            "event": event
        })
    }
    
    return response

# AWS CloudFormation Template
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Web Application Stack'

Parameters:
  Environment:
    Type: String
    Default: production
    AllowedValues: [development, staging, production]

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-vpc'

  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select [0, !GetAZs '']
      MapPublicIpOnLaunch: true

  ECSCluster:
    Type: AWS::ECS::Cluster
    Properties:
      ClusterName: !Sub '${Environment}-cluster'
      CapacityProviders:
        - FARGATE
      DefaultCapacityProviderStrategy:
        - CapacityProvider: FARGATE
          Weight: 1
```

### Azure Services
```yaml
# Azure Container Instances
apiVersion: 2019-12-01
location: eastus
properties:
  containers:
  - name: web-app
    properties:
      image: myregistry.azurecr.io/web-app:latest
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      ports:
      - port: 80
        protocol: TCP
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
    - protocol: TCP
      port: 80

# Azure Functions
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Threading.Tasks;

public static class HttpTrigger
{
    [FunctionName("HttpTrigger")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        string name = req.Query["name"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        name = name ?? data?.name;

        string responseMessage = string.IsNullOrEmpty(name)
            ? "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response."
            : $"Hello, {name}. This HTTP triggered function executed successfully.";

        return new OkObjectResult(responseMessage);
    }
}
```

### GCP Services
```yaml
# Google Cloud Run
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: web-app
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/minScale: "1"
        autoscaling.knative.dev/maxScale: "10"
    spec:
      containerConcurrency: 80
      timeoutSeconds: 300
      containers:
      - image: gcr.io/PROJECT_ID/web-app:latest
        ports:
        - containerPort: 8080
        resources:
          limits:
            cpu: "1000m"
            memory: "512Mi"
        env:
        - name: NODE_ENV
          value: "production"

# Google Cloud Functions
exports.helloWorld = (req, res) => {
  let message = req.query.message || req.body.message || 'Hello World!';
  
  res.status(200).send(message);
};

# Google Kubernetes Engine (GKE)
apiVersion: v1
kind: ServiceAccount
metadata:
  name: web-app-sa
  namespace: default
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      serviceAccountName: web-app-sa
      containers:
      - name: web-app
        image: gcr.io/PROJECT_ID/web-app:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```

## 🔧 Infrastructure as Code

### Terraform
```hcl
# main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

# VPC
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "${var.environment}-vpc"
  }
}

# Subnets
resource "aws_subnet" "public" {
  count             = length(var.public_subnets)
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.public_subnets[count.index]
  availability_zone = var.availability_zones[count.index]

  tags = {
    Name = "${var.environment}-public-subnet-${count.index + 1}"
  }
}

resource "aws_subnet" "private" {
  count             = length(var.private_subnets)
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.private_subnets[count.index]
  availability_zone = var.availability_zones[count.index]

  tags = {
    Name = "${var.environment}-private-subnet-${count.index + 1}"
  }
}

# ECS Cluster
resource "aws_ecs_cluster" "main" {
  name = "${var.environment}-cluster"

  setting {
    name  = "containerInsights"
    value = "enabled"
  }

  tags = {
    Name = "${var.environment}-cluster"
  }
}

# ECS Task Definition
resource "aws_ecs_task_definition" "app" {
  family                   = "${var.environment}-app"
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = var.app_cpu
  memory                   = var.app_memory
  execution_role_arn       = aws_iam_role.ecs_task_execution_role.arn

  container_definitions = jsonencode([
    {
      name  = "app"
      image = var.app_image

      portMappings = [
        {
          containerPort = var.app_port
          protocol      = "tcp"
        }
      ]

      environment = [
        {
          name  = "NODE_ENV"
          value = var.environment
        }
      ]

      logConfiguration = {
        logDriver = "awslogs"
        options = {
          awslogs-group         = aws_cloudwatch_log_group.app.name
          awslogs-region        = var.aws_region
          awslogs-stream-prefix = "ecs"
        }
      }
    }
  ])

  tags = {
    Name = "${var.environment}-app-task"
  }
}

# variables.tf
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "environment" {
  description = "Environment name"
  type        = string
  default     = "production"
}

variable "vpc_cidr" {
  description = "CIDR block for VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "public_subnets" {
  description = "Public subnet CIDR blocks"
  type        = list(string)
  default     = ["10.0.1.0/24", "10.0.2.0/24"]
}

variable "private_subnets" {
  description = "Private subnet CIDR blocks"
  type        = list(string)
  default     = ["10.0.3.0/24", "10.0.4.0/24"]
}

variable "availability_zones" {
  description = "Availability zones"
  type        = list(string)
  default     = ["us-east-1a", "us-east-1b"]
}

variable "app_cpu" {
  description = "App CPU units"
  type        = number
  default     = 256
}

variable "app_memory" {
  description = "App memory"
  type        = number
  default     = 512
}

variable "app_port" {
  description = "App port"
  type        = number
  default     = 3000
}

variable "app_image" {
  description = "App Docker image"
  type        = string
}
```

### Ansible
```yaml
# playbook.yml
---
- name: Deploy web application
  hosts: web_servers
  become: yes
  vars:
    app_name: web-app
    app_port: 3000
    app_user: nodejs

  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Install Node.js
      apt:
        name: nodejs
        state: present

    - name: Install npm
      apt:
        name: npm
        state: present

    - name: Create application user
      user:
        name: "{{ app_user }}"
        system: yes
        shell: /bin/bash

    - name: Create application directory
      file:
        path: /opt/{{ app_name }}
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
        mode: '0755'

    - name: Copy application files
      copy:
        src: "{{ item }}"
        dest: /opt/{{ app_name }}/
        owner: "{{ app_user }}"
        group: "{{ app_user }}"
      with_fileglob:
        - "*.js"
        - "package*.json"

    - name: Install application dependencies
      npm:
        path: /opt/{{ app_name }}
        state: present

    - name: Create systemd service file
      template:
        src: web-app.service.j2
        dest: /etc/systemd/system/{{ app_name }}.service
        owner: root
        group: root
        mode: '0644'
      notify: restart web-app

    - name: Start and enable web-app service
      systemd:
        name: "{{ app_name }}"
        state: started
        enabled: yes
        daemon_reload: yes

  handlers:
    - name: restart web-app
      systemd:
        name: "{{ app_name }}"
        state: restarted

# web-app.service.j2
[Unit]
Description=Web Application
After=network.target

[Service]
Type=simple
User={{ app_user }}
WorkingDirectory=/opt/{{ app_name }}
ExecStart=/usr/bin/node app.js
Restart=on-failure
Environment=NODE_ENV=production
Environment=PORT={{ app_port }}

[Install]
WantedBy=multi-user.target
```

## ❓ Common Interview Questions

### 1. Docker vs Virtual Machines
**Q: Sự khác biệt giữa Docker containers và Virtual Machines?**

**A:**
```javascript
// Virtual Machines
const virtualMachine = {
    architecture: 'Hardware -> Hypervisor -> Guest OS -> Application',
    size: 'GBs to TBs',
    startup: 'Minutes',
    isolation: 'Complete OS isolation',
    resource: 'Heavy resource usage'
};

// Docker Containers
const dockerContainer = {
    architecture: 'Hardware -> Host OS -> Docker Engine -> Application',
    size: 'MBs to GBs',
    startup: 'Seconds',
    isolation: 'Process-level isolation',
    resource: 'Lightweight resource usage'
};

// Use Cases
const useCases = {
    virtualMachines: [
        'Legacy applications',
        'Different OS requirements',
        'Complete isolation needed'
    ],
    dockerContainers: [
        'Microservices',
        'DevOps and CI/CD',
        'Cloud-native applications'
    ]
};
```

### 2. Kubernetes Architecture
**Q: Giải thích kiến trúc Kubernetes?**

**A:**
```yaml
# Kubernetes Components
kubernetes_architecture:
  # Control Plane (Master Node)
  control_plane:
    api_server: 'REST API for cluster management'
    etcd: 'Distributed key-value store for cluster data'
    scheduler: 'Assigns pods to nodes'
    controller_manager: 'Manages cluster state'
  
  # Worker Nodes
  worker_nodes:
    kubelet: 'Agent running on each node'
    kube_proxy: 'Network proxy for services'
    container_runtime: 'Docker, containerd, etc.'
  
  # Add-ons
  add_ons:
    dns: 'CoreDNS for service discovery'
    dashboard: 'Web UI for cluster management'
    monitoring: 'Prometheus, Grafana'

# Pod Lifecycle
pod_lifecycle:
  - pending: 'Pod accepted by scheduler'
  - running: 'Pod bound to node, containers started'
  - succeeded: 'All containers terminated successfully'
  - failed: 'At least one container terminated with failure'
```

### 3. CI/CD Best Practices
**Q: Các best practices cho CI/CD pipeline?**

**A:**
```yaml
ci_cd_best_practices:
  # Security
  security:
    - 'Scan dependencies for vulnerabilities'
    - 'Use secrets management for sensitive data'
    - 'Implement least privilege access'
  
  # Testing
  testing:
    - 'Unit tests for all code changes'
    - 'Integration tests for API endpoints'
    - 'End-to-end tests for critical flows'
    - 'Performance testing for load handling'
  
  # Deployment
  deployment:
    - 'Blue-green or canary deployments'
    - 'Rollback strategies'
    - 'Health checks and monitoring'
    - 'Gradual rollout with traffic shifting'
  
  # Monitoring
  monitoring:
    - 'Application metrics and logs'
    - 'Infrastructure monitoring'
    - 'Alerting for critical issues'
```

### 4. Cloud Migration Strategies
**Q: Các strategy migrate lên cloud?**

**A:**
```javascript
const cloudMigrationStrategies = {
    // Rehost (Lift and Shift)
    rehost: {
        approach: 'Move applications without changes',
        pros: 'Fast migration, minimal risk',
        cons: 'No cloud benefits, higher costs'
    },
    
    // Refactor (Lift, Tinker and Shift)
    refactor: {
        approach: 'Optimize applications for cloud',
        pros: 'Better performance, cost optimization',
        cons: 'More complex, longer timeline'
    },
    
    // Rearchitect
    rearchitect: {
        approach: 'Redesign for cloud-native',
        pros: 'Maximum cloud benefits, scalability',
        cons: 'Most complex, highest risk'
    },
    
    // Rebuild
    rebuild: {
        approach: 'Rewrite applications from scratch',
        pros: 'Modern architecture, best practices',
        cons: 'Highest cost, longest timeline'
    }
};
```

### 5. Infrastructure as Code
**Q: Lợi ích của Infrastructure as Code?**

**A:**
```yaml
infrastructure_as_code_benefits:
  # Version Control
  version_control:
    - 'Track infrastructure changes'
    - 'Rollback to previous states'
    - 'Code review for infrastructure'
    - 'Audit trail for compliance'
  
  # Automation
  automation:
    - 'Consistent deployments'
    - 'Reduced human error'
    - 'Faster provisioning'
    - 'Repeatable processes'
  
  # Collaboration
  collaboration:
    - 'Team can review and contribute'
    - 'Standardized infrastructure'
    - 'Knowledge sharing'
    - 'Documentation as code'
  
  # Testing
  testing:
    - 'Validate infrastructure before deployment'
    - 'Automated testing of configurations'
    - 'Compliance checking'
    - 'Security scanning'
  
  # Scalability
  scalability:
    - 'Easy to replicate environments'
    - 'Consistent across regions'
    - 'Rapid scaling up/down'
    - 'Multi-cloud support'
```

## 🚀 Real-world Examples

### Microservices Deployment
```yaml
# Complete microservices deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: user-service
  template:
    metadata:
      labels:
        app: user-service
    spec:
      containers:
      - name: user-service
        image: user-service:latest
        ports:
        - containerPort: 3001
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3001
          initialDelaySeconds: 30
          periodSeconds: 10
---
apiVersion: v1
kind: Service
metadata:
  name: user-service
spec:
  selector:
    app: user-service
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3001
  type: ClusterIP
```

## 📚 Resources
- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [AWS Documentation](https://docs.aws.amazon.com/)
- [Azure Documentation](https://docs.microsoft.com/azure/)
- [Google Cloud Documentation](https://cloud.google.com/docs/)
- [Terraform Documentation](https://www.terraform.io/docs/)
- [Ansible Documentation](https://docs.ansible.com/) 