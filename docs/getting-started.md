---
title: Getting Started
nav_order: 2
---

# Getting Started

This section is a complete setup path from a clean machine to your first successful execution.

## 1) Install Prerequisites

- Node.js 20+
- npm 10+
- Git
- Optional: Docker Desktop
- Optional: Appium and emulator/device for mobile tests

Verify your setup:

```bash
node -v
npm -v
git --version
```

## 2) Clone Repository and Install Dependencies

```bash
git clone <your-repo-url>
cd <your-repo-folder>
npm install
npx playwright install chromium
```

## 3) Configure Environment (Optional but Recommended)

Set environment values before execution:

- `BASE_URL` for web and API targets
- `MOBILE_ENABLED=true` when running mobile tests
- Integration credentials for defect tools if needed

## 4) Run the Framework

```bash
npm run test
```

This command validates default sample tests and generates execution artifacts.

## 5) Verify Output and Reports

- `reports/allure-results/`
- `reports/allure-report/index.html` after `npm run allure:generate`

Generate HTML report:

```bash
npm run allure:generate
```

## 6) Next Step

- Learn folder structure: [Architecture](./architecture.md)
- Learn test creation: [User Guide](./user-guide.md)
