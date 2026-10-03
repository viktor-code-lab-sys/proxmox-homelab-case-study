# Admission webhooks time out on a hard-way cluster

**Symptom.** Creating a cert-manager `ClusterIssuer` failed every time:

```text
failed calling webhook "webhook.cert-manager.io": failed to call webhook:
Post "https://cert-manager-webhook.cert-manager.svc:443/validate?timeout=30s":
context deadline exceeded
```

The webhook pod was `Running` and its Endpoints were populated.

**Investigation.** Five layers, each hiding the next:

1. **No route to pods.** `ip route` on a control-plane host showed nothing for the pod
   CIDR: those hosts are not Calico nodes. Added static routes per node block.
2. **IPIP everywhere.** The pool was `IPIPMode: Always`, but control-plane hosts have no
   tunnel interface. Switched to `CrossSubnet`.
3. **Pod IP works, Service IP does not.** `curl https://<pod-ip>:10250/validate` returned
   HTTP 200, `curl https://<cluster-ip>:443` timed out. ClusterIPs exist only as
   iptables rules written by kube-proxy - which did not run on the control plane.
4. **kube-proxy would not start.** `open /proc/sys/net/netfilter/nf_conntrack_max: no such
   file or directory`: the module was never loaded on those hosts, and IP forwarding
   was off.
5. **Intermittent success.** After fixing one host, applies still failed randomly: the
   API VIP spreads requests over three API servers, and two were still unfixed.

**Root cause.** The API server reaches webhooks through Service ClusterIPs. Without
kube-proxy (and its kernel prerequisites) on every host that runs an API server, those
addresses do not exist there.

**Fix.** On every control-plane host: `modprobe nf_conntrack`, `ip_forward=1`, kube-proxy
as a systemd unit, idempotent static routes (`ip route replace`). Pool in CrossSubnet.

**Takeaway.** On a cluster behind a load balancer, a fix is not done until it is on
every backend; test against each node, not the VIP.
