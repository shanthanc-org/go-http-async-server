# test/unit

This directory contains unit tests.

## Purpose
- Store unit tests that test individual functions/methods in isolation
- Mock external dependencies
- Fast, focused tests

## What goes here
- Unit tests for handlers
- Unit tests for middleware
- Unit tests for worker logic
- Unit tests for utility functions
- Mock implementations

## Go Testing Conventions
- Test files end with `_test.go`
- Test functions start with `Test`
- Benchmark functions start with `Benchmark`
- Example functions start with `Example`

## Note
Go convention is to place test files alongside the code they test.
This directory is for additional test helpers or table-driven test data.
