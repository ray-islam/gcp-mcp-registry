# Security Policy

Security reports are taken seriously.

## Supported versions

GCP MCP Registry is currently pre-v1.0. Until stable releases are published, security fixes will generally target the latest version on the `main` branch.

| Version | Supported |
|---|---|
| `main` / latest pre-release | Yes |
| Older development snapshots | Best effort |

## Reporting a vulnerability

**Do not disclose suspected vulnerabilities in a public GitHub issue, discussion, pull request, or social-media post.**

Preferred reporting method:

1. Use **GitHub Private Vulnerability Reporting** for this repository once it is enabled under **Settings → Security → Private vulnerability reporting**.
2. If Private Vulnerability Reporting is not yet available, do not publish vulnerability details in a public issue or discussion. Use a private maintainer contact if one is listed by the repository.
3. Include enough information for maintainers to reproduce and assess the issue.

A useful report includes:

- affected component and version/commit;
- vulnerability type;
- reproduction steps or proof of concept;
- expected versus observed behavior;
- potential security impact;
- suggested remediation, if known.

Do not include real customer data, production credentials, access tokens, private keys, or confidential organizational information in the report.

## Scope examples

Security issues may include:

- authentication or authorization bypass;
- excessive GCP IAM permissions;
- credential or token exposure;
- command/code injection;
- server-side request forgery;
- unsafe MCP endpoint handling;
- untrusted tool metadata causing code execution;
- findings that expose secret values;
- insecure default network configuration;
- policy-bypass conditions that incorrectly mark drift as compliant.

## Security design expectations

GCP MCP Registry should:

- prefer read-only access to registries and MCP servers;
- use least-privilege IAM roles;
- avoid logging secrets or bearer tokens;
- treat MCP metadata and remote responses as untrusted input;
- validate endpoints and configuration;
- make enforcement integrations opt-in;
- preserve evidence needed to understand why a finding was produced.

## Disclosure

Please allow maintainers a reasonable opportunity to investigate and remediate a confirmed vulnerability before public disclosure.

Because this is an open-source community project, no specific response or remediation time is guaranteed.
