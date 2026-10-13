# claude-prd-skill

A [Claude Code](https://claude.com/claude-code) skill that turns an already-scoped
feature (ideally after a `/grill-me` session resolves the open decisions) into a
Product Requirements Document broken into dependency-ordered, checkbox-tracked
phases.

## Install

Drop `SKILL.md` into `~/.claude/skills/prd/` (personal) or
`.claude/skills/prd/` (project-scoped repo skill).

## What it does

- Refuses to fill in unresolved decisions with guesses — if a fork is still
  open, it asks (same one-at-a-time rule as grill-me) instead of writing a
  PRD on top of an assumption.
- Detects scope automatically: a dedicated feature branch/worktree (or no git
  repo at all) gets a `PRD.md` at its root; a shared repo on its main branch
  gets `docs/prds/<slug>.md` per feature.
- Phases are ordered by dependency, not calendar time, and each has a
  concrete "done when" condition plus a markdown checkbox list.
- Re-running on an existing PRD updates it in place and never unchecks a
  completed phase.

See `SKILL.md` for the full procedure and template.
