---
title: Data Flow
last_reviewed: 2026-08-27
tags: [data, data-flow, network]
folder: data
sources:
  - code:charts/memgraph/templates/service.yaml
  - code:charts/memgraph-high-availability/templates/services-coordinators.yaml
  - code:charts/memgraph-high-availability/templates/services-data.yaml
  - code:charts/memgraph-high-availability/templates/cluster-setup.yaml
  - code:docker-compose/docker-compose.yml
  - code:docker-compose/HA_register.cypher
agent_routing:
  code_change_relevance: conditional
  task_intents: [data-model-change]
  applies_to_paths:
    - charts/memgraph-high-availability/**
  applies_to_symbols: []
---

# Data Flow

## Client to Memgraph (Standalone)

```
Client (Bolt protocol:7687)
    |
    v
Kubernetes Service (ClusterIP/LoadBalancer)
    |
    v
Memgraph StatefulSet Pod
    |
    ├── In-Memory Graph Data
    ├── Periodic Snapshots --> PersistentVolume (data)
    └── Write-Ahead Log --> PersistentVolume (log)
```

## Client to HA Cluster

```
Client (Bolt protocol)
    |
    v
LoadBalancer / Ingress
    |
    v
Coordinator Services
    |         |         |
  Coordinator 1, 2, 3 (RAFT consensus)
    |
    v
Data Services
    |       |
  Data 1   Data 2 (replication)
    |
    v
Persistent Volumes (per instance)
```

## Monitoring Data Flow

```
Memgraph (HTTP monitoring :9091)
    |
    v
Prometheus Exporter (port 9115, pull every 5s)
    |
    v
ServiceMonitor (scrape interval 15s)
    |
    v
Prometheus / kube-prometheus-stack
```

## Docker Compose Data Flow (Local HA Testing)

```
docker-compose.yml defines:
- 2 data instances
- 3 coordinator instances
- Bridged Docker network for communication
- HA registration via HA_register.cypher
```

The `HA_register.cypher` script registers the cluster instances after initial deployment.

## Source References

- `code:charts/memgraph/templates/service.yaml` — Standalone service definition.
- `code:charts/memgraph-high-availability/templates/services-coordinators.yaml` — Coordinator service definitions.
- `code:charts/memgraph-high-availability/templates/services-data.yaml` — Data instance service definitions.
- `code:charts/memgraph-high-availability/templates/cluster-setup.yaml` — Cluster initialization Job.
- `code:docker-compose/HA_register.cypher` — Cypher script for HA cluster registration.
