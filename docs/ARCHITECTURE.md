# GCP MCP Registry Architecture

## Purpose

GCP MCP Registry provides a continuous-assurance layer between an **approved MCP registry state** and the **observed runtime state** of MCP servers.

The project is GCP-native in its primary deployment and integration model while keeping the core assurance logic modular where practical.

The architecture described here represents the intended design. Because the project is currently pre-v1.0, some components are planned rather than implemented.

## Core assurance loop

```text
1. Read approved state
        |
        v
2. Discover live MCP capabilities
        |
        v
3. Normalize approved + observed data
        |
        v
4. Compare states
        |
        v
5. Evaluate policy
        |
        v
6. Produce evidence-backed findings
        |
        v
7. Export findings to security/operations systems
```

The central design principle is:

> **Registry state is not runtime truth.**

An approved registry record expresses organizational intent. GCP MCP Registry independently observes runtime MCP capabilities and compares them with that approved baseline.

## Proposed GCP topology

```text
                    +---------------------------+
                    | Google Cloud              |
                    | Agent Registry            |
                    | Approved MCP inventory    |
                    +-------------+-------------+
                                  |
                                  | read-only API
                                  v
+-------------------+   +---------+----------+     +-------------------+
| Cloud Scheduler   +-->| GCP MCP Registry   |<--->| Secret Manager    |
| recurring scans   |   | Cloud Run / Jobs   |     | endpoint secrets  |
+-------------------+   +---------+----------+     +-------------------+
                                  |
                                  | MCP discovery
                                  v
                       +----------+-----------+
                       | Live MCP servers     |
                       | tools/resources/etc. |
                       +----------+-----------+
                                  |
                                  v
                       +----------+-----------+
                       | Drift + Policy       |
                       | Evaluation Engine    |
                       +----------+-----------+
                                  |
                   +--------------+--------------+
                   |                             |
                   v                             v
          +--------+---------+          +--------+---------+
          | Pub/Sub / Logs   |          | Security Command |
          | events & audit   |          | Center findings  |
          +------------------+          +------------------+
```

The specific Google Cloud services shown above are proposed integration targets and may evolve as the implementation matures.

## Major components

### 1. Registry connector

Responsibilities:

- authenticate to Google Cloud;
- read the approved MCP server inventory;
- retrieve approved endpoint and capability metadata;
- normalize registry resources into GCP MCP Registry's internal approved-state model.

The connector should be read-only for normal assurance scans.

The initial implementation may use local YAML/JSON approved-state files before the Google Cloud Agent Registry connector is available.

### 2. MCP runtime scanner

Responsibilities:

- connect to a configured MCP endpoint;
- perform supported MCP capability discovery;
- enumerate tools and relevant metadata;
- safely handle timeouts, errors, malformed responses, and unreachable servers;
- return normalized observed-state evidence.

Initial runtime discovery should prioritize `tools/list`.

Remote MCP responses must be treated as untrusted input.

The scanner should not invoke arbitrary business tools merely to determine whether they exist.

### 3. Normalization layer

Registry data and runtime data may represent the same capability differently. The normalization layer produces stable records suitable for deterministic comparison.

Candidate normalized attributes include:

- server identifier;
- endpoint identifier;
- tool name;
- tool description hash;
- input schema hash;
- output schema hash, where available;
- annotations or security metadata;
- scan timestamp;
- source/evidence identifier.

Normalization must be deterministic enough that semantically equivalent data does not generate unnecessary drift findings.

### 4. Drift engine

Initial drift categories may include:

- `UNAUTHORIZED_TOOL_ADDED`
- `APPROVED_TOOL_MISSING`
- `TOOL_SCHEMA_CHANGED`
- `TOOL_METADATA_CHANGED`
- `SERVER_UNREACHABLE`
- `SERVER_UNREGISTERED`

A scan may produce one overall state:

- `COMPLIANT`
- `DRIFTED`
- `UNREGISTERED`
- `UNREACHABLE`
- `ERROR`

The exact finding schema and status model may evolve before v1.0.

The drift engine should remain separate from vendor-specific registry and export integrations so that comparison behavior can be tested independently.

### 5. Policy engine

The initial project may use built-in policy checks. The architecture should allow a future external policy engine or policy-as-code implementation.

Example policy concepts include:

- destructive tools forbidden in production;
- endpoint must use TLS;
- server must have an approved owner;
- high-risk tool additions receive elevated severity;
- registry approvals older than a configured period require reassessment.

Policy evaluation should remain distinguishable from pure approved-versus-observed drift detection.

### 6. Finding exporters

Exporters should be independent of the detection engine.

Potential destinations include:

- Cloud Logging;
- Pub/Sub;
- Security Command Center;
- BigQuery for analysis;
- SIEM integrations.

An exporter failure should not silently change a finding from non-compliant to compliant.

## Data model

A minimal finding may contain:

```json
{
  "finding_id": "...",
  "server_id": "finance-mcp",
  "status": "DRIFTED",
  "category": "UNAUTHORIZED_TOOL_ADDED",
  "severity": "HIGH",
  "approved": {
    "tools": ["lookup_stock", "get_market_data"]
  },
  "observed": {
    "tools": ["lookup_stock", "get_market_data", "delete_account"]
  },
  "difference": {
    "added": ["delete_account"]
  },
  "observed_at": "2026-08-31T00:00:00Z"
}
```

Evidence should be sufficient to explain why the finding was produced without storing unnecessary sensitive data.

## Trust boundaries

GCP MCP Registry crosses several trust boundaries:

1. Google Cloud control-plane APIs to GCP MCP Registry.
2. GCP MCP Registry to remote MCP endpoints.
3. GCP MCP Registry to persistence and event systems.
4. Findings to downstream users and automation.

All remote MCP content is untrusted.

Registry metadata should also be validated before use rather than assumed to be safe merely because it comes from an approved source.

## Authentication and authorization

The intended GCP model is workload identity or service-account authentication with least-privilege IAM.

The scanner should not require broad administrative access to Google Cloud Agent Registry simply to read approved state.

MCP endpoint authentication is connector- or endpoint-specific and must never result in secret values being written to findings, logs, or error output.

## Failure behavior

Security-relevant failures should be visible.

Examples include:

- approved state cannot be loaded;
- endpoint authentication fails;
- endpoint is unreachable;
- runtime response is malformed;
- comparison cannot be completed;
- policy evaluation fails;
- findings cannot be exported.

A failed scan should not default to `COMPLIANT`.

Where practical, the implementation should distinguish between a security mismatch and an inability to validate runtime state.

## Non-goals for initial releases

The early project does not attempt to:

- become an enterprise identity provider;
- replace Google Cloud Agent Registry;
- proxy every MCP request;
- automatically block production traffic;
- execute arbitrary MCP tools to test them;
- infer that a tool is safe merely because it is registered;
- guarantee that an MCP server is secure;
- replace organizational risk review or secure software development practices.

## Future architecture

Potential future modules include:

- shadow MCP discovery;
- delegated user → agent → MCP authorization validation;
- continuous endpoint attestation;
- signed MCP capability manifests;
- CI/CD approval gates;
- OPA/Rego integration;
- Tetrate, Envoy, or other enforcement adapters;
- cross-cloud registry connectors;
- dashboard and investigation workflows;
- provenance and supply-chain evidence.

Future enforcement integrations should remain optional and separate from the core assurance engine unless the project scope changes formally.
