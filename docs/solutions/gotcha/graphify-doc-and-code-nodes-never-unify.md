---
title: graphify never links doc nodes to code nodes — path-scoped IDs by design; a bridge must be built
date: 2026-07-22
category: gotcha
tags: [graphify, skill, extraction-spec, node-id, docs-code-bridge, graph-html, subagents]
---
**Problem:** Expected "connect docs/solutions to the code it fixes" to fall
out of graphify's one-corpus flow (its skill turns code+docs into one graph
with graph.html). It doesn't: doc entries and code render as disconnected
constellations; `explain` on a doc node shows doc-side edges only.

**Root cause:** By design. Node IDs are path-scoped —
`{full_path_stem}_{entity}` (extraction-spec.md:61) — so a doc's
`composedPath` concept (`docs_solutions_..._composedpath`) can never share
an ID with the code symbol (`src_..._composedpath`), and the skill's merge
dedupes by ID only (`if n['id'] not in seen`, skill.md ~:149). Cross
doc↔code edges appear only if one subagent chunk happens to hold both
files. Doc↔code co-retrieval in queries is label matching at seed time,
not edges.

**Fix / outcome:** A deterministic bridge is required (designed, not yet
built): match entries' backticked identifiers against code-node labels
(exact match + god-node stoplist), inject `references` edges into the
merged graph, re-run `cluster-only` for graph.html. Also learned: the
graphify SKILL (vs CLI) needs no key at all — "the host agent itself is
the LLM" via session subagents (skill.md:154), outputs graph.html +
graph.json + GRAPH_REPORT.md.

**Verification:** 2026-07-22 — read from the installed package
(`site-packages/graphify/`): skill.md:8 (three outputs), :154 (no-key
subagent pass), extraction-spec.md:61 (ID format), merge dedup code; live
`graphify explain` on a doc node in the advicerev merged graph showed
degree 3, all doc-side. Related: [[graphify-ollama-base-url-needs-v1]].
