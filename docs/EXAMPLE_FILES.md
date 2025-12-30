# Example File Structure

This document shows what actual files you might create in each directory as you build your project.

## cmd/server/
```
cmd/server/
├── main.go           # Application entry point
└── README.md         # Documentation
```

**main.go** would contain:
- Server initialization
- Router setup
- Middleware registration
- Graceful shutdown handling
- Signal handling

---

## internal/handler/
```
internal/handler/
├── health.go         # Health check endpoint
├── async.go          # Async job submission handler
├── status.go         # Job status query handler
├── handler.go        # Handler struct and common logic
└── README.md
```

Example handlers:
- `GET /health` - Health check
- `POST /async/task` - Submit async task
- `GET /async/task/:id` - Get task status
- `GET /async/task/:id/result` - Get task result

---

## internal/middleware/
```
internal/middleware/
├── logging.go        # Request/response logging
├── cors.go          # CORS headers
├── auth.go          # Authentication
├── ratelimit.go     # Rate limiting
├── timeout.go       # Request timeout
├── recovery.go      # Panic recovery
├── requestid.go     # Request ID tracking
└── README.md
```

Each middleware wraps the next handler in the chain.

---

## internal/worker/
```
internal/worker/
├── pool.go          # Worker pool implementation
├── job.go           # Job definition
├── queue.go         # Job queue management
├── status.go        # Job status tracking
└── README.md
```

Key components:
- **pool.go**: Worker pool that manages N goroutines
- **job.go**: Job interface and concrete job types
- **queue.go**: Channel-based job queue
- **status.go**: Thread-safe job status storage

---

## internal/config/
```
internal/config/
├── config.go        # Config struct and loading
├── validation.go    # Config validation
└── README.md
```

Configuration might include:
```go
type Config struct {
    ServerPort    int
    WorkerCount   int
    QueueSize     int
    Timeout       time.Duration
    LogLevel      string
}
```

---

## pkg/utils/
```
pkg/utils/
├── response.go      # HTTP response helpers
├── logger.go        # Logging utilities
├── uuid.go          # UUID generation
└── README.md
```

Utility functions that could be used by external packages.

---

## api/v1/
```
api/v1/
├── openapi.yaml     # OpenAPI specification
├── requests.go      # Request DTOs
├── responses.go     # Response DTOs
└── README.md
```

API contracts and data transfer objects.

---

## docs/
```
docs/
├── ARCHITECTURE.md          # System architecture
├── CONCURRENCY_PATTERNS.md  # Concurrency patterns guide
├── API.md                   # API documentation
├── DEPLOYMENT.md            # How to deploy
├── DEVELOPMENT.md           # Development guide
└── README.md
```

---

## test/unit/
```
test/unit/
├── helpers.go       # Test helper functions
├── mocks.go         # Mock implementations
└── README.md
```

Note: Go convention is to place `*_test.go` files next to the code they test.

---

## test/integration/
```
test/integration/
├── server_test.go   # End-to-end server tests
├── worker_test.go   # Worker pool integration tests
├── fixtures.go      # Test fixtures
└── README.md
```

Integration tests start a real server and make actual HTTP requests.

---

## Root Level Files
```
.
├── .gitignore              # Git ignore patterns
├── README.md               # Project overview
├── PROJECT_STRUCTURE.md    # This structure guide
├── go.mod                  # Go module definition
├── go.sum                  # Go module checksums
├── Makefile               # Build automation (optional)
└── Dockerfile             # Container definition (optional)
```

---

## Example File Counts by Phase

### Phase 1: Basic HTTP Server (Minimal)
- `cmd/server/main.go`
- `internal/handler/health.go`
- `internal/config/config.go`
- Total: ~3-5 files

### Phase 2: Add Concurrency (Growing)
- Previous files
- `internal/worker/pool.go`
- `internal/worker/job.go`
- `internal/handler/async.go`
- Total: ~8-10 files

### Phase 3: Production Ready (Complete)
- Previous files
- Multiple middleware files
- Multiple handler files
- Test files
- Documentation
- Total: ~20-30 files

---

## Testing Structure

Each package should have test files:
```
internal/handler/
├── health.go
├── health_test.go       # Unit tests for health.go
├── async.go
├── async_test.go        # Unit tests for async.go
└── handler_test.go      # Common test helpers
```

Test file naming convention: `{filename}_test.go`

---

## Building the Project

Typical development workflow:

```bash
# Initialize module (do this first!)
go mod init github.com/shanthanc-org/go-http-async-server

# Add dependencies as you use them
go get github.com/gorilla/mux  # Example router

# Update dependencies
go mod tidy

# Build
go build -o bin/server cmd/server/main.go

# Run
./bin/server

# Test
go test ./...

# Test with race detection
go test -race ./...

# Run specific tests
go test ./internal/worker

# Benchmark
go test -bench=. ./internal/worker
```

---

## Optional: Additional Directories

As your project grows, you might add:

```
├── scripts/         # Build and deployment scripts
├── deployments/     # Kubernetes, docker-compose, etc.
├── migrations/      # Database migrations (if using DB)
├── proto/          # Protocol buffer definitions (if using gRPC)
└── vendor/         # Vendored dependencies (optional)
```

---

## Key Takeaway

Start small with just a few files, then grow the structure as needed. Don't create every file upfront - create them when you have a specific need. The directory structure provides guidance on where files should go, not a mandate to create them all immediately.

**Recommended Starting Point:**
1. Create `go.mod`
2. Create `cmd/server/main.go` with basic HTTP server
3. Create `internal/handler/health.go` with a health check
4. Get it running first
5. Then incrementally add features
