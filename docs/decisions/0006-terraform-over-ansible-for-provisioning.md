# ADR-0006: Terraform for provisioning

- **Status:** Accepted
- **Date:** 2026-09

## Context

Creating a VM touches three systems - hypervisor, firewall, Vault - that must stay
consistent, and deleting a VM must clean up all three.

## Decision

Terraform with the `bpg/proxmox`, `fortinetdev/fortios` and `hashicorp/vault`
providers, one map entry per VM.

## Alternatives considered

- **Ansible** - great for configuring what exists; weaker at tracking and removing
  what it created.
- **Shell scripts against three APIs** - no plan, no state, no drift detection.

## Consequences

Plans show exactly what changes on all three systems before it happens. State must be
shared and locked ([ADR 0008](0008-gitlab-managed-terraform-state.md)), and existing
firewall objects must be adopted carefully
([ADR 0009](0009-adopt-firewall-records-without-owning-them.md)).
