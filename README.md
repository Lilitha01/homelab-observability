# homelab-observability

A personal observability stack running on a local home server via Docker Compose.

## Stack

| Component | Role | Port |
|---|---|---|
| Prometheus | Metrics scraping and storage | 9090 |
| Node Exporter | Host metrics (CPU, memory, disk, network) | 9100 |
| Loki | Log aggregation | 3100 |
| Promtail | Log shipping to Loki | — |
| Grafana | Dashboards and visualisation | 3000 |

## Structure

```
config/
  loki/promtail.yml         — Loki storage, schema, and retention config
  prometheus/prometheus.yml — Scrape config (Prometheus + Node Exporter)
  promtail/promtail.yml     — Log scraping and Loki push config
scripts/
  health_checker.py         — CLI script to verify all services are reachable
docker-compose.yml
```

## Getting started

```bash
docker compose up -d
```

Grafana will be available at `http://localhost:3000`. Default credentials are `admin / admin` — change these.

Prometheus scrapes metrics every 15 seconds and retains data for 15 days. Promtail tails `/var/log/*log` on the host and ships to Loki. Node Exporter exposes host-level metrics via `/proc` and `/sys`.

## Health check

```bash
pip install requests
python3 scripts/health_checker.py
```

See `scripts/health_checker.py` for usage and configuration.
