# Examples

This directory will contain reproducible example environments for **GCP MCP Registry**.

The project is currently pre-v1.0, so these examples describe the intended validation scenarios. Example directories should only be marked as implemented when the corresponding fixtures and runnable code exist.

## Planned examples

### `safe-server/`

An MCP server whose observed runtime capabilities exactly match the approved baseline.

Expected result:

```text
COMPLIANT
```

This example should demonstrate the normal case where:

- the server is present in the approved baseline;
- the runtime server is reachable;
- approved tools match observed tools;
- no unauthorized capability drift is detected.

### `drifted-server/`

An MCP server that exposes one additional unauthorized tool at runtime.

Expected result:

```text
DRIFTED
+ unauthorized_tool
```

This example should demonstrate that the registry record is not treated as runtime truth. The scanner should independently observe the live MCP server and detect the additional capability.

Additional drift fixtures may later cover:

- an approved tool disappearing;
- a tool schema changing after approval;
- multiple unauthorized capabilities appearing;
- approved and observed metadata differing.

### `unregistered-server/`

A live MCP server that can be discovered or scanned but does not have a corresponding approved registry record.

Expected result:

```text
UNREGISTERED
```

This scenario is intended to model shadow or unmanaged MCP infrastructure.

### `unreachable-server/`

A configured or approved MCP endpoint that cannot be reached within the scanner timeout.

Expected result:

```text
ERROR
```

The finding should make clear that the runtime state could not be validated. An unreachable server should not automatically be reported as compliant.

If the implementation later introduces a more specific `UNREACHABLE` status, this example should be updated to match the canonical result model.

### `schema-drift-server/`

An MCP server whose tool name remains approved but whose observed tool schema differs from the approved baseline.

Expected result:

```text
DRIFTED
```

This example should help validate that GCP MCP Registry can detect capability changes that are more subtle than simply adding or removing a tool.

## Example structure

A completed example should ideally contain:

```text
example-name/
├── README.md
├── approved-state.yaml
├── server/               # Synthetic MCP server or fixture
└── expected-result.json
```

The exact structure may evolve as the v0.1 implementation is developed.

Each example README should explain:

1. the approved baseline;
2. the observed runtime state;
3. the expected finding;
4. how to run the example when runnable tooling exists;
5. any security behavior the example is intended to demonstrate.

## Safety and test data

Example environments must use **synthetic data**.

Never include:

- real company credentials;
- access tokens or private keys;
- production or internal endpoints;
- proprietary policies;
- customer or production records;
- confidential organizational information.

Examples should be safe to run in a local or controlled test environment and should not require destructive permissions.
