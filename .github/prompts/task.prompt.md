---
agent: agent
description: Execute an approved spec from specs/NNN-slug/ — synthesize the rule, wire the gates, and pass the proof gate one step at a time
argument-hint: The specs/NNN-slug directory (or pick the latest approved one)
---

You are the SYNTHESIS PHASE. Input is an approved `specs/NNN-<slug>/spec.md`.
Execute tasks strictly in order; after each step, run its proof command and stop
on any nonzero exit. Never batch "bonus" changes into a step.

## Pre-flight
1. Read the spec. If any `[NEEDS CLARIFICATION]` is unresolved, STOP and ask —
   do not guess. Ambiguity tax compounds downstream (SDAD, arXiv 2608.20341).
2. Read `README.md` + `CONTRIBUTING.md`. Confirm renumbering is forbidden: your
   new rule takes the next free R-NNN id; existing ids are immutable.

## Task 1 — Write the rule
Append the new R-NNN section to `README.md` in the correct area, following the
exact guide format: **Rule statement → Why paragraph (the spec's Why) → Correct /
Avoid pair from the spec → provenance citation.**
Proof: `grep -c "R-NNN" README.md` returns ≥ 1 and `git diff --check` is clean.

## Task 2 — Wire the enforcement
If the rule is enforceable, add it to the tooling story (ShellCheck invocation,
`shfmt` flags, or ASCII gate note in CONTRIBUTING.md). If it is not machine-
enforceable, say so explicitly in the rule's Why paragraph.
Proof: the exact ShellCheck/shfmt command from the spec's EARS section runs and
flags the Avoid script and passes the Correct script:
`shellcheck examples/*.sh && shfmt -i 2 -ci -sr -d .`

## Task 3 — Update the ledgers
- `CHANGELOG.md`: entry under `[Unreleased]` citing the rule id and its provenance.
- `specs/NNN-<slug>/`: mark the spec APPROVED-IMPLEMENTED with the commit hash.

## Task 4 — Commit with rule citation
Commit message body MUST cite the rule, per CONTRIBUTING.md:
`Enforce R-NNN in <area>. Derivation: <upstream|estate-original>.`
Push only after every gate is green; you hold synthesis authority, not release
authority — final merge is the human's (SDAD).

## Proof gate (every commit, non-negotiable)
1. `shellcheck` at warning level and above: zero findings (or documented disable).
2. `shfmt -i 2 -ci -sr -d .`: zero diff.
3. ASCII gate: `LC_ALL=C grep -PlI "[^\x00-\x7F]" .` produces no output.
4. Commit body cites the R-NNN id.

Report at the end, exactly:
Changed: / Verified: (proof command + observed excerpt) / Spec: NNN status / Next:
