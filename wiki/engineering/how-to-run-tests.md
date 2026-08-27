---
title: How to Run Tests
last_reviewed: 2026-08-27
tags: [readiness-placeholder, test-commands]
folder: engineering
sources:
  - repo-doc:.github/workflows/lint-test-memgraph.yml
  - repo-doc:.github/workflows/lint-test-high-availability.yaml
  - repo-doc:.github/workflows/lint-test-memgraph-lab.yaml
  - repo-doc:.github/workflows/pre-commit.yaml
  - repo-doc:.pre-commit-config.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# How to Run Tests

> Readiness page: generated from repo evidence for `test-commands` and still needs human review.

## Pre-commit Hooks

The repository uses pre-commit hooks to enforce code quality. To run pre-commit locally:

```bash
pip install pre-commit
pre-commit run --all-files --show-diff-on-failure
```

Configured hooks include:
- `trailing-whitespace` — removes trailing whitespace
- `end-of-file-fixer` — ensures files end with a newline
- `check-json` — validates JSON files
- `check-yaml` — validates YAML files (excluding Helm templates)
- `mixed-line-ending` — normalizes line endings
- `check-merge-conflict` — detects merge conflict markers
- `detect-private-key` — detects accidentally committed private keys

## Helm Chart Linting

Lint Helm charts using chart-testing:

```bash
# Lint a specific chart
ct lint --target-branch main --check-version-increment false --charts charts/memgraph

# For HA chart with custom lint config
ct lint --target-branch main --check-version-increment false --lint-conf lintconf.yaml --charts charts/memgraph-high-availability
```

## Helm Chart Installation Tests

Run the chart-testing installation test, which deploys the chart on a Kubernetes cluster:

```bash
# For memgraph standalone chart
ct install --target-branch main --helm-extra-set-args "--set secrets.enabled=true" --excluded-charts memgraph-high-availability,memgraph-lab --namespace default
```

## CI Pipeline

The repository has the following CI workflows defined in `.github/workflows/`:

| Workflow | Trigger | Charts |
| --- | --- | --- |
| `lint-test-memgraph.yml` | PRs affecting `charts/memgraph/**` | memgraph standalone |
| `lint-test-high-availability.yaml` | PRs affecting `charts/memgraph-high-availability/**` | memgraph-high-availability |
| `lint-test-memgraph-lab.yaml` | PRs affecting `charts/memgraph-lab/**` | memgraph-lab |
| `pre-commit.yaml` | All PRs | All files |
| `release.yml` | Manual (workflow_dispatch) | All charts |

## Quick Test Commands

```bash
# Pre-commit check
pre-commit run --all-files

# Helm syntax check (without ct)
helm lint charts/memgraph

# Validate YAML syntax
yamllint -c lintconf.yaml charts/memgraph/templates/
```

## Source References

- `repo-doc:.github/workflows/lint-test-memgraph.yml` — CI workflow for linting and testing the Memgraph standalone chart.
- `repo-doc:.github/workflows/lint-test-high-availability.yaml` — CI workflow with Minikube testing for the HA chart.
- `repo-doc:.github/workflows/lint-test-memgraph-lab.yaml` — CI workflow for the Memgraph Lab chart.
- `repo-doc:.github/workflows/pre-commit.yaml` — Pre-commit CI workflow.
- `repo-doc:.pre-commit-config.yaml` — Pre-commit hook configuration.

## Suggested Content

- Add details on required test data and fixtures.
- Document common failure modes for test execution.
- Add instructions for running service-level or integration tests locally without CI.
