# 4. Vault PKI

**Prerequisites:** steps 2-3; the offline root CA key.

1. Install Vault (single node, integrated Raft storage, TLS), initialise with 3 key
   shares / threshold 2, store the shares and root token offline.
2. Enable `pki`, tune max TTL to 10 years, generate an intermediate CSR inside Vault.
3. Sign it on the root CA with `CA:TRUE, pathlen:0`, `keyCertSign, cRLSign`, SKI/AKI;
   import with `pki/intermediate/set-signed`; set the default issuer and the URLs.
4. Create the role for the internal domain: subdomains allowed,
   `use_csr_common_name=true`, `use_csr_sans=true`, `require_cn=false`.
5. Enable AppRole; create roles and policies: VM issuer (`pki/issue` only), Atlantis
   (KV read, secret_id create/lookup, `auth/token/create`), cert-manager (`pki/sign`
   only), each bound to its CIDRs.
6. Enable KV v2 for automation secrets.
7. Revoke the initial root token once a new admin token exists.

**Check:** `vault write pki/issue/<role> common_name=test.homelab.example` returns a
certificate that `openssl verify` accepts against the full chain.
