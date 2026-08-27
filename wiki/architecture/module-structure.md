---
title: Module Structure
last_reviewed: 2026-08-27
tags: [architecture, module-structure, helm, charts]
folder: architecture
sources:
  - code:charts/memgraph/templates/
  - code:charts/memgraph/templates/_helpers.tpl
  - code:charts/memgraph/templates/statefulset.yaml
  - code:charts/memgraph/templates/service.yaml
  - code:charts/memgraph/templates/serviceaccount.yaml
  - code:charts/memgraph/templates/service-loadbalancer.yaml
  - code:charts/memgraph/templates/storageclass.yaml
  - code:charts/memgraph/templates/mg-exporter.yaml
  - code:charts/memgraph/templates/NOTES.txt
  - code:charts/memgraph-lab/templates/
  - code:charts/memgraph-lab/templates/deployment.yaml
  - code:charts/memgraph-lab/templates/service.yaml
  - code:charts/memgraph-lab/templates/ingress.yaml
  - code:charts/memgraph-lab/templates/hpa.yaml
  - code:charts/memgraph-lab/templates/serviceaccount.yaml
  - code:charts/memgraph-lab/templates/NOTES.txt
  - code:charts/memgraph-high-availability/templates/
  - code:charts/memgraph-high-availability/templates/coordinators.yaml
  - code:charts/memgraph-high-availability/templates/data.yaml
  - code:charts/memgraph-high-availability/templates/cluster-setup.yaml
  - code:charts/memgraph-high-availability/templates/services-coordinators.yaml
  - code:charts/memgraph-high-availability/templates/services-data.yaml
  - code:charts/memgraph-high-availability/templates/services-coordinators-external.yaml
  - code:charts/memgraph-high-availability/templates/services-data-external.yaml
  - code:charts/memgraph-high-availability/templates/ingress-nginx.yaml
  - code:charts/memgraph-high-availability/templates/mg-exporter.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [architecture-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Module Structure

## Memgraph Standalone (`charts/memgraph/`)

The standalone chart deploys Memgraph as a StatefulSet. Template modules:

| Template | Resource | Purpose |
| --- | --- | --- |
| `_helpers.tpl` | Template helpers | Naming conventions, labels, probe templates |
| `statefulset.yaml` | StatefulSet | Main Memgraph database pod with persistent volumes |
| `service.yaml` | Service | Internal ClusterIP service for Bolt, WebSocket, HTTP |
| `service-loadbalancer.yaml` | Service | Optional LoadBalancer for external access |
| `serviceaccount.yaml` | ServiceAccount | RBAC service account |
| `storageclass.yaml` | StorageClass | Optional custom storage class |
| `mg-exporter.yaml` | ConfigMap + Deployment + Service + ServiceMonitor | Prometheus metrics exporter |
| `NOTES.txt` | Instructions | Post-install usage notes |

### Memgraph Standalone Key Configurations

- **StatefulSet**: Defines the Memgraph pod with init containers (sysctl, core dumps), security context, resource limits, probes (readiness, liveness, startup), and volume claim templates for data, logs, core dumps, and user storage.
- **Service**: Exposes Bolt (7687), WebSocket monitoring (7444), and HTTP monitoring (9091) ports.
- **LoadBalancer**: Optional external service with configurable `externalTrafficPolicy`, `ipFamilyPolicy`, `loadBalancerSourceRanges`.

## Memgraph Lab (`charts/memgraph-lab/`)

The Lab chart deploys Memgraph Lab as a Deployment. Template modules:

| Template | Resource | Purpose |
| --- | --- | --- |
| `_helpers.tpl` | Template helpers | Naming and label conventions |
| `deployment.yaml` | Deployment | Memgraph Lab web UI container |
| `service.yaml` | Service | Internal ClusterIP service on port 3000 |
| `ingress.yaml` | Ingress | Optional ingress for external access |
| `hpa.yaml` | HorizontalPodAutoscaler | Optional auto-scaling |
| `serviceaccount.yaml` | ServiceAccount | RBAC service account |
| `NOTES.txt` | Instructions | Post-install usage notes |

## Memgraph High Availability (`charts/memgraph-high-availability/`)

The HA chart deploys a complete Memgraph cluster. Template modules:

| Template | Resource | Purpose |
| --- | --- | --- |
| `_helpers.tpl` | Template helpers | Naming and label conventions |
| `coordinators.yaml` | StatefulSet | Three coordinator instances (RAFT consensus) |
| `data.yaml` | StatefulSet | Two data instances for graph storage |
| `cluster-setup.yaml` | Job | Initialization job to register the cluster |
| `services-coordinators.yaml` | Service | Internal coordinator services |
| `services-data.yaml` | Service | Internal data services |
| `services-coordinators-external.yaml` | Service | External coordinator access |
| `services-data-external.yaml` | Service | External data access |
| `ingress-nginx.yaml` | Ingress | NGINX ingress configuration |
| `mg-exporter.yaml` | Deployment + Service | Prometheus metrics exporter |

## Source References

- `code:charts/memgraph/templates/` — All templates for the Memgraph standalone chart.
- `code:charts/memgraph-lab/templates/` — All templates for the Memgraph Lab chart.
- `code:charts/memgraph-high-availability/templates/` — All templates for the HA cluster chart.
