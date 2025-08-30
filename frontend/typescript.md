# 📘 TypeScript Deep Dive - Complete Knowledge Map

## 🧠 Mindmap Overview
```
TypeScript Deep Dive
├── 🎯 Core Concepts
│   ├── Static Typing → Compile-time type checking
│   ├── Type Inference → Automatic type detection
│   ├── Structural Typing → Duck typing, shape-based
│   └── Type Safety → Catch errors before runtime
├── 🏗️ Advanced Types
│   ├── Union Types → Multiple possible types
│   ├── Intersection Types → Combine multiple types
│   ├── Generic Types → Reusable type parameters
│   └── Conditional Types → Type-level programming
├── 🔧 Type System
│   ├── Interfaces → Object shape definitions
│   ├── Type Aliases → Custom type names
│   ├── Enums → Named constants
│   └── Namespaces → Module organization
├── ⚡ Advanced Features
│   ├── Decorators → Metadata and annotations
│   ├── Utility Types → Built-in type helpers
│   ├── Mapped Types → Transform existing types
│   └── Template Literal Types → String-based types
└── 🚀 Best Practices
    ├── Type Guards → Runtime type checking
    ├── Error Handling → Typed error patterns
    ├── Performance → Type optimization
    └── Migration → JavaScript to TypeScript
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Advanced Types](#advanced-types)
- [Type System](#type-system)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Core Concepts

### Static Typing
```typescript
// Basic types
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let hobbies: string[] = ["reading", "coding"];

// Type inference
let inferredString = "Hello"; // TypeScript infers string
let inferredNumber = 42; // TypeScript infers number

// Function types
function greet(name: string): string {
  return `Hello, ${name}!`;
}

const greetArrow: (name: string) => string = (name) => `Hello, ${name}!`;

// Optional and default parameters
function createUser(
  name: string,
  age: number,
  email?: string,
  isAdmin: boolean = false
): User {
  return { name, age, email, isAdmin };
}
```

### Type Safety
```typescript
// Type safety prevents runtime errors
interface User {
  id: number;
  name: string;
  email: string;
}

function updateUser(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}

// TypeScript catches errors at compile time
const user: User = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

// This would cause a compile error:
// updateUser(user, { invalidField: "value" });

// This is valid:
updateUser(user, { name: "Jane" });
```

## 🏗️ Advanced Types

### Union Types
```typescript
// Union types allow multiple possible types
type Status = "loading" | "success" | "error";
type ID = string | number;

// Function with union types
function processId(id: ID): string {
  if (typeof id === "string") {
    return id.toUpperCase();
  } else {
    return id.toString();
  }
}

// Union types with interfaces
interface Car {
  type: "car";
  brand: string;
  model: string;
}

interface Motorcycle {
  type: "motorcycle";
  brand: string;
  engineSize: number;
}

type Vehicle = Car | Motorcycle;

function getVehicleInfo(vehicle: Vehicle): string {
  switch (vehicle.type) {
    case "car":
      return `${vehicle.brand} ${vehicle.model}`;
    case "motorcycle":
      return `${vehicle.brand} ${vehicle.engineSize}cc`;
  }
}
```

### Intersection Types
```typescript
// Intersection types combine multiple types
interface HasName {
  name: string;
}

interface HasAge {
  age: number;
}

interface HasEmail {
  email: string;
}

type Person = HasName & HasAge & HasEmail;

const person: Person = {
  name: "John",
  age: 30,
  email: "john@example.com"
};

// Intersection with utility types
type UserWithPermissions = User & {
  permissions: string[];
  role: "admin" | "user" | "moderator";
};
```

### Generic Types
```typescript
// Generic function
function identity<T>(arg: T): T {
  return arg;
}

const stringResult = identity<string>("hello");
const numberResult = identity<number>(42);

// Generic interface
interface Container<T> {
  value: T;
  getValue(): T;
  setValue(value: T): void;
}

class NumberContainer implements Container<number> {
  constructor(public value: number) {}

  getValue(): number {
    return this.value;
  }

  setValue(value: number): void {
    this.value = value;
  }
}

// Generic constraints
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

// Generic classes
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }
}

const numberStack = new Stack<number>();
const stringStack = new Stack<string>();
```

### Conditional Types
```typescript
// Basic conditional type
type NonNullable<T> = T extends null | undefined ? never : T;

type StringOrNull = string | null;
type NonNullString = NonNullable<StringOrNull>; // string

// Conditional type with inference
type ElementType<T> = T extends (infer U)[] ? U : never;

