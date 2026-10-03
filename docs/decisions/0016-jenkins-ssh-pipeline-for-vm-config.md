# ADR-0016: Jenkins SSH pipeline for VM configuration

- **Status:** Accepted
- **Date:** 2026-09

## Context

Small configuration tasks across existing VMs (installing an exporter) do not justify
re-provisioning, and Jenkins was already running.

## Decision

A parameterised pipeline with a host list, an SSH key and the registry credential from
the Jenkins store, and an idempotent install script piped over SSH.

## Alternatives considered

- **Ansible** - the natural tool; would add a control node and inventory for one job.

## Consequences

Reusable for any "run this on these hosts" task. The pipeline must be idempotent by
design: it checks the service before touching anything.
