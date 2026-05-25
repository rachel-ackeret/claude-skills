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

### 1. Draft first, then ask only where it matters

After Rachel's opening pitch:

1. **Investigate the repo** enough to make informed guesses about relevant files, current wiring, and constraints.
2. **Fill in `scope.md` and `plan.md` with your best recommendation** — don't leave sections as `_TBD_` if you can make a defensible guess. Treat the draft as a strawman for Rachel to redirect, not a blank form for her to fill out.
3. **Air your assumptions explicitly.** At the top of the first reply, list the load-bearing assumptions you made while drafting (e.g. "I assumed the constraint is X, that we're keeping stack Y, that Z is out of scope"). Rachel can knock any of them down in one line.
4. **Then surface real decisions** as multi-choice questions using `AskUserQuestion`, batched where independent. Examples:
   - "For the design, I'd go (a) approach A. Worth considering (b) B or (c) C?"
   - "Scope assumes we only handle case X. Should we also cover (a) Y, (b) Z, (c) neither?"
5. Only fall back to open-ended questions when there's genuinely no defensible default (unclear motivation, unknown stakeholder, missing domain knowledge).

The goal: Rachel reviews and redirects, rather than answering a questionnaire.

### 2. Investigate before you draft

The draft is only useful if it's informed. Before writing the strawman, read the relevant files yourself — don't ask Rachel "which files are involved?" when you can grep. Sharper question beats lazier question: "I see `X.tsx` handles upload — keep that entry point or new flow?" beats "Which files are involved?"

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

- **Draft before asking.** Investigate the repo and write a best-guess scope/plan before turning to Rachel. No blank-form interrogations.
- **Air assumptions up front.** List the load-bearing guesses behind the draft so Rachel can knock any down in one line.
- **Prefer multi-choice over open-ended.** Use `AskUserQuestion` with concrete options when there's a real fork; reserve open questions for cases with no defensible default.
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
