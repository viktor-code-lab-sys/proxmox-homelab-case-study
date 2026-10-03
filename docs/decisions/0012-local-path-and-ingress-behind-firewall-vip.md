# ADR-0012: local-path storage and ingress-nginx behind a firewall VIP

- **Status:** Accepted
- **Date:** 2026-09

## Context

A bare-metal cluster has neither a cloud load balancer nor network storage, and the
first application needs both.

## Decision

local-path-provisioner as the default StorageClass; ingress-nginx on fixed NodePorts
behind a firewall server-load-balancer VIP, reusing the pattern of the API VIP.

## Alternatives considered

- **NFS provisioner on the NAS** - shared storage, but a hard dependency on the NAS
  for every pod; kept as a later migration.
- **MetalLB** - a second load-balancing layer when the firewall already does it.

## Consequences

Simple and fast; volumes are tied to one node, so stateful pods do not move. One DNS
record per app points at the ingress VIP; no wildcard records.
