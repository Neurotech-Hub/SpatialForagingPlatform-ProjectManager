# Synchronization and Recording

How the platform marks experiment start for cameras and recording systems, and
how BNC I/O will later start and stop a session from an external TTL.

## Sync ownership

The **base station** is the time master. Nodes do not generate TTL; they only
flash their upward-facing status LED when the base station broadcasts
`CanCmd::SyncFlash`. All BNC I/O lives on the Raspberry Pi CAN HAT.

## Camera sync (current)

When a session **activates** (GUI Start, or later if the template has a deferred
`start_when`), the base station:

1. Broadcasts `CanCmd::SyncFlash` (`0x0A`) to all nodes.
2. Each node holds its **upward-facing status LED solid ON for 500 ms** (payload
  can override duration; firmware clamps 50–5000 ms).
3. Fires a coincident **BNC OUT** pulse so ephys/video DAQs can latch the same
  instant.

Purpose: a visible flash in overhead camera footage, aligned to the same moment
as the BNC pulse and the session-start log row — no manual clapperboard.

A node already in **Fault** ignores the flash (its status LED is already solid
for the fault). Those node IDs are listed in the logged `SYNC` row.

## BNC I/O


| Connector | Direction       | Role today                                                                                        | Planned                        |
| --------- | --------------- | ------------------------------------------------------------------------------------------------- | ------------------------------ |
| BNC Out   | Base → external | Pulse at session start (with the LED flash); can also pulse on a chosen CAN event (e.g. `Loaded`) | Same                           |
| BNC In 1  | External → base | Logged edges; templates can react (`@exp.on_bnc_in`)                                              | **TTL start / end experiment** |
| BNC In 2  | External → base | Secondary input (same as In 1)                                                                    | Spare or second trigger        |


Signals run on an isolated **5 V** rail. HAT Schmitt inverters invert the BNC
inputs in hardware; host software undoes that so HIGH means TTL present.
Default BNC OUT pulse width is **10 ms** (configurable). Channels are 0-based:
`0` = IN1, `1` = IN2.

## Planned: TTL start / stop

At some point an external system (camera controller, Open Ephys, SpikeGLX, etc.)
should **start and end the experiment** over BNC rather than the GUI button.

Intended mapping (not locked):

- Rising edge on a BNC input → **start** experiment (same session-activate path
as GUI Start, including the 500 ms LED flash + BNC OUT).
- Falling edge, or a pulse on the other input → **stop** experiment.

The HAT and GUI already have per-edge action placeholders (`start_experiment` /
`stop_experiment`). This is **not** yet the operator workflow — polarity, which
input, pulse vs level, and whether a start TTL is required before GUI Start all
still need a decision.

## Timestamp schema

Every CAN frame, heartbeat, BNC edge, and experiment row lands in one session
CSV (`<log_dir>/<session_name>.csv`) with host timestamps, `run_id`, `trial`,
`source`, and `fields_json`. Restarting under the same session name **appends**
and increments `run_id`. Clock source is the base-station host clock; nodes do
not carry a shared experiment clock in the 11-bit CAN ID.

## Compatibility targets

- Overhead video — align frames to the 500 ms status-LED flash.
- Electrophysiology DAQs that accept 5 V TTL on BNC (Open Ephys, SpikeGLX, and
HLAB rigs). Exact pulse width, polarity, and jitter budget are still open.



## Verification

1. Start a session from the GUI; all online nodes flash the top LED ~500 ms;
  BNC OUT pulses; a `SYNC` row appears in the log.
2. Faulted nodes do not flash; they appear in that `SYNC` row.
3. (Once TTL start/stop is enabled) pulse BNC IN and confirm session start/stop
  plus the same LED/BNC OUT mark.

See `[function-checks.md](function-checks.md)` for HAT BNC bring-up (`test_hat.py`).

## Open questions

- BNC IN polarity and which connector starts vs stops.
- Required vs optional TTL start (GUI-only still allowed?).
- BNC OUT pulse width / polarity lock for HLAB DAQs.
- Whether session **end** also flashes LEDs, or only start.

