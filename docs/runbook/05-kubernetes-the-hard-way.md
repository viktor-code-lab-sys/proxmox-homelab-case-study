# 5. Kubernetes the hard way

**Prerequisites:** steps 3-4; five VMs from the base template (3 control plane, 2 workers).

1. Generate CSRs for admin, controller-manager, scheduler, kube-proxy, each kubelet
   and the API server; sign them with `pki/sign-verbatim` so Subjects stay intact.
2. Write the kubeconfigs; the API server SAN list includes the API VIP, all
   control-plane addresses and the first Service IP.
3. Firewall: server-load-balancer VIP on 6443 over the three control-plane nodes, with
   a TCP health check; the policy references the zone, not the interface.
4. etcd on the three control-plane nodes with peer and client TLS; start all three
   together.
5. API server, controller manager and scheduler as systemd units; encryption at rest
   for Secrets; RBAC for API server → kubelet.
6. Workers: containerd (systemd cgroups), kubelet, kube-proxy.
7. **Control-plane hosts also get kube-proxy**, `nf_conntrack`, `ip_forward=1` and
   static routes to the pod subnets.
8. Calico with the pod CIDR, images from the quay proxy, IP pool in CrossSubnet mode.
9. CoreDNS forwarding to the firewall resolver (not `/etc/resolv.conf`, which loops).

**Check:** `kubectl get nodes` all Ready; a pod resolves `kubernetes.default`; from a
control-plane host `curl -k https://<any ClusterIP>` connects.
