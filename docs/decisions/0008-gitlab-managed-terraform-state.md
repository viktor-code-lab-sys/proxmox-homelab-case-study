# ADR-0008: GitLab-managed Terraform state

- **Status:** Accepted
- **Date:** 2026-09

## Context

State created on a workstation is invisible to Atlantis, which then tries to create
objects that already exist.

## Decision

Use GitLab's built-in HTTP state backend with locking. Engineers and Atlantis read and
write the same state.

## Alternatives considered

- **S3-compatible object storage** - needs another service in an air-gapped lab.
- **Consul** - another cluster to run.

## Consequences

No new infrastructure. The backend token must reach `terraform init`; in Atlantis this
works reliably only through `-backend-config` flags.
