# Engine/tooling batch: one-skill, agent-toolkit, outlines, serena, synto, speq, autology, SkillOpt

*Opus agent report 2026-07-23, saved verbatim. Owner's note on outlines: "THIS for scheman/templates??"*

Method note from the agent: GitHub's API and HTML were both blocked by the session proxy (403; only `raw.githubusercontent.com` worked), so no live star/push metadata. Signal judged from README evidence (badges, download counts, self-reported predecessor stars, papers) and prior knowledge of the well-known ones.

---

Framing reminder: seshat's bottleneck is **retrieval**, not capture. A repo earns a high verdict only if it changes how the *machine layer* (rebuildable indexes) or *harness* (skills/tools that navigate) surfaces the right note at the right time. Capture-side cleverness is discounted.

### rebelytics/one-skill-to-rule-them-all (owner has a fork)
- **Verdict:** STEAL-PATTERNS
- **What it is:** A "task-observer" meta-skill that runs alongside every session, watches for corrections/gaps/patterns, and emits a structured observation log proposing new skills and edits to existing ones (including itself). Review-gated; never auto-edits.
- **Signal:** README-only skill bundle (SKILL.md + 3 `references/` files), self-reported "900 improvements across 50 skills in 6 months." No code, no stars visible (API blocked). Marketing-flavored but the mechanism is coherent and genuinely a harness pattern, not hype. Owner's fork = confirmed interest.
- **Unique mechanism:** a *cross-cutting principles log* — observations that aren't specific to one skill get promoted to a separate file, and every new/edited skill is checked against it. That's a self-improving quality floor for the harness, distinct from anything in the prior set (Hindsight revises beliefs about *content*; this revises the *harness itself*).
- **Stealable for seshat:** the observation-log-as-derived-artifact pattern maps cleanly onto the machine layer. Run a passive observer over sessions that logs "retrieval failed here / the right note existed but wasn't surfaced" events into `machine/observations.jsonl`, and periodically promote recurring retrieval failures into skill edits or new index types. This is exactly the feedback loop seshat needs to attack retrieval — capture *retrieval misses* as first-class signal. Keep it review-gated, keep the log rebuildable/disposable (fits the machine-layer "never source-of-truth" rule).
- **Answer to owner's note:** the fork is justified, but adopt the *loop*, not the skill wholesale. Its self-reported ROI only compounds at large skill-library scale; for a one-person system, wire the observer specifically to retrieval failures rather than generic "corrections," or it's overhead.

