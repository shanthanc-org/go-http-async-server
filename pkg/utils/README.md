# pkg/utils

This directory contains shared utility functions.

## Purpose
- Provide reusable utility functions
- Helper functions that could be used by external packages
- Generic, application-agnostic code

## What goes here
- Common helper functions
- Utility types
- String manipulation helpers
- Time/date utilities
- Conversion functions
- HTTP response helpers

## Note
Code in `pkg/` is intended to be importable by external projects.
If utilities are application-specific, consider putting them in `internal/` instead.
