# Skills/lists batch (8 repos)

*Opus agent report 2026-07-23, saved verbatim.*

Method note: GitHub API gated for the session; star counts from shields.io/badgen, cross-checked where possible.

### Nutlope/hallmark
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A design skill (Claude Code/Cursor/Codex) by Together AI that generates UI which "refuses to look AI-generated" — picks a macrostructure, applies one of 20 themes, runs ~57 slop-test gates plus a pre-emit self-critique before returning.
- **Signal:** ~15.7k stars, last commit June, real and polished (live demo, OG art, worked examples). Genuinely high-craft, not hype — but it is SURFACE/design work, orthogonal to retrieval.
- **Unique mechanism:** a "run N quality gates + self-critique BEFORE emitting" loop baked into a skill, and a `study` verb that extracts design DNA from a URL/screenshot into a portable `design.md`.
- **Stealable for seshat:** only by analogy — the "gate-before-emit self-critique" harness pattern could wrap a retrieval-answer skill (verify citations resolve, verify claims trace to substrate before returning). The design content itself is irrelevant to a PKS.
- **Answer to owner's note:** n/a.

### ibelick/ui-skills
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A collection of UI/motion skills for design engineers, fronted by a CLI (`npx ui-skills start`) that routes the agent to the correct skill subset for a task.
- **Signal:** ~6k stars, very active (commits within the week). Real but README is near-empty; the value is in the CLI + site, not documented here.
- **Unique mechanism:** a **task-router CLI** — `start` inspects the job and dispatches to the right skill bundle (`list --category motion`, `get baseline-ui`). A dispatch layer over a skill library.
- **Stealable for seshat:** the routing idea only — seshat's harness could use a thin "which retrieval skill fits this question" dispatcher rather than one mega-skill. The actual skills (UI/motion) are off-domain.
- **Answer to owner's note:** n/a.

### sjpiper145/MakerSkillTree
- **Verdict:** SKIP
- **What it is:** Printable hexagonal "skill tree" templates for tracking progress in physical maker crafts (soldering, sewing, 3D printing). A naming collision with "agent skills."
- **Signal:** ~3.4k stars, last commit May, genuinely loved in the maker community — but has nothing to do with AI, agents, or software.
- **Unique mechanism:** none relevant.
- **Stealable for seshat:** nothing.
- **Answer to owner's note:** wrong meaning of "skill"; this batch entry is a false positive.

### skillmatic-ai/awesome-agent-skills
- **Verdict:** INSPIRATION-ONLY (worth bookmarking)
- **What it is:** A curated awesome-list structured as a phased learning path (fundamentals → use → build → benchmarks/research), including arXiv papers.
- **Signal:** ~643 stars, last commit May. Small but the best-organized list in the batch — phased, annotated, and it uniquely surfaces primary research, not just repos.
- **Unique mechanism (as a list):** curation includes the security + benchmark literature most other lists omit.
- **Stealable for seshat:** two entries matter — the prompt-injection paper "Agent Skills Enable a New Class of Realistic and Trivially Simple Prompt Injections" (arXiv 2510.26328) — directly relevant since seshat's harness will load third-party skills into a system that touches your whole vault — and SkillsBench for judging whether a skill actually works.
- **Answer to owner's note:** n/a.

### BehiSecc/awesome-claude-skills
- **Verdict:** INSPIRATION-ONLY (bookmark)
- **What it is:** A category-organized list of actual, installable Claude skills (documents, dev tools, data, research, security).
- **Signal:** ~9.8k stars, last commit June. Real, actively maintained, lightly monetized (one sponsor banner). Points at real skill repos, not just articles.
- **Unique mechanism (as a list):** points to obra/superpowers, whose sub-skills (TDD, git-worktrees, finishing-a-branch, brainstorming) are the substantive harness patterns.
- **Stealable for seshat:** the two most PKS-relevant pointers are obra/superpowers (composable workflow skills) and the anthropics/skills document set (docx/pdf/xlsx) if seshat ever ingests office files into substrate. The list itself is a discovery surface, not a mechanism.
- **Answer to owner's note:** n/a.

