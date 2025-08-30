# 🟢 Node.js & Express.js - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Node.js & Express.js
├── 🏗️ Node.js Core Concepts
│   ├── Event Loop → Single-threaded, non-blocking I/O
│   ├── V8 Engine → JavaScript runtime, garbage collection
│   ├── CommonJS vs ES Modules → require() vs import/export
│   ├── Streams → Readable, Writable, Transform, Duplex
│   └── Buffer → Binary data handling, memory management
├── 🚀 Express.js Framework
│   ├── Middleware → Request processing pipeline
│   ├── Routing → RESTful routes, parameter handling
│   ├── Error Handling → Global error middleware
│   ├── Template Engines → EJS, Pug, Handlebars
│   └── Static Files → Asset serving, caching strategies
├── 🔄 Asynchronous Programming
│   ├── Callbacks → Traditional async pattern
│   ├── Promises → Promise chaining, error handling
│   ├── Async/Await → Cleaner async code
│   ├── Event Emitters → Custom events, pub/sub
│   └── Worker Threads → CPU-intensive tasks
├── 🗄️ Database Integration
│   ├── MongoDB → Mongoose ODM, schemas
│   ├── PostgreSQL → Sequelize ORM, migrations
│   ├── Redis → Caching, sessions, pub/sub
│   ├── Connection Pooling → Database management
│   └── ORM vs Query Builders → Prisma, TypeORM
├── 🔒 Authentication & Security
│   ├── JWT → Token-based authentication
│   ├── Passport.js → Authentication strategies
│   ├── bcrypt → Password hashing, security
│   ├── Helmet → Security headers, protection
│   └── Rate Limiting → API protection, DDoS mitigation
└── 📊 API Design & Validation
    ├── RESTful APIs → HTTP methods, status codes
    ├── API Versioning → URL, header versioning
    ├── Input Validation → Joi, Yup, express-validator
    ├── Response Formatting → Consistent API responses
    └── Documentation → Swagger/OpenAPI, testing
```

## 📋 Table of Contents
- [Node.js Core Concepts](#nodejs-core-concepts)
- [Express.js Framework](#expressjs-framework)
- [Asynchronous Programming](#asynchronous-programming)
- [Database Integration](#database-integration)
- [Authentication & Security](#authentication--security)
- [API Design & Validation](#api-design--validation)
- [Testing & Debugging](#testing--debugging)
- [Common Interview Questions](#common-interview-questions)

## 🏗️ Node.js Core Concepts

### Event Loop
**Event Loop** là trái tim của Node.js, cho phép xử lý non-blocking I/O operations:

```javascript
// Event Loop phases
console.log('1. Start');

setTimeout(() => {
    console.log('2. Timer phase');
}, 0);

setImmediate(() => {
    console.log('3. Check phase');
});

process.nextTick(() => {
    console.log('4. Next tick queue');
});

Promise.resolve().then(() => {
    console.log('5. Microtask queue');
});

console.log('6. End');

// Output:
// 1. Start
// 6. End
// 4. Next tick queue
// 5. Microtask queue
// 2. Timer phase
// 3. Check phase
```

### V8 Engine
**V8** là JavaScript engine mạnh mẽ của Node.js:

```javascript
// Memory management
const v8 = require('v8');

// Get heap statistics
const heapStats = v8.getHeapStatistics();
console.log('Total heap size:', heapStats.total_heap_size);
console.log('Used heap size:', heapStats.used_heap_size);

// Garbage collection
if (global.gc) {
    global.gc(); // Force garbage collection
}

// Performance optimization
const { performance } = require('perf_hooks');
const start = performance.now();

// Your code here
const end = performance.now();
console.log(`Execution time: ${end - start} milliseconds`);
```

### Streams
**Streams** cho phép xử lý dữ liệu lớn một cách hiệu quả:

```javascript
const fs = require('fs');
const { Transform } = require('stream');

// Readable stream
const readStream = fs.createReadStream('large-file.txt');

// Transform stream
const transformStream = new Transform({
    transform(chunk, encoding, callback) {
        const transformed = chunk.toString().toUpperCase();
        callback(null, transformed);
    }
});

// Writable stream
const writeStream = fs.createWriteStream('output.txt');

