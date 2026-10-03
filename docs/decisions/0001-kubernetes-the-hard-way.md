# ADR-0001: Build Kubernetes the hard way

- **Status:** Accepted
- **Date:** 2026-09

## Context

The cluster exists to learn how Kubernetes works, not only to run workloads. Managed
installers hide certificate generation, component flags and bootstrap order, which
are exactly the parts that fail in production.

## Decision

Assemble the cluster from release binaries and systemd units, with every certificate
issued by the internal PKI and every kubeconfig written by hand.

## Alternatives considered

- **kubeadm** - fastest path to a cluster, but generates its own CA and hides the
  wiring this project wants to own.
- **k3s / RKE2** - excellent for small clusters; even more opinionated.

## Consequences

Every component is understood and replaceable. The cost is operational: there is no
`kubeadm upgrade`, and gaps that installers close silently appear as incidents - see
[kube-proxy on the control plane](0011-kube-proxy-on-control-plane.md).
