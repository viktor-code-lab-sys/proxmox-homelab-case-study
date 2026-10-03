# 9. Observability

**Prerequisites:** steps 6 and 8; Helm on the mirror and a control-plane host.

1. Mirror the `kube-prometheus-stack` chart into `helm-charts`.
2. Install with [`kubernetes/monitoring/values.yaml`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes/monitoring);
   patch the pull secret into every ServiceAccount, recreate pods.
3. Mirror the node_exporter release; run the Jenkins job from
   [`jenkins/`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/jenkins).

**Check:** the Prometheus targets page shows every VM exporter `up`; Grafana opens over
HTTPS.
