# Graph/visual batch (10 repos)

*Opus agent report 2026-07-23, saved verbatim. Owner section header on excalidraw-animate: "this might be one of the best ones".*

Caveat on Signal from the agent: `api.github.com` and `github.com` HTML both 403 through this session's proxy, so exact star/push numbers could not be pulled. Signal inferred from README badges, dependency recency, release versions, and author reputation.

### neo4j-labs/llm-graph-builder
- **Verdict:** STEAL-PATTERNS
- **What it is:** Neo4j's reference app that turns PDFs/docs/YouTube/web into a Neo4j knowledge graph via LangChain + LLMs, with Bloom visualization and a "chat with your graph" retriever.
- **Signal:** Neo4j-Labs official, clearly active (targets Neo4j 5.23+, current LLM roster incl. Deepseek/Bedrock, token-usage tracking). Real, well-maintained, but heavyweight.
- **Unique mechanism:** Production GraphRAG *retrieval composition* — chunk→entity→community hierarchy with local/global retrievers and source-provenance metadata returned per answer. That retriever design is more mature than anything in the prior set (Graphify builds graphs but this focuses on querying them).
- **Stealable for seshat:** The retrieval query patterns only — how it fuses vector similarity + graph traversal + entity lookup and returns citations. Do NOT adopt the substrate: it makes Neo4j the source of truth, which violates seshat's "machine layer is rebuildable, never source-of-truth" rule. Steal the query shapes for seshat's MACHINE LAYER; keep markdown+git canonical.
- **Answer to owner's note:** n/a.

### neomjs/neo
- **Verdict:** SKIP
- **What it is:** neo.mjs, a real multi-threaded (worker-based) frontend framework, re-narrated as a "self-evolving software organism" with an AI-swarm/"Neural Link possession"/"Active Hybrid GraphRAG" story layered on top.
- **Signal:** The underlying framework is real and has npm traction; the README is near-pure hype ("900+ merged PRs," "inhabits live applications"). As a *knowledge system* it is off-target. Real code, hype framing, wrong domain.
- **Unique mechanism:** none that serves retrieval. The one transferable line — "agents come and go, the graph remains" — is stated more cleanly by graphwork/wg.
- **Stealable for seshat:** nothing.
- **Answer to owner's note:** n/a.

### AgriciDaniel/claude-obsidian
- **Verdict:** STEAL-PATTERNS (closest sibling to seshat in the whole batch)
- **What it is:** A Claude Code + Obsidian self-organizing second brain, explicitly built on Karpathy's LLM Wiki pattern — the exact lineage seshat descends from. 15 skills, multi-writer-safe, methodology modes (LYT/PARA/Zettelkasten).
- **Signal:** MIT, versioned releases (v1.9.2), CI badge, active. Some marketing surface (Skool "Pro community" upsell), but the core is real and shipping.
- **Unique mechanism:** Hybrid retrieval implemented for a markdown vault — contextual-prefix + BM25 + cosine rerank per Anthropic's Sept-2024 contextual-retrieval research, plus per-file advisory locking to close a multi-writer corruption hole. This is the retrieval stack seshat needs, built on the *same substrate* seshat mandates. Also ships a `/canvas` visual layer.
- **Stealable for seshat:** (1) The contextual-retrieval index (prefix+BM25+rerank) as seshat's MACHINE LAYER — directly rebuildable from markdown. (2) Per-file advisory locking pattern for safe concurrent agent writes. (3) The `/canvas` command as a SURFACE-layer precedent. This is the single best functional template for seshat's retrieval bottleneck in this batch.
- **Answer to owner's note:** n/a.

### AlmanacCode/codealmanac
- **Verdict:** STEAL-PATTERNS
- **What it is:** A living wiki for a codebase, maintained by coding agents — plain markdown in the repo, locally indexed, reviewed in Git; ships `search`/`show`/`serve` CLI and scheduled `launchd` jobs.
- **Signal:** On PyPI, Discord, YC S26, telemetry infrastructure — a funded, active product. Code-domain focused. Real.
- **Unique mechanism:** The **"Garden" job** — a scheduled background pass that reviews the whole wiki for *stale, duplicated, or poorly-connected* knowledge. That is a retrieval-*quality* maintenance loop (fights the exact rot that kills retrieval over 30 abandoned attempts), and it's domain-independent even though the product is code-centric. Sync/Garden/Update split is a clean substrate-hygiene model.
- **Stealable for seshat:** The Garden loop as a seshat HARNESS hook (periodic "find orphans / dupes / weak links" over the vault), and the git-native-markdown-with-local-index architecture (which matches seshat exactly). The `serve` local web viewer is a thin SURFACE precedent.
- **Answer to owner's note:** Correct that it's mostly inspiration since it's code-oriented — but the Garden retrieval-hygiene loop transcends code and is the one concretely stealable piece.

