# 🔮 GraphQL - Complete Knowledge Map

## 🧠 Mindmap Overview
```
GraphQL
├── 🎯 Core Concepts
│   ├── Query Language → Declarative data fetching
│   ├── Schema Definition → Type system, introspection
│   ├── Single Endpoint → One endpoint for all operations
│   └── Strong Typing → Type safety, validation
├── 🏗️ Schema Design
│   ├── Types → Object, Scalar, Input types
│   ├── Fields → Data properties, relationships
│   ├── Resolvers → Data fetching logic
│   └── Directives → Field-level instructions
├── 🔄 Operations
│   ├── Queries → Read operations
│   ├── Mutations → Write operations
│   ├── Subscriptions → Real-time updates
│   └── Fragments → Reusable field sets
├── ⚡ Performance
│   ├── DataLoader → Batching, caching
│   ├── Field Selection → Only fetch needed data
│   ├── Query Complexity → Prevent expensive queries
│   └── Caching → Response caching strategies
└── 🛠️ Tools & Ecosystem
    ├── Apollo → Client/server libraries
    ├── Relay → Facebook's GraphQL client
    ├── GraphiQL → Interactive playground
    └── Code Generation → Type-safe development
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Schema Design](#schema-design)
- [Operations](#operations)
- [Performance](#performance)
- [Tools & Ecosystem](#tools--ecosystem)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Core Concepts

### What is GraphQL?
GraphQL là **ngôn ngữ truy vấn cho API** và **runtime để thực thi các truy vấn** trên dữ liệu. Nó cung cấp:
- **Single endpoint** → Chỉ cần 1 URL cho tất cả operations
- **Strong typing system** → Hệ thống kiểu dữ liệu chặt chẽ, ít lỗi
- **Client-specified queries** → Client chỉ lấy đúng dữ liệu cần thiết
- **Real-time updates** → Cập nhật dữ liệu theo thời gian thực

### Key Principles (Nguyên tắc chính)
```graphql
# 1. Hierarchical Structure → Cấu trúc phân cấp rõ ràng
query {
  user(id: "123") {
    name
    posts {
      title
      comments {
        text
      }
    }
  }
}

# 2. Product-centric → Tập trung vào sản phẩm, không phải server
# 3. Strong typing → Kiểu dữ liệu chặt chẽ, ít lỗi runtime
# 4. Introspective → API có thể tự mô tả chính nó
# 5. Version-free evolution → Không cần version, thay đổi schema an toàn
```

## 🏗️ Schema & Types

### Basic Types (Các kiểu cơ bản)
```graphql
# Scalar Types → Kiểu dữ liệu nguyên thủy
type User {
  id: ID!          # ID! = Bắt buộc, không null
  name: String!    # String! = Bắt buộc
  email: String    # String = Có thể null
  age: Int         # Số nguyên
  isActive: Boolean! # Boolean = true/false
  createdAt: DateTime # Kiểu thời gian
}

# Object Types → Kiểu đối tượng phức hợp
type Post {
  id: ID!
  title: String!
  content: String
  author: User!    # Quan hệ với User
  comments: [Comment!]! # Mảng Comment, không null
}

# Input Types → Kiểu dữ liệu đầu vào cho mutations
input CreateUserInput {
  name: String!
  email: String!
  age: Int
}
```

### Advanced Types (Các kiểu nâng cao)
```graphql
# Union Types → Kiểu kết hợp, có thể là 1 trong nhiều kiểu
union SearchResult = User | Post | Comment
# Kết quả tìm kiếm có thể là User, Post hoặc Comment

# Interface Types → Giao diện chung cho nhiều kiểu
interface Node {
  id: ID!  # Tất cả Node phải có id
}

type User implements Node {  # User thực hiện interface Node
  id: ID!
  name: String!
}

type Post implements Node {  # Post cũng thực hiện interface Node
  id: ID!
  title: String!
}

# Enum Types → Kiểu liệt kê, chỉ cho phép các giá trị cụ thể
enum UserRole {
  ADMIN      # Chỉ có thể là ADMIN, MODERATOR hoặc USER
  MODERATOR
  USER
}

