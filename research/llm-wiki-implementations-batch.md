# llm-wiki implementations batch (11 repos)

*Opus agent report 2026-07-23, saved verbatim. Question: does any of these have an idea nashsu/Pratiyush lack?*

### cablate/llm-atomic-wiki
- **Verdict:** STEAL-PATTERNS
- **What it is:** One person's battle-tested extension of Karpathy's gist, adding an atom layer between raw and wiki (one atom = one claim), run end-to-end on 584 posts → 630 atoms → 83 pages.
- **Signal:** 144 stars, last push April, framework-only (private content), but concrete and honest with real scale numbers. Real, small, substantive.
- **Unique mechanism:** The **atom layer as immutable source-of-truth with the wiki as a derived, rebuildable cache** — "wiki is rebuildable from atoms; atoms are not rebuildable from wiki." Plus a **parallel-compile naming lock** (pre-lock the slug namespace before fanning out N agents so they fill pre-named slots instead of inventing divergent filenames for the same content).
- **Stealable for seshat:** This *is* seshat's SUBSTRATE/MACHINE-LAYER distinction stated crisply, one layer finer. Claim-level atoms make retrieval finer-grained than page-level — you retrieve the exact claim, not a whole page, and can trace a wrong wiki fact back to its atom rather than re-reading raw. The naming lock is a real fix if seshat ever compiles pages in parallel. The two-layer lint (deterministic script pass before LLM semantic pass) is also directly liftable.
- **Answer to owner's note:** n/a.

### green-dalii/obsidian-llm-wiki
- **Verdict:** STEAL-PATTERNS (retrieval-relevant — the standout of the batch)
- **What it is:** A polished Obsidian community plugin implementing the LLM-Wiki pattern with a chat-over-your-vault interface, 10 languages, 12+ providers.
- **Signal:** 342 stars, **pushed yesterday**, actively maintained, on the Obsidian marketplace (95/100 official score), v1.25.x. Real and alive.
- **Unique mechanism:** **Personalized PageRank run over the existing `[[wikilink]]` graph as the retrieval engine — zero embeddings, zero API calls, works with local models.** Plus "zero-LLM Tier B section extraction" (i18n-aware deterministic section pulling). This is the one repo in the batch whose headline feature is retrieval, not capture.
- **Stealable for seshat:** Directly attacks the owner's actual bottleneck. nashsu has a 4-signal graph relevance score; this is the principled special case — Personalized PageRank seeded on the query's entry nodes, walked over links seshat already has, needing no vector index in the MACHINE LAYER (which is exactly "derived, rebuildable, never source-of-truth"). This is the cheapest good retrieval seshat can add: an index rebuild, not an embedding pipeline. Steal the PPR-over-wikilinks approach outright.
- **Answer to owner's note:** n/a.

### shannhk/llm-wikid
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A clone-and-go Obsidian vault where `CLAUDE.md` is the whole schema; ingest pipeline resolves URLs (yt-dlp transcripts, X API, scrapling) and runs a bias check before compiling.
- **Signal:** 310 stars, last push April, clean but capture-focused. Real but not deep on retrieval.
- **Unique mechanism:** A **bias-check phase** as a first-class step in the ingest pipeline (step 6 of 8), and inbox-style auto-sorting of clippings by URL type. Minor.
- **Stealable for seshat:** The bias-check-during-ingest idea is worth a line in seshat's HARNESS — flag one-sided sources at compile time rather than trusting synthesis blindly. Otherwise this is capture plumbing seshat doesn't need.
- **Answer to owner's note:** n/a.

### NicholasSpisak/second-brain
- **Verdict:** SKIP
- **What it is:** A clean four-skill packaging (setup/ingest/query/lint) of the standard pattern, distributed via `npx skills add`, browsed in Obsidian.
- **Signal:** 640 stars, last push April, well-made but zero mechanisms beyond the canonical loop. Real repo, no new idea.
- **Unique mechanism:** None — it's the reference pattern with a nice install wizard and qmd for at-scale local search.
- **Stealable for seshat:** Nothing seshat lacks. (The `npx skills add` cross-agent distribution is a nice-to-have but irrelevant to a one-person system.)
- **Answer to owner's note:** n/a.

