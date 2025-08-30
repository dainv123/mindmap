# 🐹 Go (Golang) - Complete Knowledge Map

## 🧠 Mindmap Overview
```
Go (Golang)
├── 🎯 Core Concepts
│   ├── Static Typing → Kiểu dữ liệu tĩnh, compile-time checking
│   ├── Garbage Collection → Tự động quản lý bộ nhớ
│   ├── Concurrency → Goroutines, channels, concurrency patterns
│   └── Simplicity → Cú pháp đơn giản, ít keywords
├── 🏗️ Language Features
│   ├── Structs & Interfaces → Struct embedding, interface composition
│   ├── Error Handling → Explicit error handling, no exceptions
│   ├── Pointers → Pointer receivers, value vs pointer semantics
│   └── Generics → Type parameters, constraints (Go 1.18+)
├── ⚡ Concurrency
│   ├── Goroutines → Lightweight threads, managed by runtime
│   ├── Channels → Communication between goroutines
│   ├── Select → Non-blocking channel operations
│   └── Context → Cancellation, timeouts, deadlines
├── 🛠️ Standard Library
│   ├── HTTP → net/http package, mux, middleware
│   ├── JSON → encoding/json, struct tags
│   ├── Database → database/sql, drivers
│   └── Testing → testing package, benchmarks
└── 🚀 Best Practices
    ├── Project Structure → cmd, internal, pkg, api
    ├── Error Handling → Wrapping, custom error types
    ├── Testing → Table-driven tests, mocks
    └── Performance → Profiling, optimization
```

