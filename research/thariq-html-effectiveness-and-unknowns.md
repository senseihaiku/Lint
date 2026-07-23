# Thariq Shihipar: The unreasonable effectiveness of HTML + Know your unknowns

*Opus agent report 2026-07-23, saved verbatim. Sources: https://thariqs.github.io/html-effectiveness/ (20 example pages) and https://thariqs.github.io/html-effectiveness/unknowns/ (11 example pages). All 33 pages read. Note: the owner's `finding-unknowns-skills` repo CLAUDE.md is a distillation of the unknowns material; the mapping is verified below.*

## The thesis (Collection A)

The site is the companion to Thariq's blog post *"The unreasonable effectiveness of HTML."* The index frames it in one line:

> "Twenty self-contained .html files an agent produced instead of a wall of markdown. Each one trades **a document you'd skim for one you'd actually read** — open any of them directly in a browser. Grouped by the kind of work they replace."

The index even carries a `.md` / `.html` toggle as its central visual — the whole argument is *this format vs. that one*. The closing line drives it home: **"Everything on this page is itself a single .html file."** The artifact is the argument.

The "why HTML instead of chat text/markdown" case is made per-category, and each category names a specific thing markdown destroys:

- **Exploration & Planning** — fan out across directions and lay them side by side "so you can point at one — **instead of reading three sequential walls of text and trying to hold them all in your head**."
- **Code Review** — *"Diffs and call-graphs are **spatial information; markdown flattens them**."* Render the change as an annotated diff or boxes-and-arrows "so the shape of the code is visible at a glance."
- **Design** — *"HTML is **the medium your design system ships in**, so it's the natural format for talking about it."* Tokens become swatches, and the artifact "can be **fed straight back into the next prompt**."
- **Prototyping** — *"**Motion and interaction can't be described, only felt.** A throwaway page with the real easing curve or the real click-through tells you in five seconds what a paragraph of prose never could."*
- **Illustrations & Diagrams** — *"Inline SVG gives the agent **a real pen**."* Vector art you tweak by hand or paste into the final doc.
- **Decks** — *"A handful of `<section>` tags and twenty lines of JS is a slide deck… **no Keynote, no export step**."*
- **Research & Learning** — *"An explainer with collapsible sections, tabbed code samples and a glossary in the margin **reads very differently from the same words dumped linearly**."*
- **Reports** — *"A small chart and a colored timeline turn something people skim into something they actually read."*
- **Custom Editing Interfaces** — *"Sometimes it's hard to describe what you want in a text box. Ask for a throwaway editor for the exact thing you're working on — and **always end with an export button that turns whatever you did in the UI back into something you can paste into the agent or commit. You stay in the loop; the loop gets tighter.**"*

Net thesis: an agent should render work as **self-contained, throwaway, browser-openable HTML artifacts**, not linear prose, because the highest-value information in real work — spatial (diffs, call graphs), temporal (motion, timelines), comparative (options side by side), and interactive (click-through, tunable sliders) — is exactly what gets flattened out when you serialize to markdown in a chat window. Two secondary properties recur: artifacts are **disposable** (each is "throwaway"), and they **round-trip** — export buttons and copyable prompts turn the artifact back into text you paste into the next prompt or commit.

## The unknowns model (Collection B)

Companion to the blog post *"Know your unknowns."* Framing line:

> "**The map is not the territory — the gap between them is your unknowns.** Eleven self-contained .html artifacts for discovering them before, during, and after implementation. **Each page shows the exact prompt at the top and the artifact Claude produced below it: paste the prompt, get something like the page.**"

The index renders the Johari-window quadrants — *Known knowns / Known unknowns / Unknown knowns / Unknown unknowns* — and organizes the 11 examples along an implementation timeline: **Pre-implementation (8), During implementation (1), Post-implementation (2)**, each stage with its own thesis:

