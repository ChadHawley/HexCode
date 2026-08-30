# PIE — Plan · Implement · Evaluate

A universal development loop for any kind of creation: code, documents, images, video, configuration. Simple, gated, evidence-based.

## The Loop

```
Plan ──► Implement ──► Evaluate
 │       │              │
 What    Where/How     Does it pass?
 Pass/Fail gate  Checks & runs are explicit
```

- **One cycle per task**: Plan → Implement → Evaluate in sequence. No skipping. No forward skips.
- **No ceremony overhead**: Keep it light. If the plan is clear, skip straight to implementation. But always evaluate before declaring done.
- **Memory compounds**: Lessons from Evaluate feed back into Plan for the next iteration.

## Phase 1 — Plan

Define what needs to be created and how to verify it's done. This can come from memory, a markdown file, GitHub issue, or direct conversation.

### What to include

- **Goal**: What are we building? One sentence.
- **Scope**: What files, artifacts, or outputs are expected? List them explicitly.
- **Pass/Fail gate**: How do we know it's done? Concrete criteria — not "looks good" but "builds without errors", "compiles in target format", "passes test suite".
- **Inputs**: Source material, specs, references. Note what the AI agent needs to read before starting.
- **Outputs**: List every artifact (code files, docs, images, videos, configs). Specify formats, versions, locations.

### Plan variants

| Scenario | Plan format | Detail level |
|----------|-------------|--------------|
| Simple fix or small task | One-liner in conversation | Minimal — just the pass/fail gate |
| Medium task (one file or one feature) | Inline markdown or issue reference | Enough to execute without asking questions |
| Complex creation (multi-file, multi-agent) | Structured markdown with sections | Includes dependencies, order, test specs |

### Plan examples

**Code:**
```markdown
Goal: Refactor auth module to use JWT instead of session cookies.
Pass/Fail: `npm run test` passes; `curl /api/auth/test` returns 200 OK.
Outputs: src/auth/jwt.ts, src/middleware/auth-guard.ts, tests/auth.test.ts
Inputs: src/auth/session.ts (current implementation)
```

**Document:**
```markdown
Goal: Write architecture overview for the API layer.
Pass/Fail: Output in `/docs/architecture.md`; reads clearly; includes diagram of request flow from client → gateway → service.
Outputs: /docs/architecture.md, diagrams/request-flow.svg
Inputs: src/server/routes.ts, src/services/api-layer/
```

**Image:**
```markdown
Goal: Create logo for "HexCode" brand.
Pass/Fail: Outputs PNG at 512x512px and SVG; transparent background; reads clearly at small sizes.
Outputs: logos/hexcode-logo.png, logos/hexcode-logo.svg
Inputs: Brand guidelines from /docs/brand.md (if available)
```

**Video:**
```markdown
Goal: Create 30-second product demo for API overview video.
Pass/Fail: Outputs `demo/demo.mp4`; plays without stutter; includes title card and closing credit.
Outputs: demo/demo.mp4, demo/demo-final.png (thumbnail)
Inputs: Screenshots from /screenshots/, brand assets from /logos/
```

### Plan gates

- **No ambiguity**: If the plan leaves anything open ("figure it out", "whatever works"), define it explicitly during Plan. Ambiguity → back to Plan before Implementing.
- **No TODOs left in code or docs**: Every placeholder must be resolved or noted as a risk with explicit pass/fail for next iteration.
- **All dependencies listed**: If the plan references external files, APIs, or services — list them so the agent can read them before starting.

## Phase 2 — Implement

Execute the plan by producing the specified artifacts. Follow the instructions exactly; no improvisation beyond what's explicitly allowed in the plan.

### Execution rules

- **Follow the plan**: Execute steps in order as defined. If a step requires reading other files first, read them before writing.
- **No skipped steps**: Every output listed in the plan must exist when done — no partials masked by "close enough".
- **Incremental commits**: Each logical unit of work is committed separately; no broken intermediate state between commits.
- **Small context for agents**: When delegating to sub-agents, each gets a self-contained task description tokens with explicit pass/fail criteria.

### Execution by type

| Type | Process | Checks during execution |
|------|---------|--------------------------|
| **Code** | Write code → run tests → verify build | `npm test`, `python -m pytest`, compilation, linting |
| **Docs** | Write markdown → check structure → verify links | Readability, heading hierarchy, image references resolve |
| **Images** | Generate/create → validate format → check dimensions | File size within limits, correct format, no corruption |
| **Video** | Render/edit → validate playback → check duration/format | No stutter, correct codec, length within range |