// Pipe streams together
readStream
    .pipe(transformStream)
    .pipe(writeStream);

// Custom readable stream
class CounterStream extends require('stream').Readable {
    constructor(max) {
        super();
        this.max = max;
        this.count = 0;
    }

    _read() {
        if (this.count < this.max) {
            this.push(String(this.count++));
        } else {
            this.push(null); // End stream
        }
    }
}

const counter = new CounterStream(5);
counter.pipe(process.stdout);
```

## 🚀 Express.js Framework

### Middleware Pattern
**Middleware** là functions xử lý request theo thứ tự:

```javascript
const express = require('express');
const app = express();

// Custom middleware
const logger = (req, res, next) => {
    console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
    next(); // Call next middleware
};

const auth = (req, res, next) => {
    const token = req.headers.authorization;
    
    if (!token) {
        return res.status(401).json({ error: 'No token provided' });
    }
    
    // Verify token logic here
    req.user = { id: 1, name: 'John' };
    next();
};

// Apply middleware
app.use(logger);
app.use(express.json());

// Route-specific middleware
app.get('/protected', auth, (req, res) => {
    res.json({ message: 'Protected route', user: req.user });
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong!' });
});
```

### Routing & Controllers
**Routing** tổ chức API endpoints một cách có cấu trúc:

```javascript
// Route organization
const express = require('express');
const router = express.Router();

// User routes
router.get('/users', async (req, res) => {
    try {
        const users = await User.find();
        res.json(users);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

router.post('/users', async (req, res) => {
    try {
        const user = new User(req.body);
        await user.save();
        res.status(201).json(user);
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

// Parameter handling
router.get('/users/:id', async (req, res) => {
    try {
        const user = await User.findById(req.params.id);
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        res.json(user);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Query parameters
router.get('/users', async (req, res) => {
    const { page = 1, limit = 10, search } = req.query;
    
    let query = {};
    if (search) {
        query.name = { $regex: search, $options: 'i' };
    }
    
    const users = await User.find(query)
        .limit(limit * 1)
        .skip((page - 1) * limit);
    
    res.json(users);
});
```

## 🔄 Asynchronous Programming

### Promises & Async/Await
**Promises** và **Async/Await** giúp xử lý async code dễ dàng hơn:

```javascript
// Promise chaining
function fetchUser(id) {
    return fetch(`/api/users/${id}`)
        .then(response => {
            if (!response.ok) {
                throw new Error('User not found');
            }
            return response.json();
        })
        .then(user => {
            return fetch(`/api/users/${user.id}/posts`);
        })
        .then(posts => {
            return { user, posts };
        })
        .catch(error => {
            console.error('Error:', error);
            throw error;
        });
}

// Async/Await version
async function fetchUserWithPosts(id) {
    try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
            throw new Error('User not found');
        }
        
        const user = await response.json();
        const postsResponse = await fetch(`/api/users/${user.id}/posts`);
        const posts = await postsResponse.json();
        
        return { user, posts };
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}

// Parallel execution
async function fetchMultipleUsers(ids) {
    const promises = ids.map(id => fetchUser(id));
    const users = await Promise.all(promises);
    return users;
}

// Race condition
async function fetchWithTimeout(url, timeout = 5000) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeout);
    
    try {
        const response = await fetch(url, {
            signal: controller.signal
        });
        clearTimeout(timeoutId);
        return response.json();
    } catch (error) {
        clearTimeout(timeoutId);
        if (error.name === 'AbortError') {
            throw new Error('Request timeout');
        }
        throw error;
    }
}
```

### Event Emitters
**Event Emitters** cho phép tạo custom events:

```javascript
const EventEmitter = require('events');

class UserService extends EventEmitter {
    constructor() {
        super();
    }
    
    async createUser(userData) {
        try {
            const user = await User.create(userData);
            
            // Emit events
            this.emit('user:created', user);
            this.emit('user:registered', { userId: user.id, email: user.email });
            
            return user;
        } catch (error) {
            this.emit('user:error', error);
            throw error;
        }
    }
    
    async deleteUser(userId) {
        try {
            await User.findByIdAndDelete(userId);
            this.emit('user:deleted', { userId });
        } catch (error) {
            this.emit('user:error', error);
            throw error;
        }
    }
}

// Usage
const userService = new UserService();

userService.on('user:created', (user) => {
    console.log('New user created:', user.email);
    // Send welcome email
});

userService.on('user:deleted', ({ userId }) => {
    console.log('User deleted:', userId);
    // Cleanup related data
});

userService.on('user:error', (error) => {
    console.error('User service error:', error);
    // Log to monitoring service
});
```

## 🗄️ Database Integration

### MongoDB với Mongoose
**MongoDB** là NoSQL database phổ biến với Node.js:

```javascript
const mongoose = require('mongoose');

// Schema definition
const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, 'Name is required'],
        trim: true,
        minlength: [2, 'Name must be at least 2 characters']
    },
    email: {
        type: String,
        required: [true, 'Email is required'],
        unique: true,
        lowercase: true,
        match: [/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/, 'Invalid email format']
    },
    age: {
        type: Number,
        min: [18, 'Must be at least 18 years old'],
        max: [120, 'Age cannot exceed 120']
    },
    createdAt: {
        type: Date,
        default: Date.now
    }
}, {
    timestamps: true,
    toJSON: { virtuals: true },
    toObject: { virtuals: true }
});

// Virtual fields
userSchema.virtual('fullName').get(function() {
    return `${this.name.first} ${this.name.last}`;
});

// Instance methods
userSchema.methods.getAge = function() {
    return Math.floor((Date.now() - this.createdAt) / (365.25 * 24 * 60 * 60 * 1000));
};

// Static methods
userSchema.statics.findByEmail = function(email) {
    return this.findOne({ email: email.toLowerCase() });
};

// Middleware
userSchema.pre('save', function(next) {
    if (this.isModified('password')) {
        this.password = bcrypt.hashSync(this.password, 10);
    }
    next();
});

userSchema.post('save', function(doc) {
    console.log('User saved:', doc.email);
});

const User = mongoose.model('User', userSchema);
```

### PostgreSQL với Sequelize
**Sequelize** là ORM mạnh mẽ cho PostgreSQL:

```javascript
const { Sequelize, DataTypes, Model } = require('sequelize');

const sequelize = new Sequelize('database', 'username', 'password', {
    host: 'localhost',
    dialect: 'postgres',
    logging: false,
    pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    }
});

// Model definition
class User extends Model {}

User.init({
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    name: {
        type: DataTypes.STRING,
        allowNull: false,
        validate: {
            len: [2, 50]
        }
    },
    email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true,
        validate: {
            isEmail: true
        }
    },
    status: {
        type: DataTypes.ENUM('active', 'inactive', 'suspended'),
        defaultValue: 'active'
    }
}, {
    sequelize,
    modelName: 'User',
    tableName: 'users',
    timestamps: true,
    paranoid: true // Soft deletes
});

