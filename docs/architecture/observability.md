# Observability

kube-prometheus-stack runs on the cluster; node_exporter runs on every service VM and
is rolled out by an idempotent Jenkins pipeline.

| Source | Collector | Notes |
| --- | --- | --- |
| Kubernetes objects | kube-state-metrics | |
| Cluster nodes | node-exporter DaemonSet | |
| Service VMs | node_exporter as systemd unit | GitLab uses port 9101: Omnibus already binds 127.0.0.1:9100 |
| Firewall | SNMP exporter | open: the device accepts but never answers ([lesson](../lessons-learned/fortigate-snmp-silent.md)) |

Grafana sits behind the shared ingress VIP with a certificate from the Vault issuer;
its admin password comes from an existing Secret, not the values file.

## Rollout pipeline

The Jenkins job reads a host list, pulls the exporter binary from the internal
registry with ORAS, installs a systemd unit and verifies `/metrics`. A host that
already answers on the expected port is skipped, so the job can run any time
([ADR 0016](../decisions/0016-jenkins-ssh-pipeline-for-vm-config.md)).

Code: [kubernetes/monitoring](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes/monitoring),
[jenkins/](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/jenkins).
