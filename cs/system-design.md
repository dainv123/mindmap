# 🏗️ System Design - Complete Knowledge Map

## 🧠 Mindmap Overview
```
System Design
├── 🎯 Core Principles
│   ├── Scalability → Horizontal vs Vertical scaling
│   ├── Availability → High availability, fault tolerance
│   ├── Performance → Latency, throughput, optimization
│   └── Reliability → Data consistency, error handling
├── 🏗️ Architecture Patterns
│   ├── Monolithic → Single application, simple deployment
│   ├── Microservices → Independent services, loose coupling
│   ├── Event-Driven → Asynchronous communication
│   └── CQRS → Command Query Responsibility Segregation
├── 🔄 Data Management
│   ├── Database Design → Normalization, indexing, sharding
│   ├── Caching Strategy → Redis, CDN, application cache
│   ├── Data Consistency → ACID, BASE, eventual consistency
│   └── Data Replication → Master-slave, multi-master
├── ⚡ Performance & Scaling
│   ├── Load Balancing → Round-robin, least connections
│   ├── Horizontal Scaling → Add more servers
│   ├── Vertical Scaling → Upgrade server resources
│   └── Database Optimization → Query optimization, indexing
└── 🔒 Security & Reliability
    ├── Authentication → JWT, OAuth, SSO
    ├── Authorization → RBAC, permissions
    ├── Rate Limiting → API throttling
    └── Monitoring → Logging, metrics, alerting
```

