---
title: Known Tradeoffs and Rejected Alternatives
last_reviewed: 2026-08-27
tags: [readiness-placeholder, tradeoffs-rejected-alternatives]
folder: decisions
sources:
  - repo-doc:charts/memgraph-high-availability/README.md
agent_routing:
  code_change_relevance: conditional
  task_intents: [architecture-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Known Tradeoffs and Rejected Alternatives

> Readiness page: generated from repo evidence for `tradeoffs-rejected-alternatives` and still needs human review.

## Architecture Decisions

### High Availability Architecture

The HA chart deploys a Memgraph cluster with two data instances and three coordinators. This is the recommended architecture for production deployments requiring high availability.

A `docker-compose/` variant provides a way to test the HA setup locally without Kubernetes using Docker Compose and a bridged Docker network.

### Enterprise License Requirement

High availability requires Memgraph Enterprise Edition with a valid license. The standalone chart does not require an Enterprise license.

## Source References

- `repo-doc:charts/memgraph-high-availability/README.md` — Overview of the HA cluster architecture.

## Suggested Content

- Add ADR (Architecture Decision Record) entries for key architectural decisions.
- Document why specific design choices were made for each chart.
- Add information about rejected approaches and alternatives considered.
- Document known tradeoffs in the current implementation.
