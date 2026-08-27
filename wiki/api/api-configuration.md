---
title: API Configuration
last_reviewed: 2026-08-27
tags: [api, ports, configuration]
folder: api
sources:
  - code:charts/memgraph/values.yaml
  - code:charts/memgraph/memgraph.conf
  - code:charts/memgraph/templates/service.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [api-contract-change]
  applies_to_paths:
    - charts/memgraph/**
    - charts/memgraph-lab/**
  applies_to_symbols: []
---

# API Configuration

## Connection Protocols

Memgraph uses the **Bolt protocol** (TCP) for client connections. The primary interface is via the Bolt port.

| Protocol | Port | Description |
| --- | --- | --- |
| Bolt | 7687 | Primary query interface (Cypher queries) |
| WebSocket Monitoring | 7444 | Optional real-time monitoring WebSocket |
| HTTP Monitoring | 9091 | Optional HTTP metrics endpoint |

## Service Types

| Type | Description |
| --- | --- |
| ClusterIP | Internal cluster access (default, secure) |
| NodePort | External port (30000-32767) accessible to all nodes |
| LoadBalancer | Cloud provider load balancer on port 7687 |

## Memgraph Lab API

Memgraph Lab is a web UI served on port 3000, providing:
- Visual graph exploration
- Query editing and execution
- Database management interface

Environment variables for Memgraph Lab connection:
```yaml
env:
  - name: QUICK_CONNECT_MG_HOST
    value: memgraph
  - name: QUICK_CONNECT_MG_PORT
    value: "7687"
  - name: BASE_PATH
    value: /
```

## Enterprise Features

High availability clustering requires:
- Memgraph Enterprise License
- Organization name
- Three coordinators (RAFT consensus)
- Two data instances

## Source References

- `code:charts/memgraph/values.yaml` — Service configuration with port definitions.
- `code:charts/memgraph/memgraph.conf` — Memgraph configuration file with all available settings.
- `code:charts/memgraph/templates/service.yaml` — Service template defining exposed ports.
