---
name: prompt-for-goal
description: Turn vague, long-running, or evidence-sensitive user requests into strong Codex `/goal` prompts. Use when the user asks to write, draft, improve, review, or translate a task into a Codex Goal; mentions Goals, `/goal`, persistent objectives, evidence-based completion, "keep working until done", or wants help deciding whether a Goal or a normal prompt fits.
---

# Prompt for Goal

## Overview

Use this skill to help users turn plain-language work into a Codex Goal with a clear finish line, verification surface, constraints, boundaries, iteration policy, and blocked stop condition.

Do not create or activate a Goal unless the user explicitly asks to set or start one. By default, draft goal text the user can review.

## Workflow

1. Decide whether a Goal is appropriate.
2. Extract the Goal contract from the user's request.
3. Ask only for missing details that are necessary to make the Goal auditable.
4. Draft the `/goal` prompt.
5. Explain any assumptions or tradeoffs briefly.

## Goal Fit

Use a Goal when the task has:

- A durable objective.
- An evidence-based finish line.
- A path that may require several turns of investigation, testing, tuning, or iteration.

Prefer a normal prompt for a one-line edit, short explanation, simple code review, or question where the user wants one answer and then a stop.

## Contract Fields

Capture these six fields before drafting:

- Outcome: what should be true when the work is done.
- Verification surface: the test, benchmark, report, artifact, command output, or source material that proves completion.
- Constraints: what must not regress or be violated.
- Boundaries: which files, repositories, tools, data, or resources Codex may use.
- Iteration policy: how Codex should choose the next action after each attempt.
- Blocked stop condition: when Codex should stop and report that no defensible path remains.

If a field is unknown but not critical, write a reasonable assumption into the draft. If a field is critical to safety or auditability, ask one concise question before drafting.

## Draft Pattern

Use this shape, adapting it to the user's domain:

```text
/goal Achieve <outcome>, validated by <specific evidence>, while maintaining <constraints>. Work within <boundaries>. After each attempt, <iteration policy>. If the goal cannot be verified under these limits, stop with <blocked report>.
```

Keep the draft compact enough to paste, but include enough detail that another Codex thread can tell when to complete, continue, or stop.

## Output Format

Default to:

````markdown
Draft goal:

```text
/goal ...
```

Why this works:
- ...

Assumptions:
- ...
````

Omit "Assumptions" when the request already supplies the important details. Offer a shorter version only when the draft is bulky or the user asks for options.

## Examples

Weak request:

```text
Make this migration work and keep going until it is done.
```

Strong draft:

```text
/goal Complete the dependency migration, validated by the repository's migration test suite and a clean local build, while preserving public API behavior and existing lint/type checks. Work only in the migration-related packages, tests, and configuration. After each failed run, inspect the nearest failing evidence, make the smallest targeted change, and rerun the relevant check before broadening scope. If the migration cannot be verified because required upstream fixes or credentials are unavailable, stop with a blocker report naming the missing dependency and the evidence collected.
```

Weak request:

```text
Reproduce this paper as much as possible.
```

Strong draft:

```text
/goal Produce the strongest evidence-backed reproduction possible from the available paper, data, and local resources, validated by a final report that maps each headline claim to reproduced results, proxy evidence, or blockers. Keep uncertainty labels explicit and avoid claiming exact reproduction without matching artifacts. After each attempt, prioritize the claim with the highest evidence value that is still feasible locally. If exact reproduction is blocked by missing data, seeds, checkpoints, or implementation details, stop with an audit separating confirmed, partially supported, and blocked claims.
```

## Quality Checks

Before presenting the draft, verify that:

- The outcome is specific enough to audit.
- The evidence is named, not implied.
- The constraints protect likely regressions.
- The boundaries give Codex room to investigate without making the Goal unbounded.
- The iteration policy says how to choose the next attempt.
- The blocked condition prevents endless work or overclaiming.

## References

Read `references/goals-cookbook-notes.md` when the user wants rationale, source-backed guidance, or examples for debugging, performance, migration, research, or artifact-generation Goals.
