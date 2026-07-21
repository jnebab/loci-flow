# loci-flow — project rules

## The compounding loop (every task)

1. **Recall first.** Start every non-trivial task with `/recall "<topic>"`
   — one budget-capped query over the workspace graph (past learnings +
   code, cross-linked). Read-only. If the graph is missing, /recall says
   so — build via /graphify, don't grep around it.
2. **Think → plan → build** per your skill routing: brainstorming for
   features, systematic-debugging for bugs, writing-plans for multi-step
   work, and a visual review of plans before execution.
3. **Visual when visual.** Whenever the user asks to explain, visualize,
   compare, or walk through something — or the answer is easier grasped
   as a diagram/table/comparison than prose — ask one question: interactive
   HTML, or markdown? HTML → render via lavish (`npx -y lavish-axi <file>`),
   then poll for annotations. Markdown → apply the `i-have-adhd` skill if
   installed, plus `skills/init-loci/references/markdown-output.md`
   (its rules alone carry this path if `i-have-adhd` isn't installed).
   Applies to plan reviews (step 2) and standalone explanations alike;
   don't answer in prose what's clearer drawn or structured.
4. **Compound last.** After solving anything non-obvious, run /compound.
   Entries name the repo-relative file path and backticked `symbol` they
   touch — that line is what links them to code in the graph. Learnings
   live in docs/solutions/{bug,decision,gotcha,pattern}/ — check there
   before re-debugging anything.
5. **Map when graphed files changed.** If the task changed docs/solutions/
   or skills/ (a /compound entry counts), run `/graphify --update` once
   from the workspace root before handoff — AST re-extract for code
   (free), semantic re-extract for changed docs only (cache skips the
   rest). Skip when nothing graphed changed. No merge step: one corpus,
   one graph.
6. **Handoff closes the loop.** After a task or bugfix is verified
   complete (steps 4–5 done if the session produced a learning), run
   `/handoff` to record real gate output, real commit hash, and what's
   next for the following session. Also runnable manually anytime the
   user wants to wrap up or leave a note ("wrap up", "leave a note for
   next time") — it does not require compound/map to have run first.

## Workspace facts

- **The workspace graph is LIVE** (built 2026-07-22 via the graphify
  skill's no-key subagent path; corpus = docs/solutions + skills, one
  merged graph, scan root = repo root). Recall = `/recall`; map =
  `/graphify --update`. NEVER bare `graphify extract` — its backend
  auto-detect silently grabs whatever API key is in the env. The CLI
  Ollama path (`OLLAMA_API_KEY=local
  OLLAMA_BASE_URL=http://localhost:11434/v1 … --backend ollama --model
  qwen3:4b` — the `/v1` suffix is required) exists only as a non-skill
  fallback.
- graphify-out/ is machine-generated (and deny-listed from reads in
  .claude/settings.json) — never read it directly; use `graphify query` /
  `graphify explain`.
- Visual graph map: `graphify-out/graph.html` (skill build output —
  interactive, docs + code in one map) and `graphify tree` (collapsible
  tree view) — offer either when the user asks to "see" the codebase
  structure or how things connect. No LLM involved. (The /viz command
  was removed 2026-07-22.)

<!-- rtk-instructions v2 -->
## RTK (token-optimized command output)

A PreToolUse hook (.claude/settings.local.json) auto-rewrites Bash commands
through `rtk`, compressing git/test/lint/build output 60-90% — no action
needed. If the hook is unavailable, prefix commands with `rtk` manually
(safe passthrough for unknown commands). Meta: `rtk gain` (savings stats),
`rtk discover` (missed opportunities), `rtk proxy <cmd>` (bypass filter).
<!-- /rtk-instructions -->
