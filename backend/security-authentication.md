# 🔒 Security & Authentication - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Security & Authentication
├── 🔐 Authentication Methods
│   ├── JWT → Token-based authentication
│   ├── OAuth 2.0 → Authorization framework
│   ├── SAML → Enterprise SSO
│   └── Multi-Factor → 2FA, biometrics
├── 🛡️ Security Principles
│   ├── CIA Triad → Confidentiality, Integrity, Availability
│   ├── Defense in Depth → Multiple security layers
│   ├── Principle of Least Privilege → Minimal access rights
│   └── Zero Trust → Never trust, always verify
├── 🔒 Encryption & Hashing
│   ├── Symmetric → AES, DES, same key
│   ├── Asymmetric → RSA, ECC, public/private keys
│   ├── Hashing → SHA, MD5, bcrypt
│   └── Digital Signatures → Message authenticity
├── 🌐 Web Security
│   ├── OWASP Top 10 → Common vulnerabilities
│   ├── XSS Protection → Cross-site scripting
│   ├── CSRF Prevention → Cross-site request forgery
│   └── SQL Injection → Database security
└── 🚀 Implementation
    ├── Best Practices → Security guidelines
    ├── Tools & Libraries → Security frameworks
    ├── Monitoring → Security monitoring
    └── Incident Response → Security incidents
```

## 📋 Table of Contents
- [Authentication Methods](#authentication-methods)
- [Security Principles](#security-principles)
- [Encryption & Hashing](#encryption--hashing)
- [Web Security](#web-security)
- [Implementation](#implementation)
- [Common Interview Questions](#common-interview-questions)

## 🔐 Authentication

### JWT (JSON Web Tokens)
```javascript
// JWT Implementation
const jwt = require('jsonwebtoken');

class JWTService {
  constructor(secretKey) {
    this.secretKey = secretKey;
  }

  // Generate JWT token
  generateToken(payload, options = {}) {
    const defaultOptions = {
      expiresIn: '24h',
      issuer: 'myapp',
      audience: 'myapp-users'
    };

    return jwt.sign(payload, this.secretKey, { ...defaultOptions, ...options });
  }

  // Verify JWT token
  verifyToken(token) {
    try {
      return jwt.verify(token, this.secretKey);
    } catch (error) {
      throw new Error('Invalid token');
    }
  }
}

// Authentication middleware
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN

  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }

  try {
    const jwtService = new JWTService(process.env.JWT_SECRET);
    const user = jwtService.verifyToken(token);
    req.user = user;
    next();
  } catch (error) {
    return res.status(403).json({ error: 'Invalid token' });
  }
};
```

### OAuth 2.0
```javascript
// OAuth 2.0 Implementation
class OAuth2Service {
  constructor() {
    this.clients = new Map();
    this.authorizationCodes = new Map();
    this.accessTokens = new Map();
  }

  // Register client
  registerClient(clientId, clientSecret, redirectUri) {
    this.clients.set(clientId, {
      clientSecret,
      redirectUri,
      scope: ['read', 'write']
    });
  }

  // Authorization endpoint
  authorize(req, res) {
    const { client_id, redirect_uri, scope, state, response_type } = req.query;

    // Validate client
    const client = this.clients.get(client_id);
    if (!client || client.redirectUri !== redirect_uri) {
      return res.status(400).json({ error: 'invalid_client' });
    }

    // Generate authorization code
    const authCode = this.generateAuthCode(client_id, scope);
    
    // Redirect to client with authorization code
    const redirectUrl = `${redirect_uri}?code=${authCode}&state=${state}`;
    res.redirect(redirectUrl);
  }

  // Token endpoint
  token(req, res) {
    const { grant_type, code, client_id, client_secret, redirect_uri } = req.body;

    if (grant_type === 'authorization_code') {
      // Validate authorization code
      const authData = this.authorizationCodes.get(code);
      if (!authData) {
        return res.status(400).json({ error: 'invalid_grant' });
      }

      // Validate client
      const client = this.clients.get(client_id);
      if (!client || client.clientSecret !== client_secret) {
        return res.status(400).json({ error: 'invalid_client' });
      }

      // Generate access token
      const accessToken = this.generateAccessToken(authData.userId, authData.scope);
      
      // Clean up authorization code
      this.authorizationCodes.delete(code);

      res.json({
        access_token: accessToken,
        token_type: 'Bearer',
        expires_in: 3600,
        scope: authData.scope.join(' ')
      });
    }
  }

