# Compute and templates

Everything runs on one Proxmox VE host: AMD Ryzen 5 PRO 4650G (6 cores / 12 threads)
with 46 GB RAM.

## Storage tiers

| Tier | Device | Holds |
| --- | --- | --- |
| Boot | 250 GB NVMe | Proxmox OS |
| `datastore` (ZFS) | 1 TB SATA SSD | all VM disks, including etcd |
| `backup_store` | 512 GB NVMe (QLC) | local backups, scratch |
| `iso_store` | 128 GB SATA SSD | ISOs and images |

etcd is latency-sensitive (fsync under 10 ms at the 99th percentile). Both candidate
pools were measured with `fio` in an fsync-heavy profile; both passed, and the ZFS pool
won on endurance, since QLC NAND wears quickly under constant small writes.

## VM templates

Two templates are built from the official Debian 13 cloud image with `virt-customize`
and cloud-init:

| Template | Used by | Contents |
| --- | --- | --- |
| base | manual clones | guest agent, internal CA, NTP, DNS fix, SSH keys, prompt |
| vault-agent | Terraform | base + Vault Agent with AppRole auto-auth |

Templates are updated by cloning, changing, and converting back under the same VMID,
because Proxmox cannot rename a VMID and everything that clones a template refers to
it by number.

Two template-level fixes matter for every VM:

- **DNS:** `systemd-resolved` is disabled and `nsswitch.conf` uses plain DNS; see
  [the lesson](../lessons-learned/lan-domain-and-systemd-resolved.md).
- **Time sync on clones:** a vendor cloud-init snippet restarts `chrony` on first
  boot, so a clone does not start with the template's stale clock.

Code: [templates/](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/templates).
