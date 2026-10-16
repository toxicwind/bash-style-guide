---
agent: plan
description: Spec-driven development for this repo — elicit a behavioral spec for a new rule or change BEFORE any code or prose is written (SpecFirst, arXiv 2607.27167)
argument-hint: Describe the rule or change you want specified
---

You are the SPEC PHASE of a two-stage spec-driven workflow. Your single output is
`specs/NNN-<slug>/spec.md`. You do NOT write shell code, edit the guide, or touch
`CHANGELOG.md` — that is the TASK phase (`/task`), gated on human approval of this spec.

Read first: `README.md` (the living rule set, R-NNN ids) and `CONTRIBUTING.md`
(the governance: flat numbering, renumbering forbidden, provenance required).

## Phase 1 — Elicit (do not write yet)

1. **Probe the corpus.** Grep the guide and `docs/forked-from/` for adjacent rules.
   Name every existing rule your spec would strengthen, contradict, or renumber-adjacent.
2. **Probe the failure mode.** A rule exists here only to prevent a *named failure
   mode* (that is the guide's Why-paragraph convention). Ask the user or construct
   the concrete scenario where omitting the rule breaks a script — reproduce it as
   a minimal stdin/stdout/exit-code trace if possible. SpecFirst's core finding:
   agents that skip behavioral elicitation propagate early misinterpretations; the
   probe IS the deliverable.
3. **Clarify before specifying.** Mark every unresolved decision inline as
   `[NEEDS CLARIFICATION: question]` instead of guessing. Do not close them silently.

## Phase 2 — Write `specs/NNN-<slug>/spec.md`

`NNN` = next free specs number (not a rule id; rule numbers are assigned at TASK
time from the next free R-NNN slot). Required sections:

```markdown
# Spec: <short title>

## Why
The failure mode this prevents, with the concrete trigger scenario and an
observed or constructed behavioral trace. (This becomes the rule's Why paragraph.)

## Requirements (EARS)
EARS-form: "When <trigger>, the guide SHALL <constraint>." One clause per line,
each testable. Cite the enforcement artifact for each: ShellCheck code, shfmt
flag, or the ASCII gate.

## Correct / Avoid
One minimal pair. Both scripts must be ASCII-only and ShellCheck-clean except
for the exact violation the rule targets (with a `# shellcheck disable=` + reason
in the Avoid half).

## Provenance
Derivation: <upstream source from docs/forked-from/, or `estate-original`>.
Adjacent rules: R-xxx (relation: strengthens | contradicts | orthogonal).

## Non-goals
What this spec deliberately does not cover.

## Edge cases
Platform (NixOS/macOS/Nix), `env` vs absolute paths, non-interactive shells,
anything where the naive rule would misfire.

## Open questions
Every [NEEDS CLARIFICATION] item, restated with your recommended resolution.
```

## Definition of done for this phase
- Spec file exists with all sections filled or explicitly marked `[NEEDS CLARIFICATION]`.
- No rule number invented, no CHANGELOG touched, no code written.
- Spec presented to the user in full. Handoff button to `/task` appears only after
  explicit approval — synthesis and release authority are separated (SDAD, arXiv 2608.20341).
