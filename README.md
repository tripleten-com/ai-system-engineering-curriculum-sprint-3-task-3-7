# Coldline Task 3.7 — Project Defense

This checkpoint is the complete, settled Coldline platform. Everything Tasks 1 through 6 assessed
now ships supplied and correct: the release manifest and health gate from Task 3.1, the bounded
provider from Task 3.2, the SQS transport and dead-letter policy from Task 3.3, the alert from Task
3.4, the wired CI gate from Task 3.5, and the recovery runbook and extended `JobQueue` fidelity
record from Task 3.6. This Task adds no code. What you do instead is assemble the pull request
record you built across Tasks 1 through 6 into one evidence index in this Task's pull request
description, open a pull request that touches only `submission.yaml`, and then deliver a live
Project Defense of at most 10 minutes to your instructor, from the repositories, the check runs,
and the runbook on screen.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/tripleten-com/ai-system-engineering-curriculum-sprint-3-task-3-7/tree/main)

## Start the system

Prerequisites are Python 3.12 and Docker with Compose v2. The supplied bootstrap supports macOS
arm64/x86-64, Windows x86-64, and Linux x86-64/aarch64, and installs pinned uv 0.11.8 under
`.tools/bin`. If your computer cannot run the stack locally, use the Codespaces button above.

On macOS and most Linux distributions the interpreter is `python3`; substitute it wherever these
commands say `python`.

```shell
python infra/scripts/bootstrap.py
./.tools/bin/uv sync --frozen
./.tools/bin/uv run --frozen poe preflight
./.tools/bin/uv run --frozen poe start
./.tools/bin/uv run --frozen poe ready
```

PowerShell and POSIX wrappers are available under `infra/scripts/`. After uv is on `PATH`, the
shorter `uv run --frozen poe <task>` form works.

| Service | Local URL | Purpose |
|---|---|---|
| API | `http://localhost:8000` | Submit exception workflows and retrieval queries; `/version` names the build that answers |
| Grafana | `http://localhost:3000` | Use the focused diagnostics dashboard |
| Prometheus | `http://localhost:9090` | Query bounded metrics and inspect the deployed alert rule |
| Alertmanager | `http://localhost:9093` | Inspect firing and resolved alerts |
| Jaeger | `http://localhost:16686` | Inspect local traces |
| LocalStack S3/SQS | `http://localhost:4566` | Inspect the emulated object-storage and queue endpoint |

Each of these ports can be overridden by setting the matching `COLDLINE_API_HOST_PORT`,
`COLDLINE_GRAFANA_HOST_PORT`, `COLDLINE_PROMETHEUS_HOST_PORT`, `COLDLINE_ALERTMANAGER_HOST_PORT`,
`COLDLINE_JAEGER_HOST_PORT`, or `COLDLINE_LOCALSTACK_HOST_PORT` environment variable in your shell
environment or a local `.env` file (copy `.env.example`) if a default collides with something
already running on your machine. Keep the override in place for every `poe` command.

This Task runs as its own Compose project, `coldline-task-3-7`. If an earlier Task's stack is
still running, run `poe stop` in that Task's repository first; otherwise `poe start` here fails
because the published ports are already taken.

PostgreSQL, Redis, worker metrics, and OTLP remain inside the Compose network. Codespaces uses the
same `compose.yaml` and keeps every forwarded port private. Redis keeps running in this Task only
for an earlier checkpoint's own contract test; no composition root reads it anymore.

## Command path

For this Task, run the supplied commands in this order:

```text
poe start
poe verify
```

`poe answers` is the narrower form of this Task's own check, if you want to confirm the answer
sheet and the diff boundary on their own before running the whole public path.

| Command | Use |
|---|---|
| `poe verify` | The public student verification path: it starts the stack, exercises the settled platform you inherited, and runs this Task's own answer-sheet and diff-boundary check |
| `poe answers` | This Task's own check on its own: `submission.yaml` still records `answers: {}`, and the diff from your merge base touches nothing but `submission.yaml` |
| `poe runbook-contract` | Inherited from Task 3.6, over the supplied runbook and fidelity record; passes as shipped |
| `poe queue-contract` | Run Task 3.3's own automated dead-letter redrive check |
| `poe slo-contract` | Run Task 3.4's own automated alert-bound, firing, and resolution checks |
| `poe gate-contract` | Run Task 3.5's own automated static-wiring and live-rejection checks |
| `poe contract` | Check interfaces, boundaries, submissions, and repository structure |
| `poe smoke` | Check the initialized running platform |
| `poe e2e` | Run the external API-to-worker workflow |
| `poe student-tests` | Run the supplied tests under `tests/student/`; this Task permits no additions there |
| `poe dev-failure-lab` | Inherited Task 3.6 exercise, still runnable; not part of this Task — do not rerun it for fresher output |
| `poe trigger-alert-load`, `poe verify-alert-recovery` | Inherited Task 3.4 exercise, still runnable; not part of this Task |
| `poe inject-failure`, `poe redrive` | Inherited Task 3.3 exercise, still runnable; not part of this Task |
| `poe restart` | Restart the existing API and worker containers **without rebuilding** |
| `poe stop` | Remove containers and the network, keeping named volumes |
| `poe reset` | Remove containers, the network, and local named volumes |

