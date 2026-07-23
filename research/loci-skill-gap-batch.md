# LOCI batch: autopreso, skill-gap cluster, meetily

*Opus agent report 2026-07-23, saved verbatim. For the separate LOCI project (workshop engine), secondarily seshat.*

Method note: `api.github.com` and `github.com` HTML are firewalled in this session (repository-scoped). READMEs pulled from `raw.githubusercontent.com`, star/recency from shields.io badge SVGs. Star counts exact; "last commit" month-granular.

---

### kunchenguid/autopreso
- **Verdict:** INSPIRATION-ONLY (leaning STEAL-PATTERNS for the realtime layer)
- **What it is:** A local Node CLI (`npx autopreso`) that runs a live Excalidraw canvas plus a listening agent — you talk, transcripts stream to a model, and the model draws/labels/rearranges the whiteboard in real time. "Let the whiteboard whiteboard itself."
- **Signal:** 409 stars, last commit June 2026, published to npm, has CI + release-please + Discord. Alpha but genuinely real and actively built by a solo dev. This is the highest-quality "realtime presentation" reference in the batch.
- **Unique mechanism:** The **staging→live handoff loop with a warmup prime**. In "staging" you sketch seed elements client-side; on "start" the canvas is handed to the agent, OpenAI Realtime transcription is *biased toward the staging text and labels* (domain vocabulary priming), and a warmup loop primes the agent against your staging content so the first spoken sentence doesn't hit a cold model. The pipeline is mic (24kHz) → STT (Moonshine local / OpenAI WS) → whiteboard agent (tool calls) → live Excalidraw scene.
- **Stealable for LOCI:** This is close to LOCI's core. Steal concretely: (1) the audio→STT→tool-calling-agent→live-canvas architecture using the **Vercel AI SDK tool loop** for provider-agnostic agents; (2) **transcription bias toward on-screen/staging text** so a facilitator's domain terms transcribe correctly — directly applicable to workshop facilitation where jargon matters; (3) the **warmup/priming loop** to kill cold-start latency at "session start"; (4) the **live session cost card** (token + Realtime-audio cost estimate that resets per session) — a nice operator affordance for a paid workshop tool; (5) BYO-model with graceful local fallback (Moonshine + Ollama for fully offline). Excalidraw as the mutable scene model is itself a stealable choice for "presentations generated on the fly."
- **Answer to owner's note:** Your instinct is right — this is the strongest LOCI seed in the batch and worth a deep read of its source, not just the README. It solves the exact realtime-facilitation problem. Caveat: macOS-first (Moonshine sidecar is darwin-only; other platforms need OpenAI Realtime), and it's whiteboard-centric rather than slide-centric, so the "generate slides on the fly" ambition needs a different renderer — but the realtime plumbing transfers wholesale.

### Vanshika26Ramjas/SkillGap_AI
- **Verdict:** SKIP
- **What it is:** A Python/Streamlit + Power BI portfolio project that scores "career match" and detects skill gaps from job-market data.
- **Signal:** 1 star, last commit June 2026, badge marked "Status: Completed." Marketing-heavy README (emoji, badges, executive-summary framing) around a small student/portfolio codebase. More pitch than product.
- **Unique mechanism:** None novel — resume vs. market-demand comparison with an "AI scoring methodology" that the README describes but doesn't substantiate with real code.
- **Stealable for LOCI/seshat:** Nothing engineering-grade. At most, the *framing* of skill-gap output (career-match %, gap detection, market intelligence) as a conceptual checklist for what a gap report should contain.
- **Answer to owner's note:** The idea is sound and worth pursuing, but this repo isn't the vehicle — it's a completed portfolio piece, not a reusable engine.

