# GCP MCP Registry Roadmap

The roadmap is intentionally incremental. Each phase should produce a demonstrable security or assurance capability before the project expands.

Because GCP MCP Registry is pre-v1.0, roadmap items should be treated as planned until they are implemented, tested, and documented.

## v0.1 — Local drift engine

**Goal:** Prove the core approved-versus-runtime comparison model.

- [ ] Define approved-state YAML/JSON format
- [ ] Connect to a test MCP server
- [ ] Discover runtime tools using `tools/list`
- [ ] Compare approved and runtime tool names
- [ ] Return deterministic scan status
- [ ] CLI output
- [ ] JSON output
- [ ] Unit tests
- [ ] Synthetic safe and drifted example MCP servers

Initial statuses should include:

- `COMPLIANT`
- `DRIFTED`
- `UNREACHABLE` or `ERROR` for validation failures, depending on the final result model

### Definition of done

A developer can run one documented command against a synthetic or controlled MCP endpoint and receive a deterministic finding showing whether runtime tools match an approved local baseline.

## v0.2 — Google Cloud Agent Registry

**Goal:** Replace or complement the local baseline with a real GCP registry source.

- [ ] Google Cloud authentication
- [ ] Read-only Google Cloud Agent Registry connector
- [ ] List registered MCP servers where supported
- [ ] Read approved MCP/tool metadata where supported
- [ ] Normalize registry data into the internal model
- [ ] Scan one or more registered MCP endpoints
- [ ] Document minimum IAM requirements
- [ ] Add connector-specific tests and mocks

### Definition of done

GCP MCP Registry can retrieve an approved MCP definition from a controlled GCP environment and compare it with the independently observed live endpoint.

## v0.3 — GCP-native operations

**Goal:** Run recurring assurance scans on Google Cloud.

- [ ] Container image
- [ ] Cloud Run deployment
- [ ] Cloud Run Job scanner
- [ ] Terraform module
- [ ] Secret Manager integration
- [ ] Cloud Scheduler recurring scans
- [ ] Cloud Logging structured findings
- [ ] Pub/Sub finding events
- [ ] Deployment security guidance

### Definition of done

A documented GCP deployment can run scheduled scans with least-privilege identity, produce structured findings, and operate without embedding credentials in source or configuration.

## v0.4 — Security findings and policy

**Goal:** Make results useful to security operations.

- [ ] Severity model
- [ ] Built-in policy rules
- [ ] Security Command Center findings
- [ ] Finding deduplication
- [ ] Finding lifecycle: open/resolved/reopened
- [ ] Evidence retention controls
- [ ] Policy evaluation tests
- [ ] Clear separation between drift findings and policy violations

### Definition of done

Security teams can distinguish configuration drift, runtime validation failures, and policy violations and can trace each finding back to supporting evidence.

## v0.5 — Shadow MCP discovery

**Goal:** Detect MCP infrastructure that is running but not represented in the approved registry.

Possible discovery sources should be researched before implementation. The project should avoid assuming that any single inventory source gives complete enterprise coverage.

- [ ] Pluggable discovery-source interface
- [ ] Reconcile discovered endpoints with approved registry state
- [ ] `UNREGISTERED` finding type
- [ ] Ownership and triage workflow
- [ ] Evidence showing discovery source and observed endpoint
- [ ] Discovery-source safety and permission guidance

### Definition of done

A discovered MCP endpoint that lacks an approved registry record can be identified as unmanaged without automatically assuming that every undiscovered endpoint is absent.

## v0.6 — Capability integrity

**Goal:** Detect more than simple tool-name changes.

- [ ] Input-schema drift
- [ ] Output-schema drift, where available
- [ ] Tool-description and metadata drift
- [ ] Server version/provenance evidence
- [ ] Canonical normalization rules
- [ ] Signed approved-state snapshot research
- [ ] Regression fixtures for subtle drift scenarios

### Definition of done

GCP MCP Registry can detect a security-relevant capability change even when the tool name itself does not change.

## v0.7 — Authorization assurance

**Goal:** Research and validate whether effective access remains consistent with approved policy.

Research areas:

- user → agent delegation;
- agent/workload identity;
- MCP server authorization;
- tool-level permissions;
- downstream identity propagation;
- effective privilege versus approved privilege;
- revocation and session propagation.

This phase is research-heavy and should not be represented as implemented until the project can observe or verify the required authorization evidence reliably.

### Definition of done

A concrete authorization-assurance model, supported evidence requirements, and implementable validation approach are documented and demonstrated for at least one controlled integration path.

## v0.8 — Enforcement integrations

**Goal:** Allow findings to inform or trigger optional runtime enforcement.

Potential work:

- [ ] generic enforcement-adapter interface
- [ ] Tetrate/Envoy integration research
- [ ] policy decision integration
- [ ] explicit opt-in enforcement mode
- [ ] safe failure behavior
- [ ] audit trail for enforcement actions
- [ ] emergency disable/revocation workflow research

Enforcement should remain separate from detection by default so assurance can operate safely in read-only mode.

## v0.9 — Supply chain and provenance

**Goal:** Strengthen confidence in the deployed MCP artifact and its approved state.

Potential work:

- [ ] artifact provenance evidence
- [ ] signed manifests or attestations
- [ ] CI/CD approval gates
- [ ] dependency/SBOM integration research
- [ ] approved artifact versus deployed artifact reconciliation

## v1.0 — Stable assurance platform

Candidate graduation criteria:

- stable connector interface;
- stable finding schema;
- documented threat and security model;
- repeatable Cloud Run deployment;
- least-privilege IAM guidance;
- production-quality tests;
- migration policy;
- documented compatibility matrix;
- documented failure semantics;
- reproducible security fixtures;
- clear support and release policy.

## Community priorities

Features should be prioritized using:

1. security impact;
2. interoperability;
3. reproducibility;
4. maintainability;
5. real-world contributor demand.

The project should avoid expanding breadth faster than its core approved-versus-runtime assurance behavior can be tested and explained.