type StringArray = string[];
type StringElement = ElementType<StringArray>; // string

// Multiple conditions
type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object";
```

## 🔧 Type System

### Interfaces
```typescript
// Basic interface
interface User {
  id: number;
  name: string;
  email: string;
  age?: number; // Optional property
  readonly createdAt: Date; // Readonly property
}

// Interface extension
interface Employee extends User {
  department: string;
  salary: number;
  manager?: Employee;
}

// Interface implementation
class UserService implements User {
  constructor(
    public id: number,
    public name: string,
    public email: string,
    public readonly createdAt: Date = new Date()
  ) {}
}

// Interface with methods
interface Repository<T> {
  find(id: number): Promise<T | null>;
  save(entity: T): Promise<T>;
  delete(id: number): Promise<boolean>;
  findAll(): Promise<T[]>;
}

// Interface with index signatures
interface StringDictionary {
  [key: string]: string;
}

interface NumberDictionary {
  [key: string]: number;
  length: number; // Must be number
}
```

### Type Aliases
```typescript
// Basic type alias
type Point = {
  x: number;
  y: number;
};

type ID = string | number;

type Status = "pending" | "approved" | "rejected";

// Function type alias
type EventHandler<T = Event> = (event: T) => void;

type ClickHandler = EventHandler<MouseEvent>;
type KeyHandler = EventHandler<KeyboardEvent>;

// Union type alias
type Result<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

// Type alias with generics
type AsyncResult<T> = Promise<Result<T>>;

// Type alias for complex types
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

type PartialUser = DeepPartial<User>;
```

### Enums
```typescript
// Numeric enum
enum Direction {
  North = 0,
  East = 1,
  South = 2,
  West = 3
}

// String enum
enum UserRole {
  Admin = "ADMIN",
  User = "USER",
  Moderator = "MODERATOR"
}

// Computed enum
enum FileAccess {
  None = 0,
  Read = 1 << 0,
  Write = 1 << 1,
  ReadWrite = Read | Write,
  All = Read | Write | (1 << 2)
}

// Const enum (more efficient)
const enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE"
}

// Enum with reverse mapping
enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE"
}

// Reverse mapping works for numeric enums
enum NumericColor {
  Red,
  Green,
  Blue
}

const colorName = NumericColor[0]; // "Red"
```

## ⚡ Advanced Features

### Utility Types
```typescript
// Partial - makes all properties optional
type PartialUser = Partial<User>;

// Required - makes all properties required
type RequiredUser = Required<User>;

// Pick - selects specific properties
type UserBasicInfo = Pick<User, "name" | "email">;

// Omit - excludes specific properties
type UserWithoutId = Omit<User, "id">;

// Record - creates object type with specific keys and values
type UserRoles = Record<string, UserRole>;

// ReturnType - extracts return type of function
type GreetReturn = ReturnType<typeof greet>;

// Parameters - extracts parameter types of function
type GreetParams = Parameters<typeof greet>;

// InstanceType - extracts instance type of class
class UserClass {
  constructor(public name: string) {}
}
type UserInstance = InstanceType<typeof UserClass>;

// NonNullable - removes null and undefined
type NonNullUser = NonNullable<User | null | undefined>;

// Extract - extracts types that are assignable to U
type StringOrNumber = string | number;
type ExtractedString = Extract<StringOrNumber, string>; // string

// Exclude - excludes types that are assignable to U
type ExcludedString = Exclude<StringOrNumber, string>; // number
```

### Mapped Types
```typescript
// Basic mapped type
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

type ReadonlyUser = Readonly<User>;

// Mapped type with modifiers
type Optional<T> = {
  [P in keyof T]?: T[P];
};

type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};

// Mapped type with conditional
type ConditionalReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object ? Readonly<T[P]> : T[P];
};

// Mapped type with template literal types
type EventMap = {
  click: MouseEvent;
  keydown: KeyboardEvent;
  load: Event;
};

type EventHandlers = {
  [K in keyof EventMap as `on${Capitalize<K>}`]: (event: EventMap[K]) => void;
};

// Mapped type with filtering
type FilteredProperties<T, U> = {
  [K in keyof T as T[K] extends U ? K : never]: T[K];
};

type StringProperties = FilteredProperties<User, string>;
```

### Template Literal Types
```typescript
// Basic template literal type
type Greeting = `Hello ${string}`;
type Email = `${string}@${string}.${string}`;

// Template literal type with unions
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type ApiEndpoint = `/api/${string}`;
type FullUrl = `${HttpMethod} ${ApiEndpoint}`;

