# Project Structure Guide

This document explains the folder structure for the go-http-async-server project.

## Overview

This project follows Go best practices and the standard Go project layout. The structure is designed to help you learn Go concurrency, distributed systems concepts, and build a scalable HTTP server.

## Directory Structure

```
go-http-async-server/
├── cmd/                    # Application entry points
│   └── server/            # Main HTTP server application
│       ├── main.go        # Entry point (you'll create this)
│       └── README.md      # Guidance for this directory
│
├── internal/              # Private application code
│   ├── handler/          # HTTP request handlers
│   ├── middleware/       # HTTP middleware components
│   ├── worker/           # Async worker pools and concurrency logic
│   └── config/           # Configuration management
│
├── pkg/                   # Public library code (reusable by external projects)
│   └── utils/            # Utility functions
│
├── api/                   # API contracts and specifications
│   └── v1/               # Version 1 of your API
│
├── docs/                  # Documentation
│
├── test/                  # Additional test files
│   ├── integration/      # Integration tests
│   └── unit/             # Unit test helpers
│
├── .gitignore            # Git ignore file
└── README.md             # Project README
```

## Key Design Principles

### 1. Separation of Concerns
- **cmd/** - Application entry points (main packages)
- **internal/** - Private application logic (cannot be imported by other projects)
- **pkg/** - Public, reusable libraries (can be imported by other projects)

### 2. Internal Package
The `internal/` directory is a special Go convention. Code in `internal/` cannot be imported by code outside of this project. This enforces encapsulation and prevents external dependencies on your internal implementation.

### 3. Handler-Worker Pattern
For async request processing:
- **Handlers** receive HTTP requests and quickly return responses
- **Workers** process tasks asynchronously in the background
- Communication happens via channels or job queues

## Learning Path

As you build this project, here's a suggested order to implement components:

### Phase 1: Basic HTTP Server
1. Create `cmd/server/main.go` with a basic HTTP server
2. Implement a simple handler in `internal/handler/`
3. Add basic middleware (logging) in `internal/middleware/`
4. Learn: Go's `net/http` package, handler functions, ServeMux

### Phase 2: Configuration
1. Create configuration struct in `internal/config/`
2. Load config from environment variables
3. Learn: `os` package, struct tags, environment variables

### Phase 3: Concurrency & Workers
1. Implement a worker pool in `internal/worker/`
2. Create job queue using channels
3. Connect handlers to worker pool
4. Learn: Goroutines, channels, `sync` package, worker pool pattern

### Phase 4: Advanced Patterns
1. Implement context-based cancellation
2. Add timeout handling
3. Implement graceful shutdown
4. Learn: Context package, signal handling, graceful shutdown

### Phase 5: Middleware & Cross-cutting Concerns
1. Add authentication middleware
2. Add rate limiting
3. Add request tracing
4. Learn: Middleware pattern, rate limiting algorithms

### Phase 6: Testing
1. Write unit tests for handlers and workers
2. Write integration tests for end-to-end flows
3. Add benchmarks for concurrent operations
4. Learn: `testing` package, table-driven tests, benchmarks

## Important Go Concepts for Async HTTP Servers

### Goroutines
Lightweight threads managed by Go runtime. Use for concurrent operations.

### Channels
Pipes for communication between goroutines. Essential for coordinating async work.

### Context
Used for cancellation, deadlines, and request-scoped values. Critical for HTTP servers.

### sync Package
- `WaitGroup` - Wait for multiple goroutines to finish
- `Mutex` - Protect shared data
- `Once` - Run initialization code exactly once

### Worker Pool Pattern
Instead of spawning unlimited goroutines, create a fixed pool of workers that process jobs from a queue. This controls resource usage.

## Example Flow

Here's how an async request might flow through your system:

1. Client sends HTTP request
2. Middleware processes request (logging, auth, etc.)
3. Handler receives request
4. Handler creates a job and sends it to worker pool via channel
5. Handler immediately returns response with job ID
6. Worker goroutine picks up job from channel
7. Worker processes job asynchronously
8. Client can query status using job ID

## Go Modules

Don't forget to initialize your Go module:
```bash
go mod init github.com/shanthanc-org/go-http-async-server
```

## Resources for Learning

- [Effective Go](https://golang.org/doc/effective_go.html)
- [Go Concurrency Patterns](https://www.youtube.com/watch?v=f6kdp27TYZs) - Rob Pike talk
- [Advanced Go Concurrency Patterns](https://www.youtube.com/watch?v=QDDwwePbDtw) - Rob Pike talk
- [Go by Example - Goroutines](https://gobyexample.com/goroutines)
- [Standard Go Project Layout](https://github.com/golang-standards/project-layout)

## Best Practices

1. **Keep handlers thin** - They should coordinate, not contain business logic
2. **Use contexts everywhere** - Pass context through your call stack
3. **Close channels** - The sender should close channels, not receivers
4. **Avoid goroutine leaks** - Always ensure goroutines can exit
5. **Handle errors explicitly** - Don't ignore errors
6. **Test concurrent code** - Use `-race` flag to detect race conditions
7. **Document concurrency** - Comment on goroutine lifetimes and channel usage

## Testing Concurrency

Use these flags when testing:
```bash
go test -race ./...           # Detect race conditions
go test -cover ./...          # Code coverage
go test -bench . ./...        # Benchmarks
go test -timeout 30s ./...    # Prevent hanging tests
```

## Common Patterns You'll Implement

1. **Worker Pool** - Fixed number of workers processing from a job queue
2. **Fan-out/Fan-in** - Distribute work to many goroutines, aggregate results
3. **Pipeline** - Chain of stages connected by channels
4. **Cancellation** - Use context to stop in-progress work
5. **Rate Limiting** - Control request rate using time.Ticker or semaphores
6. **Graceful Shutdown** - Wait for in-flight requests before exiting

## Next Steps

1. Initialize Go module: `go mod init github.com/shanthanc-org/go-http-async-server`
2. Start with `cmd/server/main.go` - Create a basic HTTP server
3. Add one feature at a time
4. Write tests as you go
5. Experiment with different concurrency patterns
6. Document your learnings in `docs/`

Happy learning! 🚀
