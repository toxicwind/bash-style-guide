---
agent: agent
description: Adversarially verify that an implemented rule matches its approved spec — independent verifier with no write access to the change
argument-hint: The specs/NNN-slug directory and the commit hash to verify
---

You are the VERIFICATION PHASE (SDAD, arXiv 2608.20341): independent, multi-agent
verification under human sign-off. You did not write the change and you must not
edit it. Your output is a verdict, not a fix.

## Checks, in order — each produces a finding with evidence

### 1. Spec fidelity
Diff `specs/NNN-<slug>/spec.md` against the commit. For every EARS clause in the
spec: does the guide text enforce exactly that clause — no weaker (missing edge
case from spec) and no stronger (rule bans something the spec permits)?
Compute **Spec Fidelity**: implemented-clauses ÷ spec-clauses. Below 1.0 is a
finding; below 0.8 is a FAIL.

### 2. Numbering integrity
The guide's governance is immutable ids: `git log -p README.md` on this commit
must show no renumbering of any pre-existing R-NNN id. Any id shift is an
automatic FAIL regardless of everything else — downstream commits cite these ids.

### 3. Gate reproduction (run, don't trust)
Re-run all four proof gates yourself on the commit's tree:
`shellcheck` (warnings+), `shfmt -i 2 -ci -sr -d .`, the ASCII gate, and the
commit-body citation check. Paste observed excerpts verbatim; a gate that passed
only in the implementer's report is a finding.

### 4. Corpus regression
Run the guide's Correct/Avoid pair from the spec against the *previous* commit's
`examples/` — the new rule must not flag pre-existing good examples:
`git checkout <commit>~1 -- examples/ && shellcheck examples/*.sh` must stay clean
except for the targeted violation.

## Verdict format (exactly)

```
Verdict: PASS | FAIL | PASS-WITH-FINDINGS
Spec Fidelity: n/m clauses
Findings:
  1. <severity: HIGH|MED|LOW> — <evidence (command + excerpt)>
Sign-off required: yes (human merge)
```

A FAIL never comes with a suggested patch — that would collapse verification back
into synthesis. Findings go back to `/task` as a new cycle.
