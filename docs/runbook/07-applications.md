# 7. Applications

**Prerequisites:** step 6.

- **Nextcloud** - [`kubernetes/apps/nextcloud`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes/apps/nextcloud):
  secrets from Vault, apply, then turn off the internet-connectivity self-check.
- **Guacamole** - [`kubernetes/apps/guacamole`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/kubernetes/apps/guacamole):
  generate the schema with `initdb.sh` on the mirror, ship it as an artifact, load it
  into a ConfigMap, apply, change the default admin password.

Both namespaces carry a NetworkPolicy: ingress only from ingress-nginx, egress only
inside the namespace plus DNS (Guacamole also reaches the lab subnet).

**Check:** both sites answer over HTTPS with a certificate from the internal CA.
