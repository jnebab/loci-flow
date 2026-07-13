---
description: Generate and open graphify's interactive graph visualization for a graphed target
argument-hint: [target-name] [--relabel]
---

Open graphify's interactive community-graph visualization (graph.html) for a
graphed target in this workspace.

Target argument: $ARGUMENTS

## Steps

1. **Resolve the target.** Targets live under `graphify-out/targets/<name>/`.
   If the argument above is empty or doesn't match, run
   `ls graphify-out/targets/` and ask the user to pick one
   (use AskUserQuestion with the available names).
2. **Check size.** Read node count via
   `python3 -c "import json;g=json.load(open('graphify-out/targets/<name>/graphify-out/graph.json'));print(len(g['nodes']))"`.
   If >5000 nodes, warn that the browser rendering will be sluggish and
   offer `graphify tree` (collapsible tree, lighter) as the alternative.
3. **Generate if needed.** If `graphify-out/targets/<name>/graphify-out/graph.html`
   is missing, or older than the target's `graph.json`, or `--relabel` was
   passed:
   - With named communities (costs LLM calls via the claude CLI):
     `graphify label graphify-out/targets/<name> --backend=claude-cli`
   - Free but placeholder names ("Community N"):
     `graphify cluster-only graphify-out/targets/<name> --no-label`
   Default to the labeled version if the graph has never been labeled;
   reuse existing labels otherwise (plain `cluster-only` keeps them).
4. **Open it:** `open graphify-out/targets/<name>/graphify-out/graph.html`
   (macOS; `start` on Windows, `xdg-open` on Linux).
5. Report in one line which target opened and how many communities it has.
