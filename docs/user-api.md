# User API

Defines how specialized experiments are authored, executed, synchronized,
and analyzed.

Canonical authoring guide:
[packages/dev_gui/base_station/experiment/README.md](https://github.com/Neurotech-Hub/SFM/blob/main/packages/dev_gui/base_station/experiment/README.md).
Copy-from starters:
[packages/dev_gui/examples/templates/](https://github.com/Neurotech-Hub/SFM/tree/main/packages/dev_gui/examples/templates).
Analysis SDK:
[packages/sfm-analysis](https://github.com/Neurotech-Hub/SFM/tree/main/packages/sfm-analysis).

## Experiment language / API

Hybrid: a Python `build()` factory plus a JSON parameter schema. There is no
base class to inherit. Sessions **always start from the GUI** — pick a template,
fill the form, enter a session name, click Start. Nodes stay dumb (commands in,
events out); the template decides what to do next.

A GUI-hosted template is two files with the same name:

- `packages/dev_gui/experiments/<name>.json` — GUI form (parameters, types, defaults)
- `packages/dev_gui/base_station/experiment/templates/<name>.py` — `def build(...) -> Experiment`

Dropping both files is enough; there is no central registry. Optional matching
report design lives under `packages/sfm-analysis` (see Output format below).

## Python scripting

Allowed and required for custom tasks. `build()` returns a configured
`Experiment`. Register behavior with `@exp.on_*` decorators, write a sequential
generator with `@exp.script`, or mix both. Iterate `control.nodes` — never a
hard-coded node list — so the GUI node multi-select works.

```python
from ..runner import Experiment

def build(nodes=None, *, name="my_task", delay_s=2.0, **_):
    exp = Experiment(nodes=list(nodes or [1, 2, 3]), name=name)

    @exp.on_start
    def start(control):
        for n in control.nodes:
            control.dispense(n)

    @exp.on_pellet_taken
    def reload(control, event):
        control.after(delay_s, lambda: control.dispense(event.node_id),
                      node=event.node_id)

    return exp
```

## Device functions exposed to users

All actions go through `ExperimentControl` (`control`). Dispense is a single
choke point with three automatic vetoes (halted node, cycle already in flight,
pellet already on the plate) before anything is sent on CAN.

| Call | Effect |
| ---- | ------ |
| `control.dispense(node)` | Dispense on one node |
| `control.dispense(node, feed=False)` | Same motion, empty plate (bandit mimic) |
| `control.recover(node)` | Stop motion and clear a firmware fault |
| `control.broadcast_dispense()` / `broadcast_recover()` | Same, all session nodes |
| `control.bnc_pulse(duration_us=100)` | Pulse BNC OUT |
| `control.set_heartbeat_interval(node, ms)` | Reconfigure heartbeat rate |
| `control.after(seconds, cb, node=0)` / `every(...)` | Runner-clock timers |
| `control.counter` / `incr` / `set_counter` | Named integer counters |
| `control.next_trial()` | Advance trial counter (one delivery decision) |
| `control.log(name, node=0, **fields)` | Experiment row in GUI log + CSV |
| `control.stop(reason=...)` | End the session |

**Events** (`EventKind`): `ON_PLATE`, `LOADED`, `PELLET_TAKEN`,
`NO_FEED_PRESENTED`, `FEED_SKIPPED`, `FAULT`, phase events,
`PRESENCE_CHANGED`, `PG_CHANGED`, derived `DOME_OPENED` / `DOME_CLOSED`,
`NODE_ONLINE` / `NODE_OFFLINE`, and base-station `BNC_IN`, `SESSION_START`,
`SESSION_END`.

**Lifecycle**: `start_when(condition)`, `end_after(hours=…, pellets=…)`,
`end_when(condition)`. Shared helpers in `kit.py` (`session`, `wire_advance`,
`next_trial_wait`, `log_faults`).

At session activate the GUI broadcasts `CanCmd.SyncFlash` (status LED solid
~500 ms) and fires a coincident BNC OUT pulse. See
[`sync-and-recording.md`](sync-and-recording.md).

## Built-in templates

| Name | Behavior |
| ---- | -------- |
| `free_feeding` | Dispense all nodes at start; after each pellet taken, wait (`fixed_delay` or `presence_clear`) and re-dispense; end on duration and/or pellet cap |
| `fixed_and_random` | Per-node role (off / fixed / random); advance by fixed delay, presence-clear, or BNC |
| `probability_delivery` | Each cycle delivers on one node, chosen by weighted random draw |
| `two_armed_bandit` | Both arms move each trial, only one delivers; reward probability flips every `block_size` trials |

## Boundary: what users can do vs what NTH must support

| Capability | User-implementable | Requires NTH support |
| ---------- | ------------------ | -------------------- |
| New experiment template (`.py` + `.json`) | Yes — drop in `templates/` + `experiments/`, relaunch GUI | No |
| Custom report design / metrics | Yes — optional files under `packages/sfm-analysis` | No (falls back to `default.json`) |
| Session start / stop, node select, logging | GUI-hosted | NTH owns the GUI host |
| CAN protocol, discovery, MAC↔ID registry | No | Firmware + `discovery_manager` / `~/.sfm/mac_id_registry.json` |
| HAT GPIO (BNC, AEO, button) | Configure labels/actions in GUI | NTH owns `io_manager` and HAT |
| Session CSV column schema | No | `sfm-analysis` log schema; NTH if columns change |
| Firmware pinout / dispense FSM | No | Node library `SFM` |

## External hardware requirements

| Item | Role |
| ---- | ---- |
| Raspberry Pi 5 + custom CAN HAT | Base station; SocketCAN `can0` at 250 kbps |
| 12 V supply (8 A recommended) | Daisy-chained module power |
| RJ45 patch cables | CAN + addressing GPIOs between nodes |
| BNC cables | Sync I/O to cameras / ephys DAQs |
| Overhead camera | Align footage to the 500 ms status-LED flash |
| 120 Ω termination | Fixed on HAT; auto on last node when CAN-out is unplugged |

Support-tool BOM items should match this list when the production BOM is locked.

## Minimum viable custom experiment

Shipped tasks are the four built-in templates above. HLAB custom work: copy a
starter from `packages/dev_gui/examples/templates/` into `templates/` +
`experiments/`, relaunch the GUI. Optional matching report design:
`packages/sfm-analysis/examples/report_design/`. Tests can use
`exp.make_runner(); runner.inject(...); runner.step(now)` with no CAN.

## Output format

The base station writes one **unified 15-column session CSV**
(`<log_dir>/<session_name>.csv`) for every CAN frame, heartbeat, BNC edge, and
experiment row. Host timestamps, `run_id`, `trial`, `source`, and `fields_json`
are on every row. Restarting under the same session name **appends** and
increments `run_id`. Clock source is the base-station host clock (see
[`sync-and-recording.md`](sync-and-recording.md)). Default log dir: `~/sfm_logs`
(or an auto-detected external drive).

Reports are generated by **[sfm-analysis](https://pypi.org/project/sfm-analysis/)**
(`pip install sfm-analysis`, CLI `sfm-report`). On the Pi,
[`run_report.py`](https://github.com/Neurotech-Hub/SFM/blob/main/packages/dev_gui/run_report.py)
is a thin wrapper around that CLI. No pandas/matplotlib; charts are inline SVG.
Combined reports (`--combine`) overlay several sessions. Demo with no rig:
`sfm-report --demo --open`.

A report **design**
(`packages/sfm-analysis/src/sfm_analysis/report/designs/<name>.json`) lists
**sections** (Python functions under `report/sections/`). The design is chosen
from the session’s `experiment` field; anything unmatched uses `default.json`.
Per-experiment metrics live in `report/analyses/` so they can be tested without
parsing HTML. A `--json` export of those metrics is planned.

Session names are parsed into subject / cohort / day via regex patterns in
`~/.sfm/report_settings.json` (defaults cover `cohortA_M014_d3`-style names).
Unmatched names still report; identity fields stay empty.

## Cross-references

- Operator install / run: [README — Operations](../README.md#operations).
- Sync wiring and timing: [`sync-and-recording.md`](sync-and-recording.md).
- Function checks before running: [`function-checks.md`](function-checks.md).
- Analysis vocabulary and log columns:
  [ANALYSIS_GUIDE.md](https://github.com/Neurotech-Hub/SFM/blob/main/packages/sfm-analysis/docs/ANALYSIS_GUIDE.md).

## Open questions
