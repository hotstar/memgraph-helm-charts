---
title: Build, Test, and Deploy
last_reviewed: 2026-08-27
tags: [engineering, build, test, deploy, ci]
folder: engineering
sources:
  - code:.github/workflows/lint-test-memgraph.yml
  - code:.github/workflows/lint-test-high-availability.yaml
  - code:.github/workflows/lint-test-memgraph-lab.yaml
  - code:.github/workflows/pre-commit.yaml
  - code:.github/workflows/release.yml
  - code:.pre-commit-config.yaml
  - code:lintconf.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Build, Test, and Deploy

## CI/CD Workflows

The repository uses GitHub Actions for continuous integration and delivery.

### Workflow Summary

| Workflow | Trigger | Purpose |
| --- | --- | --- |
| `lint-test-memgraph.yml` | PRs to `charts/memgraph/**` | Lint and install-test Memgraph standalone |
| `lint-test-high-availability.yaml` | PRs to `charts/memgraph-high-availability/**` | Lint and install-test HA chart (5-node Minikube) |
| `lint-test-memgraph-lab.yaml` | PRs to `charts/memgraph-lab/**` | Lint and install-test Memgraph Lab |
| `pre-commit.yaml` | All PRs | Run pre-commit hooks on all files |
| `release.yml` | Manual (`workflow_dispatch`) | Package and publish charts |

### Lint and Test Pipeline

For each chart-specific workflow:

1. **Checkout** — fetch repository with full depth
2. **Setup Helm** — install Helm v3.14.0
3. **Setup Python** — install Python 3.12 (for chart-testing)
4. **Setup chart-testing** — install `ct` v2.6.1
5. **List changed charts** — detect which charts changed vs. target branch
6. **Lint** — run `ct lint` on changed charts
7. **Create cluster** — kind (standalone/Lab) or Minikube with 5 nodes (HA)
8. **Setup environment** — configure Memgraph Enterprise license variables
9. **Create secrets** — create Kubernetes secret for test authentication
10. **Install** — deploy chart via `ct install`
11. **Wait for pods** — poll until all pods are Ready
12. **Run Helm tests** — execute test Jobs
13. **Collect logs** — gather test pod logs

### Pre-commit Pipeline

1. Install pre-commit
2. Run `pre-commit run --all-files --show-diff-on-failure`
3. Cache pre-commit environment

### Release Pipeline

1. Configure git user
2. Add Prometheus community Helm repo
3. Update chart dependencies
4. Run `chart-releaser-action` to package and publish charts

## Deployment

Charts are published to a GitHub Pages Helm repository at:
```
https://memgraph.github.io/helm-charts
```

### Local Deployment

```bash
# Add repository
helm repo add memgraph https://memgraph.github.io/helm-charts
helm repo update

# Install
helm install my-release memgraph/memgraph

# Upgrade
helm upgrade my-release memgraph/memgraph

# Uninstall
helm uninstall my-release
```

### Using Custom Values

```bash
helm install my-release memgraph/memgraph \
  --set image.tag=3.8.1 \
  --set service.type=LoadBalancer
```

Or with a values file:

```bash
helm install my-release memgraph/memgraph -f my-values.yaml
```

## Source References

- `code:.github/workflows/lint-test-memgraph.yml` — CI/CD for Memgraph standalone chart.
- `code:.github/workflows/lint-test-high-availability.yaml` — CI/CD for HA chart with Minikube.
- `code:.github/workflows/lint-test-memgraph-lab.yaml` — CI/CD for Memgraph Lab chart.
- `code:.github/workflows/pre-commit.yaml` — Pre-commit CI workflow.
- `code:.github/workflows/release.yml` — Release workflow.
- `code:.pre-commit-config.yaml` — Pre-commit hook configuration.
- `code:lintconf.yaml` — Lint configuration file.
