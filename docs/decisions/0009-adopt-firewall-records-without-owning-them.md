# ADR-0009: Adopt firewall DHCP and DNS without owning other people's records

- **Status:** Accepted
- **Date:** 2026-09

## Context

The DHCP server and DNS zone already contain reservations and records for devices
outside the lab (switches, NAS, desktops). The FortiOS provider manages each as one
resource with nested blocks: whatever is not in the configuration is deleted.

## Decision

Import both objects, export the existing entries into "legacy" lists, and build the
nested blocks as `concat(legacy, managed)`. The first plan must be zero adds and zero
destroys before any apply.

## Alternatives considered

- **A separate DHCP server / zone for the lab** - clean ownership, but every client
  in the network uses this resolver.
- **Manage only new records with individual resources** - the provider does not offer
  per-entry resources for these tables.

## Consequences

The legacy lists describe the home network, so they live in a private variables file,
never in the public repository.
