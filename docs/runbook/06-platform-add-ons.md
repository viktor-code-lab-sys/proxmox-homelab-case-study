# 6. Platform add-ons

**Prerequisites:** step 5.

1. local-path-provisioner, marked as the default StorageClass.
2. ingress-nginx (bare-metal manifest) with NodePorts fixed to 30080/30443.
3. Firewall VIP for ingress on 80/443 over the two workers; one DNS record per app
   pointing at it.
4. cert-manager; Vault AppRole for it; `ClusterIssuer` from
   [`kubernetes/platform`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes/platform).

Every manifest is fetched on the mirror, stored as an OCI artifact, pulled onto the
cluster and has its image references rewritten to the proxy-cache projects.

**Check:** `kubectl get clusterissuer vault-issuer` shows `READY True`; a test Ingress
gets a certificate.
