---
title: Operations
nav_order: 5
---

# Operations

Use this page to run, monitor, and deploy the framework in local and enterprise environments.

## Run Commands

```bash
npm run test
npm run dev
```

## Execute with Payload File

```bash
curl -X POST http://localhost:3000/execute \
  -H "Content-Type: application/json" \
  -d @src/data/execution-payload.json
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

Generate report:

```bash
npm run allure:generate
```

## Deploy with Docker

Build image:

```bash
docker build -t cognitest-engine:latest .
```

Run service:

```bash
docker run --rm -p 3000:3000 cognitest-engine:latest
```

Compose stack:

```bash
docker compose up -d
```

## Deploy with Kubernetes

Use included Helm chart:

```bash
helm upgrade --install cognitest-engine ./helm-chart
```

Key chart files:

- `helm-chart/templates/deployment.yaml`
- `helm-chart/templates/service.yaml`
- `helm-chart/values.yaml`

## CI/CD Integration

- Jenkins pipeline template: `Jenkinsfile`
- Azure pipeline template: `azure-pipelines.yml`

Use these pipelines to run lint, typecheck, tests, and report publication in your CI flow.

## Troubleshooting

- Install browser runtime: `npx playwright install chromium`
- Check types: `npm run typecheck`
- Check lint: `npm run lint`

## Read Next

- Day-to-day usage: [User Guide](./user-guide.md)
- System internals: [Technical Documentation](./technical-documentation.md)
