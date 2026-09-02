# GCP MCP Registry

**GCP-native, open-source continuous assurance for Model Context Protocol (MCP) infrastructure.**

> GCP MCP Registry compares **approved MCP capabilities** with **live runtime capabilities** to detect capability drift, unmanaged servers, and policy violations.

> [!IMPORTANT]
> GCP MCP Registry is an **early-stage community project**. APIs, configuration formats, and deployment patterns may change before v1.0. It is not yet intended to be used as a sole production security control.

## Why GCP MCP Registry?

MCP registries can tell an organization what servers and tools are **registered or approved**. Security teams also need to know whether the live MCP environment still matches that approved state.

GCP MCP Registry is designed to answer questions such as:

- Does the live MCP server expose only the tools that were approved?
- Has an MCP server added, removed, or changed capabilities since approval?
- Is an MCP server running without a corresponding registry record?
- Does the live configuration violate organizational policy?
- Can security teams produce evidence showing approved state versus observed runtime state?

## Project goal

GCP MCP Registry is intended to complement, not replace, a registry such as **Google Cloud Agent Registry**.

```text
Google Cloud Agent Registry / Approved Baseline
                    |
                    v
             GCP MCP Registry
        +-----------+-----------+
        |           |           |
        v           v           v
  MCP Discovery   Drift      Policy
    Scanner       Engine     Evaluation
        |           |           |
        +-----------+-----------+
                    |
                    v
              Security Result
      COMPLIANT / DRIFTED / UNREGISTERED
                    |
                    v
       Logging / Pub/Sub / SCC / SIEM
```

## Current scope

### v0.1 — Foundation

- [x] Define the approved-versus-runtime assurance model
- [x] Define MCP capability-drift concepts
- [x] Establish project structure and contribution model
- [ ] Local approved-state configuration
- [ ] MCP `tools/list` runtime discovery
- [ ] Tool-level drift comparison
- [ ] CLI and JSON scan output
- [ ] Automated tests

### Planned

- Google Cloud Agent Registry connector
- Google Cloud IAM authentication
- Cloud Run deployment
- Cloud Scheduler recurring scans
- Pub/Sub findings and alerts
- Security Command Center integration
- Policy-as-code
- Shadow/unmanaged MCP discovery
- Web dashboard
- Additional cloud and registry connectors

See [docs/ROADMAP.md](docs/ROADMAP.md) for the detailed roadmap.

## Example

Approved registry state:

```yaml
server: finance-mcp
approved_tools:
  - lookup_stock
  - get_market_data
```

Observed runtime state:

```yaml
server: finance-mcp
runtime_tools:
  - lookup_stock
  - get_market_data
  - delete_account
```

GCP MCP Registry result:

```text
Server: finance-mcp
Status: DRIFTED

Unauthorized runtime capabilities:
  + delete_account

Approved tools: 2
Runtime tools:  3
```

## Proposed GCP architecture

GCP MCP Registry is **GCP-first** while keeping its core assurance engine modular.

| Capability | Proposed GCP service |
|---|---|
| Approved MCP inventory | Google Cloud Agent Registry |
| GCP MCP Registry API | Cloud Run |
| Scan execution | Cloud Run Jobs |
| Authentication and authorization | IAM |
| Secrets | Secret Manager |
| Findings/events | Pub/Sub |
| Scheduled assurance scans | Cloud Scheduler |
| Operational logging | Cloud Logging |
| Security findings | Security Command Center |
| Optional persistence | Cloud SQL for PostgreSQL |
| Infrastructure as code | Terraform |

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Repository layout

```text
gcp-mcp-registry/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── SECURITY.md
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── ROADMAP.md
│   └── SECURITY_MODEL.md
├── examples/
│   └── README.md
├── server/
│   └── README.md
├── deploy/
│   └── gcp/
│       └── README.md
├── tests/
│   └── README.md
└── .github/
    ├── ISSUE_TEMPLATE/
    │   ├── bug_report.md
    │   ├── feature_request.md
    │   └── good_first_issue.md
    └── pull_request_template.md
```

## Implementation status

The repository currently defines the project model, architecture, security expectations, roadmap, and contribution structure. The v0.1 scanner, registry connector, drift engine, CLI, deployment modules, and automated tests are still planned or under development unless explicitly marked complete in the roadmap.

This distinction is intentional: the documentation describes the **target design and implementation direction**, while the checklist above identifies what is available today.

## Getting started

The project is currently establishing the v0.1 implementation. After this repository is published, use GitHub's **Code** menu to copy its HTTPS or SSH clone URL.

As the scanner implementation lands, this section will include the exact local installation and scan commands.

For now, contributors can review the architecture, roadmap, and open issues and begin with items marked `good first issue`.

## Principles

GCP MCP Registry is being designed around several principles:

1. **Registry is not runtime truth.** Approved state must be independently compared with observed state.
2. **Read-only by default.** Discovery and assurance should not require destructive permissions.
3. **Least privilege.** GCP integrations should request the minimum IAM roles necessary.
4. **Evidence over assumptions.** Findings should include the approved state, observed state, comparison, and timestamp.
5. **Open interfaces.** Connectors and policy engines should be replaceable and extensible.
6. **Cloud-native, not cloud-locked.** GCP is the primary integration target, while the core engine should remain modular.

## What GCP MCP Registry is not

GCP MCP Registry is not intended to be:

- a replacement for Google Cloud Agent Registry;
- an identity provider;
- a general-purpose SIEM;
- a substitute for MCP server authentication or authorization;
- a guarantee that an MCP server or tool is secure;
- a replacement for organizational risk review or secure software development practices.

## Contributing

Contributions are welcome. Good contributions include:

- MCP discovery/scanner improvements;
- Google Cloud Agent Registry integration;
- IAM and authentication improvements;
- new drift-detection rules;
- policy-as-code integrations;
- GCP deployment modules;
- tests and documentation;
- reproducible MCP security test cases.

Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## Security

Please **do not open a public GitHub issue for a suspected vulnerability**. Follow the process in [SECURITY.md](SECURITY.md).

## License

Licensed under the [Apache License 2.0](LICENSE).

## Project status

**Experimental / pre v1.0.** The repository is intended for research, development, testing, and community collaboration while the architecture and implementation mature.

## Trademark and affiliation notice

Disclosure: Google Cloud, GCP, and related product names are trademarks of Google LLC. GCP MCP Registry is an independent open-source project and is **not affiliated with, sponsored by, or endorsed by Google LLC**.
