# 🟨 JavaScript Patterns - Complete Knowledge Map

## 🧠 Mindmap Overview
```
JavaScript Patterns
├── 🏗️ Design Patterns
│   ├── Creational → Factory, Singleton, Builder
│   ├── Structural → Adapter, Decorator, Proxy
│   ├── Behavioral → Observer, Strategy, Command
│   └── Module Pattern → IIFE, ES6 modules
├── 🔄 Functional Programming
│   ├── Pure Functions → No side effects
│   ├── Higher-Order Functions → Functions as parameters
│   ├── Composition → Function composition
│   └── Immutability → Immutable data structures
├── ⚡ Performance Patterns
│   ├── Debouncing → Limit function calls
│   ├── Throttling → Rate limiting
│   ├── Memoization → Cache function results
│   └── Lazy Loading → Load resources on demand
├── 🎯 Async Patterns
│   ├── Promises → Promise chaining, Promise.all
│   ├── Async/Await → Clean async code
│   ├── Event Loop → Microtasks vs macrotasks
│   └── Generators → Iterator functions
└── 🛡️ Error Handling
    ├── Try-Catch → Exception handling
    ├── Error Boundaries → Graceful degradation
    ├── Custom Errors → Error inheritance
    └── Logging → Error tracking, monitoring
```

## 📋 Table of Contents
- [Design Patterns](#design-patterns)
- [Functional Programming](#functional-programming)
- [Performance Patterns](#performance-patterns)
- [Async Patterns](#async-patterns)
- [Error Handling](#error-handling)
- [Common Interview Questions](#common-interview-questions)

## 🏗️ Design Patterns

### Creational Patterns
Patterns để tạo objects:

```javascript
// Factory Pattern
class UserFactory {
  static createUser(type, data) {
    switch (type) {
      case 'admin':
        return new AdminUser(data)
      case 'regular':
        return new RegularUser(data)
      case 'guest':
        return new GuestUser(data)
      default:
        throw new Error('Invalid user type')
    }
  }
}

// Singleton Pattern
class DatabaseConnection {
  constructor() {
    if (DatabaseConnection.instance) {
      return DatabaseConnection.instance
    }
    
    this.connection = this.createConnection()
    DatabaseConnection.instance = this
    return this
  }
  
  createConnection() {
    // Database connection logic
    return { connected: true }
  }
  
  query(sql) {
    console.log(`Executing: ${sql}`)
    return { result: 'data' }
  }
}

// Builder Pattern
class UserBuilder {
  constructor() {
    this.user = {}
  }
  
  setName(name) {
    this.user.name = name
    return this
  }
  
  setEmail(email) {
    this.user.email = email
    return this
  }
  
  setAge(age) {
    this.user.age = age
    return this
  }
  
  build() {
    return this.user
  }
}

const user = new UserBuilder()
  .setName('John Doe')
  .setEmail('john@example.com')
  .setAge(30)
  .build()
```

### Structural Patterns
Patterns để compose objects:

```javascript
// Adapter Pattern
class OldAPI {
  request() {
    return 'old data format'
  }
}

class NewAPI {
  fetch() {
    return { data: 'new data format' }
  }
}

class APIAdapter {
  constructor(newAPI) {
    this.newAPI = newAPI
  }
  
  request() {
    const result = this.newAPI.fetch()
    return result.data // Convert to old format
  }
}

// Decorator Pattern
class Coffee {
  cost() {
    return 10
  }
  
  description() {
    return 'Simple coffee'
  }
}

class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee
  }
  
  cost() {
    return this.coffee.cost() + 2
  }
  
  description() {
    return this.coffee.description() + ', milk'
  }
}

class SugarDecorator {
  constructor(coffee) {
    this.coffee = coffee
  }
  
  cost() {
    return this.coffee.cost() + 1
  }
  
  description() {
    return this.coffee.description() + ', sugar'
  }
}

let coffee = new Coffee()
coffee = new MilkDecorator(coffee)
coffee = new SugarDecorator(coffee)

console.log(coffee.cost()) // 13
console.log(coffee.description()) // Simple coffee, milk, sugar

// Proxy Pattern
class UserService {
  getUsers() {
    return fetch('/api/users').then(res => res.json())
  }
}

class UserServiceProxy {
  constructor(userService) {
    this.userService = userService
    this.cache = new Map()
  }
  
  async getUsers() {
    if (this.cache.has('users')) {
      console.log('Returning cached users')
      return this.cache.get('users')
    }
    
    const users = await this.userService.getUsers()
    this.cache.set('users', users)
    return users
  }
}
```

### Behavioral Patterns
Patterns để handle communication between objects:

```javascript
// Observer Pattern
class EventEmitter {
  constructor() {
    this.events = {}
  }
  
  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = []
    }
    this.events[event].push(callback)
  }
  
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data))
    }
  }
  
  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback)
    }
  }
}

// Strategy Pattern
class PaymentStrategy {
  pay(amount) {
    throw new Error('pay method must be implemented')
  }
}

class CreditCardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid $${amount} using Credit Card`)
  }
}

class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid $${amount} using PayPal`)
  }
}

class CryptoPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid $${amount} using Cryptocurrency`)
  }
}

