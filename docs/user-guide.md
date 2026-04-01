---
title: User Guide
nav_order: 3
---

# Cognitest Engine User Guide

This guide shows complete, practical flow for web, API, and mobile testing: test creation, test data externalization, locator strategy, execution, and report generation.

## Quick Navigation

- Setup and prerequisites
- Test design model (`HybridTestCase`)
- Web/API/Mobile sample tests
- External test data and locators
- End-to-end execution and reporting

## 1) Setup and Prerequisites

```bash
git clone <your-repo-url>
cd <your-repo-folder>
npm install
npx playwright install chromium
```

For mobile execution also prepare:

- Appium server
- Android/iOS emulator or real device

## 2) Test Design Model

Every test follows `HybridTestCase` from `src/core/base-test.ts`:

- `id`, `name`, `suite`
- `platform` as `web`, `api`, or `mobile`
- `tags` for filtering
- `run(context)` with platform context (`page`, `apiContext`, or `mobileDriver`)

Create test files in:

- Web: `src/tests/web/`
- API: `src/tests/api/`
- Mobile: `src/tests/mobile/`

## 3) Web Test Case Example

Use `LoginPage` and `CoreActions` for reusable actions and healing locators.

```ts
import type { HybridTestCase } from "../../core/base-test";
import { CoreActions } from "../../core/core-actions";
import { LoginPage } from "../../components/web/login-page";
import { readJsonData } from "../../data/readers/json-reader";

type User = { username: string; password: string };

export const webLoginTest: HybridTestCase = {
  id: "WEB-010",
  name: "Web user can login",
  suite: "smoke",
  platform: "web",
  tags: ["login", "web"],
  run: async ({ page }) => {
    if (!page) {
      throw new Error("Web page context missing");
    }

    const users = await readJsonData<User[]>("src/data/sample-users.json");
    const actions = new CoreActions(page);
    const loginPage = new LoginPage(actions);

    await page.goto("https://example.com/login");
    await loginPage.login(users[0].username, users[0].password);
    await actions.clickWithHealing(["[data-testid='profile']", "#profileMenu"]);
  }
};
```

## 4) API Test Case Example

Use API client classes from `src/components/api/` for clean API test design.

```ts
import type { HybridTestCase } from "../../core/base-test";
import { HealthClient } from "../../components/api/health-client";

export const apiHealthTest: HybridTestCase = {
  id: "API-010",
  name: "Health API returns expected response",
  suite: "smoke",
  platform: "api",
  tags: ["health", "api"],
  run: async ({ apiContext }) => {
    if (!apiContext) {
      throw new Error("API context missing");
    }

    const client = new HealthClient(apiContext);
    const response = await client.getHealth();
    if (!response.ok()) {
      throw new Error(`Health API failed: ${response.status()}`);
    }
  }
};
```

## 5) Mobile Test Case Example

Use mobile screen objects from `src/components/mobile/` and enable mobile in environment.

```ts
import type { HybridTestCase } from "../../core/base-test";
import { SkipTestError } from "../../core/base-test";
import { LoginScreen } from "../../components/mobile/login-screen";
import { readJsonData } from "../../data/readers/json-reader";

type User = { username: string; password: string };

export const mobileLoginTest: HybridTestCase = {
  id: "MOB-010",
  name: "Mobile user can login",
  suite: "smoke",
  platform: "mobile",
  tags: ["login", "mobile"],
  run: async ({ mobileDriver }) => {
    if (process.env.MOBILE_ENABLED !== "true") {
      throw new SkipTestError("Mobile execution disabled");
    }
    if (!mobileDriver) {
      throw new Error("Mobile driver not initialized");
    }

    const users = await readJsonData<User[]>("src/data/sample-users.json");
    const screen = new LoginScreen(mobileDriver);
    await screen.login(users[0].username, users[0].password);
  }
};
```

## 6) Test Data Externalization

Keep test data in files and read it at runtime:

- Reader utility: `src/data/readers/json-reader.ts`
- Example data file: `src/data/sample-users.json`

Example:

```ts
const users = await readJsonData<Array<{ username: string; password: string }>>(
  "src/data/sample-users.json"
);
```

Benefits:

- No hardcoded credentials in test files
- Easy environment-specific data replacement
- Reusable data across web/API/mobile scenarios

## 7) Locator Strategy

Use stable selectors first and fallback selectors second.

Recommended order:

1. `data-testid` or accessibility ID
2. Stable `id` or `name`
3. CSS/XPath as fallback

Current project examples:

- Web healing selectors in `src/components/web/login-page.ts`
- Mobile accessibility IDs in `src/components/mobile/login-screen.ts`
- Healing helpers in `src/core/core-actions.ts`

## 8) End-to-End Execution Flow

Run test suite directly:

```bash
npm run test
```

Run through service:

```bash
npm run dev
curl -X POST http://localhost:3000/execute \
  -H "Content-Type: application/json" \
  -d @src/data/execution-payload.json
```

Execution payload file:

- `src/data/execution-payload.json`

## 9) Report Generation

After execution:

- Raw result files: `reports/allure-results/`
- Generate HTML report:

```bash
npm run allure:generate
```

Open report:

```text
reports/allure-report/index.html
```

## 10) Troubleshooting

- Browser missing: `npx playwright install chromium`
- Mobile execution issue: validate Appium server and device
- Type check:

```bash
npm run typecheck
```

- Lint:

```bash
npm run lint
```
