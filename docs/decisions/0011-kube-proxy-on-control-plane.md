# ADR-0011: Run kube-proxy on control-plane hosts

- **Status:** Accepted
- **Date:** 2026-09

## Context

In the hard-way layout the control-plane hosts run the API server but no kubelet or
kube-proxy. The API server calls admission webhooks through their Service ClusterIP,
which exists only as iptables rules written by kube-proxy.

## Decision

Run kube-proxy on every control-plane host (with `nf_conntrack` loaded and
`ip_forward=1`) without registering the hosts as Nodes, plus static routes to the pod
subnets.

## Alternatives considered

- **Register control-plane hosts as tainted Nodes** - also works; more moving parts
  (kubelet certificates, CNI on masters).
- **Webhooks by URL instead of Service** - would have to be patched in every chart.

## Consequences

Any webhook-based add-on works out of the box. Three more units to keep in step with
the workers. Full story: [the lesson](../lessons-learned/admission-webhook-timeouts.md).
