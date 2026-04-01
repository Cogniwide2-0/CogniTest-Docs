---
title: Technical Documentation
nav_order: 6
---

# Cognitest Engine Technical Documentation

This document explains how the framework is structured, how execution flows internally, and where to extend it for enterprise scale.

{: .warning }
Treat integration sections as extension contracts. Validate authentication, rate limits, and payload schemas before production rollout.

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
- `src/execution/`: test selection and run orchestration
- `src/core/drivers/`: platform drivers
- `src/tests/`: test cases
- `src/components/`: reusable page objects and API clients
- `src/utils/`: logging and utilities

## 3) Execution Flow

1. Client sends `POST /execute`.
2. Engine filters tests by suite and tags.
3. Runner starts proper platform driver.
4. Test executes with retry and optional fail-fast.
5. Results and artifacts are written.
6. Summary is returned in JSON.

## 4) Example Execution Payload

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

## 5) Operations Notes

- Start service: `npm run dev`
- Run tests directly: `npm run test`
- Generate report: `npm run allure:generate`
- Report output: `reports/allure-report/index.html`

## 6) Extension Points

- Add new tests under `src/tests/`
- Add reusable components under `src/components/`
- Extend integrations under `src/integrations/`
- Add custom reporting in `src/utils/` or execution layer
