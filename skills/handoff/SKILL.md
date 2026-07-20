---
name: handoff
description: Use when wrapping up a work session or writing/updating handoff notes for the next session — triggers on "/handoff", "write the handoff", "update HANDOFF.md", "hand off to the next session", "wrap up this session", "note where we are", or any request to record what changed, what's verified, and what's next. Runs the project's verification gates and captures the REAL pass/fail output and REAL commit hash from git (never remembered or guessed), then prepends a dated session block to HANDOFF.md in its existing format. Use even when the user only says "wrap up" or "leave a note for next time".
license: MIT
metadata:
  author: jnebab
  version: "1.0"
---

# Handoff

The next session is a fresh Claude that reads the handoff first and trusts it. A fabricated "all green", a from-memory hash, or a claim your own change just made false costs it a full rework cycle to discover the lie. Governing rule:

**Write only what you verified *this turn*, and mark what your changes made stale.**

## Workflow

1. **Run the gates — capture real output.** Discover the commands (project `CLAUDE.md`, then `package.json` scripts): typecheck, tests, build, any `validate`/`verify`. Record actual numbers — `test 18/18`, not "tests pass". If a gate can't run in this environment, say so and why (`verify NOT run — needs sandbox-disabled Chrome; removals are dev-only, low-risk`); an un-run gate written as green is the exact false positive that makes a handoff untrustworthy.
2. **Read git state this turn — never recall it.** `git rev-parse --short HEAD` / `git log --oneline -n 5` for hashes; `git status -sb` for branch/push/PR state; note untracked strays. If a commit you made carries an AI-attribution trailer (`Co-Authored-By: Claude`), flag it — don't quietly bless a violated rule.
3. **Match the existing HANDOFF.md format — read before writing.** Don't impose a template over an established one. Default shape: reverse-chronological session log on top (dated blocks, recency marker), stable reference sections below. Prepend a new block for this session; continue the file's own recency-marker convention (e.g. runs of `⟢` — the newest block's run just needs to be at least as long as the block below it).
4. **Write the block for a stranger:** one-line header (date · what changed · verified hash · branch/push) — changes with `file:line` refs — gate line (real N/N per gate, or "not run, because…") — **Supersedes:** whatever in the blocks below is now false. Supersedes is the most valuable line: it's what keeps an append-only log from rotting.
5. **Update orientation + open items.** Fix "Start here"/current-state to describe *now*; update open items with what's unfinished and known traps. Sync handoff notes in `CLAUDE.md` only if the project already keeps them there — don't invent a section.
6. **Report a summary, not a dump:** gate summary in a line or two plus the `HANDOFF.md` path. The file is the artifact — don't paste the block or gate logs back into the reply.
7. **If this workspace runs the loci-flow loop** and the session produced a verified learning, run `/compound` (then let it map into the graph) *before* this skill's step 4, so the handoff block can reference the new entry by name. This is an ordering note, not a dependency — `/handoff` run standalone (no compound, no loop) still completes normally.

## Greenfield: no HANDOFF.md yet

Scaffold a minimal one in the same shape, then write the first block into it:

```markdown
# HANDOFF — <project / ticket>

> For the next session. Read this first.
>
> **<recency-marker> LATEST SESSION (<date>) — <what changed>, <commit state>.**
> - <what changed, with file:line>
> - **Green:** <gate N/N> · <gate> · <gate not run — why>
> - **Commit:** <verified short hash> — <pushed? PR?>
> - **Supersedes:** <what below is now stale — or "n/a, first session">

---

## Start here (30-second orientation)
- **What:** <one line>
- **Where:** <repo>, branch <branch>
- **State:** <current state + gate status>
- **If you change code,** re-run the gates in Run & verify before claiming done.

## Locked decisions (do not relitigate)
## Key files
## Run & verify
## Open items
```

## Not this skill's job

- **No commits.** Recording commit facts ≠ making commits. If the user wants `HANDOFF.md` itself committed and it isn't gitignored, ask first.
- **No relitigating decisions, refactoring, or "improving" the format** while passing through — record state faithfully.
