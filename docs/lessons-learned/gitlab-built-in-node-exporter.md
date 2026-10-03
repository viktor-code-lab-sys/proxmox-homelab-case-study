# node_exporter will not start on the GitLab VM

**Symptom.** The rollout pipeline succeeded on four hosts; on the GitLab VM the unit
failed within 48 ms in a restart loop. The pipeline's own install step showed no error.

**Investigation.** The journal had the answer:
`listen tcp :9100: bind: address already in use`. `ss -tlnp` showed a `node_exporter`
process already listening on `127.0.0.1:9100`.

**Root cause.** GitLab Omnibus bundles and starts its own node_exporter for its internal
monitoring, bound to localhost only - unreachable for an external Prometheus, but enough
to block the port.

**Fix.** Leave Omnibus alone; run our exporter on `:9101` on that host only. The pipeline
reads per-host port overrides, and the Prometheus target uses 9101.

**Takeaway.** "Exit code 0" from a remote install is not health; check that the service
answers, which is what the pipeline's verify stage now does.