class ShoppingCart {
  constructor() {
    this.paymentStrategy = null
  }
  
  setPaymentStrategy(strategy) {
    this.paymentStrategy = strategy
  }
  
  checkout(amount) {
    if (!this.paymentStrategy) {
      throw new Error('Payment strategy not set')
    }
    this.paymentStrategy.pay(amount)
  }
}

// Command Pattern
class Command {
  execute() {
    throw new Error('execute method must be implemented')
  }
  
  undo() {
    throw new Error('undo method must be implemented')
  }
}

class AddCommand extends Command {
  constructor(calculator, value) {
    super()
    this.calculator = calculator
    this.value = value
  }
  
  execute() {
    this.calculator.add(this.value)
  }
  
  undo() {
    this.calculator.subtract(this.value)
  }
}

class Calculator {
  constructor() {
    this.value = 0
    this.history = []
  }
  
  add(value) {
    this.value += value
  }
  
  subtract(value) {
    this.value -= value
  }
  
  executeCommand(command) {
    command.execute()
    this.history.push(command)
  }
  
  undo() {
    if (this.history.length > 0) {
      const command = this.history.pop()
      command.undo()
    }
  }
  
  getValue() {
    return this.value
  }
}
```

## 🔄 Functional Programming

### Pure Functions
Functions không có side effects:

```javascript
// Pure function
function add(a, b) {
  return a + b
}

// Impure function (has side effect)
let total = 0
function addToTotal(value) {
  total += value // Side effect: modifies external state
  return total
}

// Pure function with object
function updateUser(user, updates) {
  return { ...user, ...updates } // Returns new object, doesn't modify original
}

// Impure function with object
function updateUserImpure(user, updates) {
  Object.assign(user, updates) // Modifies original object
  return user
}
```

### Higher-Order Functions
Functions nhận functions làm parameters:

```javascript
// Higher-order function
function withLogging(fn) {
  return function(...args) {
    console.log(`Calling function with args:`, args)
    const result = fn(...args)
    console.log(`Function returned:`, result)
    return result
  }
}

// Usage
const addWithLogging = withLogging((a, b) => a + b)
addWithLogging(2, 3) // Logs the call and result

// Another example
function retry(fn, maxAttempts = 3) {
  return async function(...args) {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
      try {
        return await fn(...args)
      } catch (error) {
        if (attempt === maxAttempts) {
          throw error
        }
        console.log(`Attempt ${attempt} failed, retrying...`)
        await new Promise(resolve => setTimeout(resolve, 1000 * attempt))
      }
    }
  }
}

