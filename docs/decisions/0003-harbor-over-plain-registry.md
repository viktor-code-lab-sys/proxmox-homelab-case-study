# ADR-0003: Harbor instead of a plain registry

- **Status:** Accepted
- **Date:** 2026-09

## Context

The mirror must proxy three upstream registries, store non-image artifacts, restrict
pulls to authenticated clients and show what is stored.

## Decision

Harbor with one proxy-cache project per upstream (`dockerhub-proxy`, `k8s-proxy`,
`quay-proxy`), regular projects for binaries and charts, private access, a pull-only
robot account, and Trivy scanning.

## Alternatives considered

- **`registry:2` in pull-through mode** - one upstream per instance, no UI, no
  per-project access control, no scanning.
- **Nexus / Artifactory OSS** - broader formats, heavier to run and operate.

## Consequences

One robot credential is distributed to nodes, pipelines and namespaces and must be
rotated everywhere at once.
