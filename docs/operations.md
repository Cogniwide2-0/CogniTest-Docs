---
title: Operations
nav_order: 5
---

# Operations

Use this page to run, monitor, and troubleshoot test execution.

## Run Commands

```bash
npm run test
npm run dev
```

## Trigger by API

```bash
curl -X POST http://localhost:3000/execute \
  -H "Content-Type: application/json" \
  -d '{"suite":"smoke","tags":["smoke"],"parallelism":1,"retries":0,"failFast":false,"defectProvider":"none"}'
```

## Reports

- Raw output: `reports/allure-results/`
- HTML report: `reports/allure-report/index.html`

## Troubleshooting

- Install browser runtime: `npx playwright install chromium`
- Check types: `npm run typecheck`
- Check lint: `npm run lint`

## Read Next

- Day-to-day usage: [User Guide](./user-guide.md)
- System internals: [Technical Documentation](./technical-documentation.md)
