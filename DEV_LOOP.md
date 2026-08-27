# Development Loop

Codified cycle: **Plan → Review → Implement → Verify → Assess → Test → Evaluate**.

The loop is sequential and gated. Each phase has an entry condition (what must be true to start), a body (what happens), and an exit gate (what must hold before the next phase). Failures dispositioned locally at their source phase return directly to the earliest responsible phase; scope-level failures — Review rejection, or Verify/Assess/Test findings touching scope, standards interpretation, or goal attainment — pass to Evaluate, which prepares full communication back to Plan. Nothing routes silently forward, and no failure is dropped, deferred silently, or waived by inference.

## Phase Table

| # | Phase | Question answered | Entry condition | Exit gate |
|---|-------|-------------------|-----------------|-----------|
| 1 | Plan | What is the full foundation of the requested work? | A need exists; scope is bounded | Complete working document: zero ambiguity over the plan's own terms — every assumption eliminated by direct questioning until none remains; residual external unknowns explicitly enumerated as monitored risks; entry points and end states fixed; execution mode resolved (main model or subagents) |
| 2 | Review | Can the plan survive a second glance? | Phase 1 gate passed | Devil's Advocate challenges exhausted, each with an owner disposition recorded; missing information surfaced and resolved; owner acceptance recorded — or scope-level rejection passed to Evaluate for communication back to Plan |
| 3 | Implement | Does the change exist in code? | Phase 2 gate passed; execution mode resolved (main model or subagents) | Change present at every named callsite; obsolete paths removed; no stubs or placeholders |
| 4 | Verify | Does the code properly cover the plan's requests and needs? | Phase 3 gate passed | Every planned element present in code; missed elements found, named, and dispositioned (implemented or waived by owner) |
| 5 | Assess | Does the code meet proper standards? | Phase 4 gate passed; review mode resolved (main agent or subagents) | Code review complete; standard violations found, named, and dispositioned (fixed or waived by owner); regression surface mapped with risks classified |
| 6 | Test | Does the code hold under exercise? | Phase 5 gate passed | Small test code, fixtures, and/or mocks written where needed; all supporting unit tests run and pass; failures dispositioned (fixed or waived by owner) |
| 7 | Evaluate | Does the cycle achieve its goal? | Phase 6 gate passed, or a scope-level failure exists (Review rejection; Verify/Assess/Test findings touching scope, standards interpretation, or goal attainment) | If clean: ship decision with named justification. If issues found: return to Plan with full communication of each issue and a revised plan showing how the loop will satisfy them toward the goal |

## Gates in Detail

### Plan → Review
- **Plan is a complete founding of the requested work** — a working document carried from start to finish, not an outline or hypothesis.
- **No assumptions, no guessing — over the plan's own terms.** Every assumption about what changes, where, and to what end state is eliminated by direct questioning until none remains. Residual external unknowns — third-party behavior, environment state, future requirements unknowable at planning time — are not guessable; they are explicitly enumerated as monitored risks with named check points, and questioning continues until every open item within the plan's terms is answered by owner or observable fact.
- **Expected entry points named** — the exact starting conditions, states, and interfaces the work begins from.
- **Expected end states fixed** — concrete, observable terminal states: commands, UI states, errors, data shapes. Acceptance criteria are these end states, asserted verbatim where exact.
- Change described at symbol level: files, APIs, state fields touched.
- Non-goals named explicitly so scope creep has no entry point.
- **Execution mode resolved** — the plan answers whether the main model does the coding or subagents do, given the host's capacity. Delegation is chosen deliberately at founding time, never improvised during implementation. The same question answered for review: whether the main agent performs the Assess code review or subagents do. Local large models of this kind have no room for subagents — on such hosts the main agent reviews directly and delegation is out of scope.

### Review → Implement
- **Second-glance review.** The full Plan is re-read with an eye specifically for missing information — gaps the author could not see from inside their own framing.
- **Devil's Advocate challenges the plan** to build a better one, not to shoot it down. Every challenge must respect the original author's ideas: attack the weakest load-bearing assumption, propose the alternative that preserves intent.
- **Bounded turns.** The DA relents after a few turns if a human is in the loop — persistence without concession ends review, not debate.
- **Human in the loop.** A reviewer may participate directly: answer the DA's questions, ask their own, and steer the discussion toward perfection of the plan. Owner answers are authoritative; they close questions immediately.
- **Disposition after discussion** — acceptance: continue to Implement. Scope-level rejection or unresolved challenge: pass to Evaluate, which prepares the communication back to Plan; every adjustment is recorded against the challenge that produced it. No silent edits. A relented challenge never evaporates: it exits as a recorded artifact — the challenge itself, why it was not conceded, and the owner's disposition (accepted risk / deferred question with named check point / waived) — retained in the cycle record.

