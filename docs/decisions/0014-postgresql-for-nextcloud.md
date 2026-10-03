# ADR-0014: PostgreSQL for Nextcloud

- **Status:** Accepted
- **Date:** 2026-09

## Context

Nextcloud supports MariaDB and PostgreSQL; Guacamole would need a database as well.

## Decision

PostgreSQL 16 as a StatefulSet per application.

## Alternatives considered

- **MariaDB** - equally supported; PostgreSQL is the upstream recommendation for
  larger file libraries and keeps one database engine for both apps.

## Consequences

One engine to operate, back up and monitor.
