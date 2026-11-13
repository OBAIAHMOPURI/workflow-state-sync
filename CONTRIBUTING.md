# Contributing to Workflow State Sync

[Root README](README.md) · [Architecture](docs/ARCHITECTURE.md) · [Setup](docs/SETUP.md) · [API guide](docs/API.md) · [Network Ecosystem](README.md#network-ecosystem)

## Project status

This repository is a documentation-only reconciliation concept. No source/target adapter, webhook, schedule, credential configuration, checkpoint store, runtime, or test suite is implemented. Documentation does not enable or authorize writes to any external system.

## Contribution workflow

Read the guides and agree a narrow change and acceptance criteria with the owner. Start a local branch:

```sh
git clone https://github.com/OBAIAHMOPURI/workflow-state-sync.git
cd workflow-state-sync
git switch -c docs/describe-change
```

Use focused pull requests for normal review. This batch's explicit direct-to-main approval is not a standing merge policy. Do not invent install/run commands or assume any external app integration exists.

## Repository-specific priorities

- Identify exact source and target systems and the source of truth for each field, including permitted synchronization direction.
- Define canonical entity IDs, state transitions, versions, conflict ownership, deletion/tombstone semantics, and update-loop prevention.
- Specify conditional/idempotent target writes, durable checkpoints after verified persistence, bounded retries, rate-limit handling, and replay/recovery.
- Establish least-privilege connections and explicit approval boundaries before processing real records or performing side effects.

## Quality, tests, and review evidence

For documentation, run `git diff --check`, inspect rendering, and verify root README/architecture/setup/API links plus all seven siblings. Keep proposed design separate from current behavior.

Future code should include reproducible synthetic source/target fixtures and tests for duplicates, stale versions, concurrent edits, partial writes, target rate limits, expired cursors, tombstones, update loops, and recovery without duplicated side effects. Report exact commands/versions, outcomes, and checks not run; no existing automated test or CI success is implied. Pull requests should explain scope, field mappings, compatibility, permission boundaries, evidence, unresolved risks, and migration/replay/rollback behavior. Update setup and contract guides when implementation changes.

## Security and operational boundary

Never commit credentials, tokens, real synchronized records, private operational notes, or raw sensitive logs. Use sandbox systems and synthetic data. Report sensitive vulnerabilities through an owner-approved private channel. Enabling schedules/webhooks, writing to live targets, changing permissions, provisioning infrastructure, and deployment require separate authorization. This guide selects no license.

## Sibling repository network

Navigation only; no live synchronization or runtime integrations are claimed.

- [alert-notification-dispatcher](https://github.com/OBAIAHMOPURI/alert-notification-dispatcher/blob/main/README.md)
- [Brainwave_Matrix_Intern — Banky Application](https://github.com/OBAIAHMOPURI/Brainwave_Matrix_Intern/blob/main/README.md)
- [BrainWave_Matrix_Intern1 — Hospital Management System](https://github.com/OBAIAHMOPURI/BrainWave_Matrix_Intern1/blob/main/README.md)
- [edge-gateway-service](https://github.com/OBAIAHMOPURI/edge-gateway-service/blob/main/README.md)
- [edge-gateway-service1](https://github.com/OBAIAHMOPURI/edge-gateway-service1/blob/main/README.md)
- [identity-auth-proxy](https://github.com/OBAIAHMOPURI/identity-auth-proxy/blob/main/README.md)
- [telemetry-stream-worker](https://github.com/OBAIAHMOPURI/telemetry-stream-worker/blob/main/README.md)
