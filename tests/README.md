# Tests

GCP MCP Registry should be tested at multiple levels as the implementation develops.

The project is currently pre-v1.0, so this document describes the intended testing strategy. Individual test suites should only be marked as implemented when the corresponding code and automated checks exist.

## Unit tests

Unit tests should cover deterministic behavior in the assurance engine, including:

- normalized tool and capability comparison;
- added and removed capability detection;
- tool schema drift;
- severity mapping;
- invalid or malformed MCP responses;
- secret-redaction behavior;
- configuration validation;
- policy-evaluation logic;
- handling of duplicate or unexpected capability metadata.

## Integration tests

Integration tests should validate interactions between GCP MCP Registry and external or simulated components, including:

- MCP endpoint discovery;
- MCP `tools/list` runtime discovery;
- Google Cloud Agent Registry connector behavior using a controlled test project, fixture, or mock;
- authentication and authorization failure handling;
- unreachable endpoint behavior;
- timeout and retry behavior;
- registry-to-runtime reconciliation;
- findings and event-output integrations when those components are implemented.

Tests that require live GCP resources should be clearly separated from local tests and should use least-privilege test identities.

## Security tests

Security-focused tests should cover cases such as:

- oversized metadata responses;
- malformed schemas;
- malicious or adversarial strings in MCP metadata;
- SSRF-relevant endpoint validation;
- unsafe or unexpected endpoint schemes;
- secrets not appearing in logs, findings, exceptions, or test output;
- authorization failures being handled safely;
- untrusted metadata not being interpreted as executable instructions;
- policy-bypass attempts;
- malformed responses that could cause the scanner to incorrectly report a compliant result.

## Drift-detection fixtures

Synthetic fixtures should represent both approved and observed runtime states.

Useful cases include:

- approved tools exactly match runtime tools;
- an unauthorized runtime tool is added;
- an approved tool disappears;
- a tool name remains the same but its schema changes;
- an MCP server exists at runtime without an approved registry record;
- an approved registry record exists but the endpoint is unreachable;
- multiple simultaneous drift conditions occur.

Each fixture should make the expected result explicit, such as:

```text
COMPLIANT
DRIFTED
UNREGISTERED
ERROR
```

The exact result model may evolve before v1.0 and should remain aligned with the implementation and architecture documentation.

## Test data

All test fixtures should use **synthetic data**.

Do not include:

- real customer or production data;
- credentials or access tokens;
- private keys;
- proprietary organizational information;
- internal-only endpoints;
- secrets copied from development or production environments.

## Test reliability

Tests should be:

- deterministic where practical;
- independent of test execution order;
- explicit about required external dependencies;
- isolated from production resources;
- safe to run repeatedly;
- clear about expected security outcomes.

When a test depends on a live cloud service or external MCP endpoint, document that dependency and provide a mock or local alternative when practical.

## Running tests

Exact test commands will be added when the v0.1 implementation and automated test tooling are available.

Until then, contributors should not add placeholder commands or claim test coverage that has not been implemented.

When the test runner is introduced, this section should document:

1. local unit-test commands;
2. integration-test commands;
3. any required environment variables;
4. GCP test-project requirements, if applicable;
5. expected coverage or quality checks;
6. CI validation performed for pull requests.