### Implementation checklist per artifact type

- [ ] **Code**: Written in correct language; imports resolved; builds/compiles/runs successfully.
- [ ] **Config**: Correct syntax (YAML/JSON/TOML); validates against schema if available.
- [ ] **Docs**: Readable, properly structured, links intact, consistent terminology with existing docs.
- [ ] **Images**: Valid format at specified resolution; no artifacts or corruption; file size reasonable.
- [ ] **Video**: Plays without errors; correct codec and container; duration within spec; thumbnail generated if required.

## Phase 3 — Evaluate

Verify that the work meets the plan's pass/fail gate. Evidence over inference — every check is explicit, not assumed.

### Evaluation checklist

| Check | How |
|-------|-----|
| **Code review** | Read through code for correctness, naming conventions, edge cases; run linter (`eslint`, `pylint`, etc.) |
| **Tests pass** | Run full test suite (`npm test`, `pytest`, custom scripts); no skipped tests remaining |
| **Build succeeds** | Compile/build artifact without errors or warnings that matter |
| **Text consistency** (for docs) | Verify terminology matches across files; headings follow hierarchy; links resolve |
| **Artifacts valid** | Images render correctly at specified sizes; video plays smoothly; config loads without errors |

### Evaluation by type

- **Code**: `npm test` / `pytest` / `go test` — whatever the project uses. If no test suite exists, write a focused one for the changed behavior. No stubs or mocks standing in for real checks.
- **Docs**: Read through; check heading hierarchy (H1 → H2 → H3); verify image references resolve (`ls -la img/`); ensure terminology is consistent with existing docs. If writing a new document, check it against the style guide if one exists.
- **Images**: Validate format and dimensions using `file` command or ImageMagick; check for corruption by opening in preview tool. No artifacts at specified resolution.
- **Video**: Render complete; no stutter during playback; correct codec/container specification met; duration within range. If thumbnail exists, verify it matches the video's first frame.

### Evaluation rules

- **No forward skips**: Every artifact must be verified against its pass/fail gate before declaring done.
- **Evidence over inference**: "It looks right" is not enough — run the check, capture the output.
- **All-or-nothing**: If one check fails, return to Implement (or Plan if criteria need adjustment). Don't patch and hope.
- **Scope-level findings**: If a check reveals something outside the plan's scope (e.g., unrelated module breaks), record it as a risk for next iteration — don't fix it here unless explicitly asked.

## Agent Execution

When tasks are delegated to sub-agents, each runs its own PIE loop:

```
Agent Plan → Agent Implement → Agent Evaluate → PR/Merge into main branch
```

- **Self-contained**: Each agent receives a complete task description ≤4096 tokens with explicit pass/fail criteria.
- **No shared mutable state between steps**: Each commit is in a clean state; no broken intermediate states.
- **Review gate**: No merge without human acceptance or AI-corrected feedback loop on the PR.

### Agent communication protocol

| Artifact | Purpose | Location |
|----------|---------|----------|
| GitHub Issue | Task description, acceptance criteria | `github.com/<owner>/<repo>/issues/<N>` |
| Feature branch | Working code/docs during implementation | `feature/<agent>-<task-slug>` |
| Pull Request | Review gate; merged to milestone/main | PR from feature → main or milestone branch |

## Failure Handling

| Scenario | Action |
|----------|--------|
| **Build fails** | Fix in same commit chain; re-run until clean. No skipped steps. |
| **Test fails** | Investigate — is it a bug or a plan gap? If bug → fix and retest. If plan gap → return to Plan, record discrepancy. |
| **Text inconsistency found** | Cross-reference with existing docs; update to match terminology used elsewhere. |
| **Image/video doesn't render correctly** | Re-render with correct parameters; verify codec/format matches spec. |

## Memory Across Cycles

- **Lessons recorded**: Each cycle's Evaluate produces findings that feed into the next Plan (e.g., "watch out for memory leak in auth module", "use SVG over PNG for icons").
- **No reset**: The plan compounds — each iteration builds on previous decisions. No clean slate unless explicitly stated.

## Quick Reference

```
Plan:    What → How many → Pass/Fail gate
Implement: Write it → Commit it → Check it before commit
Evaluate:  Did it pass the gate? Evidence > inference
```

**Done means**: Tests pass, build succeeds, artifacts exist at specified quality, text is consistent. Nothing left hanging.
