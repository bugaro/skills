# Architecture 

## Technology Stack

- **Language:** TypeScript (`v6.0+`)
- **Runtime:** Node.js (v24 LTS) — native `--experimental-strip-types`, no build step in dev
- **Framework:** Hono (`^4.12+`) via `@hono/node-server`
- **Infrastructure:**
  - Docker & Docker Compose
- **Observability:**
  - **Metrics:** Prometheus (`v3.11.3`) · `prom-client ^15.1.2`
  - **Logs:** Grafana Loki (`3.7.1`) · Pino structured JSON (`^9.0.0`)
  - **Tracing:** OpenTelemetry SDK (`@opentelemetry/sdk-node ^0.220.0`) · OTLP gRPC exporter · `x-correlation-id` propagation via `AsyncLocalStorage`
  - **Visualization:** Grafana (`13.0.1`)
  - **Collector:** Grafana Alloy (`v1.16.0`) — unified log, metric & trace collection

## Orchestration
`./infrastructure/docker-compose.orchestration.yml` up all services
