# PIE Process — Plan · Implement · Evaluate

A layered development process where the **outer lifecycle** (Plan → Implementation Plan → Plan Implementation → Project Close) orchestrates **inner PIE loops** run by AI agents on individual tasks. Git branches, issues, and PRs are the persistent state.

## Outer Lifecycle

```
Grand Plan ──► Implementation Plan ──► Plan Implementation ──► Project Close
 │                │                      │                    │
 Interactive     Branches/Issues      Agents run PIE       Compare outcome
 Devil's        Milestones & Steps   (1 to N parallel)    vs Grand Plan
 Advocate       PRs merged           Commit → close issue→ PR review
```

## Phase 0 — Project Open (Grand Plan)

- **Input**: Interactive session or direct document from user.
- **Process**: AI grills the user about the plan using Devil's Advocate questioning, probing assumptions and edge cases. Once scope is clear: create repo, document Grand Plan.
- **Exit gate**: Repo exists; Grand Plan documented with goals, constraints, end states, and known risks.

## Phase 1 — Implementation Plan

- Break objectives into Milestones → steps.
- Each milestone/task has a clear pass/fail goal.
- Includes code, tests, examples, documentation specifications.
- Instructions sized for AI agent small-context windows (≤4096 tokens per task).
- **Exit gate**: Implementation plan complete; milestones sequenced; each step self-contained with acceptance criteria.

## Phase 2 — Plan Implementation

For each milestone:

1. Create **milestone branch** from `main`.
2. Assign tasks to agents (configurable: 1–N parallel background agents).
3. For each agent, create a **change branch** (`feature/<agent>-<task-slug>`) off the milestone branch.
4. Create **GitHub issue** for each task — this is the agent's source of truth.
5. Agent runs PIE internally on its assigned task: Plan → Implement → Evaluate.
6. Agent commits code, closes GitHub issue, creates PR to milestone branch.
7. **Human reviews** PR → accepts or comments with fixes; AI addresses feedback in same branch.
8. Upon milestone completion: **AI reviews the merged milestone against the Implementation Plan**.
   - If failure: fix process repeats (plan → implement → evaluate on the milestone).
   - If acceptable: create PR from milestone branch into `main`.
9. Next milestone begins; repeat until all milestones complete.

## Phase 3 — Project Close

- AI reviews entire process: task descriptions, GitHub issues, PRs, commits.
- Compares final outcome against the Grand Plan.
- Writes closing documentation.
- Verifies: all documentation complete, all PRs accepted, all branches merged and pruned, all issues resolved.
- **Exit gate**: Closing document written; `main` contains all work; branches pruned; no open issues.

## Agent Configuration

Agents run their own PIE loop internally per task. The outer process coordinates them through Git artifacts:

```yaml
# process_config.yaml
agent_concurrency: 1  # configurable from 1 to N parallel agents
context_window: 4096  # tokens — each agent gets a self-contained task description
issue_tracker: github  # or gitea
branch_strategy: milestone-then-feature  # milestone branch → feature branches per agent
```

## Branch Model

```
main
 └── milestone/<project>-<milestone-sul>      ← Milestone branch
     ├── feature/agent-a-task1                   ← Agent A's work
     │   └── PR → merged into milestone branch
     ├── feature/agent-b-task2                    ← Agent B's work (parallel)
     │   └── PR → merged into milestone branch
     └── PR → main                               ← Milestone accepted, merged to main
```

## Rules

1. **Outer process gates**: Each phase must exit before the next begins. No skipping.
2. **Inner loop per agent**: Every agent runs Plan → Implement → Evaluate on its own task; commits are always in a clean state between steps.
3. **Git is truth**: Branches, issues, PRs, and commits are the persistent state. No external tracking.
4. **Human review gate**: No PR merges without human acceptance or AI-corrected feedback loop.
5. **Memory compounds**: Closing documentation feeds lessons into the next Grand Plan iteration.