### atomicstrata/llm-wiki-compiler
- **Verdict:** STEAL-PATTERNS (most substantial engineering in the batch)
- **What it is:** A production-grade TypeScript npm package + MCP server + SDK that compiles sources into typed, citation-traceable pages with lifecycle profiles, eval harness, and hybrid retrieval.
- **Signal:** ~1.8k stars, **pushed July** (active), CI, hosted docs, npm-published, v1.0. Real and by far the most built-out.
- **Unique mechanism:** Two things nashsu/Pratiyush lack: (1) **hybrid retrieval that assembles a compact "context pack" per query** — semantic chunk search + BM25 rerank + wikilink graph expansion fused into a small evidence bundle for the agent, rather than dumping whole pages; (2) **Configurable Lifecycle Profiles** — typed entities/relations/lifecycle gates enforced *at the write path* (fail-closed), not as prompt conventions. Also an eval harness reporting citation coverage/precision and per-page health.
- **Stealable for seshat:** The **context-pack builder** is the single most seshat-relevant artifact in this whole batch — it's precisely a SURFACE-layer answer to "retrieval is the bottleneck": don't feed the agent pages, feed it a graph-expanded, reranked evidence pack scoped to the query. Also steal the **eval harness idea** (citation precision + per-page health score) so seshat can measure whether retrieval is actually improving instead of guessing. Write-path trust gates are worth studying but heavier than a one-person system needs.
- **Answer to owner's note:** n/a.

### ussumant/llm-wiki-compiler
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A Claude Code/Codex plugin that compiles both scattered markdown **and entire codebases** into topic-based articles, claiming ~90% context reduction, with an interactive knowledge-graph canvas.
- **Signal:** 301 stars, **pushed July** (active), Mintlify docs, v2.1. Real, actively developed.
- **Unique mechanism:** **Codebase mode** — treat a repo as a raw source and compile it into a synthesized wiki with coverage badges, so the agent reads "INDEX + 2 topic articles (~330 lines)" instead of 13+ raw files. A different ingest target than the note-taking crowd.
- **Stealable for seshat:** The before/after framing (measure tokens read per session before vs after) is a good retrieval-quality metric. Codebase-as-source is off-scope for a personal knowledge system, but the coverage-badge idea (mark how completely a topic page covers its sources) is a cheap signal seshat could show at the SURFACE.
- **Answer to owner's note:** n/a.