For Task 3.7, `poe verify` starts the stack, ingests the supplied corpus, runs the smoke tests,
the end-to-end exception workflow, the queue-contract, SLO-contract, and gate-contract checks, the
inherited runbook and fidelity checks over the supplied files, this Task's own answer-sheet and
diff-boundary check, and the supplied student tests. Nothing in it grades the evidence index or
the defense; a failure here means the checkpoint, not your index, needs attention. The earlier
Tasks' exercise commands remain in the table above because they still run, but the lesson is
explicit: point at the record you attached when each Task was accepted, do not rerun an exercise
now for fresher output.

## Folder map

```text
repository root/
├── docs/                Student guidance, public contracts, and fidelity notes
│   ├── contracts/       Machine-readable public contracts
│   ├── fidelity/        Local-runtime boundary notes, including the settled JobQueue record
│   ├── architecture/    Supplied vector engine technical profiles, in prose
│   ├── retrieval/       Supplied retrieval pipeline reference
│   └── student/         This Task's contract, and the supplied Task 6 runbook
├── config/              Retrieval configuration, settled and supplied from Sprint 2
├── infra/               Local setup and runtime configuration
│   ├── containers/      The API and worker Dockerfiles, with the build identity arguments
│   ├── observability/   Prometheus, Alertmanager, and Grafana configuration
│   ├── release/         The supplied Task 3.1 release manifest, unchanged
│   ├── corpus/          Supplied synthetic corpus, query set, and designated investigation
│   ├── judge/           Supplied cached judge evidence and its provenance record
│   ├── profiles/        Supplied engine and emulator profiles, and their provenance record
│   └── postgres/        Database initialization and the migration baseline stamp
├── loadtest/            Supplied traffic profile and provider-latency harness
├── migrations/          Alembic environment, revision template, and revisions
├── src/
│   ├── api/             HTTP application code, the retrieval and document paths, composition
│   ├── worker/          Background application code, including the dead-letter depth poller
│   ├── domain/          Shared domain code, contracts, the failure taxonomy, service and repository contracts
│   ├── ports/           Application interfaces
│   └── adapters/        Technology-specific implementations, including the supplied SQS/DLQ queue adapter
└── tests/
    ├── unit/            Isolated behavior checks
    ├── benchmark/       Supplied evaluation harness, metrics, and adoption policy
    ├── contract/        Interface, retrieval, and repository checks, and this Task's answer-sheet check
    ├── diagnostics/     Supplied stage inspector
    ├── doubles/         Supplied deterministic test doubles
    ├── failure/         Supplied failure-lab and exercise scripts from Tasks 3.3, 3.4, and 3.6 — not this Task's work
    ├── student/         Supplied student tests; no additions in this Task
    ├── smoke/           Running-platform checks
    └── e2e/             Supplied workflow tools and checks
```

## Overview

Use the Task 7 lesson (Task 3.7 in this repository) to decide what to do. This README covers
local setup and repository orientation.

1. `README.md` — local setup, commands, and permitted changes.
2. [`docs/student/task-3-7-contract.md`](docs/student/task-3-7-contract.md) — what this Task
   assesses and who assesses it, what belongs in the pull request description, what the checks
   verify, and the one permitted path.
3. [`docs/student/runbook.md`](docs/student/runbook.md) — a completion/reference version of the
   Task 6 recovery runbook, supplied here for orientation; not your evidence, use your own runbook
   linked at Task 6's accepted commit at the defense.
4. [`docs/fidelity/JobQueue.md`](docs/fidelity/JobQueue.md) — Task 3.3's record of what LocalStack
   SQS does not prove, with the settled ECS section Task 3.6 added; the limits you bring forward.

The application source lives in five flat packages:

| Package | Responsibility |
|---|---|
| `api` | HTTP delivery, API use cases, the retrieval workflow, versioned routes, configuration, and composition |
| `worker` | Background processing, retries, the dead-letter depth poller, configuration, and composition |
| `domain` | Provider-neutral contracts, state rules, identity, redaction, embedding, chunking, fusion, access constraints, failure classification, service and repository contracts |
| `ports` | Exactly five visible application interfaces |
| `adapters` | PostgreSQL, pgvector retrieval, LocalStack SQS/DLQ, S3-compatible object storage, deterministic model, the resilient model-provider wrapper, logs, traces |

`src/api/bootstrap.py` and `src/worker/bootstrap.py` compose each process from its settings and
adapters. Process settings live in `src/api/config.py` and `src/worker/config.py`.

## The five ports

Find the available interfaces in `src/ports/`. A port describes an application capability; an
adapter provides it using a concrete technology.

| Port | General responsibility |
|---|---|
| `ModelProvider` | Call an AI model service |
| `Retriever` | Look up relevant context or documents |
| `ObjectStore` | Store large binary objects or files |
| `JobQueue` | Publish and consume background work |
| `SecretProvider` | Read API keys and credentials |