  // Generate authorization code
  generateAuthCode(clientId, scope) {
    const code = crypto.randomBytes(32).toString('hex');
    this.authorizationCodes.set(code, {
      clientId,
      scope: scope.split(' '),
      userId: 'user123', // In real app, get from session
      expiresAt: Date.now() + 600000 // 10 minutes
    });
    return code;
  }

  // Generate access token
  generateAccessToken(userId, scope) {
    const token = crypto.randomBytes(32).toString('hex');
    this.accessTokens.set(token, {
      userId,
      scope,
      expiresAt: Date.now() + 3600000 // 1 hour
    });
    return token;
  }
}
```

### Session-based Authentication
```javascript
// Session-based authentication
const session = require('express-session');

// Session configuration
const sessionConfig = {
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: process.env.NODE_ENV === 'production',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  }
};

// Session middleware
app.use(session(sessionConfig));

// Login with session
const loginWithSession = async (req, res) => {
  const { email, password } = req.body;

  try {
    // Verify user credentials
    const user = await User.findOne({ email });
    if (!user || !await bcrypt.compare(password, user.password)) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    // Store user in session
    req.session.userId = user.id;
    req.session.userRole = user.role;
    req.session.isAuthenticated = true;

    res.json({
      message: 'Login successful',
      user: {
        id: user.id,
        email: user.email,
        role: user.role
      }
    });
  } catch (error) {
    res.status(500).json({ error: 'Login failed' });
  }
};

// Session authentication middleware
const requireAuth = (req, res, next) => {
  if (!req.session.isAuthenticated) {
    return res.status(401).json({ error: 'Authentication required' });
  }
  next();
};
```

## 🛡️ Authorization

### Role-Based Access Control (RBAC)
```javascript
// RBAC Implementation
class RBACService {
  constructor() {
    this.roles = new Map();
    this.userRoles = new Map();
  }

  // Define roles and permissions
  defineRole(roleName, permissions) {
    this.roles.set(roleName, permissions);
  }

  // Assign role to user
  assignRole(userId, roleName) {
    if (!this.roles.has(roleName)) {
      throw new Error(`Role ${roleName} does not exist`);
    }
    
    if (!this.userRoles.has(userId)) {
      this.userRoles.set(userId, []);
    }
    
    this.userRoles.get(userId).push(roleName);
  }

  // Check if user has permission
  hasPermission(userId, permission) {
    const userRoles = this.userRoles.get(userId) || [];
    
    for (const role of userRoles) {
      const rolePermissions = this.roles.get(role) || [];
      if (rolePermissions.includes(permission)) {
        return true;
      }
    }
    
    return false;
  }
}

// Initialize RBAC
const rbac = new RBACService();

// Define roles
rbac.defineRole('admin', [
  'user:read',
  'user:write',
  'user:delete',
  'admin:read',
  'admin:write'
]);

rbac.defineRole('moderator', [
  'user:read',
  'user:write',
  'content:moderate'
]);

rbac.defineRole('user', [
  'user:read',
  'content:read',
  'content:write'
]);

