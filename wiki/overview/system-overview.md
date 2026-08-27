---
title: Overview
last_reviewed: 2026-08-27
tags: [overview, memgraph, helm, kubernetes]
folder: overview
sources:
  - code:charts/memgraph/Chart.yaml
  - code:charts/memgraph/values.yaml
  - code:charts/memgraph-lab/Chart.yaml
  - code:charts/memgraph-lab/values.yaml
  - code:charts/memgraph-high-availability/Chart.yaml
  - code:charts/memgraph-high-availability/values.yaml
  - code:docker-compose/docker-compose.yml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Overview

This repository provides Helm charts for deploying **Memgraph**, an open-source in-memory graph database, on Kubernetes. Memgraph is a real-time, in-memory graph database built for processing complex relationships in data with low latency.

## Charts

Three Helm charts are provided:

| Chart | Description | Version | App Version |
| --- | --- | --- | --- |
| **memgraph** | Deploys a standalone Memgraph database instance as a StatefulSet with persistent storage | 0.2.16 | 3.8.1 |
| **memgraph-lab** | Deploys Memgraph Lab, a web-based visualization and management UI for Memgraph | 0.1.14 | 3.8.0 |
| **memgraph-high-availability** | Deploys a high-availability Memgraph cluster with two data instances and three coordinators (Enterprise feature) | 0.2.16 | 3.8.1 |

## Repository Layout

```
charts/
  memgraph/                  # Memgraph standalone chart
  memgraph-lab/              # Memgraph Lab visualization UI chart
  memgraph-high-availability/ # Memgraph HA cluster chart
docker-compose/              # Docker Compose configuration for local HA testing
scripts/                     # Deployment and debugging scripts for various cloud platforms
tutorials/                   # Deployment tutorials (e.g., GCP)
```

## Key Technologies

- **Helm** (v3) — Kubernetes package manager
- **Kubernetes** — container orchestration
- **Memgraph** — in-memory graph database (version 3.8.x)
- **Prometheus** — metrics and monitoring integration
- **Docker Compose** — local development and testing

## Dependency

The high-availability chart depends on the Prometheus community Helm chart (`prometheus-community/kube-prometheus-stack`) for monitoring.

## Source References

- `code:charts/memgraph/Chart.yaml` — Memgraph standalone chart metadata.
- `code:charts/memgraph-lab/Chart.yaml` — Memgraph Lab chart metadata.
- `code:charts/memgraph-high-availability/Chart.yaml` — HA chart metadata.
