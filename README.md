# proxmox-homelab-case-study

[![Pages](https://github.com/viktor-code-lab-sys/proxmox-homelab-case-study/actions/workflows/pages.yml/badge.svg)](https://github.com/viktor-code-lab-sys/proxmox-homelab-case-study/actions/workflows/pages.yml)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](LICENSE)

Case study of an air-gapped homelab platform on a single Proxmox VE host.

**Read it here:** https://viktor-code-lab-sys.github.io/proxmox-homelab-case-study/

- **Architecture** - network, PKI, supply chain, provisioning flow, Kubernetes platform
- **Decisions** - 17 architecture decision records with the alternatives that lost
- **Lessons learned** - incident write-ups: symptom, investigation, root cause, fix
- **Runbook** - how to rebuild every component
- **Code** - https://github.com/viktor-code-lab-sys/proxmox-homelab-iac

## Build locally

```bash
python3 -m venv .venv && . .venv/bin/activate
pip install -r requirements.txt
mkdocs serve        # http://127.0.0.1:8000
mkdocs build --strict
```
