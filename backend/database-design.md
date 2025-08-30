# 🗄️ Database Design - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Database Design
├── 🏗️ Design Principles
│   ├── Normalization → Reduce data redundancy
│   ├── Denormalization → Performance optimization
│   ├── ACID Properties → Transaction consistency
│   └── CAP Theorem → Consistency, Availability, Partition tolerance
├── 📊 Data Modeling
│   ├── ER Diagrams → Entity relationships
│   ├── Normal Forms → 1NF, 2NF, 3NF, BCNF
│   ├── Indexing → Performance optimization
│   └── Partitioning → Data distribution
├── 🔄 Database Types
│   ├── Relational → SQL databases, ACID compliance
│   ├── NoSQL → Document, key-value, graph databases
│   ├── NewSQL → Distributed SQL databases
│   └── Time Series → Time-based data storage
├── ⚡ Performance
│   ├── Query Optimization → Execution plans
│   ├── Caching → In-memory data storage
│   ├── Connection Pooling → Resource management
│   └── Sharding → Horizontal data distribution
└── 🛡️ Security & Maintenance
    ├── Backup Strategies → Data protection
    ├── Access Control → User permissions
    ├── Monitoring → Performance tracking
    └── Migration → Schema evolution
```

## 📋 Table of Contents
- [Design Principles](#design-principles)
- [Data Modeling](#data-modeling)
- [Database Types](#database-types)
- [Performance](#performance)
- [Security & Maintenance](#security--maintenance)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Database Types

### Relational Databases (SQL)
```sql
-- MySQL Example
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- PostgreSQL Example
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    category_id INTEGER REFERENCES categories(id)
);
```

**Ưu điểm:** ACID compliance, Strong consistency, Complex queries
**Nhược điểm:** Khó scale horizontally, Schema changes phức tạp

### NoSQL Databases
```javascript
// MongoDB Document
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "user_id": 12345,
  "username": "john_doe",
  "email": "john@example.com",
  "profile": {
    "first_name": "John",
    "last_name": "Doe",
    "age": 30
  },
  "orders": [
    {
      "order_id": "ORD001",
      "total": 150.00,
      "status": "completed"
    }
  ]
}

// Redis Key-Value
SET user:12345 '{"id": 12345, "name": "John"}'
SADD user:12345:followers 67890 11111 22222
```

**Ưu điểm:** Flexible schema, Horizontal scaling, High performance
**Nhược điểm:** Eventual consistency, Limited complex queries

## 🏗️ Design Principles

### Normalization
```sql
-- ❌ Denormalized (1NF violation)
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    user_name VARCHAR(100),      -- Duplicate data
    user_email VARCHAR(100),     -- Duplicate data
    product_id INT,
    product_name VARCHAR(100),   -- Duplicate data
    quantity INT,
    total DECIMAL(10,2)
);

-- ✅ Normalized (3NF)
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    total DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### ACID Properties
```sql
-- Atomicity Example
BEGIN TRANSACTION;
    INSERT INTO orders (user_id, product_id, quantity, total) 
    VALUES (123, 456, 2, 100.00);
    
    UPDATE products 
    SET stock = stock - 2 
    WHERE id = 456;
    
    INSERT INTO order_history (order_id, status) 
    VALUES (LAST_INSERT_ID(), 'created');
COMMIT;

-- Consistency Example
CREATE TABLE accounts (
    id INT PRIMARY KEY,
    balance DECIMAL(10,2) NOT NULL CHECK (balance >= 0)
);

-- Isolation Example
-- Transaction A cannot see uncommitted changes from Transaction B

-- Durability Example
-- Once COMMIT is executed, data is permanently stored
```

### CAP Theorem
```javascript
// Consistency (C) - All nodes see same data
const consistentSystem = {
    approach: 'Strong consistency',
    tradeoff: 'Lower availability during network partitions'
};

// Availability (A) - System remains operational
const availableSystem = {
    approach: 'High availability',
    tradeoff: 'Eventual consistency'
};

// Partition Tolerance (P) - System continues despite network failures
const partitionTolerantSystem = {
    approach: 'Distributed across multiple nodes',
    tradeoff: 'Must choose between C and A'
};
```

## 📊 Performance Optimization

