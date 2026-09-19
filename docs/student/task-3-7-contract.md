# Task 3.7 — Instructor Presentation / Review contract

Six merged pull requests already prove that the platform passed. This Task asks whether you can
say why it is built the way it is. You write no new code, no essay, and no slides. You assemble
the pull request record you built across Tasks 1 through 6 into one evidence index, structure it
into a defense of at most 10 minutes, and deliver that defense live to your instructor with the
repositories, the check runs, and the runbook on screen.

The evidence index lives in this Task's pull request **description**, not in a file. The only
change this Task's pull request makes to the repository is `submission.yaml`, which stays
`answers: {}`. Everything Tasks 1 through 6 assessed ships supplied and settled in this
checkpoint, and you do not touch any of it.

## What is assessed, and by whom

| Assessed | By |
|---|---|
| The pull request changes only `submission.yaml` | Automated, in this repository (`poe answers`, and `poe verify` repeats it) |
| `submission.yaml` still records `answers: {}` | Automated (same command) |
| The inherited Task 1–6 checkpoint still passes as supplied | Automated (`poe verify`) |
| The evidence index, the three-part structure, and the reasoning | Your instructor, at the Instructor Review and the Project Defense |

This Task has no held-out check and no protected answer key. Sprint 3's one held-out scenario
belonged to Task 3.6 and left with it; nothing in this repository is graded privately. What the
automated checks do not read — the index, the structure, the reasoning, and the limits — is read
by a human.

## What is already supplied

| Supplied | Where | Note |
|---|---|---|
| Task 3.3's own settled dead-letter redrive policy | `compose.yaml` | unchanged; not this Task's editable surface |
| Task 3.4's own settled alert window | `infra/observability/alerts.yml` | unchanged; not this Task's editable surface |
| Task 3.5's own settled reliability gate | `.github/workflows/task.yml` | unchanged; not this Task's editable surface |
| Task 3.6's settled recovery runbook | `docs/student/runbook.md` | supplied here; one of the sources you deliver the defense from; not this Task's editable surface |
| Task 3.6's settled ECS fidelity section | `docs/fidelity/JobQueue.md` | supplied here, below Task 3.3's own SQS record; bring its limits forward at the defense, do not rewrite them |
| Task 3.3's own exercise scripts and queue diagnostics | `tests/failure/force_dlq_arrival.py`, `tests/failure/redrive_and_verify.py`, `tests/failure/queue_client.py` | still runnable; not this Task's exercise; do not edit them |
| Task 3.4's own exercise scripts | `tests/failure/trigger_alert_load.py`, `tests/failure/verify_alert_recovery.py` | still runnable; not this Task's exercise; do not edit them |
| Task 3.6's development failure lab | `tests/failure/dev_failure_lab.py` | still runnable; not this Task's exercise; do not rerun it for fresher output |

None of these is this Task's editable surface. The public check compares the diff from your merge
base against the one permitted file below, and a change to any of them is reported as a boundary
violation without reading further.

## What belongs in the pull request description

The pull request description is where this Task's work lives. It is read by your instructor at
the Instructor Review; no automated check in this repository reads it. It carries:

- **Six entries**, one for each of Tasks 1 through 6: the merged pull request link, the accepted
  commit, its required check results on the platform, and the evidence excerpt that Task's lesson
  asked you to attach, with the command or check that produced it and when you ran it. One or two
  sentences per entry in your own words: what you chose, what the supplied starting state did
  instead, and what proved your choice holds.
- **Three named parts**, above the index, each with a time slice, the slices summing to 10
  minutes. Under each part, what you show on screen (a diff, a check log, a command's closing
  output, a check-run page, a section of the runbook — never a slide) and what you say about it.
- **The stated limits**, one or two lines under each part: what a single-host Compose rollout
  does not prove about a managed platform, and what LocalStack SQS does not prove about managed
  SQS and ECS. Your release record and `docs/fidelity/JobQueue.md` already hold this; bring it
  forward.

Every excerpt is from your own run, dated and attributed to the command or check that produced
it, and separated from supplied data. A Task with no link, or with a missing, skipped, or errored
required result, is not assembled; resolve it through that Task's own **Resubmit** route first.

## Commands

```shell
poe verify    # the public student verification path: the settled platform, plus this
              # Task's own answer-sheet and diff-boundary check
poe answers   # the narrower form of that check on its own: answers: {} and the diff from
              # your merge base touches only submission.yaml
```

Start the stack per `README.md` first; `poe verify` starts it again itself and ingests the
supplied corpus. Nothing in either command grades the evidence index or the defense. A failure in
`poe verify` means the checkpoint, not your index, needs attention; a failure in `poe answers`
means the diff reached outside `submission.yaml` or the answer sheet is no longer empty.

## What the checks verify

| Check | What it looks at |
|---|---|
| `tests/contract/submission_validation.py` (`poe answers`) | `submission.yaml` is one plain YAML mapping with a required, empty `answers`; the diff from the merge base with `main` touches only `submission.yaml`, with no directory prefix exempted |
| `test_submission_change_stays_within_the_permitted_diff` | The same boundary asserted as a pytest case over the repository's own diff: every changed path is `submission.yaml`, and any other path — the supplied runbook, the fidelity record, a test under `tests/student/`, an application, configuration, or workflow file — fails it |

The inherited checks from Tasks 3.3 through 3.6 (`poe queue-contract`, `poe slo-contract`,
`poe gate-contract`, `poe runbook-contract`) also run inside `poe verify`, over the supplied
settled files, and pass as shipped. They assess the checkpoint you inherited, not your work here.

## Student-editable paths

- `submission.yaml`

That is the whole list. `docs/student/runbook.md`, `docs/fidelity/JobQueue.md`, and `tests/student/`
were Task 3.6's surface and are supplied and settled here. Keep the worker, both adapters, the
failure-lab and exercise scripts, `compose.yaml`, `infra/observability/alerts.yml`,
`.github/workflows/task.yml`, and every test file exactly as supplied. Before you push, run
`git status` and `git diff --stat`: if anything besides `submission.yaml` changed, the public check
reports the boundary violation rather than your work.

## The defense

After the public check passes and your instructor has read the assembled pull request, you
deliver the Project Defense live, in at most 10 minutes, by part: state the decision, point at the
diff or the output that proves it holds, and name the limit. Have the six pull request pages, the
platform results for each Task, the two `reliability-gate` check runs from Task 5,
`docs/student/runbook.md`, and `docs/fidelity/JobQueue.md` open before the session, and share that
screen. When the local evidence cannot answer a question, say so and say what measurement would.
Sprint 3 - Project 3 is complete once all seven Task pull requests are merged, submitted, and
passed their required checks, both Instructor Reviews are recorded, and the outcome of the Project
Defense is recorded.
