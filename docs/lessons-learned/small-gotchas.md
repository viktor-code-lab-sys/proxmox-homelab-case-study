# Small gotchas

Short items that each cost from minutes to an hour.

| Area | Gotcha | Fix |
| --- | --- | --- |
| Terraform | HCL has no hex literals; `0xE0` is a syntax error | write `224` |
| Terraform | FortiOS DNS zone type is `primary` in the API, `master` in the CLI | use the API name |
| Terraform | FortiOS provider has no `port` argument | put the port into `hostname` |
| Terraform | bpg/proxmox puts disks on `local-lvm` unless `datastore_id` is set | set it explicitly |
| Terraform | Proxmox API certificate without an IP SAN fails TLS | use a name in the SAN or `insecure` knowingly |
| Proxmox | snippet upload resolves the node name over DNS from the runner | set `ssh.node.address` |
| Proxmox | a VMID cannot be renamed | clone to the target VMID, then destroy the old one |
| Bash | `!` inside double quotes triggers history expansion | single-quote tokens like `user@pve!id=...` |
| Git | during `rebase`, `--ours` is the branch being rebased onto | use `--theirs` for your own changes |
| Atlantis | MR with zero changes: "giving up polling merge_requests" | push the branch before opening the MR |
| ORAS | `oras push` rejects absolute paths; `pull` restores the pushed file name | push from the file's directory; consume that exact name |
| Downloads | a CDN edge served a certificate for another hostname | use the hostname the certificate is issued for |
| Guacamole | the image serves under `/guacamole/`, so `/` is 404 | `WEBAPP_CONTEXT=ROOT` |
