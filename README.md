# Seshat

Personal knowledge substrate: decisions, research, and the agent harness that ties them together. Named for the Egyptian goddess who both **kept the annals** and **stretched the cord** to survey the foundations of new temples — record what happened, measure out what gets built. This repo is the **substrate** — plain markdown under git that survives every tool and vendor.

## Principles

1. **Own the substrate.** Markdown + git, in this repo. Everything else is replaceable.
2. **The machine layer is derived.** Embeddings, distilled facts, graphs — always rebuildable from the substrate, never source-of-truth. (Lesson from Graphlit closing signups.)
3. **Thin harness.** Workflows and MCP tools connect substrate to machine layer. "Whoever owns your agent harness owns your results." — IndyDevDan
4. **Every session ends with its decisions landing in `decisions/`.** This is the rule that makes attempt N the last attempt. See `REALNESS.md`.

## Layout

```
decisions/    One numbered file per locked decision. Append-only in spirit.
research/     Source studies and comparisons. The inspiration corpus.
schemas/      Expected structure per note type (Picoschema-style). Validation is warn, not blocking.
harness/      Skills, prompts, and workflow definitions (ADW-style).
REALNESS.md   The unfiltered assessment that founded this repo. Re-read before adding any new system.
```

## Schemas

Note structure follows the [Basic Memory schema system](https://docs.basicmemory.com/raw/concepts/schema-system.md) pattern: each note type has a schema file in `schemas/` declaring expected fields (Picoschema syntax — `?` marks optional, `(array)` marks lists). Validation mode is **warn** — schemas describe the pattern, they don't block writing. When the format stabilizes, tighten to strict. If notes start drifting from a schema, that's signal to update the schema, not to stop writing.

## Related

- Engine candidate: [llm-wiki](https://github.com/senseihaiku/llm-wiki) (fork of [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki)) — Karpathy-spec session-to-wiki pipeline. Seshat references it; Seshat does not live inside it.
- Reference systems studied in `research/knowledge-base-inspiration-sources.md`: Cerebras Knowledge, Hindsight, Honcho, Basic Memory, Graphlit, Graphify, nashsu/llm_wiki, IndyDevDan's fusion-harness.
