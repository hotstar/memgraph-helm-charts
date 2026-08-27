---
title: IaC Info
last_reviewed: 2026-08-27
tags: [readiness-placeholder, iac-info]
folder: operations
sources:
  - code:charts/memgraph/templates/statefulset.yaml
  - code:charts/memgraph/templates/service.yaml
  - code:charts/memgraph/templates/storageclass.yaml
  - code:charts/memgraph/templates/mg-exporter.yaml
  - code:charts/memgraph-high-availability/templates/data.yaml
  - code:charts/memgraph-high-availability/templates/coordinators.yaml
  - code:charts/memgraph-high-availability/templates/cluster-setup.yaml
  - code:charts/memgraph-high-availability/templates/ingress-nginx.yaml
  - code:charts/memgraph-lab/templates/deployment.yaml
  - code:charts/memgraph-lab/templates/service.yaml
  - code:charts/memgraph-lab/templates/ingress.yaml
  - code:charts/memgraph-lab/templates/hpa.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [operations-change, infrastructure-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# IaC Info

> Readiness page: generated from repo evidence for `iac-info` and still needs human review.

## Infrastructure Overview

This repository provides Helm charts for deploying Memgraph on Kubernetes. The infrastructure is managed through Helm chart templates with Kubernetes resources.

## Charts

### Memgraph Standalone (`charts/memgraph/`)

Kubernetes resources deployed:
- **StatefulSet** — manages the Memgraph database pod with persistent storage
- **Service** — ClusterIP service exposing Bolt (7687), WebSocket monitoring (7444), and HTTP monitoring (9091) ports
- **Service (LoadBalancer)** — optional external access service
- **StorageClass** — optional custom storage class for data persistence
- **ServiceAccount** — RBAC service account
- **Prometheus Exporter** — optional monitoring resources

### Memgraph Lab (`charts/memgraph-lab/`)

Kubernetes resources deployed:
- **Deployment** — Memgraph Lab web UI
- **Service** — ClusterIP service on port 3000
- **Ingress** — optional ingress for external access
- **HPA** — optional Horizontal Pod Autoscaler
- **ServiceAccount** — RBAC service account

### Memgraph High Availability (`charts/memgraph-high-availability/`)

Kubernetes resources deployed:
- **Coordinators** — three coordinator instances for cluster management
- **Data** — two data instances for graph storage
- **Cluster Setup Job** — initialization job for cluster registration
- **Services (Internal)** — internal ClusterIP services for coordinators and data
- **Services (External)** — optional LoadBalancer services
- **Ingress NGINX** — optional ingress controller
- **Prometheus Exporter** — monitoring resources

## Cloud Deployments

The HA chart includes cloud-specific deployment guides:
- **AWS** — scripts and cluster config in `charts/memgraph-high-availability/aws/`
- **AKS (Azure)** — Terraform scripts in `charts/memgraph-high-availability/aks/`
- **GKE** — scripts in `scripts/gke.bash`

## Storage

- Data persistence uses PersistentVolumeClaims with configurable storage classes
- Memgraph standalone: `10Gi` default storage for data, `1Gi` for logs
- Snapshots require at minimum 4x the dataset size
- Default reclaim policy is `Retain`

## Source References

- `code:charts/memgraph/templates/` — Kubernetes templates for the Memgraph standalone chart.
- `code:charts/memgraph-high-availability/templates/` — Kubernetes templates for the HA chart.
- `code:charts/memgraph-lab/templates/` — Kubernetes templates for the Memgraph Lab chart.

## Suggested Content

- Document ownership details for each infrastructure component.
- Add environment-specific configurations and deployment targets.
- Document dependency on Prometheus community helm chart (prometheus-community/kube-prometheus-stack).
- Add detailed instructions for AKS/AWS/GKE deployment procedures.