// RBAC middleware
const requirePermission = (permission) => {
  return (req, res, next) => {
    if (!rbac.hasPermission(req.user.id, permission)) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
};

// Usage examples
app.get('/users', requirePermission('user:read'), (req, res) => {
  // Get users
});

app.post('/users', requirePermission('user:write'), (req, res) => {
  // Create user
});

app.delete('/users/:id', requirePermission('user:delete'), (req, res) => {
  // Delete user
});
```

## 🔒 Security Headers

### CORS Configuration
```javascript
// CORS middleware
const cors = require('cors');

const corsOptions = {
  origin: function (origin, callback) {
    const allowedOrigins = [
      'https://myapp.com',
      'https://admin.myapp.com',
      'http://localhost:3000'
    ];
    
    // Allow requests with no origin (mobile apps, etc.)
    if (!origin) return callback(null, true);
    
    if (allowedOrigins.indexOf(origin) !== -1) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Requested-With'],
  exposedHeaders: ['X-Total-Count'],
  maxAge: 86400 // 24 hours
};

app.use(cors(corsOptions));
```

### Security Headers Middleware
```javascript
// Security headers middleware
const helmet = require('helmet');

// Basic security headers
app.use(helmet());

// Custom security headers
app.use((req, res, next) => {
  // Content Security Policy
  res.setHeader('Content-Security-Policy', [
    "default-src 'self'",
    "script-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net",
    "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
    "font-src 'self' https://fonts.gstatic.com",
    "img-src 'self' data: https:",
    "connect-src 'self' https://api.myapp.com",
    "frame-ancestors 'none'"
  ].join('; '));

  // HTTP Strict Transport Security
  res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains; preload');

  // X-Frame-Options
  res.setHeader('X-Frame-Options', 'DENY');

  // X-Content-Type-Options
  res.setHeader('X-Content-Type-Options', 'nosniff');

  // X-XSS-Protection
  res.setHeader('X-XSS-Protection', '1; mode=block');

  // Referrer Policy
  res.setHeader('Referrer-Policy', 'strict-origin-when-cross-origin');

  next();
});
```

## 🚨 Security Threats

### SQL Injection Prevention
```javascript
// SQL Injection prevention with parameterized queries
const mysql = require('mysql2/promise');

class UserService {
  constructor() {
    this.pool = mysql.createPool({
      host: process.env.DB_HOST,
      user: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_NAME
    });
  }

  // Safe query with parameters
  async getUserById(id) {
    const [rows] = await this.pool.execute(
      'SELECT * FROM users WHERE id = ?',
      [id]
    );
    return rows[0];
  }

  // Safe query with multiple parameters
  async createUser(userData) {
    const { name, email, password } = userData;
    const hashedPassword = await bcrypt.hash(password, 10);
    
    const [result] = await this.pool.execute(
      'INSERT INTO users (name, email, password) VALUES (?, ?, ?)',
      [name, email, hashedPassword]
    );
    
    return result.insertId;
  }
}

// Input validation
const validateUserInput = (userData) => {
  const errors = [];
  
  if (!userData.name || userData.name.length < 2) {
    errors.push('Name must be at least 2 characters long');
  }
  
  if (!userData.email || !isValidEmail(userData.email)) {
    errors.push('Valid email is required');
  }
  
  if (!userData.password || userData.password.length < 8) {
    errors.push('Password must be at least 8 characters long');
  }
  
  return errors;
};

const isValidEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};
```

### XSS Prevention
```javascript
// XSS prevention
const xss = require('xss');

// Sanitize user input
const sanitizeInput = (input) => {
  if (typeof input === 'string') {
    return xss(input, {
      whiteList: {}, // No tags allowed
      stripIgnoreTag: true,
      stripIgnoreTagBody: ['script']
    });
  }
  return input;
};

// Sanitize object recursively
const sanitizeObject = (obj) => {
  if (typeof obj !== 'object' || obj === null) {
    return sanitizeInput(obj);
  }
  
  if (Array.isArray(obj)) {
    return obj.map(sanitizeObject);
  }
  
  const sanitized = {};
  for (const [key, value] of Object.entries(obj)) {
    sanitized[key] = sanitizeObject(value);
  }
  
  return sanitized;
};

// XSS prevention middleware
const preventXSS = (req, res, next) => {
  if (req.body) {
    req.body = sanitizeObject(req.body);
  }
  
  if (req.query) {
    req.query = sanitizeObject(req.query);
  }
  
  if (req.params) {
    req.params = sanitizeObject(req.params);
  }
  
  next();
};
```

### CSRF Prevention
```javascript
// CSRF prevention
const csrf = require('csurf');

// CSRF protection middleware
const csrfProtection = csrf({
  cookie: {
    httpOnly: true,
    secure: process.env.NODE_ENV === 'production',
    sameSite: 'strict'
  }
});

// Generate CSRF token
app.get('/csrf-token', csrfProtection, (req, res) => {
  res.json({ csrfToken: req.csrfToken() });
});

// Protected routes
app.post('/api/users', csrfProtection, (req, res) => {
  // Handle user creation
});

// CSRF error handler
app.use((err, req, res, next) => {
  if (err.code === 'EBADCSRFTOKEN') {
    return res.status(403).json({
      error: 'CSRF token validation failed',
      message: 'Form submission failed. Please try again.'
    });
  }
  next(err);
});
```

### Rate Limiting
```javascript
// Rate limiting
const rateLimit = require('express-rate-limit');

// General rate limiter
const generalLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: {
    error: 'Too many requests from this IP, please try again later.'
  },
  standardHeaders: true,
  legacyHeaders: false
});

