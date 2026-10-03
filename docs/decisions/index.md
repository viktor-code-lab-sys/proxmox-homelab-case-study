# Decisions

Architecture decision records: context, decision, the alternatives that lost, and the
consequences. New records start from the [template](0000-template.md).

| ADR | Decision |
| --- | --- |
| [0001](0001-kubernetes-the-hard-way.md) | Build Kubernetes the hard way |
| [0002](0002-air-gapped-supply-chain.md) | One mirror host is the only path to the internet |
| [0003](0003-harbor-over-plain-registry.md) | Harbor instead of a plain registry |
| [0004](0004-vault-intermediate-under-offline-root.md) | Vault intermediate CA under the existing offline root |
| [0005](0005-vault-agent-for-vm-certificates.md) | VMs fetch their own certificates with Vault Agent |
| [0006](0006-terraform-over-ansible-for-provisioning.md) | Terraform for provisioning |
| [0007](0007-atlantis-for-self-service.md) | Atlantis as the self-service interface |
| [0008](0008-gitlab-managed-terraform-state.md) | GitLab-managed Terraform state |
| [0009](0009-adopt-firewall-records-without-owning-them.md) | Adopt firewall DHCP/DNS without owning other records |
| [0010](0010-calico-crosssubnet.md) | Calico IPIP in CrossSubnet mode |
| [0011](0011-kube-proxy-on-control-plane.md) | Run kube-proxy on control-plane hosts |
| [0012](0012-local-path-and-ingress-behind-firewall-vip.md) | local-path storage and ingress behind a firewall VIP |
| [0013](0013-cert-manager-with-vault-issuer.md) | cert-manager with a Vault ClusterIssuer |
| [0014](0014-postgresql-for-nextcloud.md) | PostgreSQL for Nextcloud |
| [0015](0015-static-resolv-conf.md) | Plain glibc DNS instead of systemd-resolved |
| [0016](0016-jenkins-ssh-pipeline-for-vm-config.md) | Jenkins SSH pipeline for VM configuration |
| [0017](0017-vzdump-over-proxmox-backup-server.md) | vzdump to NFS instead of Proxmox Backup Server |
