# 10. Backup

**Prerequisites:** a NAS with NFS.

1. NAS: shared folder with NFS access for the lab subnet only, read/write.
2. Proxmox: `pvesm add nfs <name> --server <nas> --export <path> --content backup`.
3. Datacenter → Backup → Add: all VMs, snapshot mode, zstd, retention 7 daily / 4 weekly.

**Check:** a restore of one VM to a new VMID boots and serves its service.
