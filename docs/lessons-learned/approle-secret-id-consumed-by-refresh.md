# The VM's one-time secret_id is already used when it boots

**Symptom.** VMs were created successfully, but Vault Agent could not log in, and later
`terraform apply` failed while destroying the `vault_approle_auth_backend_role_secret_id`
resource: "error deleting AppRole auth backend role SecretID".

**Investigation.** The role was hardened with `secret_id_num_uses=1` and a 10-minute TTL.
Between plan and apply, Terraform refreshes the resource, which reads it through Vault -
and in the same window the TTL could expire while a merge request waited for review.

**Root cause.** Security settings designed for a human handing over a secret do not fit
a tool that reads its own resources during refresh.

**Fix.** `secret_id_num_uses=0`, a TTL measured in hours, and protection moved to CIDR
binding (`secret_id_bound_cidrs`) plus a policy that allows only `pki/issue/<role>`. The
agent deletes the file after reading it. An already expired secret_id is removed from
state with `terraform state rm`.

**Takeaway.** Model secrets around the whole lifecycle of the tool that holds them,
including refresh and review delays, not only the happy path.
