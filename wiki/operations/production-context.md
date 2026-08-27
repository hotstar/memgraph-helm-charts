---
title: Production Context
last_reviewed: 2026-08-27
tags: [readiness-placeholder, production-context, agent-context]
folder: operations
sources: []
agent_routing:
  code_change_relevance: conditional
  task_intents: [production-impact-change, operations-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Production Context

> Readiness page: generated as a human-owned production-context placeholder and still needs human review.

## Agent-Critical Constraints

<!-- Add short constraints agents must honor before making or reviewing changes, such as p95 budget, no extra network call, no response-shape change, or required feature flag. -->

## Production Service Identity

<!-- Add deployable service name, prod aliases, owning team, pager/oncall, and cases where code/module names differ from prod names. -->

## Hot Paths and Performance Budgets

<!-- Add latency targets, p50/p95/p99, QPS, timeout budgets, payload limits, cost-sensitive paths, and known bottlenecks. -->

## Runtime Change Guardrails

<!-- Add only the production constraints that directly change implementation, rollout, validation, or review choices, such as required feature flags, rollout shape, rollback expectations, or compatibility limits. Link to API, data, release, or runbook pages for details. -->

## Production Validation Pointers

<!-- Add the minimum checks an agent should know after a change, such as dashboard, metric, alert, trace, or log pointers. Link to canonical runbooks or SLO docs for full procedures. -->

## Known Risky Change Patterns

<!-- Add concise patterns that commonly cause production regressions, such as fanout, blocking calls, cache-key changes, payload growth, or fragile dependency behavior. Link to incidents or decisions for details. -->

## Source References

<!-- Add source docs, runbooks, dashboards, incident reviews, or code/config evidence used to populate this page. -->

## Suggested Content

- Fill all TBD sections with production-owner reviewed facts.
- Add only constraints that should affect implementation, review, rollout, validation, or operational planning.
- Link to canonical architecture, API, data, release, runbook, SLO, incident, or observability pages instead of duplicating them here.
