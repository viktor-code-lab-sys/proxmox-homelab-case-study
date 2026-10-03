# Lessons learned

Incident write-ups in one format: symptom, investigation, root cause, fix, takeaway
([template](template.md)).

| Write-up | Area | Status |
| --- | --- | --- |
| [Admission webhooks time out on a hard-way cluster](admission-webhook-timeouts.md) | Kubernetes networking | fixed |
| [Internal names fail to resolve while dig works](lan-domain-and-systemd-resolved.md) | DNS | fixed |
| [The one-time secret_id is already used when the VM boots](approle-secret-id-consumed-by-refresh.md) | Vault / Terraform | fixed |
| [Atlantis loses credentials between workflow steps](atlantis-env-between-steps.md) | Atlantis | fixed |
| [cert-manager certificates rejected: common_name required](cert-manager-csr-without-cn.md) | PKI | fixed |
| [kube-prometheus-stack behind an air-gapped registry](helm-image-overrides-air-gapped.md) | Helm | fixed |
| [node_exporter will not start on the GitLab VM](gitlab-built-in-node-exporter.md) | Monitoring | fixed |
| [FortiGate accepts SNMP but never answers](fortigate-snmp-silent.md) | Monitoring | open |
| [Small gotchas](small-gotchas.md) | various | - |