### dai-shi/excalidraw-animate  *(deep dive)*
- **Verdict:** STEAL-PATTERNS
- **What it is:** A tool + npm library that converts an Excalidraw scene into an animated SVG/WebM — the drawing "draws itself," with optional animated pointer.
- **Signal:** Author dai-shi is highly reputable (Jotai/Valtio/Zustand/Waku ecosystem). Actively maintained: tracks `@excalidraw/excalidraw` 0.18.1, React 19, Vite 7, Playwright e2e. Real and healthy.
- **Unique mechanism:** Clean, embeddable library API (`animateSvg`, `getBeginTimeList`, `exportToSvgFile`, `exportToWebmFile`) that works by **injecting SMIL `<animate>`/`<animateMotion>` elements into an exported SVG** — opacity fades, stroke path reveal, and pointer motion along the longest path segment, driven by per-element order/duration. Nothing in the prior set animates diagrams.
- **Stealable for seshat:** The `animateSvg` function is liftable as a SURFACE endpoint: take a generated concept-map/graph SVG and animate its construction for review/presentation ("replay how this cluster formed"). Because it consumes standard Excalidraw JSON, it chains directly onto axton's generator (below). Honest caveat: this is a *rendering* endpoint, not retrieval — it makes found knowledge delightful to present, it does not help you find it.
- **Answer to owner's note ("might be one of the best ones"):** Half-right. It is the most polished, most reputable, most reusable *library* in the batch — but it is a narrow animation renderer sitting downstream of retrieval. For seshat's actual bottleneck, tt-a1i/archify (interactive, searchable, grounded map) and AgriciDaniel/claude-obsidian (the retrieval stack) matter more. Keep excalidraw-animate as the SURFACE polish layer, not the centerpiece.

### axtonliu/axton-obsidian-visual-skills
- **Verdict:** STEAL-PATTERNS (arguably the more seshat-relevant of the two visual "best" candidates)
- **What it is:** A pack of three Claude Code skills that generate Excalidraw, Mermaid, and Obsidian Canvas (`.canvas` JSON) diagrams from plain text — installable via the Claude Code plugin marketplace, Obsidian-native.
- **Signal:** MIT, but self-labeled **Experimental** ("works for my demos… not maintaining this codebase"). Author is a content creator (upsell to a paid "Agent Skills" library). Prototype-grade, but the *pattern* is exactly seshat's stack: Obsidian + Excalidraw + Claude Code skills.
- **Unique mechanism:** A text→**JSON Canvas** skill with node-sizing/spacing/auto-edge-labeling algorithms, plus bundled Excalidraw JSON schema and Mermaid syntax-error-prevention references — i.e. a HARNESS skill that emits a visual file the owner already lives in, no new tool to store data.
- **Stealable for seshat:** The SKILL.md structure + the JSON Canvas emitter + the Excalidraw schema reference are directly liftable into seshat's HARNESS layer as a "render this retrieval result as a Canvas/Excalidraw map" skill. It is the *generation* half; excalidraw-animate is the *animation* half — they compose. For a visually-oriented owner running Obsidian+Excalidraw, this is the more load-bearing steal of the two.
- **Answer to owner's note ("best ones" section):** Between this and excalidraw-animate, this one earns the label more for seshat — it produces navigable knowledge maps in the owner's native formats, which is retrieval-adjacent, whereas animate only decorates.

### sindrel/excalidraw-converter
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A Go CLI (Homebrew-installable) that ports Excalidraw diagrams to Gliffy, draw.io, and Mermaid.
- **Signal:** Packaged, released, tidy — but narrow and one-directional (Excalidraw → other), Mermaid support limited to flowcharts with connected elements. Real but small-scope.
- **Unique mechanism:** Excalidraw → **Mermaid (text)** conversion — turning a hand-drawn visual artifact back into diffable, git-storable markdown text.
- **Stealable for seshat:** The one useful idea is the *direction*: a visual sketch → text substrate path, so an owner's Excalidraw thinking can be normalized into Mermaid inside markdown (staying in the SUBSTRATE layer, machine-rebuildable). Not worth adopting the tool; worth stealing the "visual → canonical text" reduction as a principle.
- **Answer to owner's note:** n/a.

