---
name: prompt-for-goal
description: Interactively pressure-test vague, long-running, or evidence-sensitive user requests until all six Codex Goal aspects are strong, then output only a `/goal` prompt. Use when the user asks to write, draft, improve, review, or translate a task into a Codex Goal; mentions Goals, `/goal`, persistent objectives, evidence-based completion, "keep working until done", or wants help deciding whether a Goal or a normal prompt fits.
---

# Prompt for Goal

## Overview

Use this skill to help users turn plain-language work into a Codex Goal with a clear finish line, verification surface, constraints, boundaries, iteration policy, and blocked stop condition.

Never create, activate, execute, or work on the Goal. This skill only gathers missing Goal details and writes the final Goal prompt.

## Workflow

1. Decide whether a Goal is appropriate.
2. Extract the Goal contract from the user's request.
3. Internally score all six contract fields as Pass, Weak, or Missing.
4. If any field is Weak or Missing, output only targeted follow-up questions.
5. Repeat until all six fields pass.
6. When all six fields pass, output only the final `/goal` prompt.

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

## Readiness Gate

Do not present a final Goal while any field is Weak or Missing.

Use this internal scorecard before drafting, but do not show it unless the user explicitly asks for the evaluation:

```markdown
Goal readiness:
- Outcome: Pass/Weak/Missing - <why>
- Verification surface: Pass/Weak/Missing - <why>
- Constraints: Pass/Weak/Missing - <why>
- Boundaries: Pass/Weak/Missing - <why>
- Iteration policy: Pass/Weak/Missing - <why>
- Blocked stop condition: Pass/Weak/Missing - <why>
```

If any field does not pass, ask 1-3 pointed questions that unblock the weakest fields. After the user answers, rescore all six fields and repeat. Be politely persistent: do not accept vague answers like "make it better," "use tests," "whatever files are needed," or "stop when done" when a stronger standard is needed.

If the user says "just draft it" while a field is Weak or Missing, ask the minimum questions needed to fill the missing fields instead of drafting from assumptions.

## Six-Aspect Standards

Each field passes only when it meets the relevant standard:

- Outcome: names the concrete end state, affected scope, and measurable or inspectable result.
- Verification surface: names the exact tests, commands, benchmarks, artifacts, reports, logs, or source materials that prove completion.
- Constraints: names what must not regress, including behavior, APIs, checks, performance thresholds, security/privacy limits, or documentation accuracy.
- Boundaries: names allowed files, repos, tools, data, credentials, external sources, and any exclusions. "Whole repo allowed" can pass only if explicit.
- Iteration policy: says how Codex should choose the next action after each attempt, such as nearest failing evidence, smallest targeted change, benchmark delta, risk reduction, or highest-value unresolved claim.
- Blocked stop condition: says when to stop and what the blocker report must include, such as missing credentials, unavailable data, irreproducible flake, upstream defect, exhausted evidence paths, or budget reached.

## Draft Pattern

Use this shape, adapting it to the user's domain:

```text
/goal Achieve <outcome>, validated by <specific evidence>, while maintaining <constraints>. Work within <boundaries>. After each attempt, <iteration policy>. If the goal cannot be verified under these limits, stop with <blocked report>.
```

Keep the draft compact enough to paste, but include enough detail that another Codex thread can tell when to complete, continue, or stop.

## Output Format

When any field is Weak or Missing, output only questions:

```markdown
1. ...
2. ...
3. ...
```

When all six fields pass, output only a single plain-text `/goal ...` prompt line with no label or code fence:

```text
/goal ...
```

Do not include labels, rationale, scorecards, assumptions, caveats, setup steps, extra commands to run, or commentary in either mode.

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

- All six fields are Pass.
- The outcome is specific enough to audit.
- The evidence is named, not implied.
- The constraints protect likely regressions.
- The boundaries give Codex room to investigate without making the Goal unbounded.
- The iteration policy says how to choose the next attempt.
- The blocked condition prevents endless work or overclaiming.
- The response is either only follow-up questions or only the final `/goal` prompt.
- The response does not execute, start, create, activate, or work on the Goal.

## References

Read `references/goals-cookbook-notes.md` when the user wants rationale, source-backed guidance, or examples for debugging, performance, migration, research, or artifact-generation Goals.