// Usage
const fetchWithRetry = retry(fetch, 3)
```

### Function Composition
Kết hợp multiple functions:

```javascript
// Function composition
function compose(...fns) {
  return function(x) {
    return fns.reduceRight((acc, fn) => fn(acc), x)
  }
}

function pipe(...fns) {
  return function(x) {
    return fns.reduce((acc, fn) => fn(acc), x)
  }
}

// Example functions
const addOne = x => x + 1
const double = x => x * 2
const square = x => x ** 2

// Compose: f(g(h(x))) - right to left
const composed = compose(square, double, addOne)
console.log(composed(3)) // ((3 + 1) * 2)² = 64

// Pipe: h(g(f(x))) - left to right
const piped = pipe(addOne, double, square)
console.log(piped(3)) // ((3 + 1) * 2)² = 64

// Practical example
const users = [
  { name: 'John', age: 25 },
  { name: 'Jane', age: 30 },
  { name: 'Bob', age: 35 }
]

const filterAdults = users => users.filter(user => user.age >= 18)
const sortByName = users => users.sort((a, b) => a.name.localeCompare(b.name))
const mapNames = users => users.map(user => user.name)

const processUsers = pipe(filterAdults, sortByName, mapNames)
console.log(processUsers(users)) // ['Bob', 'Jane', 'John']
```

## ⚡ Performance Patterns

### Debouncing & Throttling
Kiểm soát tần suất function calls:

```javascript
// Debouncing
function debounce(func, delay) {
  let timeoutId
  
  return function(...args) {
    clearTimeout(timeoutId)
    
    timeoutId = setTimeout(() => {
      func.apply(this, args)
    }, delay)
  }
}

// Throttling
function throttle(func, limit) {
  let inThrottle
  
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args)
      inThrottle = true
      
      setTimeout(() => {
        inThrottle = false
      }, limit)
    }
  }
}

// Usage examples
const debouncedSearch = debounce((query) => {
  console.log('Searching for:', query)
  // API call here
}, 300)

const throttledScroll = throttle(() => {
  console.log('Scroll event handled')
  // Handle scroll
}, 100)

// Input search
searchInput.addEventListener('input', (e) => {
  debouncedSearch(e.target.value)
})

// Scroll events
window.addEventListener('scroll', throttledScroll)
```

### Memoization
Cache function results:

```javascript
// Simple memoization
function memoize(fn) {
  const cache = new Map()
  
  return function(...args) {
    const key = JSON.stringify(args)
    
    if (cache.has(key)) {
      console.log('Returning cached result')
      return cache.get(key)
    }
    
    const result = fn.apply(this, args)
    cache.set(key, result)
    return result
  }
}

// Fibonacci with memoization
const fibonacci = memoize(function(n) {
  if (n <= 1) return n
  return fibonacci(n - 1) + fibonacci(n - 2)
})

console.log(fibonacci(10)) // Calculates
console.log(fibonacci(10)) // Returns cached result

// Advanced memoization with cache size limit
function memoizeWithLimit(fn, maxSize = 100) {
  const cache = new Map()
  
  return function(...args) {
    const key = JSON.stringify(args)
    
    if (cache.has(key)) {
      // Move to end (most recently used)
      const value = cache.get(key)
      cache.delete(key)
      cache.set(key, value)
      return value
    }
    
    const result = fn.apply(this, args)
    
    // Remove oldest entry if cache is full
    if (cache.size >= maxSize) {
      const firstKey = cache.keys().next().value
      cache.delete(firstKey)
    }
    
    cache.set(key, result)
    return result
  }
}
```

### Lazy Loading
Load resources khi cần thiết:

```javascript
// Lazy loading images
function lazyLoadImages() {
  const images = document.querySelectorAll('img[data-src]')
  
  const imageObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const img = entry.target
        img.src = img.dataset.src
        img.classList.remove('lazy')
        observer.unobserve(img)
      }
    })
  })
  
  images.forEach(img => imageObserver.observe(img))
}

