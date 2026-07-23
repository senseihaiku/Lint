---
title: Decision
type: schema
entity: decision
schema:
  number: string, zero-padded sequence (0001, 0002, ...) matching the filename prefix
  date: string, YYYY-MM-DD when the decision was locked
  status: string, one of locked | superseded | revisited
  decision: string, one sentence stating what was decided
  why?(array, reasons grounded in evidence or REALNESS.md): string
  consequences?(array, what changes because of this): string
  supersedes?: Decision, earlier decision this replaces
settings:
  validation: warn
---

# Decision schema

Every file in `decisions/` follows `NNNN-kebab-slug.md` and contains at minimum: date, status, the decision in one sentence, and why. A decision without a written *why* is a rescheduled debate — the why is what stops the re-litigating that killed the previous ~30 attempts.

Existing conforming examples: `0001-kos-is-the-home.md` (note: predates the rename), `0002-repo-name.md`.