// Associations
User.hasMany(Post, { foreignKey: 'authorId' });
Post.belongsTo(User, { foreignKey: 'authorId' });

// Transactions
const createUserWithPosts = async (userData, postsData) => {
    const transaction = await sequelize.transaction();
    
    try {
        const user = await User.create(userData, { transaction });
        
        for (const postData of postsData) {
            await Post.create({
                ...postData,
                authorId: user.id
            }, { transaction });
        }
        
        await transaction.commit();
        return user;
    } catch (error) {
        await transaction.rollback();
        throw error;
    }
};
```

## 🔒 Authentication & Security

### JWT Authentication
**JWT** (JSON Web Tokens) cho token-based authentication:

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// JWT configuration
const JWT_SECRET = process.env.JWT_SECRET || 'your-secret-key';
const JWT_EXPIRES_IN = '24h';

// Generate token
const generateToken = (payload) => {
    return jwt.sign(payload, JWT_SECRET, {
        expiresIn: JWT_EXPIRES_IN,
        issuer: 'your-app',
        audience: 'your-app-users'
    });
};

// Verify token
const verifyToken = (token) => {
    try {
        return jwt.verify(token, JWT_SECRET);
    } catch (error) {
        throw new Error('Invalid token');
    }
};

// Authentication middleware
const authenticateToken = (req, res, next) => {
    const authHeader = req.headers.authorization;
    const token = authHeader && authHeader.split(' ')[1];
    
    if (!token) {
        return res.status(401).json({ error: 'Access token required' });
    }
    
    try {
        const decoded = verifyToken(token);
        req.user = decoded;
        next();
    } catch (error) {
        return res.status(403).json({ error: 'Invalid token' });
    }
};

// Login endpoint
app.post('/auth/login', async (req, res) => {
    try {
        const { email, password } = req.body;
        
        const user = await User.findOne({ email });
        if (!user) {
            return res.status(401).json({ error: 'Invalid credentials' });
        }
        
        const isValidPassword = await bcrypt.compare(password, user.password);
        if (!isValidPassword) {
            return res.status(401).json({ error: 'Invalid credentials' });
        }
        
        const token = generateToken({
            userId: user.id,
            email: user.email,
            role: user.role
        });
        
        res.json({
            token,
            user: {
                id: user.id,
                email: user.email,
                name: user.name
            }
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

### Security Middleware
**Security** là yếu tố quan trọng cho production apps:

```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const cors = require('cors');