// Lazy loading modules
class LazyLoader {
  constructor() {
    this.cache = new Map()
  }
  
  async load(moduleName) {
    if (this.cache.has(moduleName)) {
      return this.cache.get(moduleName)
    }
    
    const module = await import(`./modules/${moduleName}.js`)
    this.cache.set(moduleName, module)
    return module
  }
  
  preload(moduleNames) {
    moduleNames.forEach(name => {
      this.load(name)
    })
  }
}

// Usage
const loader = new LazyLoader()

// Load when needed
button.addEventListener('click', async () => {
  const { default: module } = await loader.load('heavyModule')
  module.doSomething()
})

// Preload important modules
loader.preload(['userModule', 'authModule'])
```

## 🎯 Async Patterns

### Promise Patterns
Advanced Promise usage:

```javascript
// Promise.all with error handling
async function fetchAllWithErrors(urls) {
  const promises = urls.map(url => 
    fetch(url).catch(error => ({ error, url }))
  )
  
  const results = await Promise.all(promises)
  
  const successful = results.filter(result => !result.error)
  const failed = results.filter(result => result.error)
  
  return { successful, failed }
}

// Promise.race with timeout
function fetchWithTimeout(url, timeout = 5000) {
  const fetchPromise = fetch(url)
  const timeoutPromise = new Promise((_, reject) => {
    setTimeout(() => reject(new Error('Request timeout')), timeout)
  })
  
  return Promise.race([fetchPromise, timeoutPromise])
}

// Sequential execution
async function executeSequentially(tasks) {
  const results = []
  
  for (const task of tasks) {
    try {
      const result = await task()
      results.push({ success: true, result })
    } catch (error) {
      results.push({ success: false, error: error.message })
    }
  }
  
  return results
}

// Usage
const tasks = [
  () => fetch('/api/users'),
  () => fetch('/api/posts'),
  () => fetch('/api/comments')
]

executeSequentially(tasks).then(results => {
  console.log('All tasks completed:', results)
})
```

### Generator Functions
Iterator functions:

```javascript
// Basic generator
function* numberGenerator() {
  yield 1
  yield 2
  yield 3
}

const generator = numberGenerator()
console.log(generator.next().value) // 1
console.log(generator.next().value) // 2
console.log(generator.next().value) // 3

// Infinite generator
function* infiniteCounter() {
  let i = 0
  while (true) {
    yield i++
  }
}

const counter = infiniteCounter()
console.log(counter.next().value) // 0
console.log(counter.next().value) // 1
console.log(counter.next().value) // 2

// Generator for async operations
function* asyncGenerator() {
  try {
    const user = yield fetch('/api/user/1')
    const posts = yield fetch(`/api/users/${user.id}/posts`)
    const comments = yield fetch(`/api/posts/${posts[0].id}/comments`)
    
    return { user, posts, comments }
  } catch (error) {
    console.error('Error:', error)
  }
}

// Runner for async generator
function runAsyncGenerator(generator) {
  return new Promise((resolve, reject) => {
    function run(value) {
      const result = generator.next(value)
      
      if (result.done) {
        resolve(result.value)
        return
      }
      
      Promise.resolve(result.value)
        .then(run)
        .catch(reject)
    }
    
    run()
  })
}

// Usage
runAsyncGenerator(asyncGenerator()).then(result => {
  console.log('All data fetched:', result)
})
```

## 🛡️ Error Handling

### Custom Error Classes
Inherit từ Error class:

```javascript
// Base error class
class AppError extends Error {
  constructor(message, statusCode = 500, isOperational = true) {
    super(message)
    this.name = this.constructor.name
    this.statusCode = statusCode
    this.isOperational = isOperational
    
    Error.captureStackTrace(this, this.constructor)
  }
}

// Specific error types
class ValidationError extends AppError {
  constructor(message, field) {
    super(message, 400)
    this.field = field
  }
}

