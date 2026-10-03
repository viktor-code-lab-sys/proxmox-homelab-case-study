# ADR-0005: VMs fetch their own certificates with Vault Agent

- **Status:** Accepted
- **Date:** 2026-09

## Context

Certificates issued by Terraform would land in the Terraform state and would not renew
on their own.

## Decision

Bake Vault Agent with AppRole auto-auth into the VM template. Terraform only creates a
CIDR-bound secret_id and delivers it through cloud-init; the agent requests,
writes and renews the certificate and reloads services.

## Alternatives considered

- **Terraform `vault_pki_secret_backend_cert`** - private keys in state, renewal
  needs a new apply.
- **certbot against an ACME endpoint** - workable, but adds another moving part for
  internal names.

## Consequences

Keys never leave the VM. The template carries more logic (agent config, a reload hook
that must always exit 0), and the AppRole needs unlimited secret_id uses - see
[the lesson](../lessons-learned/approle-secret-id-consumed-by-refresh.md).
