# 8. Self-service provisioning

**Prerequisites:** steps 2 and 4; GitLab; an Atlantis VM.

1. Hypervisor API token with VM, SDN and datastore permissions on the two datastores;
   SSH key for snippet uploads.
2. Firewall API token (step 1) and its CA bundle.
3. Secrets into Vault KV; host-side refresh script on the Atlantis VM.
4. Atlantis container with the server-side `repos.yaml`; webhook from GitLab (allow the
   private address in GitLab's outbound settings).
5. Import the existing DHCP server and DNS zone, export the legacy lists into a private
   variables file, confirm a zero-change plan.
6. Open an MR that adds one entry to `vms`; comment `atlantis apply`.

Code: [`terraform/`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/terraform), [`atlantis/`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/atlantis).

**Check:** the new VM answers on its name, and `/etc/ssl/local/fullchain.pem` on it was
written by Vault Agent within a minute of boot.
