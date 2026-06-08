# Architecture Decision Records (ADRs)

This folder holds Architecture Decision Records for the Spatial Foraging
Platform. Each ADR captures a single significant decision: the context, the
options considered, the choice made, and the consequences.

ADRs are intentionally lightweight. The intent is not to slow decisions down,
but to make them recoverable later — both for new collaborators joining
mid-project and for the manuscript's Methods section in Phase 8.

## When to write an ADR

Write an ADR when the decision will be hard to revisit later. Examples:

- Communication protocol (CAN bus vs alternatives).
- Connector / cable standards.
- Module MCU family.
- Synchronization ownership (modules vs base station).
- Sync output format (TTL, serial, timestamps).
- User-facing API surface.
- Output file format / event schema.
- Manuscript publication route.

You do **not** need an ADR for routine implementation choices that are easy
to change later (variable names, internal data structures, file layout
within a sub-repo, etc.).

## How to add an ADR

1. Copy [`0000-template.md`](0000-template.md) to a new file named
   `NNNN-short-title.md`, where `NNNN` is the next zero-padded sequence
   number (`0001`, `0002`, …) and `short-title` is lowercase kebab-case.
2. Fill in the sections.
3. Set the status (see below).
4. Link the ADR from the relevant `docs/*.md` file when it lands.
5. Add it to the index below.

## ADR statuses

- **Proposed** — under discussion; not yet binding.
- **Accepted** — decision is in effect.
- **Superseded** — replaced by a later ADR; link to the replacement.
- **Deprecated** — no longer relevant but kept for history.

Once **Accepted**, ADRs are not edited in place. If a decision changes,
write a new ADR that supersedes the old one.

## Index

| ID | Title | Status | Date |
| -- | ----- | ------ | ---- |
| _none yet_ | | | |
