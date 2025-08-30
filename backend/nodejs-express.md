# 🟢 Node.js & Express.js Deep Dive

## 🏗️ Node.js Core Concepts
- **Event Loop**: Single-threaded, non-blocking I/O model với 6 phases
- **V8 Engine**: JavaScript runtime, garbage collection, performance optimization
- **CommonJS vs ES Modules**: require() vs import/export, dynamic imports
- **Streams**: Readable, Writable, Transform, Duplex streams cho data processing
- **Buffer**: Binary data handling, encoding/decoding, memory management

## 🚀 Express.js Framework
- **Middleware**: Request processing pipeline, order matters, custom middleware
- **Routing**: RESTful routes, parameter handling, query strings, route protection
- **Error Handling**: Global error middleware, async error handling, custom error classes
- **Template Engines**: EJS, Pug, Handlebars, server-side rendering
- **Static Files**: Serving static assets, caching strategies, CDN integration

## 🔄 Asynchronous Programming
- **Callbacks**: Traditional async pattern, callback hell, error handling
- **Promises**: Promise chaining, Promise.all(), Promise.race(), error propagation
- **Async/Await**: Cleaner async code, try-catch error handling, parallel execution
- **Event Emitters**: Custom events, pub/sub pattern, event-driven architecture
- **Worker Threads**: CPU-intensive tasks, parallel processing, shared memory

## 🗄️ Database Integration
- **MongoDB**: Mongoose ODM, schemas, validation, indexing, aggregation
- **PostgreSQL**: Sequelize ORM, migrations, transactions, connection pooling
- **Redis**: Caching, sessions, pub/sub, data structures, persistence
- **Connection Pooling**: Database connection management, connection limits
- **ORM vs Query Builders**: Prisma, TypeORM, Knex, performance comparison

## 🔒 Authentication & Security
- **JWT**: Token-based authentication, refresh tokens, token expiration
- **Passport.js**: Authentication strategies, OAuth, social login integration
- **bcrypt**: Password hashing, salt rounds, security best practices
- **Helmet**: Security headers, XSS protection, content security policy
- **Rate Limiting**: API protection, brute force prevention, DDoS mitigation

## 📊 API Design & Validation
- **RESTful APIs**: HTTP methods, status codes, HATEOAS, resource naming
- **API Versioning**: URL versioning, header versioning, content negotiation
- **Input Validation**: Joi, Yup, express-validator, custom validation
- **Response Formatting**: Consistent API responses, error handling, pagination
- **Documentation**: Swagger/OpenAPI, Postman collections, API testing

## 🧪 Testing & Debugging
- **Unit Testing**: Jest, Mocha, Chai, test organization, mocking
- **Integration Testing**: Supertest, API testing, database testing
- **Mocking**: Sinon.js, Jest mocks, external dependencies
- **Debugging**: Node.js debugger, logging strategies, error tracking
- **Performance Testing**: Artillery, k6, load testing, stress testing

## ❓ Common Interview Questions

### 1. Node.js Event Loop
**Q: Giải thích Node.js Event Loop và cách hoạt động?**
**A:** Single-threaded event loop với 6 phases: timers, pending callbacks, idle/prepare, poll, check, close callbacks. Non-blocking I/O operations, microtasks vs macrotasks.

### 2. Express Middleware
**Q: Middleware trong Express.js hoạt động như thế nào?**
**A:** Functions chạy theo thứ tự, có thể modify request/response, gọi next() để chuyển sang middleware tiếp theo. Global, router-level, và error-handling middleware.

### 3. Async Programming
**Q: Callbacks vs Promises vs Async/Await?**
**A:** Callbacks gây callback hell, Promises chainable, Async/Await cleanest syntax. Use async/await với try-catch, Promise.all() cho parallel operations.

### 4. Database Performance
**Q: Cách optimize database performance trong Node.js?**
**A:** Connection pooling, indexing, query optimization, caching với Redis, pagination, database sharding, monitoring và profiling.

### 5. Security Best Practices
**Q: Security measures quan trọng cho Node.js apps?**
**A:** Input validation, SQL injection prevention, XSS protection, CORS, rate limiting, HTTPS, security headers, dependency scanning, regular updates. 