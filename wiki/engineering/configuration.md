---
title: Configuration
last_reviewed: 2026-08-27
tags: [engineering, configuration, env-vars, config]
folder: engineering
sources:
  - code:charts/memgraph/values.yaml
  - code:charts/memgraph/memgraph.conf
  - code:charts/memgraph/templates/statefulset.yaml
  - code:charts/memgraph-lab/values.yaml
  - code:charts/memgraph-high-availability/values.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [repo-onboarding]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Configuration

## Memgraph Standalone Configuration

### Docker Image

```yaml
image:
  repository: docker.io/memgraph/memgraph
  tag: ""  # Defaults to chart's appVersion (3.8.1)
  pullPolicy: IfNotPresent
```

### Memgraph Configuration Flags

Database-level settings passed via `memgraphConfig`:

```yaml
memgraphConfig:
  - "--data-directory=/var/lib/memgraph/mg_data"
  - "--also-log-to-stderr=true"
```

For a full list of available database settings, see the [Memgraph Configuration Reference](https://memgraph.com/docs/memgraph/reference-guide/configuration).

### Environment Variables

| Variable | Source | Description |
| --- | --- | --- |
| `MEMGRAPH_USER` | Kubernetes secret | Database user (when `secrets.enabled=true`) |
| `MEMGRAPH_PASSWORD` | Kubernetes secret | Database password (when `secrets.enabled=true`) |
| `MEMGRAPH_ENTERPRISE_LICENSE` | Helm value | Enterprise license key |
| `MEMGRAPH_ORGANIZATION_NAME` | Helm value | Organization name for license |

Additional custom env vars via `extraEnv`:

```yaml
extraEnv:
  - name: CUSTOM_FEATURE_FLAG
    value: "true"
```

### Resource Limits

```yaml
resources: {}
# limits:
#   cpu: 100m
#   memory: 128Mi
# requests:
#   cpu: 100m
#   memory: 128Mi
```

### Pod Configuration

| Setting | Default | Description |
| --- | --- | --- |
| `nodeSelector` | `{}` | Pod node selection |
| `tolerations` | `[]` | Pod tolerations |
| `nodeAffinity` | `{}` | Node affinity rules |
| `priorityClassName` | `null` | Pod priority |
| `replicaCount` | `1` | Number of replicas |
| `secrets.enabled` | `false` | Enable Kubernetes secrets |
| `sysctlInitContainer.enabled` | `true` | Enable sysctl init container |

## Memgraph Lab Configuration

### Docker Image

```yaml
image:
  repository: docker.io/memgraph/lab
  tag: ""  # Defaults to chart's appVersion (3.8.0)
  pullPolicy: IfNotPresent
```

### Environment Variables

```yaml
env:
  - name: QUICK_CONNECT_MG_HOST
    value: memgraph
  - name: QUICK_CONNECT_MG_PORT
    value: "7687"
  - name: BASE_PATH
    value: /
```

### Scaling

```yaml
replicaCount: 1
```

Optional HPA for auto-scaling.

## Memgraph HA Configuration

### Enterprise Settings

```yaml
env:
  MEMGRAPH_ENTERPRISE_LICENSE: ""
  MEMGRAPH_ORGANIZATION_NAME: ""
```

### Storage Configuration

```yaml
storage:
  data:
    libStorageClassName: ""
    logStorageClassName: ""
  coordinators:
    libStorageClassName: ""
    logStorageClassName: ""
```

## Source References

- `code:charts/memgraph/values.yaml` — Memgraph standalone chart default values.
- `code:charts/memgraph/memgraph.conf` — Memgraph database configuration file.
- `code:charts/memgraph/templates/statefulset.yaml` — StatefulSet template where env vars are used.
- `code:charts/memgraph-lab/values.yaml` — Memgraph Lab chart default values.
- `code:charts/memgraph-high-availability/values.yaml` — HA chart default values.
