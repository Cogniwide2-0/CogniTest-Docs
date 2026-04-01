---
title: Architecture
nav_order: 4
---

# Architecture

This page explains what each important folder does and how it is used in real test implementation.

## Framework Flow in One View

1. Request comes from CLI or `POST /execute`
2. Execution engine filters tests using suite and tags
3. Runner starts web/API/mobile driver per test platform
4. Test code runs from `src/tests/*`
5. Results are saved for Allure and response summary

## Architecture Diagram

```text
Client / CI
    |
    v
src/main.ts (Fastify API: /health, /execute)
    |
    v
src/execution/execution-engine.ts
    |
    +------------------------------+
    |                              |
    v                              v
Test filtering                Integration hooks
(suite/tags)                  (JIRA/ADO/RabbitMQ/DB)
    |
    v
src/execution/test-runner-service.ts
    |
    +----------+-----------+-----------+
    |          |           |           |
    v          v           v           v
Web driver   API driver  Mobile driver  Utilities
(Playwright) (Playwright)(Appium+WDIO) (logger/report)
    |          |           |
    +----------+-----------+
               |
               v
src/tests/web | src/tests/api | src/tests/mobile
               |
               v
reports/allure-results -> reports/allure-report
```

## Folder-by-Folder Explanation

### `src/main.ts`

- Entry point for service runtime
- Exposes `/health` and `/execute`
- Example use: receives execution payload and forwards to execution engine

### `src/execution/`

- `execution-engine.ts`: selects runnable tests and coordinates result output
- `test-runner-service.ts`: runs tests with retries, parallelism, and fail-fast
- Example: if payload has `tags: ["smoke"]`, only smoke-tagged tests are executed

### `src/core/`

- `base-test.ts`: shared `HybridTestCase` contract for all test platforms
- `core-actions.ts`: common reusable actions and locator healing entry points
- `drivers/`: platform drivers (`web-driver.ts`, `api-driver.ts`, `mobile-driver.ts`)
- Example: web test receives `page`, API test receives `apiContext`, mobile test receives `mobileDriver`

### `src/components/`

- Reusable feature-level objects for clean test logic
- `web/login-page.ts`: UI operations with healing selectors
- `api/health-client.ts`: API endpoint abstraction
- `mobile/login-screen.ts`: mobile screen interactions
- Example: tests call component methods instead of writing raw selectors every time

### `src/tests/`

- `web/`, `api/`, `mobile/`: actual executable test cases
- Example files: `smoke-web.test.ts`, `smoke-api.test.ts`, `smoke-mobile.test.ts`
- Best practice: keep assertions here, keep reusable actions in `components/`

### `src/data/`

- `sample-users.json`: externalized user data
- `execution-payload.json`: sample execution API payload
- `readers/json-reader.ts`: helper to load JSON test data
- Example: load user credentials in test from file instead of hardcoding

### `src/integrations/`

- Defect and infra integration contracts
- Includes JIRA, Azure DevOps, RabbitMQ, PostgreSQL clients
- Example: on failure, defect manager can push issue creation flow

### `src/utils/`

- Logger, visual helpers, and shared interfaces
- Example: central log format or visual comparison utilities

### `helm-chart/`, `Dockerfile`, `docker-compose.yml`

- Deployment assets for container and Kubernetes environments
- Example: run framework as service inside containerized CI jobs

## Read Next

- Step-by-step test creation examples: [User Guide](./user-guide.md)
- Deployment and execution controls: [Operations](./operations.md)
