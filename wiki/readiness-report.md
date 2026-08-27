# LLM-Wiki Readiness Report

Overall score: 60/100 - Partial

## Summary

This repo has useful documentation for basic setup, testing, IaC structure, and conventions, but weak coverage for production context, customer context, and external edge cases. Readiness pages have been created and populated with repo-derived evidence for most enabled topics. Command verification was skipped in agentic mode.

## Topic Scores

| Topic | Score | Evidence | Gap |
| --- | ---: | --- | --- |
| How to Set Up the Service | 9/12 | wiki/engineering/how-to-set-up-the-service.md, README.md, charts/memgraph/README.md | Readiness page populated with setup commands; missing local service reset steps and troubleshooting. Command verification skipped in agentic mode. |
| How to Set Up an Environment for Local Testing | 7.5/10 | wiki/engineering/local-testing-environment.md, .github/workflows/lint-test-memgraph.yml, .github/workflows/lint-test-high-availability.yaml | Readiness page populated with local test environment setup; missing details on mocking and verification steps. Command verification skipped in agentic mode. |
| How to Run Tests | 10.5/14 | wiki/engineering/how-to-run-tests.md, .github/workflows/lint-test-memgraph.yml, .github/workflows/pre-commit.yaml, .pre-commit-config.yaml | Readiness page populated with test commands; missing common failure modes and service-level testing instructions. Command verification skipped in agentic mode. |
| IaC Info | 6/8 | wiki/operations/iac-info.md, charts/memgraph/templates/, charts/memgraph-high-availability/templates/, charts/memgraph-lab/templates/ | Readiness page populated with IaC structure; missing environment-specific configurations and ownership details. |
| Production Context | 3/12 | wiki/operations/production-context.md | Empty placeholder page created; no production runtime evidence available in the repository.**
| Customer Context | 2.5/10 | wiki/domains/customer-context.md | Empty placeholder page created; no customer context evidence available in the repository.**
| How to Generate Hermetic Server/Build | 7.5/10 | wiki/engineering/hermetic-build.md, .github/workflows/release.yml | Readiness page populated with build info; missing pinned tooling versions and local build verification steps. Command verification skipped in agentic mode. |
| Code and Naming Conventions | 6/8 | wiki/engineering/code-and-naming-conventions.md, CONTRIBUTING.md, .pre-commit-config.yaml | Readiness page populated with conventions; missing detailed naming conventions and review expectations. |
| Secrets and Config Management | 7.5/10 | wiki/engineering/secrets-and-config-management-rules.md, charts/memgraph/values.yaml, charts/memgraph-lab/values.yaml | Readiness page populated with secrets and config rules; missing rotation procedures and CI secrets usage. |
| Branching and Release Conventions | 6/8 | wiki/engineering/branching-and-release-conventions.md, CONTRIBUTING.md, .github/workflows/release.yml | Readiness page populated with branching/release conventions; missing branch naming conventions and rollback procedures. |
| Edge Cases Decided Outside Code | 2.5/10 | wiki/domains/edge-cases-decided-outside-code.md | Empty placeholder page created; no edge case evidence available in the repository.** |
| Known Tradeoffs and Rejected Alternatives | 5/10 | wiki/decisions/tradeoffs-and-rejected-alternatives.md, charts/memgraph-high-availability/README.md | Readiness page populated with minimal tradeoff info; missing ADRs and detailed decision records. |

** No non-empty readiness page contributed to score. Placeholder is marked `readiness-placeholder`.

## Top Gaps

1. Add production runtime context (service identity, hot paths, guardrails, validation pointers).
2. Add customer context and customer journey information.
3. Add edge cases decided outside code.
4. Add ADRs and detailed tradeoff/decision records.
5. Add local service reset and troubleshooting steps.

## Suggested Wiki Changes

| Priority | Wiki path | Exact change |
| --- | --- | --- |
| High | `wiki/operations/production-context.md` | Fill all TBD sections with production-owner reviewed facts about service identity, hot paths, performance budgets, guardrails, validation pointers, and risky change patterns. |
| High | `wiki/domains/customer-context.md` | Fill all TBD sections with customer journey, caller entry points, segments, contracts, business rules, and impact details. |
| High | `wiki/domains/edge-cases-decided-outside-code.md` | Fill with important product, business, operational, or customer edge cases whose behavior is decided outside the code. |
| Medium | `wiki/engineering/how-to-set-up-the-service.md` | Add local service reset steps, service verification commands, and troubleshooting for common installation failures. |
| Medium | `wiki/decisions/tradeoffs-and-rejected-alternatives.md` | Add ADR entries, document rejected approaches and known tradeoffs in the current implementation. |

## Repo Docs Migration

| Repo doc | Wiki target | Recommendation | Drift or conflict |
| --- | --- | --- | --- |
| `README.md` | `wiki/engineering/how-to-set-up-the-service.md` | Wiki page populated from this doc; remove or deprecate the repo doc only after the wiki page is reviewed and accepted. | Partial coverage: reset steps are still missing. |
| `CONTRIBUTING.md` | `wiki/engineering/code-and-naming-conventions.md` | Wiki page populated from this doc; remove or deprecate the repo doc only after the wiki page is reviewed and accepted. | No conflict found. |
| `CONTRIBUTING.md` | `wiki/engineering/branching-and-release-conventions.md` | Wiki page populated from this doc in part; remove or deprecate the repo doc only after the wiki page is reviewed and accepted. | No conflict found. |
| `charts/memgraph/README.md` | `wiki/engineering/how-to-set-up-the-service.md` | Wiki page populated from this doc; remove or deprecate the repo doc only after the wiki page is reviewed and accepted. | No conflict found. |

## Evidence Notes

- Secret-bearing files were detected but not read.
- Command verification for `service-setup`, `local-testing-environment`, `test-commands`, and `hermetic-build` was skipped in agentic mode because safe CLI tools (helm, ct, pre-commit) are not available in this environment and unsafe/unclear commands (helm install requiring a Kubernetes cluster) cannot be auto-approved in agentic mode.
- Readiness pages were created for all 12 enabled topics. Pages for `production-context`, `customer-context`, and `external-edge-cases` are empty placeholders with no substantive evidence from the repository.
- No existing readiness report was present; this is the first readiness run.
