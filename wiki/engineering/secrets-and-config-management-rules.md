---
title: Secrets and Config Management Rules
last_reviewed: 2026-08-27
tags: [readiness-placeholder, secrets-config-management]
folder: engineering
sources:
  - code:charts/memgraph/values.yaml
  - code:charts/memgraph-lab/values.yaml
  - code:charts/memgraph-high-availability/values.yaml
  - repo-doc:charts/memgraph/README.md
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Secrets and Config Management Rules

> Readiness page: generated from repo evidence for `secrets-config-management` and still needs human review.

## Kubernetes Secrets

The Memgraph charts support the use of Kubernetes secrets for authentication credentials.

### Memgraph Standalone

```yaml
secrets:
  enabled: false
  name: memgraph-secrets
  userKey: USER
  passwordKey: PASSWORD
```

When enabled, the secret values are passed to the Memgraph container as environment variables (`MEMGRAPH_USER` and `MEMGRAPH_PASSWORD`).

To create the Kubernetes secret:

```bash
kubectl create secret generic memgraph-secrets \
  --from-literal=USER=myuser \
  --from-literal=PASSWORD=mypassword
```

### Memgraph Lab

```yaml
secrets:
  enabled: false
  name: memgraph-secrets
  keys: []
```

When enabled, values from the specified secret keys are injected as environment variables.

## Enterprise License

Enterprise Memgraph features (such as high availability) require a license, configured via environment variables:

```yaml
memgraphEnterpriseLicense: "<your-license>"
memgraphOrganizationName: "<your-organization-name>"
```

These are set as Helm values or passed as `--set` parameters during installation.

## Configuration via Helm Values

All configuration is managed through Helm `values.yaml` files per chart. Key configuration areas include:

- **Image settings** — repository, tag, pull policy
- **Service settings** — type, ports, annotations
- **Storage settings** — PVC size, storage class, access modes
- **Resource limits** — CPU and memory requests/limits
- **Memgraph configuration** - startup flags via `memgraphConfig`

## Environment Variables

Additional environment variables can be set via `extraEnv`:

```yaml
extraEnv:
  - name: CUSTOM_FEATURE_FLAG
    value: "true"
  - name: API_KEY
    valueFrom:
      secretKeyRef:
        name: my-secret
        key: api-key
```

## Source References

- `code:charts/memgraph/values.yaml` — Memgraph standalone chart configuration with secrets support.
- `code:charts/memgraph-lab/values.yaml` — Memgraph Lab chart configuration.
- `code:charts/memgraph-high-availability/values.yaml` — HA chart configuration with enterprise license settings.
- `repo-doc:charts/memgraph/README.md` — Documentation for creating secrets.

## Suggested Content

- Document secret rotation procedures.
- Add rules for what must never be committed to the repository.
- Add details on how secrets are accessed in different environments.
- Document CI secrets usage for GitHub Actions.
