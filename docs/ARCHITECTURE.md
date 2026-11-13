# Workflow State Sync — Architecture

[README](../README.md) · [Setup](SETUP.md) · [Network Ecosystem](../README.md#network-ecosystem)

## Evidence and design status

Audit date: 13 November 2026; branch: `main`. Only a title-only README existed; no application source, build manifest, tests, or deployment configuration was present. No implementation, build manifest, tests, or runtime integrations were found. All components and contracts below are **proposed design notes for owner review**, not verified runtime behavior.

## Intended responsibility and boundaries

A candidate reconciliation service for keeping explicitly approved workflow state consistent across systems. There are no source adapters, credentials, scheduled jobs, webhooks, or active synchronizations in the repository.

This repository must not acquire authority to send messages, update third-party systems, expose production endpoints, or process sensitive data merely because those operations appear in a design. Confirm scope and approval gates before implementation.

## Candidate data flow

```text
Source change/snapshot -> canonical state -> conflict decision -> idempotent target update -> durable checkpoint
```

| Proposed component | Responsibility |
| --- | --- |
| Source adapter | Read changes with a stable cursor or version, using the narrowest necessary access. |
| Canonical model | Define entity keys, state transitions, version fields, and deletion semantics before mapping systems. |
| Reconciler | Compare versions, prevent update loops, and escalate conflicts instead of silently overwriting them. |
| Target/checkpoint | Use conditional/idempotent writes and advance a cursor only after verified target persistence. |

## Contract-first implementation

Specify entity IDs, state/version schema, allowed transitions, idempotency keys, conflict responses, and tombstone behavior. Documentation is not authorization to write to external systems.

Record payload limits, validation rules, identity/trust boundaries, correlation identifiers, timeout budgets, supported errors, and compatibility/versioning rules. Choose actual transports and persistence only after those contracts are reviewed. No endpoint, broker, storage engine, or deployment topology is prescribed by the current repository.

## Reliability and observability requirements

Classify permanent and transient errors. Bound retries with backoff/jitter and an overall deadline; do not automatically repeat a side effect without an idempotency strategy. Use explicit recovery/checkpoint behavior where persistence is involved. Track throughput, error/latency distributions, and queue depth or outstanding work where relevant. Diagnostics should use stable event/request identifiers rather than credentials or raw sensitive payloads.

## Security and data handling requirements

Validate all external input. Grant only necessary permissions and keep secrets in a managed secret store or a reviewed local development mechanism. Define log/retention/redaction policy before collecting real data. Default to sandbox destinations; do not infer deployment or sending authorization from this document.

## Proposed verification

Test duplicate events, stale versions, concurrent edits, partial writes, expired cursors, deletion propagation, loop prevention, target rate limits, and recovery without duplicate side effects.

No such test suite exists yet and no test results are claimed.

## Open architecture decisions

- Name the systems to synchronize, a source of truth for each field, and the permitted direction of updates.
- Define conflict handling, tombstones, replay windows, and manual approval boundaries.
- Choose transport, scheduler/trigger, state store, recovery strategy, and least-privilege connection management.

## Ecosystem boundary

The [sibling repository index](../README.md#network-ecosystem) is documentation navigation only. Integration with a gateway, identity service, telemetry worker, workflow synchronizer, notification dispatcher, or either Java app requires a separately implemented and tested contract. The two gateway names do not establish a canonical/secondary or versioned relationship.
