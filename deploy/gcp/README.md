# GCP Deployment

This directory will contain the GCP-native deployment assets for **GCP MCP Registry**.

The project is currently pre-v1.0, so the services, Terraform files, and deployment patterns described here represent the intended deployment model. They should not be treated as production-ready until the corresponding implementation, tests, and security guidance are available.

## Planned services

The initial GCP deployment may use:

- Cloud Run
- Cloud Run Jobs
- IAM service accounts and bindings
- Secret Manager
- Cloud Scheduler
- Pub/Sub
- Cloud Logging
- optional Cloud SQL
- optional Security Command Center integration

The exact set of services may evolve as the implementation matures.

## Intended deployment model

A typical deployment is expected to separate the following responsibilities:

1. **Approved-state access** — read approved MCP metadata from Google Cloud Agent Registry or another approved-state source.
2. **Runtime scanning** — connect to configured MCP endpoints and observe supported runtime capabilities.
3. **Comparison and policy evaluation** — compare approved and observed state and generate findings.
4. **Finding export** — send structured results to logging, Pub/Sub, Security Command Center, or other approved destinations.
5. **Scheduled execution** — use Cloud Scheduler and Cloud Run Jobs for recurring assurance scans where appropriate.

Normal assurance scans should remain read-only wherever possible.

## Infrastructure as code

Terraform is the preferred initial deployment mechanism.

Planned layout:

```text
deploy/gcp/
├── main.tf
├── variables.tf
├── outputs.tf
├── iam.tf
├── cloud_run.tf
├── scheduler.tf
├── pubsub.tf
└── README.md
```

Additional files may later be added for Secret Manager, Security Command Center, networking, monitoring, or other supported integrations.

## IAM requirements

Deployments should follow least-privilege principles.

Recommended expectations:

- use dedicated service accounts for GCP MCP Registry components;
- separate deployment permissions from runtime scanning permissions where practical;
- grant only the registry-read permissions required by the approved-state connector;
- avoid broad project-level roles such as Owner or Editor;
- grant Secret Manager access only to the runtime identity that requires a specific secret;
- restrict Pub/Sub, Logging, SCC, and other exporter permissions to the required actions;
- review IAM bindings as new connectors or exporters are introduced.

The exact minimum IAM roles should be documented once the corresponding GCP integrations are implemented and tested.

## Secrets and credentials

Do not store credentials directly in:

- Terraform source files;
- committed `.tfvars` files;
- container images;
- repository configuration;
- shell scripts checked into source control.

Prefer:

- workload identity or attached service-account identity for Google Cloud access;
- Secret Manager for endpoint credentials or other required secrets;
- short-lived credentials where supported.

Terraform state may contain sensitive values depending on the resources being managed. State storage should therefore be protected with appropriate access controls and should not be committed to the repository.

## Network security

Deployment defaults should minimize unnecessary exposure.

Recommended expectations:

- keep public ingress disabled unless a documented use case requires it;
- restrict Cloud Run ingress to the narrowest practical setting;
- use authenticated invocation;
- validate outbound MCP endpoints;
- consider VPC connectivity, private endpoints, egress controls, or firewall policies where required by the deployment environment;
- avoid allowing unrestricted internal-network scanning by default;
- use configured network timeouts and bounded retry behavior.

A runtime scanner should only be able to reach the endpoints it is expected to validate, where practical.

## Cloud Run and Cloud Run Jobs

Cloud Run may host an API or coordination surface if required.

Cloud Run Jobs are the preferred model for scheduled or batch assurance scans.

Deployment configuration should eventually define:

- runtime service account;
- CPU and memory limits;
- execution timeout;
- concurrency where applicable;
- environment variables and secret references;
- network configuration;
- retry behavior;
- logging configuration.

Resource settings should prevent malformed or hostile MCP responses from consuming unbounded compute or network resources.

## Cloud Scheduler

Cloud Scheduler may trigger recurring assurance scans.

Scheduled execution should:

- use authenticated invocation;
- target only the intended Cloud Run service or job;
- use a dedicated identity where appropriate;
- avoid embedding secrets in scheduler payloads;
- produce traceable execution identifiers where practical.

## Findings and logging

Security findings should be structured and evidence-backed.

Potential destinations include:

- Cloud Logging;
- Pub/Sub;
- Security Command Center;
- BigQuery;
- external SIEM platforms.

Logs and findings must not contain:

- bearer tokens;
- service-account keys;
- endpoint passwords;
- private keys;
- sensitive request headers;
- unnecessary production payload data.

Exporter failures should be visible and should not cause an unsuccessful scan to be reported as compliant.

## Terraform state and backend

Local Terraform state should be used only for isolated development when appropriate.

For shared environments, a protected remote backend should be used.

State storage should have:

- restricted IAM access;
- versioning or equivalent recovery capability where practical;
- encryption provided by the storage platform;
- no public access;
- separation between development and production environments.

Never commit `.tfstate`, `.tfstate.*`, or secret-bearing variable files to Git.

## Environment separation

Development, test, and production environments should use separate configuration and identities where practical.

Avoid reusing:

- production service accounts in development;
- production secrets in test environments;
- production MCP endpoints in examples or automated tests.

Synthetic or controlled resources should be used for contributor and integration testing.

## Security requirements

At minimum:

- use a dedicated runtime service account;
- grant only the minimum registry-read permissions required by the scanner;
- keep normal discovery read-only;
- use Secret Manager and workload identity where possible;
- protect Terraform state;
- keep public ingress disabled unless explicitly required;
- authenticate service and job invocation;
- restrict access to exported findings;
- ensure scan failures cannot silently become `COMPLIANT`;
- avoid storing secrets in logs, findings, environment files, or source control.

## Deployment status

No deployment instructions in this directory should be considered complete until the corresponding Terraform and runtime components are implemented.

When the initial deployment is available, this README should be expanded with:

1. prerequisites;
2. required APIs;
3. minimum IAM permissions;
4. Terraform initialization and deployment steps;
5. required variables;
6. Secret Manager setup;
7. Cloud Run/Cloud Run Jobs configuration;
8. validation steps;
9. teardown instructions;
10. security and cost considerations.