### graphwork/wg
- **Verdict:** INSPIRATION-ONLY
- **What it is:** A Rust "work OS" for human/AI orgs: a persistent task graph stored as **plain JSONL on disk** (git-friendly), with claims, handoffs, evidence, validation, and human-judgment as first-class ops, driven from a TUI.
- **Signal:** Theory-led, real Rust code (`cargo install`), self-hosting proof (a company incorporated/grant-funded through it). Ambitious, niche. Task/work domain, not PKM.
- **Unique mechanism:** "The bottleneck is validation" reframing — durable JSONL substrate + a TUI as a thin surface that never becomes the source of truth, with agents transient over a persistent graph. Structurally the same layer discipline seshat wants.
- **Stealable for seshat:** Two patterns: (1) JSONL-on-disk-as-substrate with a TUI as a *stateless* surface — a direct model for a seshat retrieval TUI. (2) The "validation is the bottleneck" framing mirrors seshat's "retrieval, not capture" — its evidence/verification primitives suggest making *retrieval-confidence* a first-class, inspectable field. Wrong domain to adopt wholesale.
- **Answer to owner's note:** n/a.

### tt-a1i/archify  *(saved twice by owner)*
- **Verdict:** STEAL-PATTERNS (leaning ADOPT for the SURFACE layer — strongest visual-retrieval candidate in the batch)
- **What it is:** An agent skill (Claude/Codex/opencode) that turns a codebase or system description into a **polished, interactive, navigable system map** — five diagram types, typed JSON IR, deterministic validation, output as one self-contained HTML plus PNG/SVG/WebM/share-card.
- **Signal:** v2.12.0 (mature), Trendshift-featured, live gallery/"Proof Lab" with checked-in scenarios and validation receipts, real repo mapped from source at a pinned commit. Active and substantive.
- **Unique mechanism:** The diagram is a **retrieval interface, not just output** — you search nodes, inspect relationships, trace authored routes, compare semantic roles ("lens"), and play guided "stories," all with a hard grounding guarantee ("without inventing topology") backed by a typed JSON IR + deterministic checks. This is exactly "see your knowledge instead of grepping it," and nothing in the prior set does interactive, grounded, navigable visual retrieval.
- **Stealable for seshat:** (1) The **typed JSON IR + deterministic validation** so a generated map is provably faithful to the substrate (no hallucinated edges) — the anti-drift discipline seshat's abandoned attempts lacked. (2) The **single self-contained interactive HTML** as a SURFACE artifact that never stores state (matches seshat's "surface never stores"). (3) The interaction grammar — search / trace-route / role-lens / guided-story — as the vocabulary for a seshat visual retrieval surface. Because it's already an agent skill, it slots into the HARNESS layer.
- **Answer to owner's note (saved twice):** Noted — you flagged it in two sections, and that instinct is right: it's the only repo here that treats a diagram as something you *query and navigate*, not just render. This is the deep-follow-up winner (see synthesis).

### riturajjhaba938/GapGraph
- **Verdict:** DEAD
- **What it is:** Unknown — the README is a single line (`# GapGraph`) and every probed source file (package.json, index.html, main.py, app.py, src/App.tsx, requirements.txt) returns 404. No code present at HEAD.
- **Signal:** Empty stub. No content, no activity signal retrievable.
- **Unique mechanism:** none.
- **Stealable for seshat:** nothing.
- **Answer to owner's note ("(visualized?!)"):** There's nothing to visualize — the repo is an empty placeholder. Whatever you saw isn't in the public HEAD. Drop it.

---

## Batch synthesis

This batch cleanly splits into two families, and the split is the lesson for seshat's SURFACE layer. Family A (neo4j-labs/llm-graph-builder, and the hype-wrapped neomjs/neo) makes a graph *database* the source of truth — powerful retrieval, but it violates seshat's rule that the machine layer stays rebuildable and markdown+git stays canonical; steal their query patterns, never their substrate. Family B (axton's skills, excalidraw-animate, archify, the converter) treats the visual as a **disposable, generated artifact rendered from the substrate** — which is exactly seshat's "surface never stores" discipline, and the right home for "seeing your knowledge instead of grepping it." The composable SURFACE pipeline that emerges: seshat retrieval query → axton-style skill emits a Canvas/Excalidraw/Mermaid map in the owner's native tools → archify-style typed-IR + deterministic checks guarantee the map is faithful to the markdown (no invented edges) → optional excalidraw-animate replay for presentation. The single repo deserving a deep follow-up is **tt-a1i/archify**: it is the only candidate that makes the diagram itself a *navigable, searchable, grounded retrieval interface* rather than static output, and its JSON-IR-plus-validation-receipt approach is the concrete answer to why the owner's 30 prior attempts rotted — visual retrieval only helps if it's provably faithful to the substrate. (Runner-up for functional retrieval, not visual: AgriciDaniel/claude-obsidian, whose contextual-retrieval stack on a plain Obsidian vault is the closest working template for the capture-to-retrieval gap.)
