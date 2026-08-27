---
title: How to Set Up the Service
last_reviewed: 2026-08-27
tags: [readiness-placeholder, service-setup]
folder: engineering
sources:
  - repo-doc:README.md
  - repo-doc:charts/memgraph/README.md
  - repo-doc:charts/memgraph-lab/README.md
  - repo-doc:charts/memgraph-high-availability/README.md
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# How to Set Up the Service

> Readiness page: generated from repo evidence for `service-setup` and still needs human review.

## Prerequisites

- Helm version 3 or above installed.
- A Kubernetes cluster (Minikube, kind, or cloud-based).

## Add the Helm Repository

Add the Memgraph Helm chart repository:

```
helm repo add memgraph https://memgraph.github.io/helm-charts
helm repo update
```

## Deploy Memgraph Standalone

To install the Memgraph standalone chart:

```
helm install my-release memgraph/memgraph
```

## Deploy Memgraph Lab

To install Memgraph Lab:

```
helm install my-release memgraph/memgraph-lab
```

## Deploy Memgraph High Availability (Enterprise)

To install a high availability Memgraph cluster (requires Enterprise license):

```
helm install my-release memgraph/memgraph-high-availability --set env.MEMGRAPH_ENTERPRISE_LICENSE=<your-license>,env.MEMGRAPH_ORGANIZATION_NAME=<your-organization-name>
```

The HA cluster includes two data instances and three coordinators. Additional steps are needed to fully operationalize the cluster; see the [Setting up the cluster](https://memgraph.com/docs/getting-started/install-memgraph/kubernetes#setting-up-the-cluster) docs.

## Docker Compose for HA

A Docker Compose setup for an HA Memgraph cluster is available in the `docker-compose/` directory. Add your license details and run:

```
cd docker-compose
docker-compose up
```

## Changing Default Chart Values

Override values via `--set` flags:

```
helm install my-release memgraph/memgraph --set image.tag=3.8.1
```

Or use a custom `values.yaml`:

```
helm install my-release memgraph/memgraph -f values.yaml
```

## Memgraph Configuration

The `memgraphConfig` parameter accepts a list of configuration flags:

```yaml
memgraphConfig:
  - "--also-log-to-stderr=true"
  - "--log-level=TRACE"
  - "--log-file=''"
```

For all available database settings, refer to the [Configuration settings reference guide](https://memgraph.com/docs/memgraph/reference-guide/configuration).

## Source References

- `repo-doc:README.md` — top-level installation and usage instructions for all three charts.
- `repo-doc:charts/memgraph/README.md` — detailed configuration options for the Memgraph standalone chart.
- `repo-doc:charts/memgraph-lab/README.md` — detailed configuration options for the Memgraph Lab chart.
- `repo-doc:charts/memgraph-high-availability/README.md` — overview of the HA cluster chart.

## Suggested Content

- Add local service reset steps.
- Document how to verify the service is running after installation.
- Add troubleshooting steps for common installation failures.
