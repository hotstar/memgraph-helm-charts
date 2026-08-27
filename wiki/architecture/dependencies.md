---
title: Dependencies
last_reviewed: 2026-08-27
tags: [architecture, dependencies, external]
folder: architecture
sources:
  - code:charts/memgraph-high-availability/Chart.yaml
  - code:.github/workflows/lint-test-high-availability.yaml
  - code:.github/workflows/release.yml
agent_routing:
  code_change_relevance: conditional
  task_intents: [architecture-change]
  applies_to_paths:
    - charts/memgraph-high-availability/**
  applies_to_symbols: []
---

# Dependencies

## External Dependencies

| Dependency | Version | Purpose | Used By |
| --- | --- | --- | --- |
| **Helm** | >= 3 | Kubernetes package manager | All charts |
| **Kubernetes** | Any | Container orchestration platform | All charts |
| **Memgraph** | 3.8.x | In-memory graph database | All charts |
| **Memgraph Lab** | 3.8.0 | Web UI for graph visualization | `memgraph-lab` chart |
| **kube-prometheus-stack** | Latest | Prometheus monitoring stack | `memgraph-high-availability` chart |
| **Prometheus Exporter** | 0.2.1 | Memgraph metrics exporter | `memgraph` and HA charts |
| **Busybox** | Latest | Init container for sysctl configuration | `memgraph` chart |
| **Docker Compose** | Latest | Local HA cluster testing | docker-compose/ |

## Chart Dependencies

The `memgraph-high-availability` chart depends on the Prometheus community Helm chart:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm dependency update charts/memgraph-high-availability
```

## CI Tooling Dependencies

| Tool | Version | Purpose |
| --- | --- | --- |
| **chart-testing (ct)** | v2.6.1 | Helm chart linting and testing |
| **pre-commit** | Latest | Code quality checks |
| **Minikube** | Latest | Local Kubernetes cluster for HA testing |
| **kind** | Latest | Local Kubernetes cluster for standalone testing |
| **Python** | 3.12 | chart-testing runtime |

## Source References

- `code:charts/memgraph-high-availability/Chart.yaml` — HA chart metadata (implicit Prometheus dependency).
- `code:.github/workflows/lint-test-high-availability.yaml` — CI workflow showing Minikube and chart-testing setup.
- `code:.github/workflows/release.yml` — Release workflow showing Prometheus repo dependency.
