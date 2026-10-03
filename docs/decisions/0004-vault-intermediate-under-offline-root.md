# ADR-0004: Vault intermediate CA under the existing offline root

- **Status:** Accepted
- **Date:** 2026-09

## Context

A manual OpenSSL root CA was already trusted by every client in the network. Issuing
leaf certificates by hand did not scale, and secrets management was also needed.

## Decision

Keep the root offline. Generate an intermediate CSR in Vault's PKI engine, sign it once
with the root (`CA:TRUE, pathlen:0`, explicit SKI/AKI), and issue everything from Vault.

## Alternatives considered

- **step-ca** - excellent ACME-style CA, but PKI only.
- **cfssl** - a toolkit, not a service with auth and audit.
- **New root in Vault** - would require re-trusting every client.

## Consequences

Nothing had to be re-trusted. Vault is now critical infrastructure: unseal keys and
the root token need proper custody, and the intermediate expires in ten years.