### softaworks/agent-toolkit
- **Verdict:** SKIP
- **What it is:** An installable marketplace of ~30 opinionated Claude Code skills (codex/gemini/perplexity wrappers, diagram generators, doc handoff, planning, refactor helpers) in the Agent Skills format.
- **Signal:** Real, maintained, npx-installable plugin marketplace with clean structure. But it's a general dev-productivity grab-bag — nothing about knowledge retrieval or a knowledge substrate.
- **Unique mechanism:** none relevant to seshat (it's breadth, not a retrieval idea).
- **Stealable for seshat:** nothing for the core problem. At most, `session-handoff` and `lesson-learned` are minor capture-side conveniences you already have better versions of in the llmwiki/self-learn skills. Don't spend follow-up here.
- **Answer to owner's note:** n/a.

### dottxt-ai/outlines — owner: "THIS for scheman/templates??"
- **Verdict:** INSPIRATION-ONLY (for seshat's substrate)
- **What it is:** A library for *structured generation* — it constrains an LLM's token sampling (FSM/regex/grammar/JSON-schema masking) so output is guaranteed to match a Pydantic model or grammar *at generation time*. Trusted by NVIDIA/vLLM/HuggingFace; very widely used and real.
- **Signal:** Large, active, production-grade, PyPI-published, corporate backing. High-quality software.
- **Unique mechanism:** logit-level constraint during decoding — the model *cannot* emit invalid structure.
- **Stealable for seshat:** almost nothing, because of *where* the value lives. Outlines needs control of the decoding loop (local model / vLLM / a provider's structured-output endpoint). Seshat runs on hosted Claude via Claude Code, where you don't control logits — so outlines' core mechanism isn't even reachable. More fundamentally, outlines constrains **generation you author**; it does not read, validate, or diff the **markdown notes that already exist on disk**. Your substrate is human-and-Claude-authored files, not a generation stream.
- **Answer to owner's note (the direct comparison):** For "scheman/templates" over a markdown+git substrate, **Basic Memory's Picoschema (schema_infer / validate / diff) is the right fit, not outlines** — and it isn't close. The unit of work in seshat is *an existing note file*: you want to infer a schema from the notes you already have, validate a note against it, and diff when a note drifts. That's a file-in → report-out operation, which is exactly Picoschema's shape and exactly what a git substrate needs (validation runs in a pre-commit hook or a machine-layer rebuild). Outlines only helps at the instant Claude is *emitting* a structured block (e.g. frontmatter), and even that requires decoding control you don't have. If you want typed frontmatter, define one Pydantic/JSON-schema contract and enforce it with a **validator over the files** (Picoschema-style), which also gives you the diff/drift signal that feeds retrieval. Outlines is worth reading for the "schema is a hard contract, not a suggestion" philosophy (same insight speq/speq below bets on) — hence inspiration — but do not adopt it as the schema layer.

### oraios/serena (special case: does symbol-level retrieval transfer to notes?)
- **Verdict:** STEAL-PATTERNS — strongest retrieval idea in the batch
- **What it is:** An LSP-backed coding-agent toolkit exposed over MCP that gives agents *semantic, symbol-level* code retrieval and editing — `find_symbol`, `find_referencing_symbols`, overview-then-drill navigation — instead of grep and line numbers. Very popular, active, MIT.
- **Signal:** Real and heavily validated (documented head-to-head agent evaluations; widely adopted MCP server). The most engineering-serious repo here alongside outlines/SkillOpt.
- **Unique mechanism:** retrieve by *structure and references*, not by string match or embedding similarity — and return **overviews (symbol names/signatures) before bodies**, so the agent pays for full content only after it has located the right symbol.
- **Stealable for seshat — and it transfers well:** notes have a symbol graph already. Headings/blocks are symbols; `[[wikilinks]]` are references; frontmatter fields are typed attributes. The pattern to steal, almost verbatim, into the machine layer + harness:
  - `get_note_overview(path)` → returns frontmatter + heading tree + outbound links **only**, never the body. This is the single highest-leverage retrieval tool for a one-person KB: it lets Claude triage 40 candidate notes for the cost of reading one.
  - `find_backlinks(note|heading)` → who references this — the reference-lookup that grep does badly and embeddings can't do at all.
  - `find_note_symbol(query)` → jump to a heading/block by name across the vault.
  - overview-then-drill as the *default* retrieval discipline in the query skill: navigate the index, read bodies last.
  This directly attacks the retrieval bottleneck and respects the layering — the symbol/backlink index is derived and rebuildable, the surface tools never store. This is the repo I'd deep-follow.
- **Answer to owner's note:** yes, the pattern transfers — but not the LSP. There's no language server for prose; you synthesize the "note LSP" yourself from markdown structure (headings, wikilinks, frontmatter). The transferable core is the *interaction contract* (overview before body, retrieve by reference not by match), which is precisely what seshat is missing.

### kytmanov/synto
- **Verdict:** INSPIRATION-ONLY (adjacent prior art; borrow one idea)
- **What it is:** A local-LLM pipeline that turns raw markdown notes into a self-improving, interlinked wiki — one article *per concept*, deduped so the same concept from many notes merges into one page. Explicitly a Karpathy-LLM-Wiki implementation; ships an agent-ready pack (INDEX.json, AGENTS.md, CLAUDE.md) readable over MCP. Successor to obsidian-llm-wiki-local (self-reported 608 stars, 9k+ downloads).
- **Signal:** Real, PyPI-published, CI, credible lineage. But it's essentially a polished sibling of the nashsu/Pratiyush llm-wiki work the owner already researched — same compile-notes-into-wiki thesis.
- **Unique mechanism:** **two-tier model split** (fast 4B for concept extraction, heavy 14B+ for article writing) and **concept-level dedup/merge** (one article per concept, fed by all relevant notes) — explicitly *without* embeddings or a vector DB.
- **Stealable for seshat:** two things. (1) The concept-merge invariant — "one article per concept, duplicates merge not multiply" — is a retrieval win: fewer, denser canonical pages beat a sprawl of near-duplicate notes, so retrieval lands on the right single page. (2) The no-vector-DB stance validates seshat's substrate bet: a structured, cross-linked machine layer can answer queries without embeddings. The tiered-model idea is less relevant since seshat rides one hosted model.
- **Answer to owner's note:** n/a. (Note it's the same category as prior llm-wiki research — don't re-litigate the whole approach, just lift the concept-merge rule.)

### speq-ai/speq
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A `.speq` file format — a parseable, grammar-backed "architectural contract" for a codebase that every AI session reads before writing code, to eliminate cross-session drift. Ships a companion SKILL.md behavioral contract.
- **Signal:** Early-stage (spec v0.2, self-described "early development"), MIT, single-author energy. More manifesto than proven system.
- **Unique mechanism:** the argument that natural-language context files (AGENTS.md/CLAUDE.md) *drift because NL isn't opinionated*, so the source of truth must have a **grammar + parser + validator**.
- **Stealable for seshat:** the thesis, not the format. Seshat's harness rules (CLAUDE.md schema) are natural language and will drift the same way. Worth stealing: make the *contracts that govern the machine layer* (frontmatter schema, index format, link conventions) machine-validated rather than prose — same instinct as the Picoschema answer above. It converges with outlines on "schema as hard contract."
- **Answer to owner's note:** n/a.

### Curt-Park/autology
- **Verdict:** STEAL-PATTERNS
- **What it is:** A Claude Code plugin for a team "living ontology": Obsidian-compatible markdown in `docs/`, typed nodes + `[[wikilinks]]`, with skills for triage / capture / **explore-knowledge (graph traversal)** / sync (doc-code drift repair), wired to a SessionStart hook.
- **Signal:** Real, installable plugin with a coherent skill set and an interactive tutorial. Team-oriented (its whole pitch is committing knowledge to git vs. per-dev private memory), which is a slight mismatch for one-person seshat, but the mechanics are single-user-friendly.
- **Unique mechanism:** **`explore-knowledge` graph operations** — overview (hubs/orphans/connected components), 2-hop neighborhood ("blast radius before refactoring"), and **shortest-path between two concepts**. Graph traversal as a first-class retrieval tool, "operations that grep alone cannot do." Plus `sync-knowledge` that detects and repairs doc↔code drift in place.
- **Stealable for seshat:** the graph-traversal retrieval verbs are directly transplantable to the machine layer's `[[wikilink]]` graph you already build in llmwiki: `overview` (find hub notes and orphans — orphans are retrieval dead zones), `neighborhood <note>` (2-hop expansion to pull in context you'd otherwise miss), and `path A B` (how are these two concepts connected). These are precisely the retrieval moves grep and embeddings both fail at, and they complement serena's symbol/backlink tools — serena gets you *to* a node, autology's verbs let you *reason over the graph between nodes*. Also steal the SessionStart-hook-injects-workflow pattern to make retrieval-first behavior automatic rather than opt-in.
- **Answer to owner's note:** n/a.

### microsoft/SkillOpt (owner saved it twice)
- **Verdict:** STEAL-PATTERNS (harness quality, not retrieval)
- **What it is:** Treats a skill document as the *trainable state* of a frozen model and "trains" it like a neural net — a separate optimizer model turns scored rollouts into bounded add/delete/replace edits, accepting an edit only if it strictly improves a held-out validation score. Adds zero inference-time cost (the deployed artifact is a compact `best_skill.md`). Ships SkillOpt-Sleep, a nightly offline harvest→mine→replay→consolidate engine.
- **Signal:** Strong and real — Microsoft, arXiv paper (claims best/tied-best on all 52 model×benchmark×harness cells, +19–25pt lifts), PyPI, Trendshift badges, Claude Code/Codex integration shells. The most rigorously validated repo in the batch.
- **Unique mechanism:** **validation-gated skill editing** — the held-out gate that only accepts a skill edit if it measurably improves performance. This is the disciplined, quantitative sibling of one-skill-to-rule-them-all's review-gated observation loop.
- **Stealable for seshat:** the *validation gate* is the missing rigor in both self-improvement candidates here. To attack retrieval, build a small held-out set of retrieval tasks ("which note answers X?") and gate every change to the query skill / index format against it — accept an edit only if retrieval accuracy improves. That turns "the observer suggested an edit" (rebelytics) into "the edit is *proven* to help before it ships." The SkillOpt-Sleep nightly consolidation loop also maps onto seshat's machine-layer rebuild cadence: harvest sessions → mine retrieval failures → replay → consolidate behind the gate.
- **Answer to owner's note (saved twice, under two headers):** not a dedup error worth dismissing — it legitimately sits in two buckets: it's both a *self-evolving harness* tool and an *offline batch/"brain" engine* (Sleep). Keep it under both; the retrieval-relevant half is the validation gate, the harness half is the training loop.

---

## Batch synthesis

This batch's real gift to seshat is the **retrieval-navigation layer**, and it's assemblable from three repos: steal **serena's** overview-before-body + backlink/symbol contract, **autology's** graph-traversal verbs (overview/neighborhood/path over the wikilink graph), and gate every harness change with **SkillOpt's** held-out validation discipline — together they turn seshat's machine layer from a grep target into a navigable graph, which is exactly the bottleneck. The self-improvement pair (rebelytics + SkillOpt) says the same thing at two rigor levels: capture *retrieval misses* as first-class signal (rebelytics) but only ship skill edits that *provably* improve retrieval (SkillOpt). Synto/speq/outlines are confirmation, not new direction — they collectively vindicate the markdown+git+no-vector-DB substrate and the "schema as a hard, validated contract" stance (which also settles the outlines question in Picoschema's favor). **Deep-follow serena**: designing a "note LSP" — headings/blocks as symbols, wikilinks as references, overview-then-drill as the default retrieval discipline — is the single highest-leverage thing seshat can build against the retrieval problem, and it's the one pattern here nothing in the prior research set already covers.