// Security headers
app.use(helmet({
    contentSecurityPolicy: {
        directives: {
            defaultSrc: ["'self'"],
            styleSrc: ["'self'", "'unsafe-inline'"],
            scriptSrc: ["'self'"],
            imgSrc: ["'self'", "data:", "https:"],
        },
    },
    hsts: {
        maxAge: 31536000,
        includeSubDomains: true,
        preload: true
    }
}));

// CORS configuration
app.use(cors({
    origin: process.env.ALLOWED_ORIGINS?.split(',') || ['http://localhost:3000'],
    credentials: true,
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization']
}));

// Rate limiting
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: 'Too many requests from this IP',
    standardHeaders: true,
    legacyHeaders: false,
});

app.use('/api/', limiter);

// Input validation
const { body, validationResult } = require('express-validator');

const validateUser = [
    body('email').isEmail().normalizeEmail(),
    body('password').isLength({ min: 6 }),
    body('name').trim().isLength({ min: 2 }),
    (req, res, next) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }
        next();
    }
];

app.post('/users', validateUser, async (req, res) => {
    // User creation logic
});
```

## 📊 API Design & Validation

### RESTful API Design
**RESTful APIs** tuân theo các nguyên tắc thiết kế chuẩn:

```javascript
// RESTful resource endpoints
app.get('/api/users', async (req, res) => {
    try {
        const { page = 1, limit = 10, sort = 'createdAt', order = 'desc' } = req.query;
        
        const options = {
            page: parseInt(page),
            limit: parseInt(limit),
            sort: { [sort]: order === 'desc' ? -1 : 1 }
        };
        
        const users = await User.paginate({}, options);
        
        res.json({
            data: users.docs,
            pagination: {
                page: users.page,
                limit: users.limit,
                totalPages: users.totalPages,
                totalDocs: users.totalDocs
            }
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

app.post('/api/users', async (req, res) => {
    try {
        const user = new User(req.body);
        await user.save();
        
        res.status(201).json({
            message: 'User created successfully',
            data: user
        });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

app.get('/api/users/:id', async (req, res) => {
    try {
        const user = await User.findById(req.params.id);
        
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        
        res.json({ data: user });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

app.put('/api/users/:id', async (req, res) => {
    try {
        const user = await User.findByIdAndUpdate(
            req.params.id,
            req.body,
            { new: true, runValidators: true }
        );
        
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        
        res.json({
            message: 'User updated successfully',
            data: user
        });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

app.delete('/api/users/:id', async (req, res) => {
    try {
        const user = await User.findByIdAndDelete(req.params.id);
        
        if (!user) {
            return res.status(404).json({ error: 'User not found' });
        }
        
        res.json({ message: 'User deleted successfully' });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});
```

## 🧪 Testing & Debugging

### Unit Testing với Jest
**Jest** là testing framework mạnh mẽ cho Node.js:

```javascript
// User service test
const UserService = require('../services/UserService');
const User = require('../models/User');

// Mock User model
jest.mock('../models/User');

describe('UserService', () => {
    let userService;
    
    beforeEach(() => {
        userService = new UserService();
        jest.clearAllMocks();
    });
    
    describe('createUser', () => {
        it('should create a new user successfully', async () => {
            const userData = {
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            };
            
            const mockUser = { ...userData, id: 1 };
            User.create.mockResolvedValue(mockUser);
            
            const result = await userService.createUser(userData);
            
            expect(User.create).toHaveBeenCalledWith(userData);
            expect(result).toEqual(mockUser);
        });
        
        it('should throw error for invalid email', async () => {
            const userData = {
                name: 'John Doe',
                email: 'invalid-email',
                password: 'password123'
            };
            
            User.create.mockRejectedValue(new Error('Invalid email'));
            
            await expect(userService.createUser(userData))
                .rejects
                .toThrow('Invalid email');
        });
    });
    
    describe('getUserById', () => {
        it('should return user when found', async () => {
            const mockUser = { id: 1, name: 'John Doe' };
            User.findById.mockResolvedValue(mockUser);
            
            const result = await userService.getUserById(1);
            
            expect(User.findById).toHaveBeenCalledWith(1);
            expect(result).toEqual(mockUser);
        });
        
        it('should return null when user not found', async () => {
            User.findById.mockResolvedValue(null);
            
            const result = await userService.getUserById(999);
            
            expect(result).toBeNull();
        });
    });
});
```

## ❓ Common Interview Questions

### 1. Node.js Event Loop
**Q: Giải thích Node.js Event Loop và cách hoạt động?**

**A:** Node.js Event Loop là single-threaded event loop với 6 phases:
- **Timers**: Xử lý setTimeout/setInterval callbacks
- **Pending callbacks**: Xử lý I/O callbacks đã hoàn thành
- **Idle, prepare**: Chỉ sử dụng internally
- **Poll**: Lấy new I/O events, execute I/O callbacks
- **Check**: Xử lý setImmediate() callbacks
- **Close callbacks**: Xử lý close event callbacks

Event Loop cho phép Node.js xử lý non-blocking I/O operations một cách hiệu quả.

### 2. Express Middleware
**Q: Middleware trong Express.js hoạt động như thế nào?**

**A:** Middleware là functions chạy theo thứ tự trong request-response cycle:
- **Global middleware**: Áp dụng cho tất cả routes
- **Route-level middleware**: Chỉ áp dụng cho specific routes
- **Error-handling middleware**: Xử lý errors (4 parameters)
- Middleware có thể modify request/response objects
- Gọi `next()` để chuyển sang middleware tiếp theo
- Thứ tự middleware rất quan trọng

### 3. Async Programming
**Q: Callbacks vs Promises vs Async/Await?**

**A:** 
- **Callbacks**: Traditional pattern, dễ gây callback hell
- **Promises**: Chainable, better error handling, Promise.all() cho parallel operations
- **Async/Await**: Cleanest syntax, try-catch error handling, dễ debug
- **Best practice**: Sử dụng async/await với try-catch, Promise.all() cho parallel operations

### 4. Database Performance
**Q: Cách optimize database performance trong Node.js?**

**A:** 
- **Connection pooling**: Quản lý database connections hiệu quả
- **Indexing**: Tạo indexes cho fields thường query
- **Query optimization**: Sử dụng aggregation, projection
- **Caching**: Redis cho frequently accessed data
- **Pagination**: Limit results, use cursor-based pagination
- **Monitoring**: Database performance metrics, slow query logs

### 5. Security Best Practices
**Q: Security measures quan trọng cho Node.js apps?**

**A:** 
- **Input validation**: Validate tất cả user inputs
- **SQL injection prevention**: Use parameterized queries
- **XSS protection**: Sanitize user inputs, use CSP headers
- **CORS**: Configure cross-origin requests properly
- **Rate limiting**: Prevent brute force attacks
- **HTTPS**: Encrypt data in transit
- **Security headers**: Helmet.js, HSTS, CSP
- **Dependency scanning**: Regular security updates

## 📚 Resources
- [Node.js Official Documentation](https://nodejs.org/docs/)
- [Express.js Documentation](https://expressjs.com/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [Express Security Best Practices](https://expressjs.com/en/advanced/best-practices-security.html)
- [Node.js Testing](https://nodejs.org/en/docs/guides/testing-and-debugging/) 