# User API

Defines how specialized experiments are authored, executed, synchronized,
and analyzed.

## Experiment language / API

_Decision: scripting language, config DSL, GUI-only, or hybrid. Justification._

## Python scripting

_Allowed, required, or optional. If allowed, what does the API surface look
like?_

## Device functions exposed to users

_Reward delivery, LED control, sensor read, sync output, event logging,
module discovery, etc. List each function with a one-line signature and
expected timing guarantees._

## Boundary: what users can do vs what NTH must support

| Capability | User-implementable | Requires NTH support |
| ---------- | ------------------ | -------------------- |

## External hardware requirements

_Cables, adapters, sync boxes, cameras, recording-system I/O. Each item
should appear in the support-tool BOM._

## Minimum viable custom experiment

_Description of the MVP experiment HLAB will implement in Phase 3B._

## Output format

The base station writes one **unified 15-column session CSV**
(`<log_dir>/<session_name>.csv`) for every CAN frame, heartbeat, BNC edge, and
experiment row. Host timestamps, `run_id`, `trial`, `source`, and `fields_json`
are on every row. Restarting under the same session name **appends** and
increments `run_id`. Clock source is the base-station host clock (see
[`sync-and-recording.md`](sync-and-recording.md)).

[`run_report.py`](https://github.com/Neurotech-Hub/VFM/blob/main/tools/dev_gui/run_report.py)
reads that CSV and writes a self-contained HTML file next to it
(`<log_dir>/reports/<session>_report.html`). No pandas/matplotlib; charts are
inline SVG. Combined reports (`--combine`) overlay several sessions.

A report **design** (`tools/dev_gui/reports/<name>.json`) lists **sections**
(Python functions under `base_station/report/sections/`). The design is chosen
from the session’s `experiment` field; anything unmatched uses `default.json`.
Per-experiment metrics live in `base_station/report/analyses/` so they can be
tested without parsing HTML. A `--json` export of those metrics is planned.

Session names are parsed into subject / cohort / day via regex patterns in
`~/.sfm/report_settings.json` (defaults cover `cohortA_M014_d3`-style names).
Unmatched names still report; identity fields stay empty.

## Cross-references

- Sync wiring and timing: [`sync-and-recording.md`](sync-and-recording.md).
- Function checks before running: [`function-checks.md`](function-checks.md).

## Open questions