### ComposioHQ/awesome-claude-skills
- **Verdict:** INSPIRATION-ONLY (noisy)
- **What it is:** A very large "1000+ skills" list that doubles as a funnel for Composio's `connect-apps` plugin (Claude → 500+ SaaS actions).
- **Signal:** ~68.8k stars (implausibly high for a young awesome-list; likely campaign-driven — treat as directional), last commit May. The star count outruns the curation quality; the top of the README is a product install flow, not a resource.
- **Unique mechanism:** the `connect-apps` plugin embodies seshat's "SURFACE never stores" principle — Claude takes actions across apps without persisting state locally. But it's a hosted SaaS with an API key, the opposite of seshat's markdown+git substrate.
- **Stealable for seshat:** conceptually, the thin-action-surface model; concretely, little — you don't want a vendor SaaS in the loop of a one-person local system. Skim for skill pointers, ignore the funnel.
- **Answer to owner's note:** your "buzzword/promo" instinct applies — high stars, diluted signal.

### agentskills/agentskills
- **Verdict:** STEAL-PATTERNS (reference/bookmark)
- **What it is:** The community-governed open specification for the `SKILL.md` format and progressive disclosure (Discovery → Activation → Execution), an open-standard continuation of Anthropic's format.
- **Signal:** ~23.4k stars, last commit July (active). This is the canonical authority for the format seshat's harness is built on, not a content dump.
- **Unique mechanism:** the formal three-stage progressive-disclosure contract (name/description loads early; full instructions load on match; resources load on execution) — the exact budgeting discipline that lets a harness carry many skills cheaply.
- **Stealable for seshat:** treat this as the spec of record. seshat's retrieval skills should follow the discovery/activation split precisely so the model can hold many narrow retrieval skills (by-topic, by-project, by-time) without context bloat — which is the crux of solving retrieval-not-capture. Bookmark and track it.
- **Answer to owner's note:** n/a.

### mattpocock/skills
- **Verdict:** STEAL-PATTERNS (the batch's main find)
- **What it is:** Matt Pocock's daily-use "skills for real engineering" — small, composable, model-agnostic skills shipped as both a skills.sh copy-in and a managed Claude Code plugin, with an explicit `/setup` that wires in your issue tracker and doc location.
- **Signal:** ~182k stars (both shields and badgen agree; unusually high, flag as directional), last commit within the week, backed by a 60k-dev newsletter. Clearly real, heavily used, opinionated. Ships ADRs for its own design decisions.
- **Unique mechanism:** a coherent knowledge-work pipeline expressed as discrete skills that write durable artifacts into the repo — not a monolithic "agent."
- **Stealable for seshat (concrete):**
  - `research` — investigates against primary sources and **captures findings as a cited Markdown file in the repo, run as a background agent**. This is almost exactly seshat's substrate-write pattern; steal the "cited-markdown-as-output + background agent" shape for retrieval answers.
  - `handoff` — compacts a conversation into a handoff doc; a capture-to-substrate primitive for session continuity.
  - `wayfinder` — plans work too big for one session as a **map of investigation tickets resolved one at a time**; a retrieval-driven planning loop that fits seshat's "the pain is finding, then acting."
  - `domain-modeling` — maintains `CONTEXT.md` + ADRs inline as the model evolves; a living-knowledge-maintenance discipline for the machine layer's source docs.
  - `grilling` — relentless one-question-at-a-time interview to resolve the decision tree; directly mirrors seshat's finding-unknowns guidelines.
  - Also worth stealing: the **two-install-philosophies** model (editable copy-in vs. read-only managed plugin) as how seshat could distribute its own harness.
- **Answer to owner's note:** this is the repo in the batch that actually changes how the harness should be built — it demonstrates the "skill that writes a cited artifact into git substrate" pattern you want, not the capture-first tools you've abandoned.

---

### Batch synthesis
Bookmark three as ongoing sources: **mattpocock/skills** (the substantive find — copy the `research`/`handoff`/`wayfinder`/`grilling` patterns; it proves the "skill writes a cited markdown artifact into git" model seshat is chasing), **agentskills/agentskills** (the format spec of record — follow it so seshat can hold many narrow retrieval skills under progressive disclosure), and **skillmatic-ai/awesome-agent-skills** (small but the only list surfacing the security/benchmark literature, which matters the moment seshat loads third-party skills over your whole vault). BehiSecc and ComposioHQ are usable discovery surfaces but noisy — Composio is largely a SaaS funnel with inflated stars, and its `connect-apps` SaaS is the antithesis of a local markdown+git substrate. Hallmark and ui-skills are high-craft but design-domain, relevant only as harness-shape analogies (gate-before-emit; task-router dispatch). MakerSkillTree is a naming-collision false positive — drop it.
