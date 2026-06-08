# Contributing

This repository holds planning, design documentation, and (eventually) the
manuscript for the Spatial Foraging Platform. It does **not** hold hardware
design files, firmware, host software, or analysis code — those live in
separate repositories linked from the [README](README.md#related-repositories).

## What belongs here

- The master plan: [`PROJECT.md`](PROJECT.md).
- Live design documents: [`docs/`](docs/).
- Meeting notes: [`meetings/`](meetings/).
- Budget tracking and BOM summaries: [`bom/`](bom/).
- Source/reference PDFs: [`references/`](references/).
- Manuscript drafts (Phase 8+): [`manuscript/`](manuscript/).

## What does **not** belong here

- CAD, STEP, STL, or other mechanical design files.
- Schematics, board layouts, gerbers.
- Firmware, host software, or analysis source code.
- Large datasets or recordings.

If an artifact does not belong here, link to it from the relevant doc and
update the [Related repositories](README.md#related-repositories) table in the
README.

## File naming

- Documentation files: lowercase kebab-case, `.md` extension. Example:
  `sync-and-recording.md`.
- Meeting notes: `YYYY-MM-DD-short-topic.md`. Example:
  `2026-06-14-architecture-kickoff.md`.
- ADRs: `NNNN-short-title.md` where `NNNN` is the next available
  zero-padded sequence number. Example: `0007-can-bus-topology.md`.

## How to make a change

1. Create a feature branch off `main`. Branch name should reflect the change,
   e.g. `docs/architecture-can-bus`, `adr/0007-can-bus-topology`,
   `meetings/2026-06-14-architecture-kickoff`.
2. Make your change. Keep PRs focused on a single document or a single phase
   when possible.
3. Open a pull request. The phase lead listed in [`PROJECT.md`](PROJECT.md) for
   the phase your change touches is the default reviewer; the NTH engineering
   lead is the catch-all reviewer.
4. Squash-merge once approved.

## Updating progress

- Tick boxes in [`PROJECT.md`](PROJECT.md) as goals and exit criteria are met.
  Each tick should be backed by either a committed doc, a linked artifact, or
  an ADR.
- When a phase or gate is crossed, update the **Current status** table in both
  [`README.md`](README.md) and [`PROJECT.md`](PROJECT.md).

## Recording meetings

After cross-lab meetings, drop a note in [`meetings/`](meetings/) using
[`meetings/TEMPLATE.md`](meetings/TEMPLATE.md). Capture decisions and action
items at minimum; full transcripts are not required.

## Style

- Plain GitHub-flavored Markdown.
- Hard-wrap prose at ~80 columns where reasonable, but do not break tables or
  links.
- Prefer relative links between files in this repo so navigation works on
  both github.com and locally.
- Diagrams use Mermaid where possible so they render natively on GitHub.

## License

By contributing, you agree your contributions to this repository are released
under [CC-BY-4.0](LICENSE).
