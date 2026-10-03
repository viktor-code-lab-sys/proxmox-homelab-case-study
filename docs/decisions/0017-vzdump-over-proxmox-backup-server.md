# ADR-0017: vzdump to NFS instead of Proxmox Backup Server

- **Status:** Accepted
- **Date:** 2026-09

## Context

All VMs need nightly backups to storage outside the host.

## Decision

Proxmox's built-in `vzdump` scheduler, snapshot mode, zstd, to an NFS export on the NAS.

## Alternatives considered

- **Proxmox Backup Server** - deduplication, incremental backups, encryption, but one
  more appliance VM to run and keep healthy.

## Consequences

Zero extra infrastructure. Full backups use more space; revisit when the VM count or
retention grows.
