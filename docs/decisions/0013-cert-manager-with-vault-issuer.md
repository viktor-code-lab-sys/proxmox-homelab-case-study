# ADR-0013: cert-manager with a Vault ClusterIssuer

- **Status:** Accepted
- **Date:** 2026-09

## Context

Every Ingress needs a certificate from the internal CA, issued and renewed without
manual work.

## Decision

cert-manager with a ClusterIssuer that authenticates to Vault with AppRole and calls
`pki/sign/<role>`; Ingresses request certificates by annotation.

## Alternatives considered

- **Certificates issued by hand into TLS Secrets** - no renewal, repeated per app.

## Consequences

New apps get TLS with one annotation. The Vault role must accept CSRs without a common
name ([lesson](../lessons-learned/cert-manager-csr-without-cn.md)), and the webhook
requires the control plane to reach Services ([ADR 0011](0011-kube-proxy-on-control-plane.md)).
