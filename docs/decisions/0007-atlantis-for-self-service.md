# ADR-0007: Atlantis as the self-service interface

- **Status:** Accepted
- **Date:** 2026-09

## Context

Provisioning should be a reviewed request, not a terminal session on someone's laptop.

## Decision

Atlantis on its own VM: plans are posted as merge request comments, `atlantis apply`
runs them, and the MR is the audit trail.

## Alternatives considered

- **Backstage** - a full developer portal; far more than one workflow needs.
- **GitLab CI job** - possible, but no locking per project and no plan/apply dialogue
  in the MR.

## Consequences

The workflow is reviewable and locked per project. Atlantis has quirks worth knowing:
steps do not share environment variables
([lesson](../lessons-learned/atlantis-env-between-steps.md)), it needs a legacy GitLab
token, and an MR with zero changes stalls its diff polling.
