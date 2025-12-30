# internal/middleware

This directory contains HTTP middleware components.

## Purpose
- Implement reusable middleware that wraps handlers
- Handle cross-cutting concerns (logging, auth, rate limiting, etc.)
- Process requests before they reach handlers
- Process responses before they're sent to clients

## What goes here
- Logging middleware
- Authentication/authorization middleware
- Rate limiting middleware
- CORS middleware
- Request ID tracking
- Panic recovery middleware
- Timeout middleware

## Middleware Pattern
Each middleware should follow the standard Go HTTP middleware pattern:
```go
func MiddlewareName(next http.Handler) http.Handler
```
