# Contributing to GCP MCP Registry

Thank you for helping improve **GCP MCP Registry**.

GCP MCP Registry is an open-source, pre-v1.0 project focused on MCP governance, runtime assurance, capability-drift detection, and integration with Google Cloud and related enforcement systems.

Contributions are welcome in areas including bug fixes, documentation, tests, Google Cloud integrations, MCP interoperability, security controls, runtime assurance, and deployment automation.

## Before you contribute

Please:

1. Read the [README](README.md), [architecture](docs/ARCHITECTURE.md), and [security model](docs/SECURITY_MODEL.md).
2. Search existing issues before creating a new one.
3. For significant architectural changes, open an issue first so the design can be discussed before implementation.
4. Never include credentials, private keys, tokens, proprietary company information, internal endpoints, production data, or other sensitive material in an issue, commit, discussion, or pull request.
5. Keep in mind that the project is still evolving before v1.0, so interfaces and implementation details may change.

## Ways to contribute

You can contribute by:

- fixing bugs;
- improving documentation;
- creating MCP test fixtures;
- expanding runtime capability discovery and comparison;
- improving approved-registry-to-runtime reconciliation;
- developing the Google Cloud Agent Registry connector;
- improving Cloud Run and Terraform deployment support;
- adding security and policy checks;
- improving detection of unregistered or unmanaged MCP servers;
- adding evidence and auditability features;
- writing tests;
- reviewing pull requests;
- proposing interoperable connector and enforcement interfaces.

## Development principles

Changes should aim to preserve these properties:

- **Read-only discovery by default**
- **Least-privilege cloud permissions**
- **Deterministic findings where practical**
- **Clear evidence for every drift or policy finding**
- **No secret values in logs**
- **Modular connectors rather than hard-coded vendor behavior**
- **Cloud-agnostic core design where practical**
- **Explicit separation between approved configuration and observed runtime state**
- **Backward-compatible configuration where practical before v1.0**
- **Security controls that fail safely and are auditable**

## Suggested workflow

1. Fork the repository.
2. Create a branch from `main`.
3. Make a focused change.
4. Add or update tests where applicable.
5. Update documentation when behavior, configuration, architecture, or security assumptions change.
6. Run the applicable project tests and validation checks that are available for the component you changed.
7. Open a pull request using the repository pull-request template.

Because GCP MCP Registry is still under active development, some components may not yet have a complete implementation or automated test suite. Do not add placeholder commands or claim validation that was not actually performed.

Example branch names:

```text
feature/gcp-agent-registry-connector
feature/runtime-capability-discovery
fix/tool-drift-comparison
docs/security-model
```

## Pull-request expectations

A good pull request should:

- solve one clearly described problem;
- explain why the change is needed;
- describe the affected component or control;
- include tests for new behavior where feasible;
- explain how the change was validated;
- avoid unrelated formatting or refactoring;
- document relevant security implications;
- identify changes to permissions, trust boundaries, or external integrations;
- avoid breaking existing interfaces without prior discussion.

For security or governance features, include enough evidence in the pull request for reviewers to understand what is being detected, compared, allowed, denied, or reported.

## Commit messages

Use clear, imperative commit messages where possible.

Examples:

```text
Add Agent Registry MCP server listing
Detect unauthorized runtime tools
Add runtime capability reconciliation
Document Cloud Run IAM requirements
```

## Architecture and connector changes

Changes to registry connectors, runtime discovery, enforcement adapters, identity handling, or policy evaluation may affect project trust boundaries.

When proposing these changes:

1. describe the source of approved configuration;
2. describe the runtime source being observed;
3. explain how identities and permissions are handled;
4. document required cloud or network permissions;
5. identify failure behavior and security implications;
6. update architecture or security documentation when necessary.

Vendor-specific integrations should remain modular so the core project does not depend unnecessarily on a single registry, cloud platform, service mesh, or policy engine.

## Security-sensitive changes

Changes involving authentication, authorization, IAM, credentials, token handling, network access, policy enforcement, runtime discovery, audit evidence, or security findings deserve additional review.

Security-sensitive code should:

- request only the permissions it needs;
- avoid logging credentials, tokens, secrets, or sensitive payloads;
- make security-relevant failures visible;
- preserve sufficient evidence for troubleshooting and review;
- avoid silently weakening an existing control.

If you discover a vulnerability rather than a normal bug, **do not file a public issue**. See [SECURITY.md](SECURITY.md) for the project's vulnerability-reporting process.

## Documentation changes

Documentation is treated as part of the project.

When changing behavior, architecture, configuration, deployment expectations, security assumptions, or supported integrations, update the relevant documentation in the same pull request when practical.

Use the project name **GCP MCP Registry** consistently unless the repository maintainers formally rename the project.

## Licensing of contributions

By submitting a contribution, you agree that your contribution will be licensed under the repository's **Apache License 2.0**.

Do not submit code, documentation, test data, or other material that you do not have the right to contribute.

## Code of conduct

Participation in this project is governed by [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).
