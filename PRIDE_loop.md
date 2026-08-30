# PRIDE Loop

Codified cycle: **Plan → Review → Implement → Document → Evaluate**

## Overview

A gated, evidence-based development loop with explicit failure routing.

## Phases

1. **Plan** — Foundation of work (zero ambiguity, entry/end states fixed)
2. **Review** — Second glance (Devil's Advocate challenges exhausted)
3. **Implement** — Code exists, exercised
4. **Document** — Work recorded (processes, choices, implementation notes, exercise evidence)
5. **Evaluate** — Goal achievement (ship/iterate/abandon)

## Key Features

- Sequential and gated; no forward skips
- Failures route locally or to Evaluate
- Owner authority on scope/risk decisions
- Memory compounds across cycles

## Exit Gates

- Plan → Review: Complete working document, assumptions eliminated  
- Review → Implement: Challenges exhausted, owner disposition recorded
- Implement → Document: Code present at all callsites, tests run
- Document → Evaluate: Record complete with exercise evidence
- Evaluate (terminal): No unresolved issues from Document

## Failure Handling

- Scope-level failures (Review rejection; Document findings) → Evaluate → Plan  
- Locally fixable findings → Earliest responsible phase (Implement)