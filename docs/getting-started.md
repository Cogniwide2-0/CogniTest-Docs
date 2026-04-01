---
title: Getting Started
nav_order: 2
---

# Getting Started

This is the quickest path from zero setup to first test run.

## 1) Install Prerequisites

- Node.js 20+
- npm 10+
- Optional: Docker Desktop
- Optional: Appium and emulator/device for mobile tests

## 2) Clone and Install

```bash
git clone <your-repo-url>
cd <your-repo-folder>
npm install
npx playwright install chromium
```

## 3) Create Your First Test

Use one of these folders:

- `src/tests/web/`
- `src/tests/api/`
- `src/tests/mobile/`

## 4) Run and Verify

```bash
npm run test
```

Then check:

- `reports/allure-results/`
- `reports/allure-report/index.html` after `npm run allure:generate`

## Need More Detail

- Full instructions: [User Guide](./user-guide.md)
- Runtime and CI operations: [Operations](./operations.md)
