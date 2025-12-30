# cmd/server

This directory contains the main application entry point.

## Purpose
- `main.go` - The main entry point for your HTTP server
- Initializes configuration, dependencies, and starts the server
- Sets up graceful shutdown handling
- Wires together all components (handlers, middleware, workers)

## What goes here
- Application bootstrapping code
- Server initialization and startup logic
- Dependency injection setup
- Signal handling for graceful shutdown
