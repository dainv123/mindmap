# �� Testing Strategies - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Testing Strategies
├── 🧪 Testing Pyramid
│   ├── Unit Tests → Fast, isolated, cheap
│   ├── Integration Tests → Component interaction
│   ├── E2E Tests → User workflows, expensive
│   └── Test Coverage → Code coverage metrics
├── 🎯 Testing Types
│   ├── Functional Testing → Feature validation
│   ├── Non-Functional Testing → Performance, security
│   ├── Manual Testing → Human verification
│   └── Automated Testing → Script-based execution
├── 🚀 Testing Tools
│   ├── Unit Testing → Jest, Mocha, Pytest
│   ├── E2E Testing → Cypress, Selenium, Playwright
│   ├── API Testing → Postman, Newman, RestAssured
│   └── Performance Testing → JMeter, k6, Artillery
├── 📊 Test Management
│   ├── Test Planning → Strategy, scope, resources
│   ├── Test Execution → Running, monitoring, reporting
│   ├── Bug Tracking → Issue management, resolution
│   └── Test Metrics → Coverage, quality, efficiency
└── 🔄 CI/CD Integration
    ├── Automated Testing → Pipeline integration
    ├── Test Environments → Staging, production
    ├── Quality Gates → Pass/fail criteria
    └── Continuous Testing → Always-on testing
```

## 📋 Table of Contents
- [Testing Pyramid](#testing-pyramid)
- [Testing Types](#testing-types)
- [Testing Tools](#testing-tools)
- [Test Management](#test-management)
- [CI/CD Integration](#cicd-integration)
- [Common Interview Questions](#common-interview-questions)

## ❓ Common Interview Questions

### 1. Testing Pyramid
**Q: Giải thích Testing Pyramid và tại sao nó quan trọng?**
**A:** Unit tests (70-80%) - fast, cheap, high coverage. Integration tests (15-20%) - medium speed/cost. E2E tests (5-10%) - slow, expensive, low coverage. Quan trọng vì cost-effective, fast feedback, catch bugs early.

### 2. Unit Testing Best Practices
**Q: Best practices cho unit testing?**
**A:** Use AAA pattern (Arrange-Act-Assert), one assertion per test, descriptive names, test isolation, mock external dependencies, use factories cho test data, aim for 80-90% coverage.

### 3. Mocking vs Stubbing
**Q: Sự khác biệt giữa Mocking và Stubbing?**
**A:** Stubbing chỉ replace function với return value. Mocking replace function với full implementation để verify behavior và interactions. Stubbing cho simple data replacement, mocking cho behavior verification.

### 4. Test Data Management
**Q: Cách quản lý test data hiệu quả?**
**A:** Use factories, Faker.js cho realistic data, in-memory databases, reset data trước mỗi test, isolate test data, clean up sau mỗi test, don't share data between tests.

### 5. Performance Testing
**Q: Cách implement performance testing?**
**A:** Load testing với expected load, stress testing beyond capacity, use tools như Artillery, k6, JMeter, measure response times, throughput, error rates, set performance baselines và budgets. 