### Megamaruthu5/Opportunity-Gap-Analyzer
- **Verdict:** SKIP
- **What it is:** Streamlit tool that ingests a resume + GitHub activity + a target role and diagnoses "career readiness" (missing skills, learning gaps).
- **Signal:** 1 star, last commit February 2026, stale. README is well-structured but the substance is a feature wishlist (resume extraction, GitHub analysis, gap scoring, Plotly dashboard). Prototype/student tier.
- **Unique mechanism:** The one differentiating idea vs. the others: **triangulate stated skills against actual GitHub contribution activity** ("compare real efforts with industry expectations") rather than trusting the resume alone.
- **Stealable for LOCI/seshat:** Just that concept — using real artifact/behavior signal (commits, repos) as ground truth against self-reported skills. Useful design principle for LOCI's participant capability analysis; no code worth lifting.
- **Answer to owner's note (typo):** The URL typo resolved fine — the repo exists as `Megamaruthu5/Opportunity-Gap-Analyzer` ("AI Opportunity Gap Analyzer"). Same verdict as SkillGap_AI: good idea, thin implementation, but its "verify skills against real GitHub behavior" angle is the one keeper concept in this sub-cluster.

### Gitesh-Nimje/SKILL-BRIDGE
- **Verdict:** SKIP
- **What it is:** A 5-person student team project: skill-gap analysis + personalized career roadmap + progress dashboard (HTML/CSS/JS front, Node/Flask back, Mongo/JSON).
- **Signal:** 4 stars, last commit February 2026, stale. Minimal README (feature bullets, team roster, "future scope"). Coursework-tier; resume analyzer and AI recommendation are explicitly listed as *future* scope, i.e. not built.
- **Unique mechanism:** None.
- **Stealable for LOCI/seshat:** Nothing.
- **Answer to owner's note:** n/a.

### LakshayD02/TalentGap_SaaS_Prototype
- **Verdict:** SKIP
- **What it is:** A pure-frontend "AI career platform" mockup — resume analysis, skill-gap ID, roadmaps, job matching — served as static HTML/CSS/JS on Netlify.
- **Signal:** 6 stars, last commit April 2025, stale (oldest in batch). The tell: **storage is browser LocalStorage and there is no backend or real model** — "AI resume analysis" runs client-side against no described inference. It's a UI prototype with animations, not a working analyzer.
- **Unique mechanism:** None — the "SaaS" is a landing-page prototype.
- **Stealable for LOCI/seshat:** Nothing functional. Only possible use is UI/UX reference for how to *present* a gap report (roadmap cards, toast feedback), and even that is generic.
- **Answer to owner's note:** n/a.

### Humaam-04-06/SkillForge
- **Verdict:** STEAL-PATTERNS (best-built of the skill-gap cluster, with caveats)
- **What it is:** A full-stack MERN + Gemini 2.0 Flash "developer growth platform": GitHub OAuth → analyze public repos/language footprint/commit activity → radar-chart skill profile → AI skill-gap report vs. pasted job listings → 30/60/90-day roadmaps → daily challenges/streaks → AI mentor chat → resume STAR-bullet optimizer.
- **Signal:** 23 stars, last commit "last Sunday" (actively developed, the most current of the gap tools). Real, coherent monorepo (client Zustand/Recharts/Framer, server Express/Mongoose/Passport GitHub OAuth/JWT/node-cron, `@google/genai` with BYOK + demo mock failsafe). Ambitious solo full-stack build; polished but unproven at scale.
- **Unique mechanism:** **Job-listing parsing → side-by-side capability diff → color-coded priority gaps → day-by-day roadmap**, all seeded from *real GitHub behavior* (aggregated language weights across 8 developer domains rendered as a radar chart), plus **node-cron midnight pre-generation** of personalized daily challenges tailored to the user's active gaps.
- **Stealable for LOCI/seshat:** Several concrete pieces. (1) The **8-domain radar/competency model derived from real artifacts** (language weights, commit activity) — a clean data model for LOCI's participant/org capability analysis. (2) The **"paste the target (job listing) → parse requirements → diff against measured capability → priority-ranked gap list"** pipeline generalizes directly to workshop skill-gap analysis (swap "job listing" for "workshop learning objectives"). (3) **BYOK + demo-mock failsafe** pattern for the AI engine — good resilience design for a tool you demo live. (4) **node-cron pre-generation** of tailored content ahead of time — useful for LOCI generating participant-specific material before a session. For seshat, less relevant (it's Mongo-backed, not markdown+git).
- **Answer to owner's note:** n/a (no note) — but this is the one skill-gap repo whose *code* is worth reading rather than just its idea.

