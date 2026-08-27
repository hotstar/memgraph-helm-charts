---
title: Code and Naming Conventions
last_reviewed: 2026-08-27
tags: [readiness-placeholder, code-and-naming-conventions]
folder: engineering
sources:
  - repo-doc:CONTRIBUTING.md
  - repo-doc:.pre-commit-config.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Code and Naming Conventions

> Readiness page: generated from repo evidence for `code-and-naming-conventions` and still needs human review.

## Contribution Guidelines

- All contributions should be submitted via GitHub Pull Requests.
- Commits should be atomic (one feature per commit).
- Bug fixes and new features should target the `main` branch.
- Two code owner reviews are required for a PR to be merged.
- CI tests must pass before merging.

## Coding Conventions

- Helm chart templates follow standard Helm best practices.
- YAML files are checked with `pre-commit` hooks.
- Template files under `charts/*/templates/` are excluded from YAML linting.

## Code Quality

Pre-commit hooks enforce:
- No trailing whitespace
- Files end with a newline
- Valid JSON and YAML syntax
- No merge conflict markers
- No accidentally committed private keys

## Repository Structure

```
charts/
  memgraph/                    # Memgraph standalone chart
    templates/                 # Kubernetes resource templates
    values.yaml                # Default configuration values
    Chart.yaml                 # Chart metadata
    README.md                  # Chart documentation
  memgraph-lab/                # Memgraph Lab chart
    templates/
    values.yaml
    Chart.yaml
    README.md
  memgraph-high-availability/  # Memgraph HA cluster chart
    templates/
    values.yaml
    Chart.yaml
    README.md
    aks/                       # Azure AKS deployment guide
    aws/                       # AWS deployment guide
docker-compose/                # Docker Compose for HA setup
scripts/                       # Utility scripts
tutorials/                     # Deployment tutorials
```

## Source References

- `repo-doc:CONTRIBUTING.md` — Contribution guidelines.
- `repo-doc:.pre-commit-config.yaml` — Pre-commit hook configuration.

## Suggested Content

- Add detailed naming conventions for chart resources and values.
- Add folder organization conventions.
- Add review expectations and examples of preferred code organization.