- **Pre-implementation** — *"Before any code is written is **the cheapest place to find an unknown**. Ask for a blindspot pass when the territory is unfamiliar, brainstorm and prototype when you'll only know it when you see it, let Claude interview you about the rest, and hand it references when words run out."*
- **During implementation** — *"No matter how much planning you do, unknowns lurk in the territory. Have Claude **keep a running log of every place the code forced a deviation from the plan**, so the next attempt starts smarter."*
- **Post-implementation** — *"Shipping means **other people inheriting your unknowns**. A pitch doc answers the objections reviewers were about to raise; a quiz proves you actually understand what changed before you merge it."*

Closing line: *"Every explainer, brainstorm, interview, and prototype is **a cheap way to find out what you didn't know**."*

What each of the 11 demonstrates, and how they sequence into a workflow:

**Pre (find the unknown before it's expensive):**
1. **Blindspot pass** — hand Claude unfamiliar code, get your *unknown unknowns* back as concrete cards. The auth example returns "4 landmines, 2 unwritten conventions, 1 missing concept, 1 reverted attempt at this exact task," each with a copyable prompt-fix, then folds all seven into one improved implementation prompt. Payoff line: *"Seven sentences you couldn't have written this morning — **each one bought with someone else's half-day**. That's the point of the pass."*
2. **Teach me my unknowns** (color grading) — when you lack the *vocabulary* of a domain, have Claude teach it (vocabulary ladder, live before/after frame) so *"your next prompt uses the words a professional would."* Payoff: turns "make the video look nicer" into "apply the Rec.709 conversion LUT first, then grade… push the lift slightly teal… use vibrance not saturation."
3. **Four design directions** — *"When you can't describe the design you want, don't describe one — ask for four incompatible ones rendered live, and let your gut do the specifying. **Reacting is easier than imagining.**"* Steal/skip chips assemble your reply automatically.
4. **Mock before you wire** — a clickable throwaway HTML mock with fake data before touching the real app: *"you'll find out what you actually want **the moment you can click it, not three PRs later**."* Ends with A/B "open questions" chips that fill a reply template.
5. **Brainstorm the intervention** — the whole option space, cheapest→most-ambitious, *grounded in the actual codebase*: *"a lot of retention machinery already exists but is disconnected — **the cheapest options below are mostly wiring, not building**."* Resonate checkboxes build the reply.
6. **The interview** — Claude interviews you one question at a time, *ordered by architectural blast radius* (architecture → data → ux → polish), each option annotated with its consequence ("This decides whether we need a job queue, a notification path, and an artifacts bucket — or none of those"), then emits a decisions table + a follow-up prompt that *"Treat[s] the architecture and data-model decisions (1–4) as fixed."*
7. **Point at a reference** — reference-as-spec, but Claude must *prove it understood* first: a semantics map of a Rust rate-limiter with side-by-side excerpts, a preserved/deliberately-changed/dropped table, and edge-case tables, gated on *"Nothing gets implemented until you sign off on this page… Reply 'semantics confirmed'."*
8. **The tweakable plan** — a plan *"sorted by **likelihood-of-tweaking**, not execution order,"* so judgment calls (schema choice, type interfaces, UX) surface at the top with toggleable alternatives, and *"Section C is mechanical and safe to skip entirely."* Ends with three copy-paste "tweak these three things" replies.

**During:**
9. **Implementation notes** — the running log Claude keeps mid-build (here, a 3-hour run: 11 entries, 4 deviations). Each deviation is *plan-said / code-revealed / conservative-choice / revisit* — *"pick the conservative option, log it under 'Deviations', and keep going"* — then three bullets *"to fold into the next plan or prompt, so the next run doesn't rediscover today's surprises."*

**Post (other people inherit your unknowns):**
10. **The buy-in doc** — packages prototype + spec + notes into one skimmable pitch that *"answers objections before they're raised,"* leading with an animated demo, pre-answering five reviewer questions with evidence links, and naming exactly who signs off on what by when.
11. **Quiz me before I merge** — a merge-readiness report on a 14-file diff ending in a 6-question quiz you must pass (6/6 = *"Cleared to merge — you can explain this change to whoever gets paged for it"*); wrong answers point back to the exact section you skimmed. *"the artifact won't let you feel done until you actually are."*

## Page-by-page (Collection A)

- **index** — the `.md`→`.html` thesis and the nine work-categories; every category names one thing markdown flattens.
- **01 · Three code approaches** — side-by-side implementation comparison (debounced search: inline hook / custom hook / external lib) with Pro/Con columns + bundle/testability/reuse/SSR badges; works because it ends in a decisive, repo-grounded **Recommendation** ("Acme already has three places that hand-roll the inline pattern").
- **02 · Visual design directions** — four empty-state designs rendered live with a **light/dark background toggle**; works because you react to real tone/density and see how each holds up on both surfaces instead of imagining it.
- **03 · Annotated pull request** — a review with a file **risk map** (safe / worth a look / needs attention), inline diffs tagged **Blocking / Nit**, and jump links; works because severity + spatial risk map make it scannable vs. scrolling a terminal.
- **04 · Module map** — a request-path box-and-arrow diagram plus a numbered call-stack walkthrough with expandable source and a "Gotchas" list; works because it draws the single trust boundary and highlights the one hot path.
- **05 · Living design system** — tokens pulled from `tokens.ts` rendered as **copyable swatches**, type scale, spacing, components; works because it's *"a portable reference when prompting"* — the artifact feeds the next prompt.
- **06 · Card variant matrix** — six structural card treatments on one contact sheet with density/border/shadow controls and per-variant "best for" notes; works because every state sits on a single sheet for review.
- **07 · Animation sandbox** — a task-complete micro-interaction with easing presets (linear/ease-out/spring), a keyframe timeline, and **copy-paste CSS**; works because you feel the real curve and tune duration/easing before wiring it in.
- **08 · Clickable flow (drag-to-reorder)** — a ~40-line native-DnD prototype plus a *"What you're feeling"* list of baked-in design decisions and *"Open questions"*; works because it surfaces the decisions so you can push back on them.
- **09 · Arrow-key slide deck** — a 6-slide deck as one HTML file (left/right nav, no build); works because you arrow-key through it in a meeting with no Keynote/export step.
- **10 · SVG figure sheet** — three hand-drawn 720×320 inline SVGs, each with its own `<style>` and a **Download SVG** button plus palette rules; works because *"inline SVG gives the agent a real pen"* — standalone, editable vector art.
- **11 · Weekly status** — stat tiles, highlights, a shipped table with risk levels, a velocity bar chart, carryover; auto-generated from `git log`/CI/deploy log; works because a small chart + color turn a skim into a read.
- **12 · Incident timeline** — TL;DR, minute-by-minute timeline, root cause **showing the offending config diff** (`max_connections: 8 # debug value, do not ship`), impact stats, owned action items with due dates; works because the colored timeline and the literal bad diff make the failure legible.
- **13 · Annotated flowchart** — the deploy pipeline drawn from `.github/workflows/`; click any step for what runs, timing, and failure/short-circuit paths; works because it's a real clickable flowchart, not ASCII.
- **14 · Feature explainer** — "How rate limiting works": TL;DR, collapsible request-path steps with `file:line` refs, **tabbed** config snippets, gotchas, FAQ, files-read sidebar; works because collapsibles + tabs + margin refs make a linear topic navigable.
- **15 · Concept explainer** — consistent hashing taught with a **live ring you add/remove nodes from** (shows keys moved), a vs-`mod N` table, hover-linked glossary; works because an interactive model teaches better than prose.
- **16 · Implementation plan** — milestones on a timeline + client→persistence data-flow diagram + inline mockups + the two pieces "most likely to be done wrong" (migration, optimistic mutation) + risk table + open questions; prompt: *"Make it easy to skim on a phone — I'm going to pass this to the implementer as-is."* Works because it bundles everything the implementer needs into one skimmable artifact.
- **17 · PR writeup for reviewers** — author-side: TL;DR, Why, before/after, file-by-file *"ordered for reading, not alphabetically,"* "Where to focus your review," "What I deliberately did not do," test plan, rollout; works because it aims the reviewer's attention instead of dumping a diff.
- **18 · Ticket triage board** — drag 24 tickets across Now/Next/Later/Cut, filter by tag, **Copy as markdown**; works because you manipulate directly then export the result back to the planning doc.
- **19 · Feature flag editor** — toggles grouped by area with live **dependency warnings** ("1 flag is enabled without its prerequisite") and a **Copy diff** of only changed keys; works because it catches dependency violations and exports just the delta.
- **20 · Prompt tuner** — an editable template with `{{double_brace}}` slots highlighted; three sample tickets re-render live as you type; unknown fields underlined in dashed clay; works because you tune a prompt against real input variance, then copy it out.

## Page-by-page (Collection B)

- **index** — "map is not the territory" + Johari quadrants; sequences 11 artifacts across before/during/after; *"paste the prompt, get something like the page."*
- **01 · Blindspot pass** — pre; scans unfamiliar code and returns *unknown unknowns* as landmine/convention/missing-concept/reverted-attempt cards, each with a copyable prompt-fix, assembled into one improved prompt; works because it converts what-you-don't-know-you-don't-know into concrete constraints "bought with someone else's half-day."
- **02 · Teach me my unknowns** — pre; teaches an unfamiliar domain's vocabulary (color grading) via a vocabulary ladder + live graded frame; works because it upgrades your *next* prompt to professional vocabulary.
- **03 · Four design directions** — pre; the same data rendered four incompatible ways with steal/skip chips that write your reply; works because *"reacting is easier than imagining."*
- **04 · Mock before you wire** — pre; a clickable throwaway HTML mock with fake data + A/B open-question chips before real code; works because clicking reveals what you want "not three PRs later."
- **05 · Brainstorm the intervention** — pre; ten churn fixes cheapest→most-ambitious, each *"Found in code"* with file paths; works because the option space is grounded in what already exists ("mostly wiring, not building").
- **06 · The interview** — pre; one question at a time, ordered by architectural blast radius, options annotated with consequences, ending in a decisions table + follow-up prompt that fixes decisions 1–4; works because it asks hardest-first and never asks what the code can answer.
- **07 · Point at a reference** — pre; a semantics map proving Claude understood a Rust reference before porting, gated on "semantics confirmed"; works because reference-as-spec becomes a reviewable proof-of-understanding, not a silent reimplementation.
- **08 · The tweakable plan** — pre; sorted by likelihood-of-tweaking, judgment calls up top with toggleable alternatives, mechanical work collapsed at the bottom; works because attention goes to the reversible decisions first.
- **09 · Implementation notes** — during; a running deviation log (plan-said/code-revealed/conservative-choice/revisit) + 3 bullets for attempt #2; works because mid-build surprises become inputs instead of vanishing into scrollback.
- **10 · The buy-in doc** — post; demo-first pitch that pre-answers reviewer objections with evidence and assigns named sign-offs by a date; works because it neutralizes objections before they're raised.
- **11 · Quiz me before I merge** — post; merge-readiness report + a 6-question quiz you must pass, wrong answers pointing back to the skimmed section; works because it won't let you feel done until you can explain the change to whoever gets paged.

## Where they fit in seshat

**Collection A → SURFACE layer (confirmed, with one extension).** This is a playbook for *how the agent presents work*: the render format is a **self-contained, throwaway HTML artifact you open in a browser**, never a stored source of truth. It matches seshat's SURFACE definition ("thin interfaces that never store") almost exactly, and the load-bearing detail is the **export-button / copyable-prompt pattern** ("always end with an export button that turns whatever you did in the UI back into something you can paste into the agent or commit") — that is precisely a SURFACE→SUBSTRATE round-trip: the surface holds no canonical state; it emits markdown/diff/prompt back into git-tracked markdown or the next agent turn. One correction to the naive reading: a subset of Collection A (design-system tokens "fed straight back into the next prompt," the SVG sheet, the editors' copyable output) also feeds the **HARNESS**, not just the human — the surface is an *input* to the next prompt, not only an output for reading. And because seshat's stated bottleneck is **retrieval, not capture**, Collection A is directly a *retrieval-presentation* pattern: the module map (04), flowchart (13), feature/concept explainers (14, 15), and status/incident reports (11, 12) are all templates for rendering *retrieved* knowledge as a navigable artifact (collapsibles, tabs, live diagrams, margin glossaries) instead of a wall of markdown you'd skim. That is the retrieval surface seshat wants.

