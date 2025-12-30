# internal/handler

This directory contains HTTP request handlers.

## Purpose
- Define HTTP request handlers that process incoming requests
- Parse request data and validate input
- Coordinate with workers for async processing
- Return appropriate HTTP responses

## What goes here
- HTTP handler functions/methods
- Request/response models specific to handlers
- Handler registration logic
- Route-specific business logic coordination

## Example handlers
- `health.go` - Health check endpoint
- `async.go` - Handlers that dispatch work to worker pools
- `status.go` - Check status of async jobs
