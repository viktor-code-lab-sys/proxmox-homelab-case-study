# Backup

Every VM is backed up nightly by Proxmox `vzdump` in snapshot mode, compressed with
zstd, to an NFS export on a NAS outside the host.

| Setting | Value |
| --- | --- |
| Target | NFS export restricted to the lab subnet |
| Mode | snapshot (no downtime) |
| Compression | zstd |
| Retention | 7 daily, 4 weekly |

Proxmox Backup Server was evaluated and dropped for now: deduplication and
encryption are not worth another appliance at this scale
([ADR 0017](../decisions/0017-vzdump-over-proxmox-backup-server.md)).

Application-consistent dumps (Vault Raft snapshots, PostgreSQL dumps, etcd
snapshots, GitLab backups) are on the [roadmap](../roadmap.md).
