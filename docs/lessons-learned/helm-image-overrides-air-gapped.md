# kube-prometheus-stack behind an air-gapped registry

**Symptom.** A series of failures on the first install:

- the pre-install hook timed out (`ImagePullBackOff`),
- `kube-state-metrics` failed with `not found`,
- Grafana tried to pull `docker.io/<registry>/dockerhub-proxy/grafana/grafana`,
- the operator stayed in `ContainerCreating` with
  `secret "...-admission" not found`.

**Investigation and root causes.**

1. Chart-created ServiceAccounts do not inherit `imagePullSecrets` from `default`.
2. `global.imageRegistry` rewrote **every** image to the quay proxy, but
   kube-state-metrics is published on registry.k8s.io.
3. Grafana's `image.repository` given as one string was treated as a Docker Hub path.
4. With admission webhooks disabled, the operator still mounted the TLS secret that
   only the webhook job creates.

**Fix.** Per-component `registry` and `repository` values; Grafana with separate
`registry` and `repository`; `prometheusOperator.tls.enabled=false` together with
disabled webhooks; a loop that patches every ServiceAccount in the namespace with the
pull secret, then recreates the pods.

**Takeaway.** In an air-gapped install, read the chart's image defaults component by
component; global overrides assume every image comes from the same place.
