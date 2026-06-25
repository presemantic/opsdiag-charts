# opsdiag-cicd-helm-pub

Public Helm charts for OpsDiag.

## Charts

- [`opsdiag-app-front`](./opsdiag-app-front) deploys the OpsDiag app frontend.
- [`opsdiag-app-api`](./opsdiag-app-api) deploys the OpsDiag app API and optional migration Job.
- [`opsdiag-app-agent`](./opsdiag-app-agent) deploys one parameterized OpsDiag app agent release.
- [`opsdiag-app-mcp-proxy`](./opsdiag-app-mcp-proxy) deploys the internal OpsDiag MCP proxy.
- [`opsdiag-app-mcp-server`](./opsdiag-app-mcp-server) deploys one parameterized OpsDiag MCP server release.
- [`opsdiag-app-sched`](./opsdiag-app-sched) deploys the OpsDiag app scheduler.
- [`opsdiag-app-connector`](./opsdiag-app-connector) deploys the customer-side OpsDiag app connector.

## OCI install

```bash
helm install opsdiag-app-connector \
  oci://europe-west1-docker.pkg.dev/prod-common-cicd/opsdiag-helm-pub/opsdiag-app-connector \
  --version 0.1.8
```

The app backend charts render `/app/config.yaml` from `values.config` and expose
ClusterIP services on port 8000. The frontend chart serves nginx on port 3000.
The agent and MCP server charts are reused for multiple releases by setting
`agent.kind` or `mcpServer.kind`/`mcpServer.providers`.

The chart defaults are compatible with OpenShift restricted SCC: they do not pin
`runAsUser`, `runAsGroup`, or `fsGroup`, allowing OpenShift to inject the
namespace-range random UID while keeping non-root and restricted container
security defaults.

The connector chart renders `/app/config.yaml` from `values.config`. Supply
the app `license` and base Control edge `url` at the top level, and put relay
settings under `relay.license` and `relay.url`; the public chart does not ship
license defaults.