## 📋 Table of Contents
- [Core Principles](#core-principles)
- [Architecture Patterns](#architecture-patterns)
- [Data Management](#data-management)
- [Performance & Scaling](#performance--scaling)
- [Security & Reliability](#security--reliability)
- [Common Interview Questions](#common-interview-questions)
- [Real-world Examples](#real-world-examples)

## 🎯 Core Principles

### What is System Design?
**System Design** là quá trình thiết kế kiến trúc của một hệ thống phần mềm để đáp ứng các yêu cầu về:
- **Scalability** → Khả năng xử lý tải cao
- **Availability** → Thời gian hoạt động cao
- **Performance** → Tốc độ xử lý nhanh
- **Reliability** → Độ tin cậy cao

### Key Principles
```javascript
// 1. Scalability - Khả năng mở rộng
// Vertical Scaling: Tăng resources của server hiện tại
Server: 8GB RAM → 32GB RAM
CPU: 4 cores → 16 cores

// Horizontal Scaling: Thêm nhiều servers
1 Server → 10 Servers → 100 Servers

// 2. Availability - Tính sẵn sàng
// High Availability: 99.9% uptime
// Fault Tolerance: Hệ thống vẫn hoạt động khi có lỗi

// 3. Performance - Hiệu suất
// Latency: Thời gian phản hồi
// Throughput: Số lượng requests/giây
// Response Time: Tổng thời gian xử lý request
```

## 🏗️ Architecture Patterns

### 1. Monolithic Architecture
```
┌─────────────────────────────────────┐
│           Monolithic App            │
├─────────────────────────────────────┤
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│  │  Auth   │ │  User   │ │  Order  │ │
│  │ Module  │ │ Module  │ │ Module  │ │
│  └─────────┘ └─────────┘ └─────────┘ │
├─────────────────────────────────────┤
│           Shared Database            │
└─────────────────────────────────────┘
```

**Ưu điểm:**
- Dễ develop và deploy
- Đơn giản để test
- Performance tốt (no network calls)

**Nhược điểm:**
- Khó scale từng phần
- Technology lock-in
- Single point of failure

### 2. Microservices Architecture
```
┌─────────┐ ┌─────────┐ ┌─────────┐
│  Auth   │ │  User   │ │  Order  │
│Service  │ │Service  │ │Service  │
└─────────┘ └─────────┘ └─────────┘
     │           │           │
     └───────────┼───────────┘
                 │
        ┌─────────────┐
        │   API       │
        │ Gateway     │
        └─────────────┘
```

**Ưu điểm:**
- Independent scaling
- Technology diversity
- Fault isolation

**Nhược điểm:**
- Distributed system complexity
- Network latency
- Data consistency challenges

### 3. Event-Driven Architecture
```javascript
// Publisher-Subscriber Pattern
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

class NotificationService {
    async handleOrderCreated(event) {
        // Send notification to user
        await this.sendEmail(event.userId, 'Order created successfully');
    }
}

class InventoryService {
    async handleOrderCreated(event) {
        // Update inventory
        await this.updateStock(event.orderId);
    }
}
```

## 🔄 Data Management

### Database Design
```sql
-- Normalization Example
-- ❌ Bad: Denormalized
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    user_name VARCHAR(100),      -- Duplicate data
    user_email VARCHAR(100),     -- Duplicate data
    product_id INT,
    product_name VARCHAR(100),   -- Duplicate data
    product_price DECIMAL(10,2), -- Duplicate data
    quantity INT,
    total DECIMAL(10,2)
);

-- ✅ Good: Normalized
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10,2)
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    product_id INT,
    quantity INT,
    total DECIMAL(10,2),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### Caching Strategy
```javascript
// Multi-level Caching
class CacheManager {
    constructor() {
        this.l1Cache = new Map();        // In-memory cache
        this.l2Cache = new Redis();      // Distributed cache
        this.l3Cache = new CDN();        // Content delivery network
    }
    
    async get(key) {
        // L1: Check in-memory cache
        if (this.l1Cache.has(key)) {
            return this.l1Cache.get(key);
        }
        
        // L2: Check Redis cache
        const value = await this.l2Cache.get(key);
        if (value) {
            this.l1Cache.set(key, value); // Populate L1
            return value;
        }
        
        // L3: Get from database
        const data = await this.database.get(key);
        
        // Update all caches
        this.l1Cache.set(key, data);
        await this.l2Cache.set(key, data, 3600); // 1 hour TTL
        
        return data;
    }
}

// Cache Patterns
const cachePatterns = {
    // Cache-Aside: Application manages cache
    cacheAside: async (key) => {
        let data = await cache.get(key);
        if (!data) {
            data = await database.get(key);
            await cache.set(key, data);
        }
        return data;
    },
    
    // Write-Through: Write to cache and database simultaneously
    writeThrough: async (key, value) => {
        await cache.set(key, value);
        await database.set(key, value);
    },
    
    // Write-Behind: Write to cache first, database later
    writeBehind: async (key, value) => {
        await cache.set(key, value);
        // Queue for database update
        await queue.push({ key, value });
    }
};
```

### Data Consistency
```javascript
// ACID vs BASE
const consistencyModels = {
    // ACID (Traditional Databases)
    ACID: {
        Atomicity: 'All operations succeed or fail together',
        Consistency: 'Data remains in valid state',
        Isolation: 'Concurrent transactions don\'t interfere',
        Durability: 'Committed data is permanent'
    },
    
    // BASE (NoSQL Databases)
    BASE: {
        BasicallyAvailable: 'System is available most of the time',
        SoftState: 'State may change over time',
        EventualConsistency: 'Data becomes consistent eventually'
    }
};

// Eventual Consistency Example
class UserService {
    async updateUserProfile(userId, profile) {
        // Update primary database
        await this.primaryDB.updateUser(userId, profile);
        
        // Asynchronously update read replicas
        this.updateReplicas(userId, profile);
        
        // Update cache
        await this.cache.set(`user:${userId}`, profile);
        
        return { success: true };
    }
    
    async updateReplicas(userId, profile) {
        const replicas = await this.getReadReplicas();
        
        // Update replicas in background
        replicas.forEach(replica => {
            replica.updateUser(userId, profile).catch(err => {
                console.error(`Failed to update replica: ${err}`);
                // Retry logic here
            });
        });
    }
}
```

## ⚡ Performance & Scaling

### Load Balancing
```javascript
// Load Balancer Algorithms
class LoadBalancer {
    constructor(algorithm = 'roundRobin') {
        this.algorithm = algorithm;
        this.servers = [];
        this.currentIndex = 0;
    }
    
    addServer(server) {
        this.servers.push(server);
    }
    
    getNextServer() {
        switch (this.algorithm) {
            case 'roundRobin':
                return this.roundRobin();
            case 'leastConnections':
                return this.leastConnections();
            case 'weightedRoundRobin':
                return this.weightedRoundRobin();
            default:
                return this.roundRobin();
        }
    }
    
    roundRobin() {
        if (this.servers.length === 0) return null;
        
        const server = this.servers[this.currentIndex];
        this.currentIndex = (this.currentIndex + 1) % this.servers.length;
        return server;
    }
    
    leastConnections() {
        return this.servers.reduce((min, server) => 
            server.connections < min.connections ? server : min
        );
    }
    
    weightedRoundRobin() {
        // Implementation for weighted round-robin
        return this.servers[this.currentIndex];
    }
}

// Health Checking
class HealthChecker {
    async checkHealth(server) {
        try {
            const response = await fetch(`${server.url}/health`, {
                timeout: 5000
            });
            
            if (response.ok) {
                server.status = 'healthy';
                server.lastCheck = Date.now();
            } else {
                server.status = 'unhealthy';
            }
        } catch (error) {
            server.status = 'unhealthy';
            server.lastError = error.message;
        }
    }
    
    async monitorServers(servers) {
        setInterval(async () => {
            for (const server of servers) {
                await this.checkHealth(server);
            }
        }, 30000); // Check every 30 seconds
    }
}
```

### Database Optimization
```sql
-- Indexing Strategy
-- ✅ Good: Composite index for common queries
CREATE INDEX idx_user_email_status ON users(email, status);

-- Query optimization
EXPLAIN SELECT * FROM users 
WHERE email = 'john@example.com' 
AND status = 'active';

-- Partitioning for large tables
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    amount DECIMAL(10,2)
) PARTITION BY RANGE (YEAR(order_date)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025)
);

-- Database Sharding
-- Shard by user_id
-- Shard 0: user_id % 4 = 0
-- Shard 1: user_id % 4 = 1
-- Shard 2: user_id % 4 = 2
-- Shard 3: user_id % 4 = 3
```

## 🔒 Security & Reliability

### Authentication & Authorization
```javascript
// JWT Implementation
class AuthService {
    generateToken(user) {
        const payload = {
            userId: user.id,
            email: user.email,
            role: user.role,
            iat: Date.now(),
            exp: Date.now() + (24 * 60 * 60 * 1000) // 24 hours
        };
        
        return jwt.sign(payload, process.env.JWT_SECRET);
    }
    
    verifyToken(token) {
        try {
            return jwt.verify(token, process.env.JWT_SECRET);
        } catch (error) {
            throw new Error('Invalid token');
        }
    }
}

// RBAC (Role-Based Access Control)
class AuthorizationService {
    constructor() {
        this.permissions = {
            admin: ['read', 'write', 'delete', 'manage_users'],
            manager: ['read', 'write', 'manage_team'],
            user: ['read', 'write_own'],
            guest: ['read']
        };
    }
    
    hasPermission(userRole, action) {
        const rolePermissions = this.permissions[userRole] || [];
        return rolePermissions.includes(action);
    }
    
    authorize(user, action, resource) {
        if (!this.hasPermission(user.role, action)) {
            throw new Error('Access denied');
        }
        
        // Additional resource-level checks
        if (action === 'write_own' && resource.userId !== user.id) {
            throw new Error('Can only modify own resources');
        }
        
        return true;
    }
}
```

### Rate Limiting
```javascript
// Token Bucket Algorithm
class RateLimiter {
    constructor(capacity, refillRate) {
        this.capacity = capacity;        // Maximum tokens
        this.refillRate = refillRate;    // Tokens per second
        this.tokens = capacity;          // Current tokens
        this.lastRefill = Date.now();
    }
    
    async allowRequest(userId) {
        this.refillTokens();
        
        if (this.tokens > 0) {
            this.tokens--;
            return true;
        }
        
        return false;
    }
    
    refillTokens() {
        const now = Date.now();
        const timePassed = (now - this.lastRefill) / 1000;
        const tokensToAdd = timePassed * this.refillRate;
        
        this.tokens = Math.min(this.capacity, this.tokens + tokensToAdd);
        this.lastRefill = now;
    }
}

// Sliding Window Rate Limiting
class SlidingWindowRateLimiter {
    constructor(maxRequests, windowSize) {
        this.maxRequests = maxRequests;
        this.windowSize = windowSize;
        this.requests = new Map(); // userId -> request timestamps
    }
    
    async allowRequest(userId) {
        const now = Date.now();
        const userRequests = this.requests.get(userId) || [];
        
        // Remove old requests outside the window
        const validRequests = userRequests.filter(
            timestamp => now - timestamp < this.windowSize
        );
        
        if (validRequests.length >= this.maxRequests) {
            return false;
        }
        
        // Add current request
        validRequests.push(now);
        this.requests.set(userId, validRequests);
        
        return true;
    }
}
```

## ❓ Common Interview Questions

### 1. Design a URL Shortener
**Q: Thiết kế hệ thống URL shortener như bit.ly?**

**A:**
```javascript
// System Requirements
const requirements = {
    functional: [
        'Shorten long URLs',
        'Redirect short URLs to original',
        'Track click analytics'
    ],
    nonFunctional: [
        'High availability (99.9%)',
        'Low latency (< 100ms)',
        'Scalable to handle millions of URLs'
    ]
};

// System Design
const urlShortener = {
    // 1. URL Generation
    generateShortUrl: (longUrl) => {
        // Hash long URL to generate unique ID
        const hash = crypto.createHash('md5').update(longUrl).digest('hex');
        const shortId = hash.substring(0, 7); // 7 characters
        
        return `https://short.ly/${shortId}`;
    },
    
    // 2. Database Schema
    database: {
        urls: {
            shortId: 'VARCHAR(7) PRIMARY KEY',
            longUrl: 'TEXT NOT NULL',
            createdAt: 'TIMESTAMP',
            userId: 'INT',
            clicks: 'INT DEFAULT 0'
        },
        clicks: {
            id: 'BIGINT PRIMARY KEY',
            shortId: 'VARCHAR(7)',
            timestamp: 'TIMESTAMP',
            ip: 'VARCHAR(45)',
            userAgent: 'TEXT'
        }
    },
    
    // 3. Architecture
    architecture: {
        loadBalancer: 'Round-robin load balancer',
        webServers: 'Multiple web servers for high availability',
        cache: 'Redis for frequently accessed URLs',
        database: 'MySQL for URL storage, MongoDB for analytics'
    }
};
```

### 2. Design a Chat System
**Q: Thiết kế hệ thống chat real-time như WhatsApp?**

**A:**
```javascript
const chatSystem = {
    // 1. Core Components
    components: {
        webSocketServer: 'Handle real-time connections',
        messageQueue: 'Kafka/RabbitMQ for message delivery',
        database: 'Store messages and user data',
        cache: 'Redis for online status and recent messages'
    },
    
    // 2. Message Flow
    messageFlow: {
        sender: 'User A sends message',
        webSocket: 'Message sent to WebSocket server',
        queue: 'Message queued in message broker',
        delivery: 'Message delivered to recipient(s)',
        storage: 'Message stored in database'
    },
    
    // 3. Database Design
    database: {
        users: {
            id: 'INT PRIMARY KEY',
            username: 'VARCHAR(50)',
            status: 'ENUM(online, offline, away)'
        },
        conversations: {
            id: 'INT PRIMARY KEY',
            type: 'ENUM(direct, group)',
            participants: 'JSON array of user IDs'
        },
        messages: {
            id: 'BIGINT PRIMARY KEY',
            conversationId: 'INT',
            senderId: 'INT',
            content: 'TEXT',
            timestamp: 'TIMESTAMP',
            status: 'ENUM(sent, delivered, read)'
        }
    },
    
    // 4. Scaling Strategy
    scaling: {
        horizontal: 'Multiple WebSocket servers',
        sharding: 'Shard by conversation ID',
        caching: 'Cache recent messages and online status'
    }
};
```

### 3. Design a Social Media Feed
**Q: Thiết kế news feed như Facebook?**

**A:**
```javascript
const socialMediaFeed = {
    // 1. Feed Generation
    feedGeneration: {
        pull: 'Client pulls feed from server',
        push: 'Server pushes updates to clients',
        hybrid: 'Combination of pull and push'
    },
    
    // 2. Data Model
    dataModel: {
        users: 'User profiles and relationships',
        posts: 'Content posted by users',
        interactions: 'Likes, comments, shares',
        feed: 'Personalized content stream'
    },
    
    // 3. Feed Algorithm
    feedAlgorithm: {
        factors: [
            'User relationship (friend, family, acquaintance)',
            'Post engagement (likes, comments, shares)',
            'Post recency (newer posts get priority)',
            'Content type (photo, video, text)',
            'User preferences and history'
        ],
        scoring: 'Weighted combination of factors'
    },
    
    // 4. Performance Optimization
    optimization: {
        caching: 'Cache user feeds in Redis',
        pagination: 'Load feed in chunks (20 posts at a time)',
        precomputation: 'Pre-compute feed scores',
        cdn: 'Serve media content from CDN'
    }
};
```

## 🚀 Real-world Examples

### E-commerce System
```javascript
const ecommerceSystem = {
    // 1. Microservices Architecture
    services: {
        userService: 'User management and authentication',
        productService: 'Product catalog and inventory',
        orderService: 'Order processing and management',
        paymentService: 'Payment processing',
        notificationService: 'Email and SMS notifications'
    },
    
    // 2. Data Flow
    orderFlow: {
        '1. User selects products': 'Add to cart',
        '2. Checkout process': 'Validate inventory and pricing',
        '3. Payment processing': 'Secure payment gateway',
        '4. Order confirmation': 'Update inventory and notify user',
        '5. Fulfillment': 'Shipping and delivery tracking'
    },
    
    // 3. Scaling Considerations
    scaling: {
        database: 'Read replicas for product catalog',
        cache: 'Redis for session and product data',
        cdn: 'Serve product images globally',
        queue: 'Process orders asynchronously'
    }
};
```

## 📚 Resources
- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [High Scalability](http://highscalability.com/)
- [Martin Fowler's Blog](https://martinfowler.com/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [Google Cloud Architecture](https://cloud.google.com/architecture) 