**Collection B → HARNESS / process layer (confirmed).** This is a method for *how to run a task so unknowns surface* — an agent-workflow spanning before/during/after implementation. It belongs in the HARNESS (skills/hooks/agent workflows). The important nuance: **each HARNESS move outputs a Collection-A-style SURFACE artifact** (the blindspot cards, the interview, the semantics map, the quiz are all self-contained HTML). So the two collections are not disjoint — **B is A applied to the specific job of de-risking unknowns**: B says *when and why* to run each move; A supplies the *medium* each move renders into.

**What `finding-unknowns-skills/CLAUDE.md` already covers vs. what the full site adds.** The owner's CLAUDE.md is a near-1:1 distillation of Collection B's *moves*:

| CLAUDE.md bullet | Collection B page |
|---|---|
| blindspot pass (before) | 01 |
| 3–5 wildly different throwaway variations, each labeled with its bet | 03 |
| interview one question at a time, architecture-changing first | 06 |
| reference-as-spec → semantics summary before reimplementing | 07 |
| plans lead with decisions most likely to be tweaked; compress mechanical work | 08 |
| implementation-notes.md (Deviations / edge cases / questions); conservative reversible option, log it, keep going | 09 |
| demo-first buy-in package (demo, bet, expert Qs answered honestly, deviations, non-scope) | 10 |
| change report + quiz, 5–8 questions weighted to edge cases/blast radius | 11 |
| "map is not the territory… the gap is the unknowns" | index framing |

