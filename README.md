# Spatial Foraging Platform

A modular home-cage platform that converts the cage floor into a programmable
foraging environment for freely moving rodents. Distributed reward delivery and
spatial cueing within a scalable, synchronized architecture enable continuous,
ethologically grounded behavior compatible with chronic neural recordings.

This repository is the **planning, documentation, and manuscript hub** for the
project. Hardware design files, firmware, host software, and analysis code all
live in separate repositories and are linked from the [Related repositories](#related-repositories)
table below.

Funded by the McDonnell Center NRP (PI: Gaidica). Total budget: $40,000 over 12
months. See [`references/McDonnell-NRP_GAIDICA.pdf`](references/McDonnell-NRP_GAIDICA.pdf)
for the funded proposal.

## Current status

| Field           | Value                                |
| --------------- | ------------------------------------ |
| Phase           | Phase 0 — NTH Architectural Engineering |
| Hardware status | Pre-alpha                            |
| Software status | Pre-alpha                            |
| Week            | Week 0 (anchored 2026-06-08; see [`PROJECT.md`](PROJECT.md)) |

## Quick links

- [`PROJECT.md`](PROJECT.md) — master roadmap: phases, milestones, exit criteria, checkbox tasks
- [`docs/`](docs/) — design + requirements documents (architecture, sync, UI/UX, calibration, etc.)
- [`decisions/`](decisions/) — Architecture Decision Records (ADRs)
- [`meetings/`](meetings/) — meeting notes
- [`bom/`](bom/) — budget tracking against the $40k grant
- [`references/`](references/) — grant proposal and source planning documents
- [`manuscript/`](manuscript/) — manuscript drafting (Phase 10+)
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how to contribute

## Collaborators

| Lab                                  | Role                                                                                       | Lead          | GitHub  |
| ------------------------------------ | ------------------------------------------------------------------------------------------ | ------------- | ------- |
| Neurotech Hub (NTH)                  | Engineering lead — electronics, firmware, host UI, fabrication, integration, dissemination | Matt Gaidica  | TBD     |
| Animal Behavior Core (ABC)           | UI/UX feedback, common task structures, behavioral benchmarking, animal metrics report     | TBD           | TBD     |
| Hengen Lab (HLAB)                    | Custom experiment authoring, in vivo electrophysiology validation, recording sync          | Keith Hengen  | TBD     |

## Related repositories

The repositories below hold the actual artifacts produced by this project. This
planning repo links to them; it does not contain their source.

| Domain                  | Repository                | Owner | Status |
| ----------------------- | ------------------------- | ----- | ------ |
| Mechanical / CAD        | TBD                       | NTH   | TBD    |
| Electronics / PCB       | TBD                       | NTH   | TBD    |
| Module firmware         | TBD                       | NTH   | TBD    |
| Base station / host UI  | TBD                       | NTH   | TBD    |
| Experiment API examples | TBD                       | HLAB  | TBD    |
| Analysis examples       | TBD                       | NTH / HLAB | TBD |

When a sub-repo comes online, replace `TBD` with the URL and update the status
column.

## How to read this repo

If you are new here, read in this order:

1. This README — what we are building and where artifacts live.
2. [`PROJECT.md`](PROJECT.md) — current phase, what is in flight, what is next.
3. [`docs/architecture.md`](docs/architecture.md) — system architecture once it exists.
4. The relevant `docs/*.md` for your role (e.g. ABC: `ui-ux.md`, `maintenance.md`; HLAB: `user-api.md`, `sync-and-recording.md`).

## Abbreviations

Abbreviations used throughout this repository.

### Collaborators

| Abbreviation | Expansion            |
| ------------ | -------------------- |
| ABC          | Animal Behavior Core |
| HLAB         | Hengen Lab           |
| NTH          | Neurotech Hub        |

### Project / process

| Abbreviation | Expansion                                                                                |
| ------------ | ---------------------------------------------------------------------------------------- |
| ADR          | Architecture Decision Record (see [`decisions/`](decisions/))                            |
| BOM          | Bill of Materials                                                                        |
| HW           | Hardware (status field)                                                                  |
| MVP          | Minimum Viable Product                                                                   |
| SW           | Software (status field)                                                                  |

### Hardware / electronics

| Abbreviation | Expansion                                                                                |
| ------------ | ---------------------------------------------------------------------------------------- |
| CAD          | Computer-Aided Design                                                                    |
| CAN          | Controller Area Network (bus)                                                            |
| LED          | Light-Emitting Diode                                                                     |
| MCU          | Microcontroller Unit                                                                     |
| PCB          | Printed Circuit Board                                                                    |
| PCBA         | Printed Circuit Board Assembly                                                           |
| TTL          | Transistor-Transistor Logic (digital sync signal level)                                  |

### Software / interfaces

| Abbreviation | Expansion                                                                                |
| ------------ | ---------------------------------------------------------------------------------------- |
| API          | Application Programming Interface                                                        |
| CLI          | Command-Line Interface                                                                   |
| UI           | User Interface                                                                           |
| UX           | User Experience                                                                          |

### External references

| Abbreviation | Expansion                                                                                |
| ------------ | ---------------------------------------------------------------------------------------- |
| FED3         | Feeding Experimentation Device 3 (open-source rodent feeder, Kravitz lab)                |

## License

Documentation, plans, and manuscript content in this repository are released
under [CC-BY-4.0](LICENSE). Code in linked sub-repositories carries its own
licensing.
