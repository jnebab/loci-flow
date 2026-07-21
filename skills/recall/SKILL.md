---
name: recall
description: Use at the start of every non-trivial task, before exploring code or docs — triggers on "/recall", "what do we know about X", "have we seen this before", or the recall step of the loci-flow loop. Runs one budget-capped knowledge-graph query over past learnings and the code map; read-only, never rebuilds anything.
license: MIT
metadata:
  author: jnebab
  version: "1.0"
---

# Recall

The read path of the loci-flow loop. One capped query, nothing else —
building and updating the graph belong to the full graphify skill.

## Workflow

1. Run from the workspace root:

   ```
   graphify query "<topic>" --budget 2000
   ```

2. Answer from the output only; cite `source_location` when quoting a
   specific fact. The graph holds past learnings (docs/solutions/) and
   the code map, cross-linked — one query surfaces both.

3. If `graphify-out/graph.json` does not exist, stop and tell the user
   to build it first with `/graphify <paths>`. Do not fall back to grep
   or file-reading exploration; do not attempt a build from this skill.

## Rules

- Read-only by construction: never run extract, `--update`,
  `--cluster-only`, or write into `graphify-out/`.
- Never load the full graphify skill for a recall — that is the
  ~10×-cost write path, needed only when the graph itself must change.
- An empty or unhelpful query result is a valid answer, not an error:
  say so and let the task proceed through its normal skills.