What the **full site adds beyond that distillation**:

1. **The medium itself.** The CLAUDE.md captures the *moves* but is silent on the *render format*. The site's entire point — deliver each move as a **self-contained HTML artifact with the exact prompt at the top and interactive, self-assembling output** — is absent from the distillation. Collection A is the missing half: *how* to present, not just *what* to do.
2. **Two pre-implementation moves the distillation folds away.** *"Teach me my unknowns"* (B/02 — have the agent teach you an unfamiliar domain's vocabulary so your next prompt is precise) and *"Brainstorm the intervention grounded in code"* (B/05 — the whole option space, cheapest→ambitious, each option "found in code") are distinct discovery moves not explicitly in the four CLAUDE.md "before implementing" bullets. B/04 *"mock before you wire"* (a single clickable mock with embedded A/B decision chips) is also distinct from B/03's N-parallel design directions, though the CLAUDE.md's "throwaway variations" bullet gestures at it.
3. **The closing mechanism.** The distillation states each principle; the site shows how each artifact *closes the loop* — copyable prompt-fixes, steal/skip and "resonate" chips that auto-write your reply, the "semantics confirmed" gate, the interview's generated follow-up prompt, the quiz-gated merge with wrong-answers-point-back. Every artifact ends by producing the text you paste back. That "make the artifact write the reply" pattern is the operational detail the CLAUDE.md doesn't encode.

## Relation to the rest of the research set

These two collections are the general theory that several other items in seshat's research are specific instances of. **planf3's self-contained HTML plans** are the same species as A/16 and B/08 — the plan as one browsable HTML file — with B/08 adding the "sort by likelihood-of-tweaking" refinement over plain execution order. **archify's interactive HTML maps** are the retrieval-presentation half of Collection A made concrete: A/04 (module map), A/13 (clickable flowchart), and A/15 (live consistent-hashing ring) are exactly "knowledge rendered as an interactive map you navigate rather than read." **i-have-adhd's output contract** is answered at the thesis level here — "a document you'd skim vs. one you'd actually read," plus the export-button round-trip is itself an output-contract rule (the surface never stores; it emits text back to the substrate). All of them converge on seshat's core bet: since **retrieval is the bottleneck**, the SURFACE should render knowledge as navigable, disposable, round-trippable HTML, and the HARNESS should run tasks (Collection B) so the unknowns surface as cheap artifacts before they become expensive reworks.
