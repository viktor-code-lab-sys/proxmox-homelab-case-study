# cert-manager certificates rejected by Vault: common_name required

**Symptom.** The `Certificate` stayed `READY=False`:

```text
Vault failed to sign certificate: ... Code: 400. Errors:
* the common_name field is required, or must be provided in a CSR with
  "use_csr_common_name" set to true, unless "require_cn" is set to false
```

**Investigation.** Setting `use_csr_common_name=true` and `use_csr_sans=true` on the role
changed nothing - `vault read` confirmed both were applied.

**Root cause.** cert-manager follows current practice and puts names only in the SAN
extension; the CSR has an empty common name. Vault's `pki/sign` still requires one unless
the role says otherwise.

**Fix.** On the role used by cert-manager: `require_cn=false` together with
`use_csr_sans=true`.

**Takeaway.** When a config change "doesn't take", confirm what the client actually
sends - here, a CSR without a CN.
