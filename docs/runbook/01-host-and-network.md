# 1. Host and network

**Prerequisites:** Proxmox VE installed; firewall with a lab VLAN trunked to the host.

1. Bond the four lab NICs into an LACP bond, attach a VLAN-aware bridge (`vmbr1`).
2. Create the storages: ZFS `datastore` (VM disks), `backup_store`, `iso_store`.
3. On the firewall: DHCP server for VLAN 104 with the pool `.100-.200`, DNS zone for
   the internal domain, NTP.
4. Allow HTTPS (and later SNMP) to the firewall from the lab VLAN:
   `set allowaccess ping https` on the VLAN interface.
5. Create the restricted API user for automation: access profile with network and
   firewall objects only, two VDOMs, trusted hosts limited to the hypervisor and lab
   subnet, admin certificate from the internal CA.

**Check:** a test VM on VLAN 104 gets an address from the pool and resolves
`fw.homelab.example`.