### Zackriya-Solutions/meetily
- **Verdict:** STEAL-PATTERNS
- **What it is:** Privacy-first, fully-local AI meeting assistant (desktop app) that captures system audio, transcribes in real time with local Whisper, and generates summaries locally — no cloud. Open-core with a paid PRO tier. (The canonical repo is `Zackriya-Solutions/meeting-minutes`; "meetily" is the product.)
- **Signal:** ~26k stars, last commit June 2026, Trendshift-featured, MIT, signed macOS/Windows installers, Discord/Reddit community, active PRO monetization. Unambiguously real and mature — by far the most established project in the batch.
- **Unique mechanism:** **End-to-end local realtime meeting intelligence**: live system-audio capture → local Whisper streaming transcription → local-LLM summarization, packaged as a distributable desktop app with zero data egress. The whole value prop is "meeting AI with no server you don't control."
- **Stealable for LOCI:** LOCI is live, in-person/remote workshop facilitation — meetily is the closest *shipped, proven* implementation of the "capture live speech → transcribe → summarize in realtime, on the facilitator's own machine" spine LOCI needs. Steal: (1) the **local system-audio capture + streaming Whisper** stack (autopreso gives you the agent/canvas half; meetily gives you the robust capture/transcription/summarization half); (2) the **privacy-first / on-device / data-sovereignty positioning** — a strong differentiator for enterprise workshop clients who won't stream sessions to a vendor; (3) the **open-core + PRO** monetization template (community edition free, advanced exports/diarization/team features paid) as a business model for LOCI; (4) their planned **speaker diarization** approach — directly needed to attribute contributions per participant in a workshop.
- **Answer to owner's note ("for loci?"):** Yes — strongly. Not the workshop *engine* itself, but the best available reference (and reusable capture/transcription/summarization plumbing) for LOCI's realtime audio backbone. Pair it mentally with autopreso: meetily = capture+transcribe+summarize locally at scale; autopreso = realtime agent that renders/manipulates visuals from that stream. LOCI ≈ the union of the two.

---

### Batch synthesis
There is **no single mature open-source "workshop/skill-gap engine"** to adopt wholesale — but there are two real, high-signal foundations to build the realtime half on, and one decent code reference for the analysis half. **meetily (~26k stars)** and **autopreso (409 stars, but exactly on-domain)** together form LOCI's realtime spine: meetily proves robust local audio-capture → streaming Whisper → local-LLM summarization with a privacy-first, open-core business model; autopreso proves the live agent loop (STT → tool-calling agent → live Excalidraw scene) with transcription-vocabulary biasing and cold-start warmup — the "presentations generated on the fly" mechanism. The entire skill-gap cluster (SkillGap_AI, Opportunity-Gap-Analyzer, SKILL-BRIDGE, TalentGap) is prototypes and student portfolios with 1–6 stars and nothing to lift beyond framing — with the **one exception of SkillForge (23 stars, actively built)**, whose "measure real GitHub artifacts → radar competency model → parse target requirements → diff → priority-ranked gaps → dated roadmap" pipeline is a genuinely reusable design and codebase for LOCI's capability-analysis feature. Recommendation for LOCI: build the realtime engine on meetily's capture/transcribe/summarize patterns + autopreso's agent/canvas loop, and model the skill-gap analytics on SkillForge's artifact-grounded competency diff (with Opportunity-Gap-Analyzer's "verify against real behavior, not self-report" principle baked in). None of these are directly relevant to seshat's markdown+git core except as loosely-coupled analytics that could emit markdown reports.
