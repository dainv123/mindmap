# 🌐 Web APIs & HTTP - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Web APIs & HTTP
├── 🌐 HTTP Fundamentals
│   ├── Methods → GET, POST, PUT, DELETE
│   ├── Status Codes → 2xx, 4xx, 5xx responses
│   ├── Headers → Request/Response metadata
│   └── Body → Request/Response data
├── 🏗️ API Design
│   ├── REST Principles → Resource-based design
│   ├── GraphQL → Query language for APIs
│   ├── gRPC → High-performance RPC
│   └── WebSockets → Real-time communication
├── 🔒 API Security
│   ├── Authentication → JWT, OAuth, API keys
│   ├── Authorization → Role-based access
│   ├── Rate Limiting → API protection
│   └── CORS → Cross-origin requests
├── 📊 Data Formats
│   ├── JSON → JavaScript Object Notation
│   ├── XML → Extensible Markup Language
│   ├── Protocol Buffers → Binary serialization
│   └── MessagePack → Binary JSON alternative
└── 🚀 Performance
    ├── Caching → Response caching strategies
    ├── Compression → Gzip, Brotli compression
    ├── CDN → Content delivery networks
    └── Load Balancing → Traffic distribution
```

## 📋 Table of Contents
- [HTTP Fundamentals](#http-fundamentals)
- [API Design](#api-design)
- [API Security](#api-security)
- [Data Formats](#data-formats)
- [Performance](#performance)
- [Common Interview Questions](#common-interview-questions)

## 🌐 HTTP Fundamentals
```
Web APIs & HTTP Deep Dive
├── 🌍 HTTP Fundamentals
│   ├── HTTP Methods
│   │   ├── GET → Retrieve data, idempotent, safe
│   │   ├── POST → Create resource, not idempotent
│   │   ├── PUT → Replace entire resource, idempotent
│   │   ├── PATCH → Partial update, may not be idempotent
│   │   ├── DELETE → Remove resource, idempotent
│   │   ├── HEAD → Get headers only, no body
│   │   └── OPTIONS → Get allowed methods
│   ├── Status Codes
│   │   ├── 2xx Success → 200 OK, 201 Created, 204 No Content
│   │   ├── 3xx Redirection → 301 Moved, 304 Not Modified
│   │   ├── 4xx Client Error → 400 Bad Request, 401 Unauthorized, 404 Not Found
│   │   └── 5xx Server Error → 500 Internal Error, 502 Bad Gateway, 503 Unavailable
│   ├── Headers
│   │   ├── Request → Authorization, Content-Type, Accept, User-Agent
│   │   ├── Response → Content-Type, Cache-Control, Set-Cookie
│   │   └── Security → X-Frame-Options, HSTS, CSP
│   ├── Request/Response Cycle
│   │   ├── Client sends request → Server processes → Server responds
│   │   ├── Stateless protocol → Each request independent
│   │   └── HTTP versions → HTTP/1.1, HTTP/2, HTTP/3 (QUIC)
│   └── HTTP Versions
│       ├── HTTP/1.1 → Persistent connections, pipelining
│       ├── HTTP/2 → Binary protocol, multiplexing, server push
│       └── HTTP/3 → QUIC transport, UDP-based, faster
```

## 🔌 RESTful API Design
```
Web APIs & HTTP Deep Dive
├── 🔌 RESTful API Design
│   ├── REST Principles
│   │   ├── Stateless → No server state between requests
│   │   ├── Client-Server → Independent evolution
│   │   ├── Cacheable → Responses can be cached
│   │   ├── Uniform Interface → Standardized interaction
│   │   ├── Layered System → Client doesn't know about layers
│   │   └── Code on Demand → Server can send executable code
│   ├── Resource Naming
│   │   ├── Nouns not verbs → /users not /getUsers
│   │   ├── Hierarchical structure → /users/{id}/posts/{postId}
│   │   ├── Consistent patterns → /api/v1/resources
│   │   └── Query parameters → Filtering, sorting, pagination
│   ├── HTTP Status Codes Usage
│   │   ├── 200 → Successful GET, PUT, PATCH
│   │   ├── 201 → Resource created successfully
│   │   ├── 204 → Success but no content (DELETE)
│   │   ├── 400 → Bad request, validation errors
│   │   ├── 401 → Authentication required
│   │   ├── 403 → Authorization failed
│   │   ├── 404 → Resource not found
│   │   └── 429 → Rate limit exceeded
│   ├── HATEOAS
│   │   ├── Hypermedia as Engine of Application State
│   │   ├── Self-documenting APIs
│   │   ├── Dynamic discovery of available actions
│   │   └── Loose coupling between client and server
│   └── API Versioning
│       ├── URL Versioning → /api/v1/users, /api/v2/users
│       ├── Header Versioning → Accept: application/vnd.api+json;version=1
│       ├── Content Negotiation → Accept: application/vnd.company.users.v1+json
│       └── Version Management → Deprecation, migration strategies
```

## 🔐 Authentication & Authorization
```
Web APIs & HTTP Deep Dive
├── 🔐 Authentication & Authorization
│   ├── Basic Auth
│   │   ├── Username/password in Authorization header
│   │   ├── Base64 encoding (not encryption)
│   │   ├── Simple but not secure over HTTP
│   │   └── Use with HTTPS only
│   ├── Token-based
│   │   ├── JWT (JSON Web Tokens)
│   │   │   ├── Stateless authentication
│   │   │   ├── Self-contained payload
│   │   │   ├── Digital signature verification
│   │   │   └── Expiration and refresh tokens
│   │   ├── OAuth 2.0
│   │   │   ├── Authorization framework
│   │   │   ├── Multiple grant types
│   │   │   ├── Access and refresh tokens
│   │   │   └── Third-party application access
│   │   └── API Keys
│   │       ├── Simple string-based authentication
│   │       ├── Rate limiting per key
│   │       ├── Key rotation strategies
│   │       └── Security considerations
│   ├── OAuth 2.0 Flows
│   │   ├── Authorization Code → Most secure, server-side apps
│   │   ├── Implicit → Less secure, client-side apps
│   │   ├── Client Credentials → Machine-to-machine
│   │   ├── Resource Owner → Username/password grant
│   │   └── Device Authorization → IoT devices
│   ├── API Security
│   │   ├── Rate Limiting
│   │   │   ├── Token bucket algorithm
│   │   │   ├── Sliding window counter
│   │   │   ├── Per-user and per-IP limits
│   │   │   └── Retry-After headers
│   │   ├── CORS (Cross-Origin Resource Sharing)
│   │   │   ├── Browser security policy
│   │   │   ├── Preflight requests
│   │   │   ├── Allowed origins configuration
│   │   │   └── Credentials handling
│   │   └── CORS Preflight
│   │       ├── OPTIONS request before actual request
│   │       ├── Server declares allowed methods
│   │       ├── Browser enforces restrictions
│   │       └── Performance considerations
│   └── Security Headers
│       ├── CORS → Cross-origin resource sharing
│       ├── CSP → Content Security Policy
│       ├── HSTS → HTTP Strict Transport Security
│       ├── X-Frame-Options → Clickjacking protection
│       └── X-Content-Type-Options → MIME type sniffing prevention
```

## 📡 API Patterns & Best Practices
```
Web APIs & HTTP Deep Dive
├── 📡 API Patterns & Best Practices
│   ├── Pagination
│   │   ├── Offset-based → page=1&limit=20
│   │   ├── Cursor-based → after=cursor123&limit=20
│   │   ├── Page-based → page=1&size=20
│   │   └── Performance considerations → Database optimization
│   ├── Filtering
│   │   ├── Query parameters → ?category=electronics&price_min=100
│   │   ├── Search operators → ?q=name:john+age:>25
│   │   ├── Advanced filtering → ?filter[status]=active&filter[role]=admin
│   │   └── Filter validation → Input sanitization, SQL injection prevention
│   ├── Sorting
│   │   ├── Single field → ?sort=name
│   │   ├── Multiple fields → ?sort=name,age
│   │   ├── Direction specification → ?sort=name:asc,age:desc
│   │   └── Custom sort orders → Business logic implementation
│   ├── Error Handling
│   │   ├── Consistent error responses
│   │   │   ├── Standard error format
│   │   │   ├── Error codes and messages
│   │   │   ├── User-friendly descriptions
│   │   │   └── Developer documentation
│   │   ├── Error codes
│   │   │   ├── HTTP status codes
│   │   │   ├── Application-specific codes
│   │   │   ├── Error categorization
│   │   │   └── Internationalization support
│   │   └── Error documentation
│   │       ├── API documentation
│   │       ├── Error code reference
│   │       ├── Troubleshooting guides
│   │       └── Support contact information
│   └── API Documentation
│       ├── OpenAPI/Swagger
│       │   ├── Machine-readable API specs
│       │   ├── Interactive documentation
│       │   ├── Code generation
│       │   └── Testing and validation
│       ├── Postman collections
│       │   ├── Request templates
│       │   ├── Environment variables
│       │   ├── Test scripts
│       │   └── Team collaboration
│       └── Interactive documentation
│           ├── Try-it-out functionality
│           ├── Request/response examples
│           ├── Authentication testing
│           └── Error simulation
```

## 🚀 Modern Web APIs
```
Web APIs & HTTP Deep Dive
├── 🚀 Modern Web APIs
│   ├── GraphQL
│   │   ├── Query language for APIs
│   │   ├── Schema definition
│   │   ├── Resolvers implementation
│   │   ├── Type system
│   │   ├── Single endpoint
│   │   └── Over-fetching/under-fetching solutions
│   ├── gRPC
│   │   ├── Protocol Buffers
│   │   ├── Bidirectional streaming
│   │   ├── Performance benefits
│   │   ├── Strong typing
│   │   └── Cross-platform support
│   ├── WebSockets
│   │   ├── Real-time communication
│   │   ├── Event-driven architecture
│   │   ├── Connection management
│   │   ├── Bi-directional communication
│   │   └── Use cases: chat, gaming, live updates
│   ├── Server-Sent Events
│   │   ├── One-way real-time updates
│   │   ├── Browser compatibility
│   │   ├── Automatic reconnection
│   │   ├── Event source API
│   │   └── Use cases: notifications, live feeds
│   └── WebRTC
│       ├── Peer-to-peer communication
│       ├── Media streaming
│       ├── Data channels
│       ├── NAT traversal
│       └── Use cases: video calls, file sharing
```

## 🔧 API Testing & Monitoring
```
Web APIs & HTTP Deep Dive
├── 🔧 API Testing & Monitoring
    ├── Testing Tools
    │   ├── Postman → GUI-based API testing
    │   ├── Insomnia → Modern API client
    │   ├── curl → Command-line tool
    │   ├── HTTPie → User-friendly CLI
    │   └── Automated testing → CI/CD integration
    ├── Automated Testing
    │   ├── Jest → JavaScript testing framework
    │   ├── Supertest → HTTP assertion library
    │   ├── Newman → Postman collection runner
    │   ├── CI/CD integration → Automated test execution
    │   └── Test coverage → API endpoint coverage
    ├── API Monitoring
    │   ├── Response times → P50, P95, P99 metrics
    │   ├── Error rates → 4xx, 5xx error tracking
    │   ├── Uptime → Service availability monitoring
    │   ├── Performance metrics → Throughput, latency
    │   └── Business metrics → User engagement, conversion
    ├── Load Testing
    │   ├── Artillery → HTTP load testing
    │   ├── k6 → Modern performance testing
    │   ├── Apache Bench → Simple load testing
    │   ├── Stress testing → Beyond capacity limits
    │   └── Performance baselines → Acceptable thresholds
    └── API Gateway
        ├── Rate limiting → Request throttling
        ├── Authentication → Centralized auth
        ├── Logging → Request/response logging
        ├── Request routing → Service discovery
        ├── Load balancing → Traffic distribution
        └── Security → WAF, DDoS protection
```

## ❓ Common Interview Questions

### 1. HTTP Methods & Status Codes
**Q: Khi nào sử dụng PUT vs PATCH?**
**A:** PUT cho complete resource replacement (idempotent), PATCH cho partial updates (có thể không idempotent). PUT yêu cầu tất cả fields, PATCH chỉ cần fields cần update.

### 2. RESTful API Design
**Q: Các nguyên tắc cơ bản của REST?**
**A:** Stateless, client-server separation, cacheable, uniform interface, layered system, code on demand. Use HTTP methods đúng mục đích, nouns cho resources, proper status codes.

### 3. Authentication Methods
**Q: JWT vs Session-based authentication?**
**A:** JWT stateless, scalable, mobile-friendly, không thể invalidate trước expiration. Sessions server-side storage, better security control, immediate invalidation, scaling challenges.

### 4. API Performance
**Q: Cách optimize API performance?**
**A:** Caching (Redis, HTTP), database optimization (indexing, connection pooling), pagination, compression, CDN, load balancing, monitoring và profiling.

### 5. Security Best Practices
**Q: Security measures cho Web APIs?**
**A:** HTTPS, input validation, rate limiting, CORS, authentication, authorization, security headers, API versioning, regular security audits, dependency scanning. 