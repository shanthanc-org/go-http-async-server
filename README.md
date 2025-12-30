# go-http-async-server

A learning project for building a Go HTTP server that processes multiple requests asynchronously using goroutines, channels, and worker pools.

## Purpose

This project is designed to help you master:
- Go concurrency primitives (goroutines, channels, select)
- Worker pool patterns
- Context-based cancellation and timeouts
- HTTP server development in Go
- Distributed systems concepts
- Graceful shutdown and error handling

## Project Structure

See [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) for a detailed explanation of the folder structure and how to use it.

```
├── cmd/server/         # Application entry point
├── internal/
│   ├── handler/       # HTTP handlers
│   ├── middleware/    # HTTP middleware
│   ├── worker/        # Worker pools and async processing
│   └── config/        # Configuration management
├── pkg/utils/         # Reusable utilities
├── api/v1/           # API specifications
├── docs/             # Documentation
└── test/             # Integration and unit tests
```

## Getting Started

**New to this project? Start here:** 👉 [QUICK_START.md](./QUICK_START.md)

The quick start guide will walk you through:
1. Initializing your Go module
2. Creating your first HTTP server
3. Adding handlers and middleware
4. Building incrementally

### Learning Path

1. **Initialize Go module**:
   ```bash
   go mod init github.com/shanthanc-org/go-http-async-server
   ```

2. **Start with the basics**:
   - Follow [QUICK_START.md](./QUICK_START.md) to create your first server
   - Implement a basic handler in `internal/handler/`
   - Run your server and test it

3. **Add concurrency**:
   - Read [docs/CONCURRENCY_PATTERNS.md](./docs/CONCURRENCY_PATTERNS.md)
   - Implement a worker pool in `internal/worker/`
   - Connect handlers to workers using channels
   - Test concurrent request processing

4. **Enhance with middleware**:
   - Add logging, authentication, rate limiting
   - Learn the middleware pattern

5. **Document your learning**:
   - Keep notes in `docs/` about patterns you implement
   - Write tests as you build

## Documentation

- **[QUICK_START.md](./QUICK_START.md)** - Get started in 5 steps!
- **[PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md)** - Detailed folder structure guide
- **[docs/CONCURRENCY_PATTERNS.md](./docs/CONCURRENCY_PATTERNS.md)** - Concurrency patterns to implement
- **[docs/EXAMPLE_FILES.md](./docs/EXAMPLE_FILES.md)** - Example file organization

Each directory has a README.md explaining its purpose and what should go there.

## Key Concepts

- **Goroutines**: Lightweight concurrent functions
- **Channels**: Communication between goroutines
- **Worker Pool**: Fixed number of workers processing from a queue
- **Context**: Cancellation and timeout handling
- **Graceful Shutdown**: Clean server shutdown

## Testing

Run tests with race detection:
```bash
go test -race ./...
```

## License

This is a personal learning project.
