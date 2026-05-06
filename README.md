# notion-plat-mesh-pipe

`notion-plat-mesh-pipe` is a Java project for Platform engineering. It turns package a Java local lab for mesh analysis with append-only fixtures, checkpoint recovery checks, and documented operating limits into a small local model with readable fixtures and a direct verification command.

## Reading Notion Plat Mesh Pipe

Start with the README, then open `metadata/project.json` to check the constants behind the examples. After that, `fixtures/cases.csv` shows the compact path and `examples/extended_cases.csv` gives a wider look at the same rule.

## Purpose

The goal is to capture the core behavior in code and make the surrounding assumptions obvious. A reader should be able to run the verifier, open the fixtures, and understand why each decision was made.

## Design Sketch

The core is a scoring model over demand, capacity, latency, risk, and weight. That keeps service shape, route policy, and rollout constraints in one explicit decision path. The threshold is 162, with risk penalty 4, latency penalty 4, and weight bonus 2. The Java implementation uses a compact package layout and direct assertion checks.

## Fixture Notes

The extended cases are not random smoke tests. `degraded` keeps pressure on the review path, while `surge` shows the model when capacity and weight are strong enough to clear the threshold.

## What It Does

- Uses fixture data to keep route policy changes visible in code review.
- Includes extended examples for rollout constraints, including `surge` and `degraded`.
- Documents environment checks tradeoffs in `docs/operations.md`.
- Runs locally with a single verification command and no external credentials.
- Stores project constants and verification metadata in `metadata/project.json`.

## Setup

The only required setup is the local Java toolchain. After cloning, stay in the repo root so fixture paths resolve correctly.

## Verification

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/audit.ps1
```

The audit command checks repository structure and README constraints before it delegates to the verifier.

## Files Worth Reading

- `src`: primary implementation
- `tests`: verification harness
- `fixtures`: compact golden scenarios
- `examples`: expanded scenario set
- `metadata`: project constants and verification metadata
- `docs`: operations and extension notes
- `scripts`: local verification and audit commands

## Limits

The examples cover useful edges, not every edge. A larger version would add malformed-input tests, richer reports, and deeper domain parsers.

## Next Directions

- Split the scoring constants into a typed configuration object and validate it before use.
- Add a comparison mode that shows how decisions change when one signal is adjusted.
- Add a loader for `examples/extended_cases.csv` and promote selected cases into the language test suite.
- Add one more platform engineering fixture that focuses on a malformed or borderline input.

## Usage

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File scripts/verify.ps1
```

This runs the language-level build or test path against the compact fixture set.
