# Server

This directory is reserved for the **GCP MCP Registry** runtime implementation.

The project is currently pre-v1.0. The structure below represents the intended modular design and should not be interpreted as confirmation that every module is already implemented.

## Planned modules

```text
server/
├── api/          # HTTP/API surface
├── connectors/   # Google Cloud Agent Registry and future registry connectors
├── scanner/      # MCP runtime capability discovery
├── normalize/    # Normalized approved/observed models
├── drift/        # Approved-versus-runtime state comparison
├── policy/       # Security and governance policy evaluation
└── exporters/    # Logging, Pub/Sub, SCC, SIEM, and related outputs
```

## Initial implementation priority

The first implementation should remain intentionally small and testable.

Recommended order:

1. Define normalized models for approved and observed MCP state.
2. Implement local approved-state input.
3. Implement MCP runtime discovery, beginning with `tools/list`.
4. Build deterministic drift comparison.
5. Produce clear CLI/JSON findings.
6. Add unit tests and synthetic fixtures.
7. Add the Google Cloud Agent Registry connector.
8. Add policy evaluation and exporter integrations.

The initial implementation should prioritize the **drift engine and runtime observation path** before dashboards or broad enterprise integrations.

## Design expectations

Server components should follow the project principles defined in the root documentation:

- use read-only discovery by default;
- request least-privilege permissions;
- keep approved state separate from observed runtime state;
- treat MCP metadata and remote responses as untrusted input;
- avoid logging secrets, credentials, or bearer tokens;
- produce evidence that explains why a finding was generated;
- keep vendor-specific connectors modular;
- keep the core assurance logic portable where practical.

## Connector boundaries

Registry connectors should retrieve or normalize the **approved baseline**.

Runtime scanners should independently observe the **live MCP state**.

These two sources should remain logically separate so that the project can detect situations where:

- an approved server exposes an unauthorized tool;
- a tool's schema changes after approval;
- an approved capability disappears;
- a runtime MCP server has no corresponding approved registry record;
- an approved server cannot be reached or validated.

A registry record must not automatically be treated as proof of the current runtime state.

## Findings

The runtime implementation should eventually produce structured findings such as:

```text
COMPLIANT
DRIFTED
UNREGISTERED
ERROR
```

Each finding should include enough context to identify:

- the MCP server;
- the approved state;
- the observed state;
- the detected difference;
- the evaluation timestamp;
- the relevant policy or comparison rule when applicable.

The exact internal result schema may evolve before v1.0.

## Security

Do not place credentials, service-account keys, tokens, proprietary endpoints, or production data in this directory.

Security-sensitive implementation changes should follow the repository's [SECURITY.md](../SECURITY.md), [security model](../docs/SECURITY_MODEL.md), and [contribution guidelines](../CONTRIBUTING.md).
