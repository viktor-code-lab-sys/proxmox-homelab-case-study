# ADR-0002: One mirror host is the only path to the internet

- **Status:** Accepted
- **Date:** 2026-09

## Context

Lab machines that download directly from the internet make builds irreproducible
(upstream changes, rate limits, outages) and widen the attack surface.

## Decision

Only the registry mirror host has egress. It caches Debian packages, proxies container
registries, and stores every other artifact (binaries, charts, SQL, manifests) as OCI
artifacts. Every other host pulls from it.

## Alternatives considered

- **Direct downloads with pinned checksums** - reproducible, but every host still
  needs egress.
- **Offline bundle copied by hand** - fully air-gapped but painful to update.

## Consequences

Builds survive upstream outages and every artifact is scanned and inventoried in one
place. Each new dependency costs one mirroring step; the scripts in `supply-chain/`
keep it to one command.
