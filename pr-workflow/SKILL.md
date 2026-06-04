---
name: pr-workflow
description: Rachel's default PR-based workflow. Cut a branch, write scope.md + plan.md, push as a draft PR, iterate on the plan until approved, then push the implementation to the same PR. User reviews and merges. Invoke explicitly via /pr-workflow (or when Rachel says "let's do this PR-style", "use the workflow", etc).
---

# PR Workflow

Rachel's default way of working on non-trivial tasks. **Only run when explicitly invoked** — don't auto-apply to small fixes or questions.

## Phases

### Phase 1 — Scope & Plan (before any implementation)

1. **Cut a branch** from `main` with a short kebab-case slug describing the task (e.g. `add-pdf-bulk-upload`, `fix-sitemap-crawl`).
2. **Create two files** under `.claude/plans/<slug>/`:
   - **`scope.md`** — the *context*. Problem statement, why it matters, constraints, relevant files/systems, design considerations, anything Rachel would need to load the problem into her head. Not action items.
   - **`plan.md`** — the *action items*. Concrete checklist of what will be done, in order. Each item should be specific enough that "done" is unambiguous.
3. **Push the branch** and **open a DRAFT PR** with these two files as the only commit. PR title = short summary; PR body = link to the two files (or paste their contents — Rachel's preference per PR, ask if unclear).
4. **Stop and wait** for Rachel to review.

### Phase 2 — Revision

- Rachel will leave feedback (in chat, PR comments, or by editing the files herself).
- Update `scope.md` / `plan.md`, push revisions to the same branch.
- Loop until Rachel approves the plan.

### Phase 3 — Implementation

- Work through `plan.md` in order. Tick items as they're done (edit the file, push).
- Push implementation commits to the same PR. Keep the PR in draft until the plan is fully executed.
- When everything in `plan.md` is checked off, mark the PR ready for review.
- Highlight to Rachel any top items that she should pay the closest attention to for review. (i.e. lower confidence decisions, assumptions, risky decisions)

### Phase 4 — Cleanup

- Ask for Rachel's verbal approval that the main work is confidently done to move forward with cleanup.
- Markdown Docs:
  - **`plan.md`** — delete it.
  - **`scope.md`** — promote to `docs/architecture/<slug>.md` summarized into a clear architecture document (remove unimportant microscopic details). 
- Delete any debug statements that will bloat the logs without clear purpose shall be removed.
- Delete any useless files or now unused code added during this session.
- Another pass to check for duplicative code that could confidently be made DRY.
- Any new bugs or issues found while working on this need to be added to a new file in /issues, documenting what you found.

## Hard Rules

- **Never commit or push without Rachel's say-so on the implementation phase** — but Phase 1 (scope + plan branch + draft PR) is part of the workflow itself; opening the draft PR with just the planning files IS the trigger Rachel wants. If unsure, ask.
- **Never merge.** Rachel approves and merges.
- **One PR per task.** Don't open a second PR for "the real work" — implementation goes on the same branch.
- **Draft until plan is executed.** Don't mark ready for review with unchecked items in `plan.md`.

## File Templates

### `scope.md`

```markdown
# Scope: <task title>

## Problem
<what we're solving, in 2-4 sentences>

## Why it matters
<motivation — user impact, deadline, dependency, etc>

## Constraints
- <hard constraints: must not break X, must ship by Y, must stay on stack Z>

## Relevant files & systems
- `path/to/file.ts` — <why it matters here>
- <external system / dashboard / docs link>

## Design considerations
<tradeoffs, options considered, anything Rachel should weigh in on>

## Out of scope
<explicit non-goals — things we are NOT doing in this PR>
```

### `plan.md`

```markdown
# Plan: <task title>

- [ ] Step 1 — <specific, verifiable>
- [ ] Step 2 — ...
- [ ] Step 3 — ...

## Verification
- [ ] <how we'll confirm this works — test, manual check, Playwright run, etc>
```

## When to suggest invoking (without auto-starting)

Don't auto-start — but **do proactively suggest** `/pr-workflow` when the conversation crosses from exploring into building. Signals:

- Rachel has stopped asking "should we?" and started asking "how do we?"
- The work clearly spans multiple files or has real architectural choices
- Rachel says something like "okay let's do it", "let's build this", "yeah do that"
- A bug investigation has landed on a fix that's more than a one-liner
- Rachel starts describing implementation details as if the decision to build is made

When you see those signals, say something brief like:

> *"This feels ready for `/pr-workflow` — want me to kick it off?"*

It's a nudge, not an auto-start. Rachel still decides. If she says no or ignores it, drop it — don't re-suggest the same task.

## When NOT to use this workflow

- Pure questions / explanations (no code change)
- Trivial one-line fixes Rachel asked for directly
- Anything Rachel explicitly says "just do it" / "skip the workflow" on
