---
name: Feature request
about: Propose a new capability or improvement
title: "[Feature] "
labels: enhancement
assignees: ""
---

## Problem

What security, MCP, governance, developer, or operational problem should this solve?

## Proposed capability

Describe the requested behavior.

## Why this belongs in GCP MCP Registry

Explain how the proposal supports one or more project goals, such as:

- approved-versus-runtime assurance;
- capability-drift detection;
- unmanaged MCP discovery;
- policy evaluation;
- evidence and auditability;
- Google Cloud integration;
- security operations;
- connector interoperability.

## Example

Show a sanitized example input/output, finding, workflow, or user experience if possible.

```text
...
```

## GCP / MCP components involved

For example:

- Google Cloud Agent Registry
- IAM
- Cloud Run / Cloud Run Jobs
- Secret Manager
- Cloud Scheduler
- Pub/Sub
- Security Command Center
- MCP tools/resources/prompts
- runtime discovery
- policy engine
- exporter or SIEM integration

## Security considerations

What permissions, trust boundaries, network access, failure behavior, or abuse cases should be considered?

Consider questions such as:

- Does this require new IAM permissions?
- Does it introduce write or enforcement capability?
- Does it consume untrusted MCP data?
- Could failure create false compliance?
- Could it expose sensitive evidence or credentials?
- Does it affect approved-versus-observed state separation?

## Implementation scope

Is this proposal:

- [ ] Small / contributor-friendly
- [ ] Medium
- [ ] Large architectural change
- [ ] Research / design exploration

## Alternatives

Are there other ways to solve the problem?

## Additional context

Add links, diagrams, references, or other useful context.

Do not include proprietary company information, internal endpoints, credentials, or sensitive data.
