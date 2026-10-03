# Kubernetes platform

Kubernetes v1.31 assembled from binaries ("the hard way"): three control-plane VMs
running etcd, API server, controller manager and scheduler as systemd units, and two
workers with containerd, kubelet and kube-proxy.

| Component | Choice |
| --- | --- |
| etcd | v3.5, three members colocated with the control plane, TLS everywhere |
| API endpoint | firewall VIP over the three API servers |
| CNI | Calico v3.31, IPIP in CrossSubnet mode ([ADR 0010](../decisions/0010-calico-crosssubnet.md)) |
| DNS | CoreDNS forwarding to the firewall resolver |
| Pod / Service CIDR | `10.200.0.0/16` / `10.32.0.0/24` |
| Storage | local-path-provisioner as the default StorageClass |
| Ingress | ingress-nginx on fixed NodePorts behind the firewall VIP |
| Certificates | cert-manager with a Vault ClusterIssuer |

## Control plane reaches Services

Control-plane nodes are not registered as Nodes, but they still run kube-proxy, load
`nf_conntrack`, enable IP forwarding and carry static routes to the pod subnets.
Without that, the API server cannot reach any ClusterIP and every admission webhook
times out ([ADR 0011](../decisions/0011-kube-proxy-on-control-plane.md),
[lesson](../lessons-learned/admission-webhook-timeouts.md)).

## Applications

| App | Data | Isolation |
| --- | --- | --- |
| Nextcloud 35 | PostgreSQL 16 StatefulSet, PVCs on local-path | NetworkPolicy: ingress from ingress-nginx only; egress inside namespace + DNS |
| Apache Guacamole 1.6 | PostgreSQL 16 with schema from `initdb.sh` | same, plus egress to the lab subnet for RDP/SSH targets |

Code: [kubernetes/](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes).
