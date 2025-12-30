# Go Concurrency Patterns for HTTP Servers

This document outlines common concurrency patterns you'll implement in this project.

## 1. Worker Pool Pattern

### Concept
Instead of spawning unlimited goroutines, maintain a fixed pool of workers that process jobs from a queue.

### Benefits
- Controlled resource usage
- Prevents system overload
- Better performance under high load

### Key Components
```
Job Queue (channel) → Worker Pool (goroutines) → Results
```

### Implementation Checklist
- [ ] Define Job struct
- [ ] Create buffered channel for jobs
- [ ] Spawn fixed number of worker goroutines
- [ ] Workers read from job channel in loop
- [ ] Handle graceful shutdown

## 2. Fan-Out/Fan-In Pattern

### Concept
Distribute work to multiple goroutines (fan-out), then aggregate results (fan-in).

### Use Case
Process independent tasks concurrently and combine results.

### Key Components
```
Input → [Worker 1, Worker 2, Worker 3] → Results Channel → Aggregated Output
```

### Implementation Notes
- Use multiple goroutines to process work in parallel
- Each worker sends results to a shared results channel
- Aggregator reads from results channel
- Use `sync.WaitGroup` to know when all workers are done
- Close results channel after all workers complete

## 3. Pipeline Pattern

### Concept
Chain of processing stages where each stage runs concurrently.

### Key Components
```
Stage 1 → Channel → Stage 2 → Channel → Stage 3 → Output
```

### Benefits
- Each stage can process different items concurrently
- Stages can have different processing rates
- Clean separation of concerns

## 4. Request-Context Pattern

### Concept
Use `context.Context` to carry deadlines, cancellation signals, and request-scoped values.

### Key Usage
```go
ctx := r.Context() // Get context from HTTP request
ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
defer cancel()
```

### Best Practices
- Always pass context as first parameter
- Respect cancellation in long-running operations
- Use `context.WithTimeout` for operations with deadlines
- Check `ctx.Done()` in loops and between operations

## 5. Graceful Shutdown Pattern

### Concept
Allow in-flight requests to complete before shutting down server.

### Implementation Steps
1. Listen for OS signals (SIGINT, SIGTERM)
2. Call `server.Shutdown(ctx)` when signal received
3. Wait for active connections to complete
4. Stop worker pools and close channels
5. Exit cleanly

### Key APIs
```go
signal.Notify() // Catch OS signals
server.Shutdown(ctx) // Graceful HTTP server shutdown
sync.WaitGroup // Wait for workers to finish
```

## 6. Rate Limiting Pattern

### Concept
Control the rate of operations using time-based limiters.

### Approaches

#### Token Bucket
```go
limiter := time.NewTicker(time.Second / 10) // 10 requests per second
for range limiter.C {
    // Process one request
}
```

#### Semaphore Pattern
```go
sem := make(chan struct{}, 10) // Max 10 concurrent operations
sem <- struct{}{} // Acquire
// Do work
<-sem // Release
```

## 7. Error Handling in Concurrent Code

### Challenges
- Errors occur in different goroutines
- Need to aggregate errors
- Must stop all workers on critical error

### Solutions

#### Error Channel
```go
errCh := make(chan error, numWorkers)
// Workers send errors to errCh
// Main goroutine monitors errCh
```

#### errgroup Package
```go
g, ctx := errgroup.WithContext(ctx)
g.Go(func() error {
    // Work that might error
})
err := g.Wait() // Wait for all, returns first error
```

## 8. Job Status Tracking

### Concept
Track status of async jobs so clients can query progress.

### Implementation
```go
type JobStatus struct {
    ID        string
    Status    string // "pending", "running", "completed", "failed"
    Result    interface{}
    Error     error
    CreatedAt time.Time
    UpdatedAt time.Time
}

// Store in map with mutex protection
var (
    jobs   = make(map[string]*JobStatus)
    jobsMu sync.RWMutex
)
```

### API Endpoints
- `POST /jobs` - Submit new job, returns job ID
- `GET /jobs/{id}` - Check job status
- `GET /jobs/{id}/result` - Get job result (if completed)

## 9. Timeout Pattern

### Concept
Ensure operations don't run indefinitely.

### Using Select
```go
select {
case result := <-resultCh:
    // Handle result
case <-time.After(5 * time.Second):
    // Handle timeout
case <-ctx.Done():
    // Handle cancellation
}
```

### Using Context
```go
ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
defer cancel()
// Pass ctx to operations
```

## 10. Channel Patterns

### Buffered vs Unbuffered
- **Unbuffered**: Synchronous, sender blocks until receiver reads
- **Buffered**: Asynchronous, sender only blocks when buffer is full

### Selecting on Multiple Channels
```go
select {
case job := <-jobCh:
    // Process job
case <-stopCh:
    // Stop processing
case <-time.After(timeout):
    // Handle timeout
default:
    // Non-blocking operation
}
```

### Closing Channels
- Only sender should close channel
- Closing signals "no more values"
- Reading from closed channel returns zero value and false
- Range loops automatically stop when channel closes

## Testing Concurrent Code

### Race Detector
```bash
go test -race ./...
```

### Stress Testing
Run tests multiple times to catch race conditions:
```bash
go test -race -count=100 ./...
```

### Benchmarking
```bash
go test -bench=. -benchmem ./...
```

### Testing Patterns
- Use `testing.T.Parallel()` for parallel tests
- Test with various number of goroutines
- Test cancellation scenarios
- Test timeout scenarios
- Test graceful shutdown

## Common Pitfalls

1. **Goroutine Leaks**: Always ensure goroutines can exit
2. **Race Conditions**: Use mutexes or channels to protect shared data
3. **Deadlocks**: Be careful with channel operations and locks
4. **Closing Closed Channels**: Causes panic, only sender should close
5. **Not Using Context**: Missing cancellation and timeout handling
6. **Ignoring WaitGroup**: Can cause premature shutdown

## Debugging Tips

1. Add logging with goroutine IDs
2. Use `runtime.NumGoroutine()` to detect leaks
3. Enable race detector during development
4. Use `pprof` for profiling
5. Add metrics for queue depths, worker utilization

## Further Reading

- [Go Concurrency Patterns: Pipelines and cancellation](https://blog.golang.org/pipelines)
- [Go Concurrency Patterns: Context](https://blog.golang.org/context)
- [Go Concurrency Patterns: Timing out, moving on](https://blog.golang.org/go-concurrency-patterns-timing-out-and)
- [Advanced Go Concurrency Patterns](https://talks.golang.org/2013/advconc.slide)

## Implementation Order

Suggested order to implement patterns:

1. **Start Simple**: Basic HTTP server with goroutine per request
2. **Add Worker Pool**: Control concurrency
3. **Add Context**: Timeout and cancellation
4. **Add Status Tracking**: Job status API
5. **Add Graceful Shutdown**: Clean exit
6. **Add Rate Limiting**: Control load
7. **Add Monitoring**: Metrics and logging
8. **Optimize**: Profile and improve

Remember: Start simple, add complexity gradually, and test thoroughly at each step!
