---
title: Architecture
nav_order: 4
---

# Architecture

This page gives a quick technical picture without deep detail.

## Main Building Blocks

- Service layer: receives `/execute` requests
- Execution layer: filters and runs tests
- Driver layer: web, API, and mobile runtime
- Test layer: platform-specific test files
- Utility layer: logging and report helpers

## Simple Flow

1. Receive execution request
2. Select tests by suite/tags
3. Start platform driver
4. Execute and collect artifacts
5. Return summary

## Read Next

- Full technical details: [Technical Documentation](./technical-documentation.md)
- Practical test execution: [Operations](./operations.md)