# Custom Scalars → Kiểu dữ liệu tùy chỉnh
scalar DateTime  # Kiểu thời gian
scalar JSON     # Kiểu JSON
```

### Schema Definition
```graphql
schema {
  query: Query
  mutation: Mutation
  subscription: Subscription
}

type Query {
  user(id: ID!): User
  users(limit: Int, offset: Int): [User!]!
  search(query: String!): [SearchResult!]!
}

type Mutation {
  createUser(input: CreateUserInput!): User!
  updateUser(id: ID!, input: UpdateUserInput!): User!
  deleteUser(id: ID!): Boolean!
}

type Subscription {
  userCreated: User!
  postUpdated(id: ID!): Post!
}
```

## 🔄 Operations

### Queries
```graphql
# Basic Query
query GetUser {
  user(id: "123") {
    id
    name
    email
  }
}

# Query with Variables
query GetUser($userId: ID!) {
  user(id: $userId) {
    id
    name
    posts {
      title
      createdAt
    }
  }
}

# Query with Aliases
query GetUsers {
  admin: users(role: ADMIN) {
    id
    name
  }
  regular: users(role: USER) {
    id
    name
  }
}

# Query with Fragments
fragment UserFields on User {
  id
  name
  email
}

query GetUserWithPosts {
  user(id: "123") {
    ...UserFields
    posts {
      title
      content
    }
  }
}
```

### Mutations
```graphql
# Create Operation
mutation CreateUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
    name
    email
  }
}

# Update Operation
mutation UpdateUser($id: ID!, $input: UpdateUserInput!) {
  updateUser(id: $id, input: $input) {
    id
    name
    email
  }
}

# Multiple Operations
mutation CreatePostAndUser {
  createUser(input: { name: "John", email: "john@example.com" }) {
    id
    name
  }
  createPost(input: { title: "Hello", content: "World" }) {
    id
    title
  }
}
```

### Subscriptions
```graphql
# Real-time Updates
subscription UserCreated {
  userCreated {
    id
    name
    email
  }
}

# Filtered Subscriptions
subscription PostUpdated($postId: ID!) {
  postUpdated(id: $postId) {
    id
    title
    content
    updatedAt
  }
}
```

## 🔧 Resolvers

### Basic Resolvers
```javascript
// User resolver
const resolvers = {
  Query: {
    user: (parent, { id }, context) => {
      return context.db.users.find(user => user.id === id);
    },
    users: (parent, { limit = 10, offset = 0 }, context) => {
      return context.db.users.slice(offset, offset + limit);
    }
  },
  
  User: {
    posts: (parent, args, context) => {
      return context.db.posts.filter(post => post.authorId === parent.id);
    }
  },
  
  Mutation: {
    createUser: (parent, { input }, context) => {
      const user = { id: generateId(), ...input };
      context.db.users.push(user);
      return user;
    }
  }
};
```

### Advanced Resolvers
```javascript
// Field-level resolvers
const resolvers = {
  User: {
    fullName: (parent) => `${parent.firstName} ${parent.lastName}`,
    
    posts: async (parent, args, context) => {
      // Batch loading to avoid N+1
      return context.loaders.posts.load(parent.id);
    },
    
    profile: async (parent, args, context) => {
      // Conditional field resolution
      if (!context.user || context.user.id !== parent.id) {
        return null; // Private data
      }
      return context.db.profiles.find(p => p.userId === parent.id);
    }
  }
};
```

## ⚡ Performance Optimization

### N+1 Problem & Solutions (Vấn đề N+1 và giải pháp)
```javascript
// ❌ Bad: N+1 Problem → Vấn đề query dư thừa
const resolvers = {
  User: {
    posts: async (parent, args, context) => {
      // Vấn đề: Nếu có 100 users, sẽ chạy 100 queries riêng biệt
      // Query 1: SELECT * FROM posts WHERE authorId = 'user1'
      // Query 2: SELECT * FROM posts WHERE authorId = 'user2'
      // ... 100 queries!
      return await context.db.posts.find({ authorId: parent.id });
    }
  }
};

