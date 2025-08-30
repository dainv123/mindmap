# 🏗️ Microservices Architecture - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Microservices Architecture
├── 🎯 Core Concepts
│   ├── Service Independence → Mỗi service độc lập, tự quản lý
│   ├── Single Responsibility → Mỗi service làm 1 việc cụ thể
│   ├── Loose Coupling → Services ít phụ thuộc lẫn nhau
│   └── Technology Diversity → Mỗi service có thể dùng tech khác nhau
├── 🏗️ Architecture Components
│   ├── API Gateway → Entry point, routing, authentication
│   ├── Service Discovery → Tìm và kết nối services
│   ├── Load Balancer → Phân tải giữa các instances
│   └── Circuit Breaker → Xử lý lỗi, fallback mechanisms
├── 🔄 Communication Patterns
│   ├── Synchronous → HTTP/REST, gRPC, direct calls
│   ├── Asynchronous → Message queues, event-driven
│   ├── Service Mesh → Istio, Linkerd, traffic management
│   └── Event Sourcing → Event store, CQRS pattern
├── 📊 Data Management
│   ├── Database per Service → Mỗi service có DB riêng
│   ├── Saga Pattern → Distributed transactions
│   ├── Eventual Consistency → BASE model thay vì ACID
│   └── Data Replication → Cross-service data sync
└── 🚀 Deployment & Operations
    ├── Containerization → Docker, container orchestration
    ├── CI/CD Pipelines → Automated deployment
    ├── Monitoring → Distributed tracing, logging
    └── Scaling → Horizontal scaling, auto-scaling
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Architecture Components](#architecture-components)
- [Communication Patterns](#communication-patterns)
- [Data Management](#data-management)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Core Concepts

### What are Microservices?
**Microservices** là kiến trúc phần mềm chia ứng dụng thành các **services nhỏ, độc lập** thay vì một ứng dụng lớn (monolith).

### Key Principles
```javascript
// 1. Service Independence
const userService = {
    technology: 'Node.js',
    database: 'PostgreSQL',
    deployment: 'Independent',
    scaling: 'Auto-scaling based on load'
};

// 2. Single Responsibility
const services = {
    userService: 'Quản lý user accounts, authentication',
    orderService: 'Xử lý orders, payments',
    inventoryService: 'Quản lý inventory, stock',
    notificationService: 'Gửi emails, SMS, push notifications'
};

// 3. Monolith vs Microservices
const comparison = {
    monolith: {
        structure: 'Single large application',
        deployment: 'Deploy entire app together',
        scaling: 'Scale entire app',
        failure: 'Single point of failure'
    },
    microservices: {
        structure: 'Multiple small services',
        deployment: 'Deploy services independently',
        scaling: 'Scale individual services',
        failure: 'Isolated failures'
    }
};
```

## 🏗️ Architecture Components

### 1. API Gateway
```javascript
// API Gateway Pattern
class APIGateway {
    async handleRequest(req, res) {
        const { path, method, headers, body } = req;
        
        // 1. Authentication & Authorization
        const token = headers.authorization;
        if (!this.authenticate(token)) {
            return res.status(401).json({ error: 'Unauthorized' });
        }
        
        // 2. Rate Limiting
        if (!this.checkRateLimit(req.ip)) {
            return res.status(429).json({ error: 'Rate limit exceeded' });
        }
        
        // 3. Service Discovery
        const serviceUrl = this.discoverService(path);
        
        // 4. Load Balancing
        const targetUrl = this.loadBalance(serviceUrl);
        
        // 5. Forward Request
        const response = await this.forwardRequest(targetUrl, {
            method, headers, body
        });
        
        res.json(response);
    }
}
```

### 2. Service Discovery
```javascript
// Service Registry Pattern
class ServiceRegistry {
    constructor() {
        this.services = new Map();
    }
    
    register(serviceName, serviceInstance) {
        const service = {
            id: serviceInstance.id,
            name: serviceName,
            host: serviceInstance.host,
            port: serviceInstance.port,
            health: serviceInstance.health,
            lastHeartbeat: Date.now()
        };
        
        if (!this.services.has(serviceName)) {
            this.services.set(serviceName, []);
        }
        
        this.services.get(serviceName).push(service);
    }
    
    getService(serviceName) {
        const instances = this.services.get(serviceName) || [];
        const healthyInstances = instances.filter(service => 
            Date.now() - service.lastHeartbeat < 30000
        );
        
        if (healthyInstances.length === 0) {
            throw new Error(`No healthy instances for service: ${serviceName}`);
        }
        
        // Round-robin load balancing
        const index = Math.floor(Math.random() * healthyInstances.length);
        return healthyInstances[index];
    }
}
```

### 3. Circuit Breaker Pattern
```javascript
// Circuit Breaker Implementation
class CircuitBreaker {
    constructor(failureThreshold = 5, timeout = 60000) {
        this.failureThreshold = failureThreshold;
        this.timeout = timeout;
        this.failures = 0;
        this.lastFailureTime = null;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    }
    
    async execute(operation) {
        if (this.state === 'OPEN') {
            if (this.shouldAttemptReset()) {
                this.state = 'HALF_OPEN';
            } else {
                throw new Error('Circuit breaker is OPEN');
            }
        }
        
        try {
            const result = await operation();
            this.onSuccess();
            return result;
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }
    
    onSuccess() {
        this.failures = 0;
        this.state = 'CLOSED';
    }
    
    onFailure() {
        this.failures++;
        this.lastFailureTime = Date.now();
        
        if (this.failures >= this.failureThreshold) {
            this.state = 'OPEN';
        }
    }
}
```

## 🔄 Communication Patterns

### 1. Synchronous Communication
```javascript
// HTTP/REST Communication
class UserService {
    async getUser(userId) {
        const response = await fetch(`http://user-service:3001/api/users/${userId}`);
        if (!response.ok) {
            throw new Error('Failed to fetch user');
        }
        return response.json();
    }
}

// gRPC Communication
const grpc = require('@grpc/grpc-js');

class UserServiceClient {
    constructor() {
        this.client = new userProto.UserService(
            'user-service:50051',
            grpc.credentials.createInsecure()
        );
    }
    
    getUser(userId) {
        return new Promise((resolve, reject) => {
            this.client.getUser({ id: userId }, (error, response) => {
                if (error) {
                    reject(error);
                } else {
                    resolve(response);
                }
            });
        });
    }
}
```

### 2. Asynchronous Communication
```javascript
// Message Queue Pattern (RabbitMQ)
class MessagePublisher {
    async publishOrderCreated(order) {
        await this.channel.assertQueue('order.created');
        this.channel.sendToQueue('order.created', Buffer.from(JSON.stringify(order)));
    }
}

class MessageConsumer {
    async consumeOrderCreated() {
        await this.channel.assertQueue('order.created');
        
        this.channel.consume('order.created', async (msg) => {
            const order = JSON.parse(msg.content.toString());
            
            // Process order created event
            await this.sendOrderConfirmation(order);
            await this.updateInventory(order);
            
            this.channel.ack(msg);
        });
    }
}

// Event-Driven Architecture
class OrderService {
    async createOrder(orderData) {
        // Create order
        const order = await this.saveOrder(orderData);
        
        // Publish events
        await this.eventBus.publish('OrderCreated', {
            orderId: order.id,
            userId: order.userId,
            amount: order.amount
        });
        
        return order;
    }
}
```

## 📊 Data Management

### 1. Database per Service
```javascript
// Each service has its own database
const serviceDatabases = {
    userService: {
        database: 'user_db',
        type: 'PostgreSQL',
        schema: {
            users: 'User accounts and profiles',
            user_preferences: 'User settings and preferences'
        }
    },
    
    orderService: {
        database: 'order_db', 
        type: 'MongoDB',
        collections: {
            orders: 'Order information',
            order_items: 'Order line items'
        }
    }
};

// Service-specific data access
class UserRepository {
    constructor() {
        this.db = new PostgreSQL('user_db');
    }
    
    async findById(userId) {
        return this.db.query(
            'SELECT * FROM users WHERE id = $1',
            [userId]
        );
    }
}
```

### 2. Saga Pattern for Distributed Transactions
```javascript
// Saga Pattern Implementation
class OrderSaga {
    constructor() {
        this.steps = [
            this.validateUser,
            this.reserveInventory,
            this.processPayment,
            this.createOrder,
            this.sendNotification
        ];
    }
    
    async execute(orderData) {
        const context = { orderData, steps: [] };
        
        try {
            for (const step of this.steps) {
                const result = await step.call(this, context);
                context.steps.push({ step: step.name, result, status: 'completed' });
            }
            
            return { success: true, orderId: context.orderId };
            
        } catch (error) {
            await this.compensate(context);
            throw error;
        }
    }
    
    async validateUser(context) {
        const user = await userService.getUser(context.orderData.userId);
        if (!user) {
            throw new Error('User not found');
        }
        context.user = user;
    }
    
    async compensate(context) {
        // Execute compensations in reverse order
        for (let i = context.steps.length - 1; i >= 0; i--) {
            const step = context.steps[i];
            if (step.status === 'completed') {
                await this.executeCompensation(step.step, context);
            }
        }
    }
}
```

### 3. Event Sourcing
```javascript
// Event Store
class EventStore {
    constructor() {
        this.events = [];
    }
    
    async append(streamId, event) {
        const eventRecord = {
            id: crypto.randomUUID(),
            streamId,
            type: event.type,
            data: event.data,
            timestamp: new Date(),
            version: await this.getNextVersion(streamId)
        };
        
        this.events.push(eventRecord);
        return eventRecord;
    }
    
    async getEvents(streamId) {
        return this.events.filter(event => event.streamId === streamId);
    }
}

// Aggregate with Event Sourcing
class OrderAggregate {
    async createOrder(orderData) {
        const event = {
            type: 'OrderCreated',
            data: {
                userId: orderData.userId,
                items: orderData.items,
                total: orderData.total
            }
        };
        
        await this.eventStore.append(orderData.id, event);
        this.apply(event);
    }
    
    apply(event) {
        switch (event.type) {
            case 'OrderCreated':
                this.state.id = event.data.id;
                this.state.userId = event.data.userId;
                this.state.items = event.data.items;
                this.state.total = event.data.total;
                break;
        }
    }
}
```

## ❓ Common Interview Questions

### 1. When to Use Microservices?
**Q: Khi nào nên sử dụng Microservices thay vì Monolith?**

**A:**
```javascript
// ✅ Use Microservices when:
const microservicesUseCases = {
    teamSize: 'Large development teams (>10 developers)',
    scalability: 'Need to scale different parts independently',
    technology: 'Want to use different technologies for different services',
    deployment: 'Need independent deployment cycles',
    complexity: 'System is complex with clear service boundaries'
};

// ❌ Stick with Monolith when:
const monolithUseCases = {
    teamSize: 'Small team (<5 developers)',
    complexity: 'Simple application with few features',
    timeToMarket: 'Need to ship quickly (MVP)',
    resources: 'Limited infrastructure and operational resources'
};
```

### 2. Data Consistency in Microservices
**Q: Làm sao đảm bảo data consistency giữa các services?**

**A:**
```javascript
// 1. Saga Pattern for distributed transactions
const sagaExample = {
    orderCreation: [
        'Reserve inventory',
        'Process payment', 
        'Create order',
        'Send notification'
    ],
    compensation: [
        'Release inventory',
        'Refund payment',
        'Cancel order'
    ]
};

// 2. Eventual Consistency
const eventualConsistency = {
    approach: 'Accept temporary inconsistency',
    sync: 'Synchronize data asynchronously',
    tolerance: 'Design for eventual consistency'
};

// 3. CQRS Pattern
const cqrsPattern = {
    command: 'Write operations (create, update, delete)',
    query: 'Read operations (get, list, search)',
    separation: 'Separate read and write models'
};
```

### 3. Service Communication
**Q: Các pattern giao tiếp giữa services?**

**A:**
```javascript
const communicationPatterns = {
    // Synchronous
    rest: {
        useCase: 'Simple request-response',
        pros: 'Simple, widely supported',
        cons: 'Tight coupling, blocking'
    },
    
    grpc: {
        useCase: 'High-performance communication',
        pros: 'Fast, strongly typed',
        cons: 'More complex setup'
    },
    
    // Asynchronous
    messageQueue: {
        useCase: 'Event-driven communication',
        pros: 'Loose coupling, reliable',
        cons: 'Eventual consistency, complexity'
    },
    
    eventSourcing: {
        useCase: 'Audit trail, temporal queries',
        pros: 'Complete history, replay capability',
        cons: 'Complex, storage overhead'
    }
};
```

### 4. Service Discovery
**Q: Làm sao services tìm và kết nối với nhau?**

**A:**
```javascript
const serviceDiscovery = {
    // Client-side discovery
    clientSide: {
        approach: 'Client queries service registry',
        pros: 'Simple, direct communication',
        cons: 'Client complexity, registry dependency'
    },
    
    // Server-side discovery
    serverSide: {
        approach: 'Load balancer queries registry',
        pros: 'Client simplicity, centralized',
        cons: 'Load balancer complexity'
    },
    
    // Service mesh
    serviceMesh: {
        approach: 'Sidecar proxy handles discovery',
        pros: 'Transparent to application',
        cons: 'Infrastructure complexity'
    }
};
```

### 5. Monitoring Microservices
**Q: Cách monitor và debug distributed system?**

**A:**
```javascript
const monitoringStrategies = {
    // Distributed tracing
    tracing: {
        tool: 'Jaeger, Zipkin',
        purpose: 'Track request flow across services',
        benefits: 'Identify bottlenecks, debug issues'
    },
    
    // Centralized logging
    logging: {
        tool: 'ELK Stack, Fluentd',
        purpose: 'Aggregate logs from all services',
        benefits: 'Search, analyze, alert on logs'
    },
    
    // Metrics collection
    metrics: {
        tool: 'Prometheus, Grafana',
        purpose: 'Collect performance metrics',
        benefits: 'Monitor health, set alerts'
    },
    
    // Health checks
    healthChecks: {
        tool: 'Built-in health endpoints',
        purpose: 'Monitor service availability',
        benefits: 'Quick failure detection'
    }
};
```

## 🚀 Real-world Examples

### E-commerce Microservices
```javascript
const ecommerceMicroservices = {
    services: {
        userService: {
            responsibility: 'User management, authentication',
            database: 'PostgreSQL',
            endpoints: ['/users', '/auth', '/profiles']
        },
        
        productService: {
            responsibility: 'Product catalog, inventory',
            database: 'MongoDB',
            endpoints: ['/products', '/categories', '/inventory']
        },
        
        orderService: {
            responsibility: 'Order processing, fulfillment',
            database: 'MySQL',
            endpoints: ['/orders', '/order-items', '/shipping']
        },
        
        paymentService: {
            responsibility: 'Payment processing, billing',
            database: 'PostgreSQL',
            endpoints: ['/payments', '/refunds', '/invoices']
        },
        
        notificationService: {
            responsibility: 'Email, SMS, push notifications',
            database: 'Redis',
            endpoints: ['/notifications', '/templates']
        }
    },
    
    communication: {
        synchronous: 'REST APIs for immediate responses',
        asynchronous: 'Event-driven for background processing',
        patterns: ['Saga for order processing', 'Event sourcing for audit']
    }
};
```

## 📚 Resources
- [Microservices.io](https://microservices.io/)
- [Martin Fowler on Microservices](https://martinfowler.com/articles/microservices.html)
- [Netflix Microservices](https://netflixtechblog.com/tagged/microservices)
- [AWS Microservices](https://aws.amazon.com/microservices/)
- [Kubernetes Documentation](https://kubernetes.io/docs/) 