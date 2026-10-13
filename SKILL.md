---
name: prd
description: |
  Turns a feature or project that's already been scoped out (ideally via a
  /grill-me session, so the open decisions are already resolved) into a
  Product Requirements Document broken into dependency-ordered, checkbox-
  tracked phases. Scopes itself to the current git worktree/branch when
  that's the whole feature, or to a docs/prds/<slug>.md file when it's one
  feature inside a shared repo. Use when asked to "write a PRD", "spec this
  out", "create a PRD for this", "break this into phases", "lay out the
  build plan", or right after a grill-me session ends and the user wants the
  agreed decisions turned into a build doc.
  Proactively suggest running /grill-me first if the feature still has open
  decisions — a PRD built on unresolved questions is just a guess with
  headers.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - Edit
  - AskUserQuestion
---

# PRD

Write the one document someone opens mid-feature and knows exactly what's
decided, why, and what's left — ordered so each phase can ship and get
checked off before the next one starts.

## Procedure

1. **Make sure there's nothing left to decide.** If this follows a
   `/grill-me` session in the same conversation, reuse its final decision
   summary directly — don't re-ask what's already settled. If no such
   session happened and the feature still has open forks (ambiguous
   requirements, unstated architecture choices), stop and run grill-me's
   procedure first: check the codebase, then ask one question at a time for
   whatever the codebase can't answer. A PRD section should never contain
   an assumption dressed up as a decision.

2. **Figure out the scope.** Run `git rev-parse --show-toplevel` and
   `git branch --show-current` (or `git worktree list` if relevant). Then:
   - If that fails because the current directory isn't a git repo at all,
     there's no shared-repo structure to collide with → treat it like the
     dedicated-worktree case below (`PRD.md` in the current directory).
   - If the current directory is a dedicated worktree, or the branch isn't
     the repo's main/default branch, the whole worktree *is* the feature →
     the PRD lives at `PRD.md` in the worktree root.
   - Otherwise (working on main/master in a shared repo, scoping out one
     feature among several) → the PRD lives at `docs/prds/<slug>.md`,
     `<slug>` being a kebab-case name for the feature. Create the directory
     if it doesn't exist.
   - If it's genuinely unclear which applies, ask once — don't guess
     silently on where the doc goes.

3. **Check for an existing PRD at that path first.** If one exists, this is
   an update, not a fresh doc: preserve every checked-off `[x]` phase
   exactly as-is, and only revise unchecked phases or append new ones.
   Never uncheck a completed phase to "clean it up."

4. **Read the codebase before writing anything.** Pull in existing
   conventions, related modules, naming patterns, and prior art the PRD
   should align with — the doc should read like it knows the codebase, not
   like it was written in a vacuum.

5. **Fill the template below.** Phases are ordered by dependency, never by
   calendar time — no "Week 1 / Week 2." A phase must be independently
   shippable and verifiable on its own; if you can't say what "done" looks
   like for a phase without referencing a later one, split it or merge it.

6. **Confirm and hand off.** After writing, give a short summary: file
   path, phase count, and what Phase 1 is. Don't paste the whole doc back
   into chat — the file is the artifact.

## Template

```markdown
# <Feature/Project Name> — PRD

> Branch: <branch> · Worktree: <path, if applicable> · Last updated: <date>

## Problem
What's broken or missing today, and for whom. One or two paragraphs, no fluff.

## Goals
- Outcome 1
- Outcome 2

## Non-goals
Explicitly out of scope — the things someone will ask "but what about...?" for.

## Success criteria
How we know this worked. Concrete and checkable, not vibes.

## Technical approach
The decisions that came out of grill-me (or this session): architecture,
data model, key tradeoffs, and *why* each one was chosen over the alternative.

## Risks / open questions
Anything still unresolved and explicitly flagged as such — never silently
folded into "Technical approach" as if it were settled.

## Phases

### Phase 1 — <name>
**Depends on:** none
**Done when:** <the single observable condition that proves this phase works>
- [ ] Step
- [ ] Step

### Phase 2 — <name>
**Depends on:** Phase 1
**Done when:** ...
- [ ] Step
```

## Rules

- Never invent a decision. If grill-me wasn't run and a fork is still open,
  ask — the same one-at-a-time rule grill-me uses — rather than filling the
  PRD with a guess.
- Phases are dependency order, not a timeline.
- One worktree → one `PRD.md`. One shared repo, many features → one
  `docs/prds/<slug>.md` each. Don't create a second file for the same
  feature; update the existing one.
- Re-running this skill on an existing PRD updates it in place. Completed
  (`[x]`) phases are never rewritten or uninverted.