// ✅ Good: DataLoader Pattern → Giải pháp batch loading
const { DataLoader } = require('dataloader');

const postsLoader = new DataLoader(async (userIds) => {
  // Giải pháp: Chỉ 1 query duy nhất cho tất cả users
  // SELECT * FROM posts WHERE authorId IN ('user1', 'user2', ..., 'user100')
  const posts = await context.db.posts.find({ 
    authorId: { $in: userIds } 
  });
  
  // Trả về posts theo đúng thứ tự userIds
  return userIds.map(userId => 
    posts.filter(post => post.authorId === userId)
  );
});

const resolvers = {
  User: {
    posts: async (parent, args, context) => {
      return await postsLoader.load(parent.id);
    }
  }
};
```

### Caching Strategies
```javascript
// Field-level caching
const resolvers = {
  User: {
    posts: async (parent, args, context) => {
      const cacheKey = `user:${parent.id}:posts`;
      let posts = await context.cache.get(cacheKey);
      
      if (!posts) {
        posts = await context.db.posts.find({ authorId: parent.id });
        await context.cache.set(cacheKey, posts, 300); // 5 minutes
      }
      
      return posts;
    }
  }
};
```

### Query Complexity Analysis
```javascript
// Query complexity rules
const complexityLimit = 1000;

const complexityRules = {
  User: 1,
  'User.posts': 10,
  'User.posts.comments': 5,
  Post: 1,
  Comment: 1
};
```

## 🔒 Security

### Authentication
```javascript
// JWT Authentication
const resolvers = {
  Query: {
    user: async (parent, { id }, context) => {
      if (!context.user) {
        throw new AuthenticationError('Not authenticated');
      }
      return context.db.users.find(user => user.id === id);
    }
  }
};

// Context setup
const context = ({ req }) => {
  const token = req.headers.authorization?.replace('Bearer ', '');
  if (token) {
    const user = jwt.verify(token, process.env.JWT_SECRET);
    return { user, db };
  }
  return { db };
};
```

### Authorization
```javascript
// Field-level authorization
const resolvers = {
  User: {
    email: (parent, args, context) => {
      // Only show email to owner or admin
      if (context.user?.id === parent.id || context.user?.role === 'ADMIN') {
        return parent.email;
      }
      return null;
    }
  }
};
```

### Rate Limiting
```javascript
// Query rate limiting
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});
```

## 🛠️ Tools & Ecosystem

### Apollo Server
```javascript
const { ApolloServer } = require('apollo-server-express');
const { typeDefs } = require('./schema');
const { resolvers } = require('./resolvers');

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => ({
    user: req.user,
    db: database
  }),
  plugins: [
    // Error handling
    {
      requestDidStart() {
        return {
          didEncounterErrors(errors) {
            console.error('GraphQL Errors:', errors);
          }
        };
      }
    }
  ]
});
```

### Apollo Client
```javascript
import { ApolloClient, InMemoryCache, gql } from '@apollo/client';

const client = new ApolloClient({
  uri: 'http://localhost:4000/graphql',
  cache: new InMemoryCache()
});

// Query
const GET_USER = gql`
  query GetUser($id: ID!) {
    user(id: $id) {
      id
      name
      posts {
        title
      }
    }
  }
`;

