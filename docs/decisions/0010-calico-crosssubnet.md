# ADR-0010: Calico IPIP in CrossSubnet mode

- **Status:** Accepted
- **Date:** 2026-09

## Context

All nodes share one L2 subnet. `IPIPMode: Always` encapsulates even traffic that could
be routed directly, and control-plane hosts without `calico-node` have no tunnel
interface at all.

## Decision

Switch the default IP pool to `CrossSubnet`: encapsulate only when crossing subnets.

## Alternatives considered

- **Keep `Always`** - extra overhead and broken paths from non-Calico hosts.
- **VXLAN** - same trade-off, different encapsulation.

## Consequences

Direct routing inside the subnet; control-plane hosts reach pods with plain static
routes.
