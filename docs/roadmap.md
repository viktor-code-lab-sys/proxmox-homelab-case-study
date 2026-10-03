# Roadmap

What I would build next, roughly in order.

- [ ] **Application-consistent backups** - Vault Raft snapshots, PostgreSQL dumps,
      etcd snapshots and GitLab backups to the NAS on a schedule.
- [ ] **Firewall SNMP** - resolve the silent responder
      ([lesson](lessons-learned/fortigate-snmp-silent.md)) and add the firewall to Grafana.
- [ ] **GitOps** - Argo CD on the cluster for the platform add-ons and applications.
- [ ] **Template builds as code** - Packer instead of hand-run clone-and-convert.
- [ ] **Shared storage** - NFS-backed StorageClass so stateful pods can move.
- [ ] **Rename the internal domain** away from `.lan` and drop the resolver workaround.
- [ ] **Alerting** - Alertmanager routes for certificate expiry, disk, and node health.
