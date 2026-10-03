# 3. Supply chain

**Prerequisites:** step 2; the mirror VM is the only one with an outbound firewall policy.

1. Install apt-cacher-ng; point every template at it with an APT proxy setting.
2. Install Docker and Harbor (with Trivy) using a certificate from the internal CA.
3. Create registry endpoints for Docker Hub, registry.k8s.io and quay.io, and one
   proxy-cache project per upstream (`dockerhub-proxy`, `k8s-proxy`, `quay-proxy`).
4. Create regular projects `k8s-binaries` and `helm-charts`.
5. Create a pull-only robot account across the five projects.
6. Install ORAS on the mirror; copy it to the other VMs once with `scp`.
7. Mirror Kubernetes binaries, etcd, containerd, runc and CNI plugins with
   [`supply-chain/mirror-binary.sh`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/supply-chain).

**Check:** `oras pull registry.homelab.example/k8s-binaries/kubelet:v1.31.0` works from
a lab VM that has no internet access.