### Indexing Strategy
```sql
-- Single Column Index
CREATE INDEX idx_user_email ON users(email);

-- Composite Index (Order matters!)
CREATE INDEX idx_user_status_created ON users(status, created_at);

-- Covering Index (Includes all needed columns)
CREATE INDEX idx_user_profile ON users(id, name, email, status);

-- Index Analysis
EXPLAIN SELECT * FROM users 
WHERE email = 'john@example.com' 
AND status = 'active';
```

### Query Optimization
```sql
-- ❌ Bad Query
SELECT * FROM users u
JOIN orders o ON u.id = o.user_id
JOIN products p ON o.product_id = p.id
WHERE u.created_at > '2023-01-01';

-- ✅ Optimized Query
SELECT 
    u.id,
    u.name,
    u.email,
    COUNT(o.id) as order_count,
    SUM(o.total) as total_spent
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE u.created_at > '2023-01-01'
    AND u.status = 'active'
GROUP BY u.id, u.name, u.email
HAVING COUNT(o.id) > 0
ORDER BY total_spent DESC
LIMIT 100;
```

### Partitioning
```sql
-- Range Partitioning
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    total DECIMAL(10,2),
    created_at DATE
) PARTITION BY RANGE (YEAR(created_at)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025)
);

-- Hash Partitioning
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
) PARTITION BY HASH(id) PARTITIONS 4;
```

### Caching Strategies
```javascript
// Multi-level Caching
class CacheManager {
    constructor() {
        this.l1Cache = new Map();        // Application cache
        this.l2Cache = new Redis();      // Distributed cache
    }
    
    async get(key) {
        // L1: Check application cache
        if (this.l1Cache.has(key)) {
            return this.l1Cache.get(key);
        }
        
        // L2: Check Redis cache
        const value = await this.l2Cache.get(key);
        if (value) {
            this.l1Cache.set(key, value);
            return value;
        }
        
        // Get from database
        const data = await this.database.get(key);
        
        // Update caches
        this.l1Cache.set(key, data);
        await this.l2Cache.set(key, data, 3600);
        
        return data;
    }
}

// Cache Patterns
const cachePatterns = {
    // Cache-Aside
    cacheAside: async (key) => {
        let data = await cache.get(key);
        if (!data) {
            data = await database.get(key);
            await cache.set(key, data);
        }
        return data;
    },
    
    // Write-Through
    writeThrough: async (key, value) => {
        await cache.set(key, value);
        await database.set(key, value);
    }
};
```

## 🔄 Data Modeling

### Entity Relationship Diagram (ERD)
```sql
-- User Entity
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Product Entity
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    stock_quantity INT NOT NULL DEFAULT 0,
    category_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);

-- Order Entity
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    status ENUM('pending', 'confirmed', 'shipped', 'delivered', 'cancelled') DEFAULT 'pending',
    total_amount DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Order Items (Many-to-Many relationship)
CREATE TABLE order_items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10,2) NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### Data Types Selection
```sql
-- Integer Types
TINYINT      -- 1 byte (-128 to 127)
SMALLINT     -- 2 bytes (-32,768 to 32,767)
INT          -- 4 bytes (-2^31 to 2^31-1)
BIGINT       -- 8 bytes (-2^63 to 2^63-1)

-- String Types
CHAR(10)     -- Fixed length, padded with spaces
VARCHAR(255) -- Variable length, max 255 characters
TEXT         -- Variable length, up to 65,535 characters

-- Date/Time Types
DATE         -- YYYY-MM-DD
TIME         -- HH:MM:SS
DATETIME     -- YYYY-MM-DD HH:MM:SS
TIMESTAMP    -- Unix timestamp, auto-update

-- Decimal Types
DECIMAL(10,2) -- Exact decimal, 10 digits total, 2 after decimal
FLOAT         -- Approximate floating point
DOUBLE        -- Double precision floating point

-- JSON Type (MySQL 5.7+)
JSON         -- Native JSON support
```

### Constraints
```sql
-- Primary Key
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL
);

-- Foreign Key
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Check Constraints
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    stock_quantity INT NOT NULL CHECK (stock_quantity >= 0)
);

-- Unique Constraints
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20) UNIQUE
);
```

## ❓ Common Interview Questions

### 1. Normalization vs Denormalization
**Q: Khi nào nên normalize và khi nào nên denormalize?**

**A:**
```sql
-- ✅ Normalize when:
-- - Data integrity is critical
-- - Storage space is limited
-- - Update operations are frequent
-- - Complex queries are needed

-- ✅ Denormalize when:
-- - Read performance is critical
-- - Storage space is not a concern
-- - Update operations are infrequent
-- - Simple queries are common

