---
title: Domain Concepts
last_reviewed: 2026-08-27
tags: [domains, glossary, memgraph, cypher]
folder: domains
sources:
  - code:charts/memgraph/Chart.yaml
  - code:charts/memgraph/memgraph.conf
agent_routing:
  code_change_relevance: conditional
  task_intents: [domain-behavior-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Domain Concepts

## Glossary

| Term | Definition |
| --- | --- |
| **Memgraph** | Open-source in-memory graph database built for real-time data processing |
| **Bolt Protocol** | TCP-based wire protocol used by Memgraph for client communication (port 7687) |
| **Cypher** | Declarative graph query language used by Memgraph |
| **StatefulSet** | Kubernetes resource for managing stateful applications with persistent storage |
| **PersistentVolumeClaim (PVC)** | Kubernetes storage request for data persistence |
| **High Availability (HA)** | Cluster configuration with multiple instances for fault tolerance |
| **RAFT** | Consensus algorithm used by coordinators for cluster management |
| **WAL (Write-Ahead Log)** | Durability mechanism that logs changes before they are applied |
| **Snapshot** | Periodic point-in-time dump of the in-memory graph data |
| **Coordinator** | HA cluster component managing metadata using RAFT consensus |
| **Data Instance** | HA cluster component storing and querying graph data |
| **Memgraph Lab** | Web-based visualization and management UI for Memgraph |
| **Prometheus Exporter** | Sidecar that exposes Memgraph metrics for Prometheus monitoring |
| **Helm Chart** | Kubernetes package defining the deployment of an application |

## Key Memgraph Configuration Settings

| Setting | Default | Description |
| --- | --- | --- |
| `--memory-limit` | Auto (90% of RAM) | Maximum memory usage in MiB |
| `--isolation-level` | SNAPSHOT_ISOLATION | Transaction isolation level |
| `--bolt-port` | 7687 | Bolt protocol port |
| `--data-directory` | `/var/lib/memgraph` | Persistent data directory |
| `--telemetry-enabled` | true | Anonymous usage telemetry |

## Source References

- `code:charts/memgraph/Chart.yaml` — Chart metadata with keywords and version.
- `code:charts/memgraph/memgraph.conf` — Database configuration reference with all available settings.
