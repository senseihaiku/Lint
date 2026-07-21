# 0001 — KOS is the home for knowledge-system work

Date: 2026-07-21 · Status: locked

## Decision

Knowledge-system decisions and research live in this repo (KOS), not in the llm-wiki fork.

## Why

- The llm-wiki fork is someone else's framework, ~1 month behind upstream, and its `wiki/` content is gitignored by design — decisions were only committable there through an ignore-rule loophole (`wiki/comparisons/` happened to be tracked).
- Research synthesis (2026-07-21): every system that works separates substrate you own → derived machine layer → thin harness. KOS is the substrate.

## Consequences

- PRs for knowledge work target KOS.
- llm-wiki remains a referenced engine, evaluated against alternatives (Hindsight, Basic Memory, nashsu/llm_wiki).
- The comparison page in the llm-wiki fork (PR #1) is superseded by `research/knowledge-base-inspiration-sources.md` here.
