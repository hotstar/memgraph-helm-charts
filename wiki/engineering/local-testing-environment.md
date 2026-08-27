---
title: How to Set Up an Environment for Local Testing
last_reviewed: 2026-08-27
tags: [readiness-placeholder, local-testing-environment]
folder: engineering
sources:
  - repo-doc:.github/workflows/lint-test-memgraph.yml
  - repo-doc:.github/workflows/lint-test-high-availability.yaml
  - repo-doc:.github/workflows/lint-test-memgraph-lab.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# How to Set Up an Environment for Local Testing

> Readiness page: generated from repo evidence for `local-testing-environment` and still needs human review.

## Prerequisites for Local Testing

- Helm version 3.14.0 or above
- Docker (for running Minikube or kind)
- Python 3.12 (for chart-testing tool)
- kubectl

## Setting Up Chart Testing

The repository uses the [chart-testing](https://github.com/helm/chart-testing) (`ct`) tool for linting and testing Helm charts.

Install chart-testing and set up a local Kubernetes cluster:

```bash
# Install kind (Kubernetes in Docker)
brew install kind
# OR install Minikube
brew install minikube

# Create a cluster
# For memgraph and memgraph-lab charts:
kind create cluster

# For memgraph-high-availability (requires 5 nodes):
minikube start --nodes=5 --driver=docker
```

## Setting Up Minikube for HA Testing

For the HA chart, additional Minikube setup is needed:

```bash
minikube start --nodes=5 --driver=docker
minikube addons disable storage-provisioner
minikube addons disable default-storageclass
minikube addons enable volumesnapshots
minikube addons enable csi-hostpath-driver
kubectl apply -f ./charts/memgraph-high-availability/storage-class.yaml
```

## Environment Variables for Memgraph

Set up Memgraph Enterprise license environment variables:

```bash
export MEMGRAPH_ENTERPRISE_LICENSE=<your-license>
export MEMGRAPH_ORGANIZATION_NAME=<your-organization-name>
```

## Source References

- `repo-doc:.github/workflows/lint-test-memgraph.yml` — CI steps for setting up and testing the Memgraph standalone chart.
- `repo-doc:.github/workflows/lint-test-high-availability.yaml` — CI steps for setting up Minikube and testing the HA chart.
- `repo-doc:.github/workflows/lint-test-memgraph-lab.yaml` — CI steps for testing the Memgraph Lab chart.

## Suggested Content

- Add details on local-only environment variables needed for testing.
- Document mocked services and test fixtures.
- Add verification steps to confirm the test environment is ready.