const { loading, error, data } = useQuery(GET_USER, {
  variables: { id: '123' }
});
```

## ❓ Common Interview Questions

### 1. GraphQL vs REST
**Q: Sự khác biệt chính giữa GraphQL và REST?**

**A:**
- **REST**: 
  - Nhiều endpoints khác nhau (/users, /posts, /comments)
  - Over-fetching: Lấy dư dữ liệu không cần thiết
  - Under-fetching: Phải gọi nhiều API để lấy đủ dữ liệu
  - Không có strong typing, dễ lỗi

- **GraphQL**: 
  - Chỉ 1 endpoint duy nhất (/graphql)
  - Precise data fetching: Chỉ lấy đúng dữ liệu cần
  - Strong typing: Kiểu dữ liệu chặt chẽ, ít lỗi
  - Real-time subscriptions: Cập nhật dữ liệu theo thời gian thực

### 2. N+1 Problem
**Q: Vấn đề N+1 trong GraphQL là gì và cách giải quyết?**

**A:** N+1 xảy ra khi resolve các field liên quan gây ra nhiều database queries dư thừa.

**Ví dụ:** Lấy 100 users, mỗi user có posts
- ❌ **Vấn đề**: 1 query lấy users + 100 queries lấy posts = 101 queries!
- ✅ **Giải pháp**:
  - **DataLoader**: Batch nhiều queries thành 1 query duy nhất
  - **Field-level caching**: Cache kết quả tính toán phức tạp
  - **Database joins**: Sử dụng JOIN để lấy dữ liệu liên quan

### 3. Schema Design
**Q: How do you design a GraphQL schema for a social media app?**

**A:**
```graphql
type User {
  id: ID!
  username: String!
  posts: [Post!]!
  followers: [User!]!
  following: [User!]!
}

type Post {
  id: ID!
  content: String!
  author: User!
  likes: [Like!]!
  comments: [Comment!]!
}
```

### 4. Authentication & Authorization
**Q: How do you implement authentication in GraphQL?**

**A:**
- **Context-based**: Pass user info through context
- **Field-level**: Check permissions in resolvers
- **Directive-based**: Use custom directives for auth

### 5. Performance Optimization
**Q: What strategies do you use to optimize GraphQL performance?**

**A:**
- **Query complexity analysis**: Limit query depth
- **Caching**: Redis, Apollo cache
- **Pagination**: Cursor-based pagination
- **DataLoader**: Batch database queries

### 6. Error Handling
**Q: How do you handle errors in GraphQL?**

**A:**
```javascript
// Custom error types
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.field = field;
  }
}

// Error handling in resolvers
const resolvers = {
  Mutation: {
    createUser: async (parent, { input }, context) => {
      try {
        const user = await context.db.users.create(input);
        return { user, errors: [] };
      } catch (error) {
        return { user: null, errors: [{ message: error.message }] };
      }
    }
  }
};
```

### 7. Subscriptions
**Q: How do GraphQL subscriptions work?**

**A:** Subscriptions use WebSockets for real-time updates:
```javascript
const resolvers = {
  Subscription: {
    userCreated: {
      subscribe: () => pubsub.asyncIterator(['USER_CREATED'])
    }
  }
};
```

### 8. Schema Evolution
**Q: How do you handle schema changes without breaking clients?**

**A:**
- **Deprecation**: Mark fields as deprecated
- **Versioning**: Use field arguments for versions
- **Backward compatibility**: Keep old fields working

## 🚀 Real-world Examples

### E-commerce Schema
```graphql
type Product {
  id: ID!
  name: String!
  price: Float!
  description: String
  category: Category!
  reviews: [Review!]!
  inventory: Int!
}

type Order {
  id: ID!
  user: User!
  items: [OrderItem!]!
  total: Float!
  status: OrderStatus!
  createdAt: DateTime!
}

enum OrderStatus {
  PENDING
  CONFIRMED
  SHIPPED
  DELIVERED
  CANCELLED
}
```

### Social Media Schema
```graphql
type Post {
  id: ID!
  content: String!
  author: User!
  likes: [Like!]!
  comments: [Comment!]!
  shares: [Share!]!
  createdAt: DateTime!
}

type Comment {
  id: ID!
  content: String!
  author: User!
  post: Post!
  replies: [Comment!]!
}
```

## 📚 Resources
- [GraphQL Official Docs](https://graphql.org/)
- [Apollo Documentation](https://www.apollographql.com/docs/)
- [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
- [GraphQL Security](https://graphql.org/learn/authorization/)
- [Apollo Server Tutorial](https://www.apollographql.com/docs/apollo-server/getting-started/) 