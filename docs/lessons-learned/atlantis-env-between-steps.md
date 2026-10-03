# Atlantis loses credentials between workflow steps

**Symptom.** `terraform init` inside Atlantis failed with "HTTP remote state endpoint
requires auth", although the previous custom step exported the credentials.

**Investigation.**

1. Exporting into `~/.bashrc` from a `run` step: no effect.
2. `env` steps with `command:` that called `vault` inside the container: the official
   image refused to execute added binaries ("operation not permitted").
3. Secrets prepared on the host and sourced in a `run` step: the next, built-in `init`
   step still had no credentials - the log line was the built-in step, not ours.
4. The repository's own `atlantis.yaml` still pointed at the default workflow, and the
   server allowed repo overrides, so the custom workflow never ran at all.

**Root cause.** Each Atlantis step is a separate process, and the version wrapper around
`terraform` does not reliably pass `TF_HTTP_*` through; on top of that, the
repo-level config silently selected another workflow.

**Fix.** One `run` step that sources the env file and runs `init` with
`-backend-config` flags and `plan`/`apply` in the same shell. The repo config names the
workflow explicitly. Secrets are written by a host-side script from Vault.

**Takeaway.** When a fix changes nothing, check that the code you are changing is the
code that runs - the log line format gave it away.
