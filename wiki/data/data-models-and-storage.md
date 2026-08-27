---
title: Data Models and Storage
last_reviewed: 2026-08-27
tags: [data, storage, persistence]
folder: data
sources:
  - code:charts/memgraph/values.yaml
  - code:charts/memgraph/memgraph.conf
  - code:charts/memgraph/templates/statefulset.yaml
agent_routing:
  code_change_relevance: conditional
  task_intents: [data-model-change]
  applies_to_paths:
    - charts/**
  applies_to_symbols: []
---

# Data Models and Storage

## Memgraph Storage Architecture

Memgraph is an in-memory graph database with optional disk persistence. It stores data in a graph model with nodes, relationships, and properties, queried via the Cypher language.

### Storage Types

| Storage | Purpose | Default Size |
| --- | --- | --- |
| Data (`lib`) | Graph data directory (`/var/lib/memgraph`) | 10Gi |
| Logs | Database logs (`/var/log/memgraph`) | 1Gi |
| Core Dumps | Crash dump files (`/var/core/memgraph`) | 10Gi (optional) |
| User | Config files, certificates, user data | 1Gi (optional) |

### Persistence Configuration

```yaml
persistentVolumeClaim:
  createStorageClaim: true
  storageClassName: ""
  storageSize: 10Gi
  libStorageAccessMode: "ReadWriteOnce"
  createLogStorage: true
  logStorageSize: 1Gi
  createUserClaim: false
  userStorageSize: 1Gi
  userStorageAccessMode: "ReadWriteOnce"
  createCoreDumpsClaim: false
  coreDumpsStorageSize: 10Gi
```

### Storage Classes

A default storage class can be created with configurable provisioner, type, filesystem, reclaim policy, and volume binding mode.

## Data Persistence Configuration (memgraph.conf)

Key settings from `memgraph.conf`:

| Setting | Default | Description |
| --- | --- | --- |
| `--data-directory` | `/var/lib/memgraph` | Path for all permanent data |
| `--storage-snapshot-interval-sec` | 300 | Snapshot creation interval (0 to disable) |
| `--storage-snapshot-on-exit` | true | Create snapshot on exit |
| `--storage-snapshot-retention-count` | 3 | Number of snapshots kept |
| `--storage-wal-enabled` | true | Write-ahead logging |
| `--storage-recover-on-startup` | true | Recover persisted data on startup |
| `--storage-properties-on-edges` | true | Whether edges can have properties |
| `--memory-limit` | 0 (auto) | Total memory limit in MiB |

### Storage Requirements

- Snapshots require at minimum **4x the maximum dataset size**
- Default retention is 3 snapshots plus WAL files
- WAL file size: 20480 KiB minimum
- WAL flush every 100000 transactions (configurable)

## Custom Query Modules

User-defined query modules (Python/Cypher procedures) can be mounted via ConfigMaps:

```yaml
customQueryModules:
  - volume: my-modules
    file: my_module.py
```

Modules are mounted to `/var/lib/memgraph/internal_modules/`.

## Source References

- `code:charts/memgraph/values.yaml` — PVC configuration, storage class, and query module settings.
- `code:charts/memgraph/memgraph.conf` — Database persistence and snapshot configuration settings.
- `code:charts/memgraph/templates/statefulset.yaml` — Volume claim templates and volume mounts.
