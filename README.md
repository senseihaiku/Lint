# KOS — Knowledge OS

Personal knowledge system: decisions, research, and the agent harness that ties them together. This repo is the **substrate** — plain markdown under git that survives every tool and vendor.

## Principles

1. **Own the substrate.** Markdown + git, in this repo. Everything else is replaceable.
2. **The machine layer is derived.** Embeddings, distilled facts, graphs — always rebuildable from the substrate, never source-of-truth. (Lesson from Graphlit closing signups.)
3. **Thin harness.** Workflows and MCP tools connect substrate to machine layer. "Whoever owns your agent harness owns your results." — IndyDevDan

## Layout

```
decisions/    One file per locked decision. Append-only in spirit.
research/     Source studies and comparisons. The inspiration corpus.
harness/      Skills, prompts, and workflow definitions (ADW-style).
```

## Related

- Engine candidate: [llm-wiki](https://github.com/senseihaiku/llm-wiki) (fork of [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki)) — Karpathy-spec session-to-wiki pipeline. KOS references it; KOS does not live inside it.
- Reference systems studied in `research/knowledge-base-inspiration-sources.md`: Cerebras Knowledge, Hindsight, Honcho, Basic Memory, Graphlit, Graphify, nashsu/llm_wiki, IndyDevDan's fusion-harness.
