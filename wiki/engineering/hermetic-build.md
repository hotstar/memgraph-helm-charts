---
title: How to Generate Hermetic Server/Build
last_reviewed: 2026-08-27
tags: [readiness-placeholder, hermetic-build]
folder: engineering
sources:
  - repo-doc:.github/workflows/lint-test-memgraph.yml
  - repo-doc:.github/workflows/lint-test-high-availability.yaml
  - repo-doc:.github/workflows/release.yml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# How to Generate Hermetic Server/Build

> Readiness page: generated from repo evidence for `hermetic-build` and still needs human review.

## Build Artifacts

This repository does not build server binaries or container images directly. It packages Helm charts for deploying Memgraph. The build artifacts are Helm chart packages (`.tgz` files) published to a GitHub Pages Helm repository.

## Release Process

Charts are released via a manual workflow:

1. Trigger the `Release Charts` workflow via GitHub Actions (`workflow_dispatch`).
2. The workflow runs `chart-releaser-action` which packages charts and publishes them to the GitHub Pages Helm repository.
3. The `index.yaml` file is updated with the new chart versions.

The chart packages are available at:
```
https://memgraph.github.io/helm-charts
```

## Verifying Chart Packages

Use `helm lint` to verify chart correctness before release:

```bash
helm lint charts/memgraph
helm lint charts/memgraph-lab
helm lint charts/memgraph-high-availability
```

## Versioning

- Chart versions are incremented manually in each chart's `Chart.yaml` file.
- The `appVersion` tracks the underlying Memgraph version (e.g., `3.8.1`).

## Source References

- `repo-doc:.github/workflows/release.yml` — Release workflow using chart-releaser-action.
- `repo-doc:.github/workflows/lint-test-memgraph.yml` — Lint and test workflow that verifies charts before release.
- `repo-doc:.github/workflows/lint-test-high-availability.yaml` — HA chart test workflow.

## Suggested Content

- Document pinned tooling versions for reproducible builds.
- Add verification steps for chart package integrity.
- Document how to manually package and test charts locally before release.
