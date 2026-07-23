# Capabilities batch: arcade-mcp, FastGPT, coze-loop, open-agent-hub, gelab-zero

*Opus agent report 2026-07-23, saved verbatim. Owner's hypothesis under test: "capabilities is the next phase with agents and skills".*

Metadata note: `api.github.com` is intercepted in this session (GitHub App scoping), so stars/last-push come from shields.io badge endpoints, which resolved fine. All READMEs fetched cleanly via `raw.githubusercontent.com`. No repo was DEAD.

---

### ArcadeAI/arcade-mcp
- **Verdict:** INSPIRATION-ONLY
- **What it is:** MIT Python framework for building MCP servers/tools; the OSS engine behind Arcade.dev's 7,500+ prebuilt tools across 81 servers.
- **Signal:** ~976 stars, last commit yesterday, PyPI-published, CI green. Real and actively maintained, but young.
- **Unique mechanism:** *Authorized tool calling* — a tool declares `requires_auth=GitHub(scopes=["repo"])` and the runtime injects OAuth tokens/secrets into the call context at invocation time; the LLM and client never see the secret values. 22 prebuilt provider auth classes.
- **Stealable for seshat:** Almost nothing on retrieval. The one transferable idea is the *secret-injection-at-call-time* pattern for a SURFACE-layer tool that reaches a private API (e.g. a "pull my Readwise/GitHub" ingest tool) without the token touching the model — but seshat is one-person and local, so this is over-engineered for the actual need. Retrieval bottleneck: untouched.
- **Answer to owner's note:** Your instinct was half-wrong. "Capabilities" here is just a marketing synonym for *authenticated MCP tools*. It is not a new abstraction above tools/skills — it is tools + an OAuth broker. Nothing about retrieval.

### labring/FastGPT
- **Verdict:** STEAL-PATTERNS
- **What it is:** Mature (29k-star) self-hostable "AI Agent build platform" — visual Flow orchestration, plus a genuinely deep RAG/knowledge-base engine.
- **Signal:** ~29k stars, commit yesterday, multilingual, commercial edition. Very real; the heaviest platform in this batch (Docker + Mongo + vector store).
- **Unique mechanism:** For *this* batch, its retrieval stack: **hybrid retrieval + rerank**, multi-KB reuse/mixing, per-chunk record edit/delete, an "API knowledge base" (external system as a live KB source), and QA-split import. This is the only repo here that treats retrieval as a first-class engineering problem.
- **Stealable for seshat:** The retrieval architecture, not the platform. Concretely: (1) hybrid lexical+vector retrieval fused with a rerank pass — directly applicable to seshat's MACHINE LAYER over its markdown substrate; (2) chunk-level edit/delete as a first-class operation so the derived index stays correctable without a full rebuild; (3) "API knowledge base" = treat an external source as a queryable KB rather than importing it, which fits seshat's "SURFACE never stores" principle. Do not adopt the whole thing — mine the retrieval design.
- **Answer to owner's note:** "Also here!!" — justified enthusiasm, but for the wrong reason. It's worth stealing from because of its *retrieval* engineering, not because of the word "capabilities" (which barely appears; its feature list says "能力/capabilities" only as generic Chinese for "features").

### coze-dev/coze-loop
- **Verdict:** SKIP (for seshat)
- **What it is:** ByteDance's open-source AI-agent *lifecycle ops* platform: prompt dev/versioning, evaluation, and observability/tracing.
- **Signal:** ~5.6k stars, commit last week, Go, Apache-2.0, Helm/Docker. Real and serious — but aimed at teams shipping agents to production.
- **Unique mechanism:** End-to-end *trace observability* of an agent run (prompt parse → model call → tool exec, with intermediate results and exceptions captured) plus systematic multi-dimensional eval sets.
- **Stealable for seshat:** Nothing on the retrieval bottleneck. The only faint transfer is philosophical — the idea of tracing every stage of a HARNESS run so you can debug *why a retrieval missed* — but seshat is one person, and standing up a Go/Helm observability platform to debug your own note-search is absurd. No "capabilities" concept here at all.
- **Answer to owner's note:** (no note) — included by keyword only; the word "capabilities" here is generic. Not relevant.

