---
name: create-prompt-for-goal
description: Interactively pressure-test vague, long-running, or evidence-sensitive user requests until Codex Goal components are strong, then output only a heading-structured `/goal` prompt. Use when the user asks to write, draft, improve, review, or translate a task into a Codex Goal; mentions Goals, `/goal`, persistent objectives, evidence-based completion, "keep working until done", or wants help deciding whether a Goal or a normal prompt fits. Stop and report may be omitted only if the user explicitly insists.
---

# Create Prompt for Goal

## Overview

Use this skill to help users turn plain-language work into a Codex Goal with heading-structured components: outcome, verification surface, constraints, boundaries, iteration policy, and stop-and-report policy.

Never create, activate, execute, or work on the Goal. This skill only gathers missing Goal details and writes the final Goal prompt.

## Workflow

1. Decide whether a Goal is appropriate.
2. Extract the Goal contract from the user's request.
3. Internally score all six contract fields as Pass, Weak, or Missing.
4. If any required field is Weak or Missing, output only targeted follow-up questions.
5. Repeat until the first five fields pass and `Stop and report` either passes or is explicitly waived by the user.
6. Output only the final `/goal` prompt with component headings.

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
- Stop and report: when Codex should stop and what it should report if no defensible path remains. This component is required by default, but the user may explicitly choose to omit it.

## Readiness Gate

Do not present a final Goal while any required field is Weak or Missing.

Use this internal scorecard before drafting, but do not show it unless the user explicitly asks for the evaluation:

```markdown
Goal readiness:
- Outcome: Pass/Weak/Missing - <why>
- Verification surface: Pass/Weak/Missing - <why>
- Constraints: Pass/Weak/Missing - <why>
- Boundaries: Pass/Weak/Missing - <why>
- Iteration policy: Pass/Weak/Missing - <why>
- Stop and report: Pass/Weak/Missing/Waived - <why>
```

If any field does not pass, ask 1-3 pointed questions that unblock the weakest fields. After the user answers, rescore all six fields and repeat. Be politely persistent: do not accept vague answers like "make it better," "use tests," "whatever files are needed," or "stop when done" when a stronger standard is needed.

If the user says "just draft it" while one of the first five fields is Weak or Missing, ask the minimum questions needed to fill the missing fields instead of drafting from assumptions.

If only `Stop and report` is Weak or Missing and the user explicitly insists on ignoring it, mark it Waived internally and omit that heading from the final prompt. Do not waive it silently.

## Six-Aspect Standards

Each field passes only when it meets the relevant standard:

- Outcome: names the concrete end state, affected scope, and measurable or inspectable result.
- Verification surface: names the exact tests, commands, benchmarks, artifacts, reports, logs, or source materials that prove completion.
- Constraints: names what must not regress, including behavior, APIs, checks, performance thresholds, security/privacy limits, or documentation accuracy.
- Boundaries: names allowed files, repos, tools, data, credentials, external sources, and any exclusions. "Whole repo allowed" can pass only if explicit.
- Iteration policy: says how Codex should choose the next action after each attempt, such as nearest failing evidence, smallest targeted change, benchmark delta, risk reduction, or highest-value unresolved claim.
- Stop and report: says when to stop and what the blocker report must include, such as missing credentials, unavailable data, irreproducible flake, upstream defect, exhausted evidence paths, or budget reached. It may be omitted only when the user explicitly waives it.

## Draft Pattern

Always use component headings inside the prompt:

```text
/goal
Outcome: <desired end state>

Verification surface: <specific evidence>

Constraints: <what must not regress>

Boundaries: <allowed files, tools, data, and exclusions>

Iteration policy: <how to choose the next action>

Stop and report: <when to stop and what to report>
```

If the user explicitly waives `Stop and report`, omit only that heading and keep the other five headings. Keep the draft compact enough to paste, but include enough detail that another Codex thread can tell when to complete, continue, or stop. Headings are part of the prompt, not commentary.

## Output Format

When any field is Weak or Missing, output only questions:

```markdown
1. ...
2. ...
3. ...
```

When all required fields pass, output only the sectioned prompt:

```text
/goal
Outcome: ...

Verification surface: ...

Constraints: ...

Boundaries: ...

Iteration policy: ...

Stop and report: ...
```

If `Stop and report` is explicitly waived, omit that heading.

Do not include labels outside the prompt, rationale, scorecards, assumptions, caveats, setup steps, extra commands to run, or commentary in either mode.

## Examples

Weak request:

```text
Make this migration work and keep going until it is done.
```

Strong draft:

```text
/goal
Outcome: Complete the dependency migration.

Verification surface: Validate completion with the repository's migration test suite and a clean local build.

Constraints: Preserve public API behavior and keep existing lint and type checks passing.

Boundaries: Work only in migration-related packages, tests, and configuration.

Iteration policy: After each failed run, inspect the nearest failing evidence, make the smallest targeted change, and rerun the relevant check before broadening scope.

Stop and report: If required upstream fixes or credentials are unavailable, stop with a blocker report naming the missing dependency and evidence collected.
```

Weak request:

```text
Reproduce this paper as much as possible.
```

Strong draft:

```text
/goal
Outcome: Produce the strongest evidence-backed reproduction possible from the available paper, data, and local resources.

Verification surface: Validate the work with a final report mapping each headline claim to reproduced results, proxy evidence, or blockers.

Constraints: Keep uncertainty labels explicit and avoid claiming exact reproduction without matching artifacts.

Boundaries: Use only the available paper, data, local resources, and reproducible implementation paths.

Iteration policy: After each attempt, prioritize the feasible claim with the highest evidence value.

Stop and report: If exact reproduction is blocked by missing data, seeds, checkpoints, or implementation details, stop with an audit separating confirmed, partially supported, and blocked claims.
```

## Quality Checks

Before presenting the draft, verify that:

- The first five fields are Pass, and `Stop and report` is Pass or explicitly Waived.
- The outcome is specific enough to audit.
- The evidence is named, not implied.
- The constraints protect likely regressions.
- The boundaries give Codex room to investigate without making the Goal unbounded.
- The iteration policy says how to choose the next attempt.
- The `Stop and report` heading prevents endless work or overclaiming, unless the user explicitly waived it.
- The response is either only follow-up questions or only the final `/goal` prompt.
- The final prompt uses component headings.
- All six headings are present unless `Stop and report` was explicitly waived.
- The response does not execute, start, create, activate, or work on the Goal.

## References

Read `references/goals-cookbook-notes.md` when the user wants rationale, source-backed guidance, or examples for debugging, performance, migration, research, or artifact-generation Goals.
