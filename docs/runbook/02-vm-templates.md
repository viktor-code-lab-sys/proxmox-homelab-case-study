# 2. VM templates

**Prerequisites:** step 1; Debian 13 generic cloud image; internal root CA certificate.

1. `virt-customize` the image: qemu-guest-agent, CA certificate, chrony pointed at the
   firewall.
2. Import as a VM with a cloud-init drive, serial console, SSH key, default user.
3. Add a vendor snippet that restarts chrony on first boot (clones start with the
   template's clock).
4. Apply the DNS fix and the prompt: [`templates/`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/templates).
5. Convert to the **base** template.
6. Clone it, install Vault Agent with [`templates/vault-agent`](https://github.com/viktor-code-lab-sys/proxmox-homelab-iac/tree/main/templates/vault-agent),
   `systemctl enable` (not start) the agent, convert to the **vault-agent** template.

**Check:** a fresh clone gets its reserved address, resolves internal names with
`getent hosts`, and has correct time within seconds of boot.
