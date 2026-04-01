---
title: Technical Documentation
nav_order: 6
---

# Cognitest Engine Technical Documentation

This document explains framework internals in a practical way, including source structure, execution lifecycle, and extension points.

## Quick Navigation

- Architecture at a glance
- Core module map
- Request-to-result flow
- Deployment and operations touchpoints

## 1) Architecture at a Glance

Cognitest Engine runs three automation channels in one framework:

- Web: Playwright browser automation
- API: Playwright request context
- Mobile: Appium + WebdriverIO

The service accepts execution requests and returns a summary with artifacts.

## 2) Core Module Map

Key folders:

- `src/main.ts`: service entry point
- `src/config/`: environment and capability loaders
- `src/execution/`: test selection and run orchestration
- `src/core/`: base test model, reusable actions, platform drivers
- `src/components/`: web pages, API clients, mobile screens
- `src/data/`: payload and user datasets plus file readers
- `src/core/drivers/`: platform drivers
- `src/tests/`: test cases
- `src/integrations/`: JIRA, ADO, RabbitMQ, and DB connectors
- `src/ai_agents/`: contracts for AI-based generation workflows
- `src/utils/`: logging and utilities

## 3) Execution Flow

1. Client sends `POST /execute`.
2. Engine filters tests by suite and tags.
3. Runner starts proper platform driver.
4. Test executes with retry and optional fail-fast.
5. Results and artifacts are written.
6. Summary is returned in JSON.

## 4) Folder Examples

- `src/components/web/login-page.ts` defines reusable login action with healing selectors.
- `src/components/api/health-client.ts` wraps endpoint request logic.
- `src/components/mobile/login-screen.ts` defines mobile element actions using accessibility IDs.
- `src/data/readers/json-reader.ts` loads JSON test data files.
- `src/tests/web/smoke-web.test.ts` demonstrates web smoke validation.
- `src/tests/api/smoke-api.test.ts` demonstrates API health assertion.
- `src/tests/mobile/smoke-mobile.test.ts` demonstrates mobile smoke flow.

## 5) How to Add a New Runnable Test

1. Add a new test file in `src/tests/web/`, `src/tests/api/`, or `src/tests/mobile/`.
2. Follow `HybridTestCase` structure.
3. Import and register the test inside `src/execution/execution-engine.ts`.
4. Run with `npm run test` or `POST /execute`.

## 6) Example Execution Payload

```json
{
  "suite": "smoke",
  "tags": ["smoke"],
  "parallelism": 1,
  "retries": 0,
  "failFast": false,
  "defectProvider": "none"
}
```

## 7) Operations Notes

- Start service: `npm run dev`
- Run tests directly: `npm run test`
- Generate report: `npm run allure:generate`
- Report output: `reports/allure-report/index.html`

## 8) Extension Points

- Add new tests under `src/tests/`
- Add reusable components under `src/components/`
- Extend integrations under `src/integrations/`
- Add custom reporting in `src/utils/` or execution layer