LocalStack SQS, with a bound dead-letter queue, still carries `JobQueue`, unchanged from Task 3.3.
The dead-letter depth poller reads the dead-letter queue's own attribute directly, alongside
`JobQueue` rather than through it; see [JobQueue fidelity](docs/fidelity/JobQueue.md).

## Test levels

| Level | Requires Compose | Main question |
|---|---:|---|
| Unit | No | Does one responsibility behave correctly, including failures? |
| Contract | Some | Do interfaces, schemas, paths, and dependency rules stay compatible? |
| Smoke | Yes | Did the complete local platform initialize and become observable? |
| E2E | Yes | Can an external client complete the supplied workflow? |

Contract checks marked `runtime` need the running stack. `poe contract` skips them; `poe verify`,
`poe runtime-contract`, `poe queue-contract`, `poe slo-contract`, and `poe gate-contract` run them.
This Task's own check, `poe answers`, is static and needs no stack. Unlike every earlier Task, a
fresh Task 3.7 checkout has no expected failures: the checkpoint is settled, so `poe verify` passes
as shipped.

## Submission checks

Run `poe verify` locally before opening your student pull request. Public GitHub CI repeats
the student checks, running `poe answers` first so a boundary violation fails fast. This Task
records `answers: {}`: the pull request record from Tasks 1 through 6 and your live defense of it
are the evidence, so there is no protected answer check. There is no protected held-out job in
this Task either; Sprint 3's one held-out scenario belonged to Task 3.6 and left with it. The
evidence index in your pull request description and the defense itself are read and judged by
your instructor, at the Instructor Review and the Project Defense, not by any automated check.
Follow the Task lesson's instructor-review and progression policy.

## Task boundary

Task 3.7 asks you to assemble the evidence index into this Task's pull request description, open
a pull request that changes only `submission.yaml`, run `poe verify`, and deliver the live Project
Defense.

The only student-editable path is:

- `submission.yaml`

Task 3.6's `docs/student/runbook.md` and the ECS section of `docs/fidelity/JobQueue.md` are
supplied here as completion/reference versions of Task 6's files, settled and useful for
orientation but not your evidence; they are not yours to change in this Task, and neither is
anything under `tests/student/`. Keep the worker, both adapters, the failure-lab and exercise
scripts, Task 3.3's own settled `compose.yaml`, Task 3.4's own settled `infra/observability/alerts.yml`,
Task 3.5's own settled `.github/workflows/task.yml`, and every test file exactly as supplied; the
public check compares the diff from your merge base against this one permitted file and reports
any other change as a boundary violation. Everything else in this repository is supplied.

### Student walkthrough

See **Task 7: Project Defense** in your course platform for the full walkthrough. In outline:
read `docs/student/task-3-7-contract.md`, open each of Tasks 1 through 6 on the platform and in
its repository, assemble the six entries and the three-part structure into this Task's pull
request description, check `git diff --stat` shows only `submission.yaml`, run `poe verify`,
open and merge your pull request, submit on the platform, rehearse the three parts against the
clock, and deliver the defense with the repositories, the check runs, and your own
`docs/student/runbook.md` and `docs/fidelity/JobQueue.md`, linked at Task 6's accepted commit, on
screen.

## Operational limits

This local system does not authenticate users, terminate TLS, or manage production secrets.
The Compose PostgreSQL password and the LocalStack access keys are local-only non-secret
credentials. Never place real credentials, personal data, or production records in this
repository.

Alertmanager here is configured with a "default" receiver that has no notification integration:
alerts are queryable through its own API but never sent anywhere real. Never add a webhook, email,
Slack, or paid integration; Sprints 1-4 are emulator-only and never call a hosted endpoint.

LocalStack's SQS emulation is a local reliability primitive, not a managed-service durability,
IAM, availability, or cost claim. Stopping and starting one Compose container is a local fault
control, not an ECS service event. See [JobQueue fidelity](docs/fidelity/JobQueue.md) for the
exact boundary; it is one of the limits the lesson asks you to state at the defense, not rewrite.

Named volumes preserve local PostgreSQL, Redis, Prometheus, Alertmanager, Grafana, and Jaeger state
across `poe stop`. LocalStack object and queue contents are deliberately not persisted; the
initializer re-uploads the supplied corpus artifacts and re-provisions the queue on every start.
The `poe reset` command deletes the named volumes. This topology makes no backup, replication,
high-availability, disaster-recovery, capacity, latency-SLO, or availability claim beyond the one
alert Task 3.4 configures, the one CI gate Task 3.5 wires to it, and the one bounded recovery
Task 3.6's failure lab demonstrates.

See [JobQueue fidelity](docs/fidelity/JobQueue.md),
[ModelProvider fidelity](docs/fidelity/ModelProvider.md),
[ObjectStore fidelity](docs/fidelity/ObjectStore.md), and
[Retriever fidelity](docs/fidelity/Retriever.md) for the active adapter boundaries. The
[local runtime evidence](docs/fidelity/local-runtime.md) records the current measurement and its
qualification limits.