-- Example: Denormalized for performance
CREATE TABLE order_summary (
    id INT PRIMARY KEY,
    user_id INT,
    user_name VARCHAR(100),      -- Denormalized
    user_email VARCHAR(100),     -- Denormalized
    total_orders INT,
    total_spent DECIMAL(10,2)
);
```

### 2. Index Selection
**Q: Làm sao chọn index phù hợp cho query?**

**A:**
```sql
-- 1. Analyze query patterns
SELECT * FROM users WHERE email = 'john@example.com'; -- Index on email
SELECT * FROM users WHERE status = 'active' AND created_at > '2023-01-01'; -- Composite index

-- 2. Consider selectivity
-- High selectivity (many unique values) = good for index
-- Low selectivity (few unique values) = poor for index

-- 3. Consider query order
-- Most selective column first in composite index
CREATE INDEX idx_created_status ON users(created_at, status); -- Better order

-- 4. Covering indexes
-- Include all columns needed in the query
CREATE INDEX idx_user_covering ON users(id, name, email, status);
```

### 3. Database Sharding
**Q: Các strategy sharding database?**

**A:**
```javascript
const shardingStrategies = {
    // Hash-based sharding
    hashBased: {
        approach: 'Hash key to determine shard',
        pros: 'Even distribution, simple',
        cons: 'Difficult to add/remove shards'
    },
    
    // Range-based sharding
    rangeBased: {
        approach: 'Shard by value ranges',
        pros: 'Easy to add/remove shards',
        cons: 'Uneven distribution, hotspots'
    },
    
    // Directory-based sharding
    directoryBased: {
        approach: 'Lookup table for shard mapping',
        pros: 'Flexible, easy to change',
        cons: 'Additional lookup overhead'
    }
};
```

### 4. ACID vs BASE
**Q: Sự khác biệt giữa ACID và BASE?**

**A:**
```javascript
// ACID (Traditional Databases)
const ACID = {
    Atomicity: 'All operations succeed or fail together',
    Consistency: 'Data remains in valid state',
    Isolation: 'Concurrent transactions don\'t interfere',
    Durability: 'Committed data is permanent'
};

// BASE (NoSQL Databases)
const BASE = {
    BasicallyAvailable: 'System is available most of the time',
    SoftState: 'State may change over time',
    EventualConsistency: 'Data becomes consistent eventually'
};

// Trade-offs
const tradeoffs = {
    ACID: {
        pros: 'Strong consistency, data integrity',
        cons: 'Lower performance, harder to scale'
    },
    BASE: {
        pros: 'High performance, easy to scale',
        cons: 'Eventual consistency, complex application logic'
    }
};
```

### 5. Query Optimization
**Q: Cách optimize query performance?**

**A:**
```sql
-- 1. Use appropriate indexes
CREATE INDEX idx_user_email_status ON users(email, status);

-- 2. Avoid SELECT *
SELECT id, name, email FROM users WHERE status = 'active';

-- 3. Use LIMIT for large result sets
SELECT * FROM orders ORDER BY created_at DESC LIMIT 100;

-- 4. Use EXISTS instead of IN for large datasets
SELECT * FROM users u 
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);

-- 5. Use JOIN instead of subqueries
SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.name;

-- 6. Use EXPLAIN to analyze query plans
EXPLAIN SELECT * FROM users WHERE email = 'john@example.com';
```

## 🚀 Real-world Examples

### E-commerce Database Design
```sql
-- Users and Authentication
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    status ENUM('active', 'inactive', 'suspended') DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Product Catalog
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    parent_id INT,
    FOREIGN KEY (parent_id) REFERENCES categories(id)
);

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INT NOT NULL DEFAULT 0,
    category_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);

-- Orders and Payments
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    status ENUM('pending', 'confirmed', 'shipped', 'delivered', 'cancelled'),
    total_amount DECIMAL(10,2) NOT NULL,
    shipping_address TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE order_items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Indexes for Performance
CREATE INDEX idx_user_email ON users(email);
CREATE INDEX idx_product_category ON products(category_id);
CREATE INDEX idx_order_user_status ON orders(user_id, status);
CREATE INDEX idx_order_created ON orders(created_at);
```

## 📚 Resources
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Database Design Best Practices](https://www.coursera.org/learn/database-design)
- [High Performance MySQL](https://www.oreilly.com/library/view/high-performance-mysql/9780596101718/) 