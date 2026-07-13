<!-- loci: append this to the project/workspace CLAUDE.md.
     Adjust the plan-review line to the user's visual-review tool, and
     delete lines that don't apply. -->

## The compounding loop (every task)

1. **Recall first.** Before exploring code or docs for any non-trivial task,
   run `graphify query "<topic>" --budget 2000` from the workspace root.
   It searches past learnings (docs/solutions/) and the code graph in one
   budget-capped call. Only fall back to grep/Read exploration for what the
   query didn't answer.
2. **Think → plan → build** per your skill routing: brainstorming for
   features, systematic-debugging for bugs, writing-plans for multi-step
   work, and a visual review of plans before execution.
3. **Compound last.** After solving anything non-obvious, run /compound.
   Learnings live in docs/solutions/{bug,decision,gotcha,pattern}/ — check
   there before re-debugging anything.
4. **Map after compounding.** After /compound writes or updates an entry,
   re-run the docs semantic pass (extraction cache skips unchanged files;
   working dir graphify-out/targets/docs-solutions), then remerge:
   `graphify merge-graphs graphify-out/targets/*/graphify-out/graph.json --out graphify-out/graph.json`
   After code changes in a graphed repo: `graphify update <repo>` (free,
   no LLM), then the same remerge.

## Workspace facts

- graphify-out/ is machine-generated (and .claudeignore'd) — never read it
  directly; use `graphify query` / `graphify explain`.

<!-- rtk-instructions v2 -->
## RTK (token-optimized command output)

A PreToolUse hook (.claude/settings.json) auto-rewrites Bash commands
through `rtk`, compressing git/test/lint/build output 60-90% — no action
needed. If the hook is unavailable, prefix commands with `rtk` manually
(safe passthrough for unknown commands). Meta: `rtk gain` (savings stats),
`rtk discover` (missed opportunities), `rtk proxy <cmd>` (bypass filter).
<!-- /rtk-instructions -->