### guanyang/open-agent-hub
- **Verdict:** STEAL-PATTERNS
- **What it is:** Zero-dependency CLI (`oah`) that manages and *activates* "capabilities" — Skills, Agents, and Commands — by symlinking them into per-project or global config dirs across Claude Code, Cursor, Codex, Gemini, etc. Ships 83+ skills.
- **Signal:** ~935 stars, commit yesterday, MIT, npm-published. Small but active and directly in seshat's domain. (Notably, its CLAUDE.md/AGENTS.md are derived from `andrej-karpathy-skills` — which is literally one of the CLAUDE.md projects in your own environment.)
- **Unique mechanism:** It operationalizes "capability" as a *unifying primitive over Skill + Agent + Command*, activated by symlink per-scope (project vs global) and portable across every coding-agent tool via a common Markdown+YAML-frontmatter contract. This is the ONE thing the prior research set lacks: a concrete activation/linking layer for the HARNESS, not just authoring.
- **Stealable for seshat:** This maps 1:1 onto seshat's HARNESS layer. Steal: (1) the symlink-activation model — capabilities live in one git-tracked repo (substrate), get *linked* into `.claude/` per project rather than copied, so the harness stays rebuildable and never becomes source-of-truth; (2) the single Markdown+frontmatter contract shared by skills/agents/commands so retrieval tooling can index all three uniformly; (3) `oah status` / `oah enable` as the model for a thin SURFACE that activates capability without storing state. This is the most seshat-shaped repo in the batch.
- **Answer to owner's note:** (no note) — but this is the repo that actually justifies the "capabilities" keyword.

### stepfun-ai/gelab-zero
- **Verdict:** SKIP
- **What it is:** StepFun's fully open-source *mobile GUI agent* — a 4B vision model plus local inference infrastructure that drives Android phones over ADB (ReAct loops, multi-device task distribution).
- **Signal:** ~2.2k stars, last commit May 2025 (stale-ish), NeurIPS 2025 paper, HF model + AndroidDaily benchmark. Real research artifact, but a different universe from personal knowledge.
- **Unique mechanism:** Local plug-and-play GUI-agent infra (device management, trajectory recording/replay) with a consumer-hardware 4B model. Impressive, irrelevant here.
- **Stealable for seshat:** Nothing. No knowledge, no retrieval, no capability abstraction — it's phone automation.
- **Answer to owner's note:** "I don't even know if this is saved because of GUI or Capabilities" — it's neither useful. It's a mobile GUI agent; the word "capabilities" appears only as generic prose ("Specific capabilities include…"). Drop it from the seshat research set.

---

### Batch synthesis

The owner's hypothesis — that "capabilities" is *the next-phase abstraction beyond agents and skills* — does not hold up. Across all five repos the word is **umbrella vocabulary, not a new primitive**: Arcade uses it to mean authenticated tools, FastGPT and gelab-zero use it as generic Chinese/English for "features," and Coze-Loop doesn't use it meaningfully at all. The single repo that gives "capability" any real operational content is **guanyang/open-agent-hub**, and even there a capability is just a *bundle-and-activate wrapper over Skill+Agent+Command* — an aggregation and a linking mechanism, not an abstraction that sits above skills. So: not a genuine emerging abstraction, but open-agent-hub demonstrates the most useful version of the label and is worth stealing its symlink-activation HARNESS pattern from. For the actual bottleneck — retrieval — the keyword is a red herring; the only repo that moves that needle is **FastGPT**, whose hybrid-retrieval-plus-rerank and chunk-level-edit design is the real prize in this batch.
