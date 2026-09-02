## What does this change do?

Describe the change, the problem it solves, and the affected GCP MCP Registry component.

## Related issue

Closes #

## Type of change

- [ ] Bug fix
- [ ] Feature
- [ ] Documentation
- [ ] Refactor
- [ ] Tests
- [ ] GCP/deployment
- [ ] Security improvement
- [ ] Connector/integration
- [ ] Policy or drift-detection change

## Affected area

Select all that apply:

- [ ] Approved-state / registry connector
- [ ] MCP runtime scanner
- [ ] Normalization
- [ ] Drift engine
- [ ] Policy evaluation
- [ ] Findings / exporters
- [ ] GCP deployment / Terraform
- [ ] Documentation only
- [ ] Other

## Testing and validation

Describe how you validated the change.

Include, where applicable:

- unit or integration tests;
- synthetic fixtures used;
- expected versus observed results;
- manual validation steps;
- commands run;
- behavior not tested and why.

Do not claim tests passed if the corresponding test suite does not yet exist.

## Security review

- [ ] This change does not add secrets, credentials, production data, or other sensitive material.
- [ ] New IAM permissions, if any, are documented and least-privilege.
- [ ] Remote MCP data is treated as untrusted input.
- [ ] Logging, findings, and exceptions do not expose credentials or tokens.
- [ ] Failed validation cannot silently become `COMPLIANT`.
- [ ] Approved state remains logically separate from observed runtime state.
- [ ] New network access or external endpoints are documented.
- [ ] Security implications are documented below.

### Security notes

Describe relevant trust boundaries, permissions, failure behavior, abuse cases, or security assumptions.

Write `None` if the change has no meaningful security impact.

## Documentation impact

- [ ] No documentation change is required.
- [ ] README updated.
- [ ] Architecture updated.
- [ ] Security model updated.
- [ ] Roadmap updated.
- [ ] Deployment documentation updated.
- [ ] Example/test documentation updated.

## Checklist

- [ ] The change is focused and does not include unrelated modifications.
- [ ] I have added or updated tests where feasible.
- [ ] I have documented how the change was validated.
- [ ] Documentation is updated where needed.
- [ ] I have not committed secrets, credentials, internal endpoints, or proprietary data.
- [ ] I have read `CONTRIBUTING.md`.
- [ ] I understand that GCP MCP Registry is pre-v1.0 and interfaces may still evolve.
