---
title: Entry Points
last_reviewed: 2026-08-27
tags: [architecture, entry-points, deployment]
folder: architecture
sources:
  - code:charts/memgraph/templates/statefulset.yaml
  - code:charts/memgraph/templates/NOTES.txt
  - code:charts/memgraph-lab/templates/deployment.yaml
  - code:charts/memgraph-lab/templates/NOTES.txt
  - code:charts/memgraph-high-availability/templates/coordinators.yaml
  - code:charts/memgraph-high-availability/templates/data.yaml
  - code:charts/memgraph-high-availability/templates/cluster-setup.yaml
  - code:charts/memgraph-high-availability/templates/NOTES.txt
agent_routing:
  code_change_relevance: conditional
  task_intents: [architecture-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Entry Points

## Helm Chart Entry Points

The three Helm charts are the primary entry points for deploying Memgraph:

| Chart | Installation Command |
| --- | --- |
| Memgraph standalone | `helm install <release> memgraph/memgraph` |
| Memgraph Lab | `helm install <release> memgraph/memgraph-lab` |
| Memgraph HA | `helm install <release> memgraph/memgraph-high-availability` |

## Memgraph Database

The Memgraph database runs as a **StatefulSet** with:
- **Main container**: `docker.io/memgraph/memgraph` with configurable tag
- **Init container (sysctl)**: `busybox` to set `vm.max_map_count`
- **Init container (core dumps)**: `busybox` to configure core dump patterns (optional)
- **User init containers**: Custom init containers (optional)

**Startup flow:**
1. Init containers execute (sysctl, core dumps, user-defined)
2. Main container starts with `memgraphConfig` arguments
3. Startup probe begins (TCP check on Bolt port 7687, up to 1440 failures/2h)
4. Readiness probe confirms service availability (TCP on 7687)
5. Liveness probe monitors ongoing health

## Memgraph Lab

Memgraph Lab runs as a **Deployment** with:
- **Main container**: `docker.io/memgraph/lab` with configurable tag
- **Optional Ingress**: For external web access
- **Optional HPA**: For auto-scaling

## Cluster Setup Job (HA)

The `cluster-setup.yaml` Job initializes the HA cluster by registering coordinator and data instances.

## Prometheus Exporter

The exporter runs as a **Deployment** that:
1. Reads config from a ConfigMap (standalone or HA mode)
2. Connects to Memgraph's HTTP monitoring endpoint (9091)
3. Exposes metrics on port 9115
4. Integrated with ServiceMonitor for Prometheus discovery

## Scripts

Additional utility scripts are available:

| Script | Purpose |
| --- | --- |
| `scripts/aks.bash` | Deploy to Azure Kubernetes Service |
| `scripts/aws.bash` | Deploy to AWS EKS |
| `scripts/gke.bash` | Deploy to Google Kubernetes Engine |
| `scripts/debug-memgraph.sh` | Debug running Memgraph pods |
| `scripts/find-memgraph-pid.sh` | Find Memgraph process IDs |

## Docker Compose

`docker-compose/docker-compose.yml` provides a local HA cluster for testing without Kubernetes.

## Source References

- `code:charts/memgraph/templates/statefulset.yaml` — StatefulSet definition with all containers.
- `code:charts/memgraph/templates/NOTES.txt` — Post-install usage notes.
- `code:charts/memgraph-lab/templates/deployment.yaml` — Lab deployment definition.
- `code:charts/memgraph-high-availability/templates/coordinators.yaml` — Coordinator StatefulSet.
- `code:charts/memgraph-high-availability/templates/data.yaml` — Data StatefulSet.
- `code:charts/memgraph-high-availability/templates/cluster-setup.yaml` — Cluster initialization Job.