### Implement → Verify
- **The coding process begins here.** Implementation follows the accepted plan's symbol-level description directly.
- **Execution mode is decided in the Plan, not improvised here.** The plan answers whether the main model does the coding or subagents do. Local large models of this kind have no room for subagents — on such hosts the main model executes directly and delegation is out of scope. Where delegation is used, each subagent receives a self-contained slice with its contract stated up front; shared mutation boundaries are serialized under one integration owner.
- Clean cutover: every caller migrated; deprecated aliases, shims, and re-exports deleted.
- No `TODO`, no mock fallback standing in for real behavior.

### Verify → Assess
- **Coverage audit.** The written code is checked against the plan's requests and needs — not merely run to see if it works. Every planned element must be present in code; partial coverage of a requirement does not count as coverage.
- **Hunt for missed elements.** Elements dropped, half-implemented, or silently substituted during implementation are found, named, and dispositioned: implemented now, or waived by explicit owner decision recorded against the element.
- The changed path is exercised on the actual surface (terminal, browser, host UI); output/state diff matches acceptance criteria verbatim where exact.

### Assess → Test
- **Code review.** The written code is examined against proper coding standards — clean code, SOLID, DRY, naming, error handling — not merely for defects. Every violation found is named against the standard it breaches and dispositioned: fixed before exit, or waived by explicit owner decision recorded against the violation.
- **Review mode was decided in the Plan, not improvised here.** Where delegation is used, each reviewer receives a self-contained slice of the change with its contract stated up front; findings converge on one report under a single integration owner.
- Blast radius enumerated: direct dependents, data migrations, external consumers.
- Each risk dispositioned: fixed, contained with named boundary, or escalated to owner.

### Test → Evaluate
- **Test the code.** Write small test code — focused scripts, fixtures, and/or mocked dependencies — exercising the changed behavior directly. Tests are instruments of exercise, not ceremony: each must fail on a plausible bug and pass on correct behavior.
- **Run every unit test written to support the code and its testing.** Existing suites touching the changed contract execute in full; newly written tests run alongside them. Failures are named against the contract they break and dispositioned: fixed before exit, or waived by explicit owner decision recorded against the failure.
- Tests assert behavior and boundaries, not plumbing or source text. Deterministic; isolated; full-suite-safe.

### Evaluate (terminal)
- **The cycle's judgment point.** Evaluate asks whether the work achieves its goal — not merely whether each phase passed in isolation.
- **Clean path.** No unresolved issues from Verify, Assess, or Test: decision is ship / iterate / abandon — one word, one reason; lessons appended to the next cycle's Plan inputs (constraints, known traps); cycle record closed, artifacts retained under version control.
- **Issue path.** Scope-level failures — Review rejection, or Verify/Assess/Test findings touching scope, standards interpretation, or goal attainment — pass to Evaluate, which returns control to Plan with proper communication of each issue: named against the requirement, standard, or contract it breaks, with observed evidence attached, plus a revised plan showing how this loop will satisfy those concerns in pursuit of the original goal. The issues travel as first-class inputs to the new founding; they are never dropped, deferred silently, or waived by inference. Locally fixable findings (a missed element, a standard violation, a failing assertion) are dispositioned at their source phase — fixed before exit, or waived by explicit owner decision recorded against the finding — and do not escalate.
- Scope shrinkage resulting from such returns requires explicit owner approval at the point of decision.

## Failure Handling
| Scope-level failure (Review rejection; Verify/Assess/Test findings touching scope, standards interpretation, or goal attainment) | Evaluate → Plan | Evaluate prepares full communication of each issue — named against the requirement, standard, or contract it breaks, with observed evidence; revised plan shows how the loop satisfies the concerns toward the goal |
| Locally fixable finding (missed element, standard violation, failing assertion) | Earliest responsible phase (Implement) | Fixed before exit, or waived by explicit owner decision recorded against the finding; no silent routing |

## Invariants

1. **No forward skips; backward only.** Gates are one-way doors; remediation never routes forward and never silently between mid-loop phases — locally fixable failures return to the earliest responsible phase, scope-level failures through Evaluate.
2. **Evidence over inference.** Every gate passage is backed by observed output, not successful compilation or plausible-looking code; every relented challenge carries its disposition artifact.
3. **Owner authority.** Scope reduction, risk waivers, ship decisions, and challenge dispositions require explicit owner sign-off at the point of decision — never inferred from silence.
4. **Memory across cycles.** Evaluate's lessons are inputs to the next Plan; the loop compounds rather than resets.