## 📋 Table of Contents
- [Core Concepts](#core-concepts)
- [Language Features](#language-features)
- [Concurrency](#concurrency)
- [Standard Library](#standard-library)
- [Common Interview Questions](#common-interview-questions)

## 🎯 Core Concepts

### What is Go?
**Go** là ngôn ngữ lập trình **statically typed, compiled** được thiết kế bởi Google với mục tiêu:
- **Đơn giản** và dễ học
- **Hiệu suất cao** như C/C++
- **Concurrency** mạnh mẽ với goroutines
- **Garbage collection** tự động

### Key Principles
```go
// 1. Simplicity - Ít keywords, cú pháp rõ ràng
package main
import "fmt"
func main() {
    fmt.Println("Hello, Go!")
}

// 2. Explicit - Không có implicit conversions
var x int = 42
var y float64 = float64(x) // Phải convert explicit

// 3. Composition over inheritance
type Animal struct { Name string }
type Dog struct {
    Animal      // Embedding, không phải inheritance
    Breed string
}

// 4. Error handling explicit
result, err := someFunction()
if err != nil {
    return err // Phải handle error
}
```

## 🏗️ Language Features

### Structs & Interfaces
```go
// Structs - Composite data types
type User struct {
    ID       int    `json:"id"`
    Name     string `json:"name"`
    Email    string `json:"email"`
    Password string `json:"-"` // Không serialize
}

// Methods với pointer receivers
func (u *User) UpdateName(name string) {
    u.Name = name // Thay đổi trực tiếp struct
}

func (u User) GetName() string {
    return u.Name // Copy struct, không thay đổi
}

// Interfaces - Implicit implementation
type Writer interface {
    Write([]byte) (int, error)
}

// User implements Writer implicitly
func (u User) Write(data []byte) (int, error) {
    return len(data), nil
}
```

### Error Handling
```go
// Custom error types
type ValidationError struct {
    Field   string
    Message string
}

func (e ValidationError) Error() string {
    return fmt.Sprintf("field %s: %s", e.Field, e.Message)
}

// Error wrapping (Go 1.13+)
func processUser(data []byte) error {
    var user User
    if err := json.Unmarshal(data, &user); err != nil {
        return fmt.Errorf("failed to unmarshal user: %w", err)
    }
    
    if err := validateUser(user); err != nil {
        return fmt.Errorf("validation failed: %w", err)
    }
    
    return nil
}

// Error handling patterns
func getUser(id int) (*User, error) {
    if id <= 0 {
        return nil, &ValidationError{
            Field:   "id",
            Message: "must be positive",
        }
    }
    
    return &User{ID: id, Name: "John"}, nil
}
```

### Generics (Go 1.18+)
```go
// Generic function
func Min[T constraints.Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}

// Generic struct
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() (T, error) {
    var zero T
    if len(s.items) == 0 {
        return zero, errors.New("stack is empty")
    }
    
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item, nil
}
```

## ⚡ Concurrency

### Goroutines
```go
// Basic goroutine
func main() {
    go func() {
        fmt.Println("Hello from goroutine")
    }()
    
    time.Sleep(time.Millisecond * 100)
}

// Goroutine với function
func worker(id int) {
    for i := 0; i < 3; i++ {
        fmt.Printf("Worker %d: %d\n", id, i)
        time.Sleep(time.Millisecond * 100)
    }
}

// WaitGroup để sync goroutines
func main() {
    var wg sync.WaitGroup
    
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            worker(id)
        }(i)
    }
    
    wg.Wait() // Đợi tất cả goroutines hoàn thành
    fmt.Println("All workers completed")
}
```

### Channels
```go
// Basic channel operations
func main() {
    ch := make(chan string, 2) // Buffered channel
    
    // Send
    ch <- "Hello"
    ch <- "World"
    
    // Receive
    fmt.Println(<-ch) // "Hello"
    fmt.Println(<-ch) // "World"
}

// Channel patterns
func producer(ch chan<- int) {
    for i := 0; i < 5; i++ {
        ch <- i
        time.Sleep(time.Millisecond * 100)
    }
    close(ch) // Đóng channel khi xong
}

func consumer(ch <-chan int) {
    for value := range ch { // Range over channel
        fmt.Printf("Received: %d\n", value)
    }
}
```

### Select Statement
```go
// Non-blocking channel operations
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(time.Second)
        ch1 <- "from ch1"
    }()
    
    go func() {
        time.Sleep(time.Second * 2)
        ch2 <- "from ch2"
    }()
    
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println("Received:", msg1)
        case msg2 := <-ch2:
            fmt.Println("Received:", msg2)
        case <-time.After(time.Second * 3):
            fmt.Println("Timeout")
            return
        }
    }
}
```

## 🛠️ Standard Library

### HTTP Server
```go
// Basic HTTP server
func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
    })
    
    http.HandleFunc("/api/users", handleUsers)
    
    fmt.Println("Server starting on :8080")
    http.ListenAndServe(":8080", nil)
}

// Custom handler
type UserHandler struct {
    userService UserService
}

func (h *UserHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    switch r.Method {
    case http.MethodGet:
        h.getUsers(w, r)
    case http.MethodPost:
        h.createUser(w, r)
    default:
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
    }
}

// Middleware pattern
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        
        next.ServeHTTP(w, r)
        
        fmt.Printf("%s %s %v\n", r.Method, r.URL.Path, time.Since(start))
    })
}
```

### JSON Handling
```go
// Struct tags
type User struct {
    ID       int    `json:"id"`
    Name     string `json:"name"`
    Email    string `json:"email,omitempty"` // Bỏ qua nếu empty
    Password string `json:"-"`               // Không serialize
    Created  time.Time `json:"created_at"`
}

// Marshal/Unmarshal
func main() {
    user := User{
        ID:      1,
        Name:    "John Doe",
        Email:   "john@example.com",
        Created: time.Now(),
    }
    
    // Marshal to JSON
    data, err := json.Marshal(user)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
    
    // Unmarshal from JSON
    var newUser User
    err = json.Unmarshal(data, &newUser)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%+v\n", newUser)
}
```

### Testing
```go
// Basic test
func TestAdd(t *testing.T) {
    result := Add(2, 3)
    expected := 5
    
    if result != expected {
        t.Errorf("Add(2, 3) = %d; want %d", result, expected)
    }
}

// Table-driven tests
func TestAddTable(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive", 1, 2, 3},
        {"negative", -1, -2, -3},
        {"zero", 0, 0, 0},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}

// Benchmarks
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(1, 2)
    }
}
```

## ❓ Common Interview Questions

### 1. Goroutines vs Threads
**Q: Sự khác biệt giữa Goroutines và Threads?**

**A:**
- **Goroutines**:
  - Lightweight (2KB stack)
  - Managed by Go runtime
  - Thousands can run simultaneously
  - Cheap to create/destroy
  
- **Threads**:
  - Heavyweight (1MB+ stack)
  - Managed by OS
  - Limited by system resources
  - Expensive to create/destroy

### 2. Channel Buffering
**Q: Khi nào nên dùng buffered channels?**

**A:**
```go
// Unbuffered channel - blocking
ch := make(chan int)
ch <- 1 // Block until receiver is ready

// Buffered channel - non-blocking until full
ch := make(chan int, 10)
ch <- 1 // Non-blocking if buffer not full

// Use buffered when:
// - Producer is faster than consumer
// - Need to handle burst of data
// - Want to decouple producer/consumer timing
```

### 3. Interface Design
**Q: Làm sao thiết kế interface tốt trong Go?**

**A:**
```go
// ❌ Bad: Interface too large
type UserService interface {
    CreateUser(user *User) error
    GetUser(id int) (*User, error)
    UpdateUser(user *User) error
    DeleteUser(id int) error
    ListUsers() ([]*User, error)
    SearchUsers(query string) ([]*User, error)
}

// ✅ Good: Small, focused interfaces
type UserCreator interface {
    CreateUser(user *User) error
}

type UserReader interface {
    GetUser(id int) (*User, error)
    ListUsers() ([]*User, error)
}

// Compose when needed
type UserService interface {
    UserCreator
    UserReader
}
```

### 4. Error Handling Patterns
**Q: Các pattern xử lý error phổ biến trong Go?**

**A:**
```go
// 1. Early return
func processUser(id int) error {
    if id <= 0 {
        return errors.New("invalid user id")
    }
    
    user, err := getUser(id)
    if err != nil {
        return fmt.Errorf("failed to get user: %w", err)
    }
    
    return saveUser(user)
}

// 2. Custom error types
type ValidationError struct {
    Field   string
    Message string
}

func (e ValidationError) Error() string {
    return fmt.Sprintf("field %s: %s", e.Field, e.Message)
}

// 3. Error wrapping
if err != nil {
    return fmt.Errorf("operation failed: %w", err)
}
```

### 5. Concurrency Patterns
**Q: Các pattern concurrency phổ biến trong Go?**

**A:**
```go
// 1. Worker Pool
func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        results <- job * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    
    // Start workers
    for i := 1; i <= 3; i++ {
        go worker(i, jobs, results)
    }
    
    // Send jobs
    for i := 1; i <= 9; i++ {
        jobs <- i
    }
    close(jobs)
    
    // Collect results
    for i := 1; i <= 9; i++ {
        fmt.Println("Result:", <-results)
    }
}

// 2. Select with timeout
select {
case msg := <-ch:
    fmt.Println("Received:", msg)
case <-time.After(time.Second):
    fmt.Println("Timeout")
}
```

## 📚 Resources
- [Go Official Documentation](https://golang.org/doc/)
- [Effective Go](https://golang.org/doc/effective_go.html)
- [Go by Example](https://gobyexample.com/)
- [Go Concurrency Patterns](https://talks.golang.org/2012/concurrency.slide)
- [Go Testing](https://golang.org/pkg/testing/) 