// Login rate limiter
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 5 login attempts per windowMs
  message: {
    error: 'Too many login attempts, please try again later.'
  },
  skipSuccessfulRequests: true
});

// API rate limiter
const apiLimiter = rateLimit({
  windowMs: 60 * 1000, // 1 minute
  max: 60, // limit each IP to 60 requests per minute
  message: {
    error: 'API rate limit exceeded, please try again later.'
  }
});

// Apply rate limiters
app.use('/api/', apiLimiter);
app.use('/auth/login', loginLimiter);
app.use(generalLimiter);
```

## ❓ Common Interview Questions

### 1. JWT vs Session-based Authentication
**Q: Khi nào nên dùng JWT và khi nào dùng Session-based authentication?**

**A:**
```javascript
const authenticationComparison = {
  // JWT Advantages
  jwt: {
    pros: [
      'Stateless - no server-side storage',
      'Scalable across multiple servers',
      'Good for microservices',
      'Mobile-friendly',
      'No database queries for validation'
    ],
    cons: [
      'Cannot be invalidated before expiration',
      'Larger token size',
      'Security concerns with client-side storage',
      'No built-in logout mechanism'
    ],
    useCases: [
      'Microservices architecture',
      'Mobile applications',
      'Single Page Applications (SPA)',
      'Third-party integrations'
    ]
  },

  // Session-based Advantages
  session: {
    pros: [
      'Can be invalidated immediately',
      'Smaller payload',
      'Better security control',
      'Built-in logout mechanism',
      'Can store complex user data'
    ],
    cons: [
      'Requires server-side storage',
      'Scaling challenges',
      'Database queries for validation',
      'Session fixation attacks'
    ],
    useCases: [
      'Traditional web applications',
      'High-security applications',
      'Applications requiring immediate logout',
      'Complex user state management'
    ]
  }
};
```

### 2. OAuth 2.0 Flow Types
**Q: Các loại OAuth 2.0 flows và khi nào sử dụng?**

**A:**
```javascript
const oauthFlows = {
  // Authorization Code Flow
  authorizationCode: {
    description: 'Most secure flow for web applications',
    steps: [
      'User visits authorization server',
      'User grants permission',
      'Authorization server returns code',
      'Client exchanges code for token',
      'Client uses token for API calls'
    ],
    useCases: [
      'Web applications with backend',
      'Mobile applications with backend',
      'High-security requirements'
    ]
  },

  // Implicit Flow
  implicit: {
    description: 'Simpler flow for client-side applications',
    steps: [
      'User visits authorization server',
      'User grants permission',
      'Authorization server returns token directly',
      'Client uses token for API calls'
    ],
    useCases: [
      'Single Page Applications (SPA)',
      'Mobile applications without backend',
      'Simple integrations'
    ],
    security: 'Less secure - token exposed in URL'
  },

  // Client Credentials Flow
  clientCredentials: {
    description: 'For server-to-server communication',
    steps: [
      'Client authenticates with credentials',
      'Authorization server returns token',
      'Client uses token for API calls'
    ],
    useCases: [
      'Server-to-server APIs',
      'Background jobs',
      'Service integrations'
    ]
  }
};
```

### 3. Security Best Practices
**Q: Các security best practices cho web applications?**

**A:**
```javascript
const securityBestPractices = {
  // Password Security
  passwordSecurity: {
    hashing: 'Use bcrypt, Argon2, or PBKDF2 for password hashing',
    salting: 'Always use unique salts for each password',
    complexity: 'Enforce strong password requirements',
    storage: 'Never store plain text passwords',
    rotation: 'Implement password expiration policies'
  },

  // HTTPS
  https: {
    certificates: 'Use valid SSL/TLS certificates',
    redirects: 'Redirect all HTTP traffic to HTTPS',
    hsts: 'Implement HTTP Strict Transport Security',
    mixedContent: 'Prevent mixed content warnings'
  },

  // Input Validation
  inputValidation: {
    sanitization: 'Sanitize all user inputs',
    validation: 'Validate data on both client and server',
    encoding: 'Use proper output encoding',
    whitelist: 'Use whitelist approach for allowed inputs'
  },

  // Authentication
  authentication: {
    mfa: 'Implement Multi-Factor Authentication',
    sessionManagement: 'Secure session handling',
    tokenExpiration: 'Set appropriate token expiration times',
    logout: 'Implement secure logout mechanisms'
  },

  // Authorization
  authorization: {
    principleOfLeastPrivilege: 'Grant minimum required permissions',
    rbac: 'Implement Role-Based Access Control',
    abac: 'Use Attribute-Based Access Control for complex scenarios',
    audit: 'Log all authorization decisions'
  }
};
```

### 4. Common Security Vulnerabilities
**Q: Cách prevent các security vulnerabilities phổ biến?**

**A:**
```javascript
const securityVulnerabilities = {
  // SQL Injection
  sqlInjection: {
    description: 'Malicious SQL code injection',
    prevention: [
      'Use parameterized queries',
      'Input validation and sanitization',
      'Use ORM libraries',
      'Principle of least privilege for database users',
      'Regular security audits'
    ],
    example: {
      vulnerable: "SELECT * FROM users WHERE id = '" + userId + "'",
      secure: "SELECT * FROM users WHERE id = ?"
    }
  },

  // Cross-Site Scripting (XSS)
  xss: {
    description: 'Malicious script injection',
    prevention: [
      'Input sanitization',
      'Output encoding',
      'Content Security Policy (CSP)',
      'HttpOnly cookies',
      'Input validation'
    ],
    types: [
      'Reflected XSS',
      'Stored XSS',
      'DOM-based XSS'
    ]
  },

  // Cross-Site Request Forgery (CSRF)
  csrf: {
    description: 'Unauthorized actions on behalf of user',
    prevention: [
      'CSRF tokens',
      'SameSite cookie attribute',
      'Double Submit Cookie pattern',
      'Custom request headers',
      'Referrer validation'
    ]
  },

  // Authentication Bypass
  authenticationBypass: {
    description: 'Unauthorized access to protected resources',
    prevention: [
      'Strong authentication mechanisms',
      'Multi-factor authentication',
      'Session management',
      'Token validation',
      'Regular security testing'
    ]
  }
};
```

### 5. Security Headers
**Q: Các security headers quan trọng và mục đích?**

**A:**
```javascript
const securityHeaders = {
  // Content Security Policy (CSP)
  contentSecurityPolicy: {
    purpose: 'Prevent XSS attacks by controlling resource loading',
    example: "Content-Security-Policy: default-src 'self'; script-src 'self'",
    benefits: [
      'Prevents inline script execution',
      'Controls external resource loading',
      'Reduces XSS attack surface'
    ]
  },

  // HTTP Strict Transport Security (HSTS)
  hsts: {
    purpose: 'Force HTTPS connections',
    example: "Strict-Transport-Security: max-age=31536000; includeSubDomains",
    benefits: [
      'Prevents protocol downgrade attacks',
      'Protects against man-in-the-middle attacks',
      'Ensures secure communication'
    ]
  },

  // X-Frame-Options
  xFrameOptions: {
    purpose: 'Prevent clickjacking attacks',
    values: [
      'DENY - No embedding allowed',
      'SAMEORIGIN - Only same origin embedding',
      'ALLOW-FROM uri - Specific origin embedding'
    ]
  },

  // X-Content-Type-Options
  xContentTypeOptions: {
    purpose: 'Prevent MIME type sniffing',
    value: 'nosniff',
    benefits: [
      'Prevents MIME confusion attacks',
      'Forces proper content type handling'
    ]
  },

  // X-XSS-Protection
  xXSSProtection: {
    purpose: 'Enable browser XSS filtering',
    values: [
      '0 - Disable filtering',
      '1 - Enable filtering',
      '1; mode=block - Enable filtering and block'
    ]
  }
};
```

## 📚 Resources
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [JWT.io](https://jwt.io/)
- [OAuth 2.0 Specification](https://tools.ietf.org/html/rfc6749)
- [Helmet.js](https://helmetjs.github.io/)
- [Security Headers](https://securityheaders.com/) 