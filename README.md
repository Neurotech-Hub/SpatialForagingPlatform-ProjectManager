# Spatial Foraging Platform

A modular home-cage platform that converts the cage floor into a programmable
foraging environment for freely moving rodents. Distributed reward delivery and
spatial cueing within a scalable, synchronized architecture enable continuous,
ethologically grounded behavior compatible with chronic neural recordings.

**[View design (Fusion 360)](https://a360.co/4dwOh8e)** — 3D model and mounting
outline. Mounting DXF and fastener notes live in [`hardware/`](hardware/).

This repository is the **planning, documentation, and manuscript hub** for the
project. Hardware design files, firmware, host software, and analysis code all
live in separate repositories and are linked from the [Related repositories](#related-repositories)
table below.

Funded by the McDonnell Center NRP (PI: Gaidica). Total budget: $40,000 over 12
months. See [`references/McDonnell-NRP_GAIDICA.pdf`](references/McDonnell-NRP_GAIDICA.pdf)
for the funded proposal.

## Current status


| Field           | Value                                                                        |
| --------------- | ---------------------------------------------------------------------------- |
| Phase           | Phase 3A — ABC UI/UX Feedback + Maintainability                              |
| Hardware status | Alpha                                                                        |
| Software status | Beta                                                                         |
| Week            | Week 10 (see [`PROJECT.md`](PROJECT.md))                    |




## Quick links

- [`PROJECT.md`](PROJECT.md) — master roadmap: phases, milestones, exit criteria, checkbox tasks
- [`docs/`](docs/) — design + requirements documents (architecture, dispense cycle, sync, UI/UX, calibration, etc.)
- [`docs/dispense-cycle.md`](docs/dispense-cycle.md) — dispense cycle flowchart, sensor pinouts, CAN events, and heartbeat snapshot
- [`docs/user-api.md`](docs/user-api.md) — experiment API, session CSV schema, and behavior reports
- [`hardware/`](hardware/) — hardware design artifacts (e.g. 3D / DXF)
- [`meetings/`](meetings/) — meeting notes
- [`bom/`](bom/) — budget tracking against the $40k grant
- [`references/`](references/) — grant proposal and source planning documents
- [`manuscript/`](manuscript/) — manuscript drafting (Phase 8+)
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how to contribute



## Collaborators


| Lab                        | Role                                                                                       | Lead         | GitHub |
| -------------------------- | ------------------------------------------------------------------------------------------ | ------------ | ------ |
| Neurotech Hub (NTH)        | Engineering lead — electronics, firmware, host UI, fabrication, integration, dissemination | Matt Gaidica | TBD    |
| Animal Behavior Core (ABC) | UI/UX feedback, common task structures, behavioral benchmarking, animal metrics report     | TBD          | TBD    |
| Hengen Lab (HLAB)          | Custom experiment authoring, in vivo electrophysiology validation, recording sync          | Keith Hengen | TBD    |




## Related repositories

The repositories below hold the actual artifacts produced by this project. This
planning repo links to them; it does not contain their source.


| Domain                  | Repository                                                                                            | Owner      | Status                              |
| ----------------------- | ----------------------------------------------------------------------------------------------------- | ---------- | ----------------------------------- |
| Mechanical / CAD        | TBD                                                                                                   | NTH        | TBD                                 |
| Electronics / PCB       | TBD                                                                                                   | NTH        | TBD                                 |
| Module firmware         | [Neurotech-Hub/VFM](https://github.com/Neurotech-Hub/VFM)                                             | NTH        | Active — Node library               |
| Base station / host UI  | [Neurotech-Hub/VFM](https://github.com/Neurotech-Hub/VFM/tree/main/tools/dev_gui) (`tools/dev_gui`)   | NTH        | Active — alpha GUI + experiment API |
| Experiment API examples | [VFM templates](https://github.com/Neurotech-Hub/VFM/tree/main/tools/dev_gui/base_station/experiment) | NTH / HLAB | Built-in templates; HLAB custom TBD |
| Analysis / reports      | [VFM `run_report.py`](https://github.com/Neurotech-Hub/VFM/blob/main/tools/dev_gui/run_report.py)     | NTH / ABC  | Active — session CSV → HTML reports |


When a sub-repo comes online, replace `TBD` with the URL and update the status
column.

## Behavior reports

The base station writes a unified session CSV. [`run_report.py`](https://github.com/Neurotech-Hub/VFM/blob/main/tools/dev_gui/run_report.py)
turns that log into a self-contained, printable HTML report (inline SVG; stdlib
only). Design is picked from the session’s experiment name; unknown templates
fall back to a generic report. Details: [`docs/user-api.md`](docs/user-api.md).

```bash
cd tools/dev_gui
python run_report.py --list
python run_report.py EXP-Test-02 --open
python run_report.py "cohortA_*" --combine -o /tmp/cohortA.html
```


| Design                 | When it applies                         | Extra sections                                      |
| ---------------------- | --------------------------------------- | --------------------------------------------------- |
| `default`              | Any session (fallback)                  | Pellet accounting, latency, presence, funnel, faults |
| `free_feeding`         | `free_feeding` template                 | Intake rate, reload delays                          |
| `fixed_and_random`     | `fixed_and_random` template             | Per-node role (off / fixed / random)                |
| `probability_delivery` | `probability_delivery` template         | Delivery-site distribution                          |
| `two_armed_bandit`     | `two_armed_bandit` template             | Choice, block/reversal curves, WSLS                 |


`--combine` builds a comparative report (cohort table, learning curve, quality
matrix). Time alignment: `relative` (default), `wall`, `trial`, or `event:<name>`.
JSON export is planned (`--json`); metrics are already computed separately from HTML.

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


| Abbreviation | Expansion               |
| ------------ | ----------------------- |
| BOM          | Bill of Materials       |
| HW           | Hardware (status field) |
| MVP          | Minimum Viable Product  |
| SW           | Software (status field) |




### Hardware / electronics


| Abbreviation | Expansion                                               |
| ------------ | ------------------------------------------------------- |
| CAD          | Computer-Aided Design                                   |
| CAN          | Controller Area Network (bus)                           |
| LED          | Light-Emitting Diode                                    |
| MCU          | Microcontroller Unit                                    |
| PCB          | Printed Circuit Board                                   |
| PCBA         | Printed Circuit Board Assembly                          |
| TTL          | Transistor-Transistor Logic (digital sync signal level) |




### Software / interfaces


| Abbreviation | Expansion                         |
| ------------ | --------------------------------- |
| API          | Application Programming Interface |
| CLI          | Command-Line Interface            |
| UI           | User Interface                    |
| UX           | User Experience                   |




### External references


| Abbreviation | Expansion                                                                 |
| ------------ | ------------------------------------------------------------------------- |
| FED3         | Feeding Experimentation Device 3 (open-source rodent feeder, Kravitz lab) |




## License

Documentation, plans, and manuscript content in this repository are released
under [CC-BY-4.0](LICENSE). Code in linked sub-repositories carries its own
licensing.