# GCP MCP Registry Security Model

## Security objective

GCP MCP Registry's primary objective is to detect when the **observed runtime capability state** of an MCP server differs from its **approved state**.

It provides assurance evidence; it does not make an MCP deployment secure by itself.

The project is currently pre-v1.0. Some controls described here are architectural expectations rather than implemented guarantees.

## Assets to protect

Important assets include:

- registry credentials and access tokens;
- MCP endpoint credentials;
- approved-state metadata;
- observed runtime metadata;
- findings and evidence;
- project configuration;
- GCP service-account or workload identities;
- policy configuration;
- exporter credentials and destinations.

## Primary threats

### Capability drift

A server exposes a tool or capability that was not part of the approved baseline.

### Silent capability modification

A previously approved tool keeps its name but its schema, description, annotations, or other security-relevant metadata changes.

### Approved capability removal

A capability that was approved is no longer present at runtime, potentially indicating deployment inconsistency, unexpected version change, or configuration drift.

### Shadow/unregistered MCP

An MCP server is reachable or used but has no approved registry record.

### Scanner manipulation

A malicious or compromised MCP endpoint returns malformed or adversarial metadata intended to crash, exploit, confuse, or mislead the scanner.

### Credential leakage

Secrets used to reach Agent Registry or MCP endpoints are exposed through logs, findings, exceptions, configuration, or test output.

### Excessive cloud permissions

GCP MCP Registry receives broader GCP permissions than it needs, increasing blast radius if compromised.

### False compliance

Normalization, discovery, comparison, or policy logic incorrectly concludes that observed and approved states match.

False compliance is particularly important because a failed validation must not silently become a successful security result.

### Registry tampering or incorrect approved state

The approved registry may contain incorrect, stale, maliciously modified, or incomplete information.

GCP MCP Registry initially treats approved state as organizational intent, but it should validate the structure of registry data and preserve evidence of its source.

### Endpoint substitution

A configured endpoint may be redirected, replaced, or resolved to an unintended target.

Endpoint-validation and authentication controls should reduce the risk that the scanner validates the wrong server.

### Finding or evidence tampering

Downstream logs, events, databases, or security findings may be altered, dropped, duplicated, or misrouted.

Export and persistence integrations should preserve enough identifiers and timestamps to support investigation.

## Security controls

### Least privilege

Registry discovery should use read-only permissions.

Write or administrative permissions should not be required for normal scans.

Cloud identities should receive only the permissions required for their specific connector, scanner, exporter, or deployment role.

### Untrusted-input handling

Remote MCP metadata must be parsed as untrusted data.

The scanner must not execute code contained in descriptions, schemas, prompts, annotations, or other metadata.

The implementation should use bounded parsing and validation where practical.

### No tool execution by default

Capability discovery should not invoke arbitrary MCP business tools simply to determine whether they exist.

Read-only metadata and supported discovery methods should be preferred.

### Secret hygiene

Security requirements include:

- no tokens in logs;
- redact credential-bearing headers;
- prefer Secret Manager or equivalent secret storage for deployed credentials;
- never include secrets in finding evidence;
- avoid storing credentials in repository files;
- ensure exception handling does not expose secret values.

### Bounded network behavior

Scanners should use:

- configured connection and read timeouts;
- response-size limits;
- endpoint validation;
- safe redirect behavior where relevant;
- controlled retry behavior;
- restrictions appropriate to the deployment environment.

These controls help reduce denial-of-service, SSRF, and unintended network-access risk.

### Evidence-backed findings

A finding should record enough normalized evidence to reproduce why a mismatch was detected without storing unnecessary sensitive data.

Useful evidence may include:

- approved capability identifiers;
- observed capability identifiers;
- normalized hashes;
- timestamps;
- source identifiers;
- comparison category;
- relevant policy rule.

### Safe failure semantics

A scan that cannot validate runtime state should not be reported as `COMPLIANT`.

Authentication failures, unreachable endpoints, malformed responses, normalization failures, and internal errors should result in explicit failure states.

### Separation of approved and observed state

Approved state and runtime state must be obtained independently enough that comparison remains meaningful.

The registry record should not be reused as proof of what is running.

### Modular integrations

Registry connectors, runtime scanners, policy engines, and exporters should remain modular.

Compromise or failure in one integration should not unnecessarily grant access to another integration.

## Trust assumptions

Initial versions may assume:

- the approved registry represents organizational intent;
- Google Cloud authentication is functioning correctly;
- configured MCP endpoints correspond to the records being evaluated;
- the runtime discovery method accurately reflects the capabilities exposed through that protocol surface;
- system clocks and timestamps are sufficiently reliable for evidence ordering.

These are assumptions, not guarantees.

Future versions may reduce these assumptions through stronger provenance, attestation, signed manifests, endpoint identity verification, and deployment evidence.

## Trust boundaries

Important trust boundaries include:

1. Google Cloud control-plane APIs to GCP MCP Registry.
2. GCP MCP Registry to remote MCP endpoints.
3. Secret storage to runtime components.
4. Runtime components to logging, Pub/Sub, SCC, databases, or SIEMs.
5. Findings and evidence to downstream users or automation.

Data crossing these boundaries should be validated and handled according to least privilege.

## Security logging

Security-relevant events should eventually include:

- scan start and completion;
- registry read failure;
- endpoint authentication failure;
- unreachable endpoint;
- malformed MCP response;
- drift finding creation;
- policy violation;
- exporter failure;
- configuration validation failure.

Logs should identify the affected server or scan without exposing secret values.

## Out of scope for initial releases

GCP MCP Registry does not initially protect against every MCP-specific threat.

Examples outside the initial scope include:

- prompt injection inside tool-returned content;
- model behavior vulnerabilities;
- complete data-loss prevention;
- downstream application authorization bugs;
- malicious code inside an approved MCP server;
- endpoint compromise that perfectly mimics approved metadata while changing hidden behavior;
- full software supply-chain compromise detection;
- comprehensive user → agent → server delegated authorization validation;
- automatic runtime blocking of malicious traffic.

These may become integration or research areas, but they should not be overstated as capabilities the project already provides.

## Security invariants

As the project develops, implementations should preserve these invariants where practical:

1. A failed scan must not silently become `COMPLIANT`.
2. Normal discovery should not require destructive permissions.
3. Secrets must not be written to findings or logs.
4. Runtime metadata must be treated as untrusted input.
5. Approved state must remain distinguishable from observed state.
6. Findings should be explainable from preserved evidence.
7. Vendor-specific integrations should not be required by the core comparison engine.
8. Enforcement should be opt-in unless the project scope changes explicitly.

## Future security research

Potential future areas include:

- delegated authorization assurance;
- continuous endpoint attestation;
- signed capability manifests;
- artifact provenance;
- CI/CD deployment reconciliation;
- OPA/Rego policy integration;
- Tetrate/Envoy enforcement integration;
- shadow MCP discovery;
- revocation propagation;
- cross-MCP data-movement analysis.

Future research items should remain clearly labeled as planned until implemented and validated.