// Template literal type with conditional
type EventName<T extends string> = `${T}Changed`;
type UserEvents = EventName<"name" | "email" | "age">;

// Template literal type with inference
type ExtractRoute<T> = T extends `${HttpMethod} /api/${infer Route}` ? Route : never;

type Route = ExtractRoute<"GET /api/users">; // "users"

// Template literal type with recursive
type Join<T extends string[], D extends string> = T extends []
  ? ""
  : T extends [infer F]
  ? F
  : T extends [infer F, ...infer R]
  ? F extends string
    ? `${F}${D}${Join<Extract<R, string[]>, D>}`
    : never
  : string;

type Path = Join<["users", "profile", "settings"], "/">; // "users/profile/settings"
```

## ❓ Common Interview Questions

### 1. TypeScript vs JavaScript
**Q: Lợi ích của TypeScript so với JavaScript?**

**A:**
```typescript
const typescriptBenefits = {
  // Type Safety
  typeSafety: {
    description: 'Catch errors at compile time',
    example: `
      function add(a: number, b: number): number {
        return a + b;
      }
      add("1", 2); // Compile error
    `
  },

  // Better IDE Support
  ideSupport: {
    description: 'IntelliSense, refactoring, navigation',
    features: [
      'Auto-completion',
      'Go to definition',
      'Find all references',
      'Rename symbol'
    ]
  },

  // Documentation
  documentation: {
    description: 'Types serve as documentation',
    example: `
      interface User {
        id: number;
        name: string;
        email: string;
        age?: number;
      }
    `
  },

  // Refactoring
  refactoring: {
    description: 'Safe refactoring with type checking',
    benefits: [
      'Rename properties safely',
      'Change function signatures',
      'Update interfaces'
    ]
  }
};
```

### 2. Type Guards
**Q: Cách implement type guards trong TypeScript?**

**A:**
```typescript
// Type guards
const typeGuards = {
  // typeof type guard
  typeofGuard: `
    function processValue(value: string | number) {
      if (typeof value === "string") {
        return value.toUpperCase();
      } else {
        return value.toFixed(2);
      }
    }
  `,

  // instanceof type guard
  instanceofGuard: `
    class Dog {
      bark() { return "Woof!"; }
    }
    
    class Cat {
      meow() { return "Meow!"; }
    }
    
    function makeSound(animal: Dog | Cat) {
      if (animal instanceof Dog) {
        return animal.bark();
      } else {
        return animal.meow();
      }
    }
  `,

  // Custom type guard
  customGuard: `
    interface User {
      id: number;
      name: string;
    }
    
    interface Admin {
      id: number;
      name: string;
      permissions: string[];
    }
    
    function isAdmin(user: User | Admin): user is Admin {
      return 'permissions' in user;
    }
    
    function processUser(user: User | Admin) {
      if (isAdmin(user)) {
        return user.permissions; // TypeScript knows user is Admin
      } else {
        return user.name; // TypeScript knows user is User
      }
    }
  `,

  // Discriminated unions
  discriminatedUnion: `
    interface LoadingState {
      status: "loading";
    }
    
    interface SuccessState {
      status: "success";
      data: string;
    }
    
    interface ErrorState {
      status: "error";
      error: string;
    }
    
    type State = LoadingState | SuccessState | ErrorState;
    
    function handleState(state: State) {
      switch (state.status) {
        case "loading":
          return "Loading...";
        case "success":
          return state.data; // TypeScript knows data exists
        case "error":
          return state.error; // TypeScript knows error exists
      }
    }
  `
};
```

### 3. Generics
**Q: Khi nào nên sử dụng generics?**

**A:**
```typescript
const genericsUseCases = {
  // Reusable data structures
  dataStructures: `
    class Stack<T> {
      private items: T[] = [];
      
      push(item: T): void {
        this.items.push(item);
      }
      
      pop(): T | undefined {
        return this.items.pop();
      }
    }
    
    const numberStack = new Stack<number>();
    const stringStack = new Stack<string>();
  `,

  // API responses
  apiResponses: `
    interface ApiResponse<T> {
      data: T;
      status: number;
      message: string;
    }
    
    async function fetchUser(id: number): Promise<ApiResponse<User>> {
      const response = await fetch(\`/api/users/\${id}\`);
      return response.json();
    }
  `,

  // Utility functions
  utilityFunctions: `
    function identity<T>(arg: T): T {
      return arg;
    }
    
    function first<T>(array: T[]): T | undefined {
      return array[0];
    }
    
    function map<T, U>(array: T[], fn: (item: T) => U): U[] {
      return array.map(fn);
    }
  `,

  // Constraints
  constraints: `
    interface Lengthwise {
      length: number;
    }
    
    function logLength<T extends Lengthwise>(arg: T): T {
      console.log(arg.length);
      return arg;
    }
    
    // Can be used with strings, arrays, objects with length
    logLength("hello"); // 5
    logLength([1, 2, 3]); // 3
    logLength({ length: 10 }); // 10
  `
};
```

### 4. Advanced Type Patterns
**Q: Các advanced type patterns phổ biến?**

**A:**
```typescript
const advancedTypePatterns = {
  // Builder pattern
  builderPattern: `
    class QueryBuilder<T> {
      private query: string = "";
      private params: any[] = [];
      
      select(fields: (keyof T)[]): QueryBuilder<T> {
        this.query += \`SELECT \${fields.join(", ")} \`;
        return this;
      }
      
      where(field: keyof T, value: any): QueryBuilder<T> {
        this.query += \`WHERE \${String(field)} = ? \`;
        this.params.push(value);
        return this;
      }
      
      build(): { query: string; params: any[] } {
        return { query: this.query.trim(), params: this.params };
      }
    }
  `,

  // Factory pattern
  factoryPattern: `
    interface Animal {
      makeSound(): string;
    }
    
    class Dog implements Animal {
      makeSound(): string { return "Woof!"; }
    }
    
    class Cat implements Animal {
      makeSound(): string { return "Meow!"; }
    }
    
    type AnimalType = "dog" | "cat";
    
    class AnimalFactory {
      static create(type: AnimalType): Animal {
        switch (type) {
          case "dog": return new Dog();
          case "cat": return new Cat();
        }
      }
    }
  `,

  // Observer pattern
  observerPattern: `
    type Observer<T> = (data: T) => void;
    
    class Observable<T> {
      private observers: Observer<T>[] = [];
      
      subscribe(observer: Observer<T>): () => void {
        this.observers.push(observer);
        return () => {
          const index = this.observers.indexOf(observer);
          if (index > -1) {
            this.observers.splice(index, 1);
          }
        };
      }
      
      notify(data: T): void {
        this.observers.forEach(observer => observer(data));
      }
    }
  `,

  // Singleton pattern
  singletonPattern: `
    class Database {
      private static instance: Database;
      private constructor() {}
      
      static getInstance(): Database {
        if (!Database.instance) {
          Database.instance = new Database();
        }
        return Database.instance;
      }
      
      query(sql: string): any {
        // Implementation
      }
    }
  `
};
```

### 5. Performance Optimization
**Q: Cách optimize TypeScript performance?**

**A:**
```typescript
const performanceOptimizations = {
  // Type optimization
  typeOptimization: {
    description: 'Use specific types instead of any',
    examples: [
      'Use string literal types instead of string',
      'Use union types instead of any',
      'Use const assertions for immutable data'
    ]
  },

  // Compiler options
  compilerOptions: {
    strict: 'Enable strict mode for better type checking',
    noImplicitAny: 'Prevent implicit any types',
    strictNullChecks: 'Enable null and undefined checking',
    noUnusedLocals: 'Remove unused variables',
    removeComments: 'Remove comments in production'
  },

  // Bundle optimization
  bundleOptimization: {
    treeShaking: 'Remove unused code',
    codeSplitting: 'Split code into smaller chunks',
    minification: 'Minify code for production'
  },

  // Runtime optimization
  runtimeOptimization: {
    typeGuards: 'Use type guards for runtime checks',
    avoidAny: 'Avoid using any type',
    useInterfaces: 'Use interfaces for object shapes'
  }
};

// Example optimizations
const optimizations = {
  // Bad - using any
  bad: `
    function processData(data: any): any {
      return data.map((item: any) => item.name);
    }
  `,

  // Good - specific types
  good: `
    interface DataItem {
      name: string;
      value: number;
    }
    
    function processData(data: DataItem[]): string[] {
      return data.map(item => item.name);
    }
  `,

  // Const assertions
  constAssertions: `
    // Without const assertion
    const colors = ["red", "green", "blue"]; // string[]
    
    // With const assertion
    const colors = ["red", "green", "blue"] as const; // readonly ["red", "green", "blue"]
  `
};
```

## 📚 Resources
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [TypeScript Playground](https://www.typescriptlang.org/play)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [TypeScript Design Patterns](https://refactoring.guru/design-patterns/typescript)
- [TypeScript Performance](https://github.com/microsoft/TypeScript/wiki/Performance) 