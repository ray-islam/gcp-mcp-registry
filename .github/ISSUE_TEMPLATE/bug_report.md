---
name: Bug report
about: Report reproducible incorrect behavior
title: "[Bug] "
labels: bug
assignees: ""
---

## Summary

Describe the problem clearly.

## Affected component

Select or describe the affected area:

- approved-state / registry connector
- MCP runtime scanner
- normalization
- drift engine
- policy evaluation
- findings / exporters
- GCP deployment / Terraform
- documentation
- other

## Steps to reproduce

1.
2.
3.

## Expected behavior

What should have happened?

If this affects drift detection or assurance, include the expected status or finding when known.

## Observed behavior

What happened instead?

## Approved versus observed state

If relevant, provide **sanitized** approved and runtime examples.

Approved state:

```text
...
```

Observed runtime state:

```text
...
```

Do not include credentials, private endpoints, proprietary metadata, or production data.

## Environment

- GCP MCP Registry version/commit:
- Python/runtime version:
- Operating system:
- Deployment: local / Cloud Run / Cloud Run Jobs / other
- MCP transport/server type:
- Registry source: local file / Google Cloud Agent Registry / other
- Relevant GCP services:

## Logs or evidence

Provide sanitized logs, findings, screenshots, or output if useful.

**Do not include credentials, access tokens, service-account keys, internal endpoints, customer data, or proprietary information.**

## Regression?

Did this work correctly in an earlier version or commit?

- [ ] Yes
- [ ] No
- [ ] Unknown

If yes, identify the last known working version or commit when possible.

## Security impact

Could this bug:

- incorrectly report `COMPLIANT`;
- expose secrets or credentials;
- require excessive permissions;
- bypass policy evaluation;
- scan or contact an unintended endpoint;
- corrupt or suppress findings;
- create another security-relevant failure?

If this appears to be a vulnerability, **stop and follow `SECURITY.md` instead of filing a public issue**.