class AuthenticationError extends AppError {
  constructor(message = 'Authentication failed') {
    super(message, 401)
  }
}

class NotFoundError extends AppError {
  constructor(resource = 'Resource') {
    super(`${resource} not found`, 404)
  }
}

// Error handler
function handleError(error, req, res, next) {
  if (error instanceof AppError) {
    return res.status(error.statusCode).json({
      error: {
        message: error.message,
        statusCode: error.statusCode,
        field: error.field
      }
    })
  }
  
  // Log unexpected errors
  console.error('Unexpected error:', error)
  
  return res.status(500).json({
    error: {
      message: 'Internal server error',
      statusCode: 500
    }
  })
}

// Usage
function validateUser(userData) {
  if (!userData.email) {
    throw new ValidationError('Email is required', 'email')
  }
  
  if (!userData.name) {
    throw new ValidationError('Name is required', 'name')
  }
  
  return userData
}
```

### Error Boundaries
Graceful error handling:

```javascript
// Error boundary class
class ErrorBoundary {
  constructor() {
    this.errors = []
  }
  
  wrap(fn) {
    return async function(...args) {
      try {
        return await fn.apply(this, args)
      } catch (error) {
        this.errors.push({
          error: error.message,
          timestamp: new Date(),
          stack: error.stack
        })
        
        // Log error
        console.error('Error caught by boundary:', error)
        
        // Return fallback or re-throw
        throw error
      }
    }
  }
  
  getErrors() {
    return this.errors
  }
  
  clearErrors() {
    this.errors = []
  }
}

// Usage
const boundary = new ErrorBoundary()

const safeFunction = boundary.wrap(async function() {
  // This function is now wrapped with error handling
  const response = await fetch('/api/data')
  return response.json()
})

// Global error handler
window.addEventListener('error', (event) => {
  console.error('Global error:', event.error)
  // Send to error tracking service
})

window.addEventListener('unhandledrejection', (event) => {
  console.error('Unhandled promise rejection:', event.reason)
  // Send to error tracking service
})
```

## ❓ Common Interview Questions

### 1. Design Patterns
**Q: Khi nào sử dụng Factory vs Singleton pattern?**

**A:** 
- **Factory**: Khi cần tạo objects với logic phức tạp, multiple types
- **Singleton**: Khi cần đảm bảo chỉ có một instance (database connection, logger)
- **Factory**: Flexible, testable, follows open/closed principle
- **Singleton**: Global state, can make testing difficult

### 2. Functional Programming
**Q: Pure functions vs impure functions?**

**A:** 
- **Pure functions**: No side effects, same input always gives same output
- **Impure functions**: Have side effects, modify external state
- **Benefits**: Easier to test, reason about, parallelize
- **Examples**: Math functions (pure), DOM manipulation (impure)

### 3. Performance Optimization
**Q: Debouncing vs throttling?**

**A:** 
- **Debouncing**: Delay execution until user stops typing/clicking
- **Throttling**: Execute at most once per time period
- **Use cases**: Search input (debounce), scroll events (throttle)
- **Implementation**: setTimeout vs timestamp checking

### 4. Async Patterns
**Q: Promise.all vs Promise.allSettled?**

**A:** 
- **Promise.all**: Fails fast if any promise rejects
- **Promise.allSettled**: Waits for all promises to complete
- **Use cases**: All-or-nothing (all), independent operations (allSettled)
- **Error handling**: Try-catch vs checking status property

### 5. Error Handling
**Q: Custom error classes benefits?**

**A:** 
- **Type checking**: instanceof operator
- **Structured errors**: Consistent error format
- **Error categorization**: Different error types
- **Debugging**: Better stack traces, error context

## 📚 Resources
- [JavaScript Design Patterns](https://www.dofactory.com/javascript/design-patterns)
- [Functional Programming in JavaScript](https://eloquentjavascript.net/)
- [Async JavaScript](https://asynchronousjavascript.com/)
- [Error Handling Best Practices](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling) 