---
name: shape
description: Collaboratively shape a rough idea into a scope.md + plan.md ready for the PR workflow. Ask one focused question at a time, write files to .claude/plans/<slug>/ from the start so Rachel can edit directly, and hand off to /pr-workflow when the plan is ready. Invoke when Rachel says /shape or "let's shape this", "help me plan X", etc.
---

# Shape

A pre-implementation skill. Turns "I have an idea" into "I have a scope.md and plan.md ready to PR." Pairs with [[pr-workflow]] — `/shape` produces the docs, `/pr-workflow` cuts the branch and opens the draft PR.

## How it works

### 0. Pick a slug

Right away, propose a short kebab-case slug for the work (e.g. `bulk-pdf-upload`, `fix-sitemap-crawl`). Confirm with Rachel in one line. Then create the directory:

```
.claude/plans/<slug>/
  scope.md
  plan.md
```

Both files exist on disk **from the first turn** — start with stubs (just the section headers from the templates in [[pr-workflow]]) and fill them in as the conversation progresses. Rachel can open and edit them directly at any time.

### 1. One question at a time

Drive the conversation by asking **one focused question per turn**. No batches, no multi-part questions. After each answer, update the relevant file on disk and ask the next.

Rough order — but follow the conversation, don't force it:

1. **What problem are we solving?** (1-2 sentences for `scope.md` → Problem)
2. **Why does it matter / what triggered this?** (motivation → Why it matters)
3. **What are the constraints?** (deadlines, must-not-breaks, stack lock-ins → Constraints)
4. **What's explicitly out of scope?** (Out of scope — prevents scope creep)
5. **What files/systems are involved?** (Relevant files & systems — investigate the repo yourself first, then confirm with Rachel)
6. **Are there design tradeoffs to weigh?** (Design considerations — present options, not a single recommendation, when there's a real choice)
7. **What's the first concrete step?** (start populating `plan.md`)
8. **What's the next step? …and the next?** (build out the checklist incrementally)
9. **How will we verify this works?** (Verification section of `plan.md`)

If Rachel already answered something in her opening pitch, skip that question — don't ask what you already know.

### 2. Investigate as you go

When a question depends on the codebase (relevant files, what currently exists, how something is wired), **do the investigation yourself** before asking. Then ask a sharper question: "I see `X.tsx` handles upload — is that the right entry point, or is there a new flow you want?" beats "Which files are involved?"

### 3. Keep the files clean

- Edit `scope.md` and `plan.md` after each meaningful answer — don't batch updates.
- Use the templates from [[pr-workflow]]'s "File Templates" section.
- If a section has no content yet, leave the header with a `_TBD_` placeholder so the structure is visible.

### 4. Know when to stop

The shaping phase is done when:
- `scope.md` has Problem, Why, Constraints, Out of scope all filled in
- `plan.md` has a concrete, ordered checklist with a Verification section
- Rachel says something like "yeah this looks good" / "let's do it"

At that point, **suggest the handoff**:

> *"Scope and plan look ready. Want me to kick off `/pr-workflow` to cut the branch and open the draft PR?"*

Don't auto-invoke `/pr-workflow` — Rachel triggers it.

## Hard rules

- **One question at a time.** No question batches, no multi-part questions.
- **Files on disk from turn 1.** Don't keep drafts in conversation only.
- **Don't write code.** Shaping is about scope and plan, not implementation. If Rachel starts asking you to implement, remind her that `/pr-workflow` is the next phase.
- **Don't cut a branch or open a PR.** That's `/pr-workflow`'s job.
- **Don't present short-term fixes alongside the real plan.** Per Rachel's standing preference, durable options only.

## When to suggest invoking `/shape`

Proactively offer `/shape` when:
- Rachel describes a problem or idea but hasn't committed to building yet
- The work is clearly going to need planning (multiple files, design choices, unclear scope)
- Rachel says "I'm thinking about…" or "what if we…" or "I want to figure out…"

Phrase as a nudge:

> *"Want to `/shape` this before we commit to building? We can get the scope and plan down first."*

## When NOT to use `/shape`

- Pure questions / explanations
- Trivial fixes where the scope is already obvious
- Cases where Rachel already knows exactly what she wants and just wants it built — go straight to `/pr-workflow`
