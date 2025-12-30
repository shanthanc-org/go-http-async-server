# Quick Start Guide

Get started building your Go HTTP async server in 5 steps!

## Step 1: Initialize Your Go Module

```bash
go mod init github.com/shanthanc-org/go-http-async-server
```

This creates a `go.mod` file that tracks your dependencies.

## Step 2: Create Your First File

Create `cmd/server/main.go`:

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    // Define a simple handler
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, Async World!")
    })

    // Start the server
    log.Println("Server starting on :8080")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
}
```

## Step 3: Run Your Server

```bash
go run cmd/server/main.go
```

## Step 4: Test It

Open another terminal and test:

```bash
curl http://localhost:8080
```

You should see: `Hello, Async World!`

## Step 5: Add Your First Handler

Create `internal/handler/health.go`:

```go
package handler

import (
    "encoding/json"
    "net/http"
)

type HealthResponse struct {
    Status string `json:"status"`
}

func HealthCheck(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(HealthResponse{Status: "healthy"})
}
```

Update `cmd/server/main.go` to use it:

```go
package main

import (
    "log"
    "net/http"
    
    "github.com/shanthanc-org/go-http-async-server/internal/handler"
)

func main() {
    http.HandleFunc("/health", handler.HealthCheck)
    
    log.Println("Server starting on :8080")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
}
```

## What's Next?

### Add Concurrency

Learn about goroutines and channels by implementing a worker pool:

1. Read `docs/CONCURRENCY_PATTERNS.md`
2. Create `internal/worker/pool.go`
3. Implement the worker pool pattern
4. Connect it to your handlers

### Add Middleware

Learn the middleware pattern:

1. Read `internal/middleware/README.md`
2. Create `internal/middleware/logging.go`
3. Wrap your handlers with middleware
4. See requests logged in real-time

### Add Tests

Learn Go testing:

1. Create `internal/handler/health_test.go`
2. Write table-driven tests
3. Run with `go test ./...`
4. Use `-race` flag to detect race conditions

### Explore More

- **Configuration**: Read `internal/config/README.md`
- **Project Structure**: Read `PROJECT_STRUCTURE.md`
- **Example Files**: Read `docs/EXAMPLE_FILES.md`

## Common Commands

```bash
# Run your server
go run cmd/server/main.go

# Build binary
go build -o bin/server cmd/server/main.go

# Run tests
go test ./...

# Run tests with race detector
go test -race ./...

# Check test coverage
go test -cover ./...

# Run benchmarks
go test -bench=. ./...

# Format code
go fmt ./...

# Check for issues
go vet ./...

# Download dependencies
go mod download

# Clean up dependencies
go mod tidy
```

## Recommended Development Flow

1. **Start Simple**: Get basic HTTP server running
2. **Add One Feature**: Implement one new thing
3. **Test It**: Write tests for new code
4. **Document It**: Add comments and docs
5. **Repeat**: Build incrementally

## Tips for Learning

- **Run Code Frequently**: Don't write too much before testing
- **Read Error Messages**: Go's errors are usually clear
- **Use the Debugger**: Learn to use `delve` debugger
- **Read Standard Library**: Study `net/http`, `sync`, `context`
- **Experiment**: Try different approaches
- **Break Things**: Learn by making mistakes

## Getting Help

- [Go Documentation](https://golang.org/doc/)
- [Go by Example](https://gobyexample.com/)
- [Effective Go](https://golang.org/doc/effective_go.html)
- [Go Standard Library](https://pkg.go.dev/std)
- [r/golang](https://reddit.com/r/golang)

## Your Learning Journey

```
Week 1: Basic HTTP Server
  ├── HTTP handlers
  ├── Routing
  └── JSON responses

Week 2: Middleware
  ├── Logging
  ├── Recovery
  └── Request ID

Week 3: Concurrency Basics
  ├── Goroutines
  ├── Channels
  └── Wait groups

Week 4: Worker Pools
  ├── Job queue
  ├── Worker pool
  └── Status tracking

Week 5: Advanced Patterns
  ├── Context
  ├── Timeouts
  └── Cancellation

Week 6: Production Ready
  ├── Graceful shutdown
  ├── Testing
  └── Documentation
```

## Success Criteria

You'll know you're making progress when you can:

- [ ] Explain what goroutines are
- [ ] Use channels to communicate between goroutines
- [ ] Implement a worker pool pattern
- [ ] Handle timeouts with context
- [ ] Write tests for concurrent code
- [ ] Implement graceful shutdown
- [ ] Debug race conditions

Remember: **Learning takes time. Be patient with yourself!**

Happy coding! 🚀
