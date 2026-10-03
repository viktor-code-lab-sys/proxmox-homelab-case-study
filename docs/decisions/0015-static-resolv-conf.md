# ADR-0015: Plain glibc DNS instead of systemd-resolved

- **Status:** Accepted
- **Date:** 2026-09

## Context

The internal domain ends in `.lan`. On the Debian images, `systemd-resolved` did not
resolve names in it through the configured upstream, while `dig` against the same
server worked.

## Decision

Bake into both templates: `systemd-resolved` disabled and masked, a static
`/etc/resolv.conf`, and `hosts: files myhostname dns` in `nsswitch.conf`.

## Alternatives considered

- **Per-link routing domains with `resolvectl`** - not persistent across DHCP renewals
  in this setup, and the NSS `resolve` module still short-circuits lookups.
- **Rename the domain** - the right long-term fix; too disruptive for a running network.

## Consequences

Name resolution behaves the same on every VM. No local DNS cache.
