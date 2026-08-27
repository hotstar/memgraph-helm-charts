---
title: Branching and Release Conventions
last_reviewed: 2026-08-27
tags: [readiness-placeholder, branching-release-conventions]
folder: engineering
sources:
  - repo-doc:CONTRIBUTING.md
  - repo-doc:.github/workflows/release.yml
  - repo-doc:.github/workflows/lint-test-memgraph.yml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Branching and Release Conventions

> Readiness page: generated from repo evidence for `branching-release-conventions` and still needs human review.

## Branching Strategy

- The default branch is `main`.
- All bug fixes and new features should be applied to the `main` branch.
- Changes should be submitted via GitHub Pull Requests.

## Pull Request Requirements

1. Two code owner reviews are required for a PR to be merged.
2. CI tests must pass remotely before merging.
3. Pre-commit checks must pass.
4. Commits should be atomic (one feature per commit).

## Versioning

- Chart versions are maintained in each chart's `Chart.yaml` file under the `version` field.
- Chart version is incremented manually for each change.
- The `appVersion` field tracks the Memgraph software version (e.g., `3.8.1` for Memgraph, `3.8.0` for Memgraph Lab).

## Release Process

- Releases are triggered manually via the `Release Charts` GitHub Actions workflow (`workflow_dispatch`).
- The workflow uses `chart-releaser-action` with `CR_SKIP_EXISTING: true` to avoid overwriting existing chart versions.
- Before release, chart dependencies are updated:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm dependency update charts/memgraph-high-availability
```

- After release, chart packages are published to the GitHub Pages Helm repository.

## Changelog

The repository does not maintain an explicit changelog file. Chart changes are tracked through:
- Git commit history
- GitHub release tags
- Individual chart README files

## Source References

- `repo-doc:CONTRIBUTING.md` — Contribution guidelines with PR and merge requirements.
- `repo-doc:.github/workflows/release.yml` — Release workflow definition.
- `repo-doc:.github/workflows/lint-test-memgraph.yml` — CI requirements for merges.

## Suggested Content

- Document branch naming conventions.
- Add deployment gates and approval process details.
- Document rollback procedures.
- Add release schedule and version numbering policy.
