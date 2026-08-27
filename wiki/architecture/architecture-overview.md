---
title: Architecture
last_reviewed: 2026-08-27
tags: [architecture, component, service-boundaries]
folder: architecture
sources:
  - code:charts/memgraph/templates/statefulset.yaml
  - code:charts/memgraph/templates/service.yaml
  - code:charts/memgraph/templates/mg-exporter.yaml
  - code:charts/memgraph-high-availability/templates/coordinators.yaml
  - code:charts/memgraph-high-availability/templates/data.yaml
  - code:charts/memgraph-high-availability/templates/cluster-setup.yaml
  - code:charts/memgraph-lab/templates/deployment.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [architecture-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Architecture

## System Components

### Memgraph Database

Memgraph is an in-memory graph database that uses the Cypher query language. It stores data in-memory with optional persistence to disk via periodic snapshots and write-ahead logging (WAL).

**Ports:**
- Bolt protocol: 7687 (primary query interface)
- WebSocket monitoring: 7444
- HTTP monitoring: 9091 (metrics endpoint)

**Storage:**
- Data directory: `/var/lib/memgraph`
- Log directory: `/var/log/memgraph`
- Query modules: `/usr/lib/memgraph/query_modules`

### Memgraph Lab

A web-based UI for visualizing and interacting with Memgraph data. Runs as a Deployment on port 3000.

### Memgraph HA Cluster

A production-grade cluster with:
- **3 Coordinators** — manage cluster metadata using RAFT consensus protocol
- **2 Data instances** — store and query graph data
- **Cluster Setup Job** — initializes the cluster registration

## Component Relationships

```
External Client
      |
      v
[LoadBalancer] --> [Service (ClusterIP)] --> [Memgraph StatefulSet]
      |
      v
[Prometheus Exporter] --> [Prometheus / kube-prometheus-stack]

HA Cluster:
External Client
      |
      v
[Ingress / LoadBalancer] --> [Coordinator Services] --> [Coordinator Pods (x3)]
                                                              |
                                                              v
                                                     [Data Services] --> [Data Pods (x2)]
                                                              |
                                                              v
                                                     [Prometheus Exporters]
```

## Service Boundaries

| Boundary | Component | Description |
| --- | --- | --- |
| Database | Memgraph standalone | Single-instance in-memory graph database |
| Visualization | Memgraph Lab | Web UI for graph data exploration |
| Cluster | Memgraph HA | Multi-instance high-availability cluster |
| Monitoring | Prometheus Exporter | Metrics collection and exposition |

## Source References

- `code:charts/memgraph/templates/statefulset.yaml` — StatefulSet definition with port and storage configuration.
- `code:charts/memgraph-high-availability/templates/coordinators.yaml` — Coordinator StatefulSet for HA cluster.
- `code:charts/memgraph-high-availability/templates/data.yaml` — Data StatefulSet for HA cluster.
- `code:charts/memgraph-high-availability/templates/cluster-setup.yaml` — Cluster initialization Job.