### Ar9av/obsidian-wiki
- **Verdict:** STEAL-PATTERNS
- **What it is:** A cross-agent "digital brain" framework (pip package + skills for Claude Code/Cursor/Codex/Gemini/etc.) pointed at an Obsidian vault, with graph query/analyse, multi-vault routing, and a trust-review gate.
- **Signal:** ~3k stars (highest in batch), **pushed last Saturday** (active), pip-installable, multi-agent. Real and popular.
- **Unique mechanism:** Two governance ideas the prior set lacks: (1) **`trust-check --strict` / `trust-record` — human-approved review fingerprints as a CI/scheduled gate**, so the wiki can distinguish "AI-written, unreviewed" from "human-signed-off" state and block on drift; (2) a **Config Resolution Protocol with `@name` request-scoped vault routing**. It also ships `graph-query`/`graph-analyse`/`ast-extract` as first-class retrieval CLIs.
- **Stealable for seshat:** The **trust fingerprint** is the genuinely new idea — a lightweight way to mark which machine-generated pages a human has actually verified, gated in CI. For a system whose owner abandoned 30 prior attempts, a "has a human ever confirmed this synthesis?" flag is more valuable than more capture. Steal that; ignore the multi-agent/multi-vault machinery (a one-person system doesn't need it).
- **Answer to owner's note:** You're right that it's Obsidian-centric and fundamentally a knowledge base — but it's *more* than "only for Obsidian": it's a provider-agnostic pip package with its own graph-retrieval and trust-governance CLIs. Your instinct that "knowledge base is one thing, that's not all this is" cuts the other way here — this one reaches past pure KB into governance, and that trust-gate is the part worth taking.

### KevinYoung-Kw/robust-llm-wiki
- **Verdict:** STEAL-PATTERNS (despite tiny star count)
- **What it is:** A **control-plane of rules for maintaining a large LLM-wiki over time**, extracted from two real production wikis (3,093 md files, 779k lines, 17,780 wikilinks). Publishes methodology/schema/runbooks, not content.
- **Signal:** Only 10 stars, last push April, bilingual docs, dated snapshot. Low visibility but unusually substantive and honest — this is a practitioner's scar-tissue document, not hype.
- **Unique mechanism:** It's the only repo framed entirely around **maintenance at scale and agent runtime drift** rather than generation: keeping wikilinks healthy, compiling new sources "without polluting old pages," making "query results trusted enough to feed later maintenance," and layering rules/checks so maintenance cost doesn't grow with corpus size. Explicitly positions itself in the Engelbart OHS/CODIAK document-centered lineage (addressable page + links as the durable unit, not the chat session).
- **Stealable for seshat:** The thesis maps exactly onto why the owner abandoned 30 attempts — the failure mode isn't capture, it's the maintenance burden and trust erosion as the corpus grows. Steal the framing that **pages+links are the long-lived unit and the HARNESS's job is to protect their integrity over time** (drift control, ingest-without-pollution, layered deterministic-then-semantic checks). Worth reading `schema/README.md` and `SPEC.md` for the maintenance rule set even though few others have.
- **Answer to owner's note:** Yes — there is something here, more than the 10 stars suggest. It's the one repo in the batch that treats long-term maintenance and trust (not generation) as the whole problem, which is precisely the axis on which your prior 30 attempts died. Skim its schema docs.

### ScrapingArt/Karpathy-LLM-Wiki-Stack
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A single long **build-ready blueprint document** ("share this with Claude Code and say set up an LLM Wiki") covering architecture, Obsidian setup, slash commands, and failure modes — a reference doc, not runnable code.
- **Signal:** 84 stars, last push April, a `status: reference` markdown guide. Real as a synthesis, but it's prose, not a tool.
- **Unique mechanism:** One opinionated design claim worth noting: **"graph architecture: tree shape over cluster shape"** — deliberately steer the wikilink graph toward hierarchy over dense clusters for navigability. Also surfaces `qmd` for search-at-scale and a "self-evolving loop" section.
- **Stealable for seshat:** The tree-over-cluster graph opinion is a concrete design lever for the MACHINE LAYER (bias link generation toward a navigable hierarchy so retrieval has clean paths). Everything else is a competent restatement of what seshat already encodes in its four-layer model.
- **Answer to owner's note:** n/a.

### NulightJens/ai-second-brain-skills
- **Verdict:** SKIP
- **What it is:** Two Claude Code skills — `llm-wiki-setup` and `wiki-self-heal` (a scheduled lint loop) — symlinked into `~/.claude/skills`.
- **Signal:** 86 stars, last push April, minimal. Clean but adds nothing past the canonical ingest/query/lint.
- **Unique mechanism:** None. "Self-heal" is the standard lint loop rebranded; the map/rooms/workspace metaphor is a re-description of Karpathy's raw→wiki.
- **Stealable for seshat:** Nothing. The one arguably-nice touch (self-heal on a schedule) seshat's project-maintainer skill already covers.
- **Answer to owner's note:** Your buzzword instinct was mostly right — "second brain / self-heal" is repackaging, not a new mechanism. It's clean and honestly credited, just not additive.

### kfchou/wiki-skills
- **Verdict:** STEAL-PATTERNS
- **What it is:** A Claude Code plugin (marketplace-installable) implementing the pattern with deterministic git-hook safety gates and a cross-model citation audit.
- **Signal:** 171 stars, **pushed July** (active), uv-based helper scripts, self-documenting `SCHEMA.md` per wiki. Real and thoughtfully engineered.
- **Unique mechanism:** Two the prior set lacks: (1) a **tracked no-LLM pre-commit hook that blocks a commit** when a staged page has an unresolved contradiction flag, missing frontmatter, a broken `[[link]]`, or a slug collision — deterministic integrity enforced at write time via `core.hooksPath`; (2) **`wiki-audit strong` — a true cross-provider adversarial citation audit** (spawns `codex`/`gemini` to verify every footnote against its source, and honestly labels a same-model fallback as the weaker signal). Plus `wiki-merge`/split for deduping overloaded slugs.
- **Stealable for seshat:** The **deterministic pre-commit gate** fits seshat perfectly — substrate is markdown+git, so a hook that refuses to commit a page with a broken wikilink or unresolved contradiction keeps the MACHINE LAYER trustworthy for free, no LLM in the loop. The **cross-model citation audit** (and its honesty about same-model = weaker) is the right way to check that a synthesized claim actually traces to a source — which is what makes retrieved answers trustworthy. Steal both.
- **Answer to owner's note:** n/a.

---

### Batch synthesis

Across all Karpathy-llm-wiki implementations now surveyed (this batch + nashsu/Pratiyush and the prior agent-memory set), only **three ideas actually matter for seshat, and all three are retrieval- or trust-shaped, not capture**: (1) **graph-native retrieval with zero embeddings** — green-dalii's Personalized-PageRank-over-wikilinks is the cheapest principled version, and atomicstrata's hybrid **context-pack builder** (graph-expand + BM25 rerank into a compact per-query evidence bundle) is the SURFACE-layer answer to "retrieval is the bottleneck"; (2) **claim-level atoms as the immutable unit** (cablate) so retrieval and provenance operate below page granularity, matching seshat's substrate/machine-layer split exactly; and (3) **trust/integrity gates** — kfchou's deterministic no-LLM pre-commit hook plus cross-model citation audit, and Ar9av's human-review fingerprints, which together address the real reason the owner abandoned 30 prior attempts (maintenance-trust erosion, not capture). Everything else in the batch is competent repackaging of ingest/query/lint. Worth actually opening: **green-dalii** (for the PPR retrieval code), **atomicstrata** (for the context-pack + eval-harness design), and **KevinYoung-Kw/robust-llm-wiki** (for its scale-maintenance rule set — the 10-star sleeper whose whole thesis is the owner's actual failure mode). The rest are README-complete.
