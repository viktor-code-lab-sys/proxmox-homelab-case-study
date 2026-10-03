# Runbook

How to rebuild the platform from an empty host, in dependency order. Each page lists
prerequisites, steps and a check; code lives in the IaC repository.

| Step | Page | Depends on |
| --- | --- | --- |
| 1 | [Host and network](01-host-and-network.md) | - |
| 2 | [VM templates](02-vm-templates.md) | 1 |
| 3 | [Supply chain](03-supply-chain.md) | 2 |
| 4 | [Vault PKI](04-vault-pki.md) | 2, 3 |
| 5 | [Kubernetes the hard way](05-kubernetes-the-hard-way.md) | 3, 4 |
| 6 | [Platform add-ons](06-platform-add-ons.md) | 5 |
| 7 | [Applications](07-applications.md) | 6 |
| 8 | [Self-service provisioning](08-self-service-provisioning.md) | 2, 4 |
| 9 | [Observability](09-observability.md) | 6, 8 |
| 10 | [Backup](10-backup.md) | 1 |

Conventions: domain `homelab.example`, lab subnet `10.0.104.0/24`, registry
`registry.homelab.example`. Replace them with your own values.
