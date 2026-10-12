# Dispense cycle

How an SFM node delivers a pellet, how it knows the pellet was taken, and what it reports along the way.


## Sensors

Each node has three optical sensors, named for the job they do.

| Sensor | Pin | Asserted when | Job |
| ------ | --- | ------------- | --- |
| **Pellet sensor** | GPIO46 | beam broken (pin LOW) | A pellet is sitting on the plate. Latching: it stays asserted from the moment the pellet lands until the pellet leaves |
| **Load position sensor** | GPIO45 | beam broken (pin LOW) | The actuator is at the load position (fully down) |
| **Dome sensor** | GPIO44 | pin HIGH (idle LOW) | The dome is lifted. The dome is spring-returned, so every access is a clean lift-and-release bout |

All three are debounced in firmware by `kSensorDebounceMs` before any logic or reporting acts on them.

A fourth input, **animal presence**, is the capacitive touch pad. It is independent of the dispense cycle and
is reported for behavioral context only.

The pellet sensor sits on the plate and reports occupancy in every state. Because it holds its state, the node
always knows whether the plate is occupied — before a dispense, during travel, and after an access.

## The cycle

```
 Dispense
    │
    ▼
 Occupancy check (pellet sensor)
    │
    ├─ plate occupied ─► FeedSkipped ──────────────────────┐
    │                                                      │
    └─ plate empty ─► Lowering ─► Feeding                  │
                              │                            │
                         presence asserts                  │
                              │                            │
                         OnPlate ──────────────────────────┤
                                                           ▼
                                                        Raising
                                                           │
                         dome open before start ─► DomeHold (motors off)
                         dome opens before 80% ──► Retracting ─► DomeHold
                         pellet leaves early ────► reload (up to 3), then Fault (PelletLost)
                         pellet leaves after 80% with dome open ─► early take
                                                           │
                                                          Loaded
                                                           │
                       ┌─ dome lifts ──────► DomeOpened ───┤
                       │                                   │
                       └─ dome held open ──► DomeOpenWarning
                                                           │
                               pellet sensor clears ─► PelletTaken ─► Idle
```

**Occupancy check.** On `Dispense` the node reads the pellet sensor first. If a pellet is already on the
plate it does not lower and does not run the feed wheel: it reports `FeedSkipped` and raises what is there
(or stays presented if the plate is already elevated). A node never stacks a second pellet on an occupied plate.

**Lowering.** Only when the plate is empty. The actuator seeks the load position: if it is already there it
first moves clear, then approaches until the load position sensor asserts. Both directions are budgeted by
`kDefaultLowerSteps` and `kDefaultLowerTimeoutMs`.

**Feeding.** M1 turns the pellet wheel until the pellet sensor asserts, which confirms a pellet has arrived on
the plate. The node halts M1, reports `OnPlate`, and begins the raise in the same tick. If no pellet
arrives within `kDefaultFeedTimeoutMs` — an empty hopper or a wheel jam — the node faults with `FeedTimeout`.

**Raising.** M2 lifts the plate by `kDefaultRaiseSteps` from the load position. The raise does not start
while the dome is open. The node waits in `DomeHold` (motors off) until the dome has stayed closed for
`kDomeCloseSettleMs` (500 ms). `DomeOpenWarning` can still fire at 30 s during that wait.

The commit point is **80%** of the raise (`kRaiseCommitPct`). During travel the load-position sensor must
clear within `kLoadClearOnRaiseMs` (otherwise `Jam`), and on a fed cycle the pellet sensor is watched:

- Dome opens before 80% (fed or empty-plate): `Retracting`. The plate returns to the drop position, then
  `DomeHold` again. The pellet sensor and the dome are ignored on the way down.
- Pellet sensor stays clear for `kPelletLostMs` (500 ms) before 80%, or at/after 80% with the dome closed:
  same retract, logged as a reload (`PelletReload`, reason lost-during-raise). Up to `kMaxPelletReloads`
  (3) reloads in one dispense. The next miss faults `PelletLost`.
- Pellet sensor stays clear for 500 ms at/after 80% with the dome open: the animal took it. The plate
  finishes the raise, then the node emits `Loaded` and `PelletTaken` and returns to `Idle`.

A no-feed (empty-plate) cycle follows the same dome hold and retract. It does not reload, because there
was no pellet to lose.

**Presented.** The pellet is available to the animal. The node stays here, watching two things:

- Every dome lift reports `DomeOpened`. There is no suppression window — one event per bout, and the
  spring return guarantees each bout is a distinct edge.
- A dome left open for `kDomeOpenWarnMs` reports `DomeOpenWarning` once per bout. This is a warning, not a
  fault: the node keeps operating.

**Taken.** When the pellet sensor clears for `kPelletTakenConfirmMs`, the pellet is gone. The node reports
`PelletTaken` and returns to `Idle` — the cycle is complete. The confirm window rejects momentary sensor
flicker as the animal reaches past the beam.

A node never reloads on its own. Deciding when the next `Dispense` goes out belongs to the base station or the
running experiment.

## Events

Node → base on CAN ID `0x300 + nodeId`. Byte 0 is the event code.

| Code | Event | Extra payload | Meaning |
| ---- | ----- | ------------- | ------- |
| `0x01` | `OnPlate` | count LE16 | The pellet sensor confirmed during loading; the raise is starting |
| `0x02` | `Loaded` | count LE16 | The plate reached the top; the pellet is ready |
| `0x03` | `DomeOpened` | count LE16, pellet present | The dome was lifted while a pellet was presented |
| `0x04` | `Fault` | `ServiceStatus` | Motion or delivery failure; sticky until `Abort` |
| `0x05` | `Pong` | — | Reply to `Ping` |
| `0x06` | `InputChanged` | input id, active | A sensor changed state |
| `0x07` | `Lowering` | count LE16 | Phase entered: seeking the load position |
| `0x08` | `Loading` | count LE16 | Phase entered: pellet wheel turning |
| `0x09` | `Raising` | count LE16 | Phase entered: lifting the plate |
| `0x0A` | `DomeOpenWarning` | count LE16 | The dome has been open for `kDomeOpenWarnMs` |
| `0x0B` | `PelletTaken` | count LE16, dome open | The pellet left the plate; retrieval confirmed |
| `0x0C` | `FeedSkipped` | count LE16 | A dispense arrived with the plate occupied; feed and lower were skipped |
| `0x12` | `DomeHold` | count LE16 | At the drop position, waiting for the dome to stay closed |
| `0x13` | `Retracting` | count LE16 | Plate returning to the drop position |
| `0x14` | `PelletReload` | count LE16, reason, attempt | Pellet missing; the wheel is loading again. Reason 1 = lost during raise, 2 = missing after retract. `attempt` is 1-based and stops at 3 |

`count` on these frames is the node's power-on total of `Loaded` milestones. Firmware zeroes it at boot
and does not clear it on Recover or on a new experiment. The number operators and reports use is the
**session** total, owned by the base station. See [Session pellet counts](#session-pellet-counts).

The context byte on dome and take events separates intent from accident. `DomeOpened` carries whether a
pellet was on the plate at the moment of the lift, so an opening with an empty plate is recognizable as
exploration rather than retrieval. `PelletTaken` carries whether the dome was open when the plate emptied: open
means a normal retrieval, closed means the pellet left without an access and the mechanism deserves a look.

`InputChanged` payloads are `[0x06, inputId, active]` with input IDs pellet sensor = `1`, load position = `2`,
dome = `3`, animal presence = `4`. These fire on every debounced edge so the base station sees sensor activity
immediately; heartbeats are the periodic recovery snapshot.

## Faults

A fault halts both motors, latches a status code, lights the status LED solid, and holds the node in `Fault`
until it receives `Recover`.

| Code | Cause |
| ---- | ----- |
| `Timeout` | A motion phase exceeded its budget — no pellet loaded (`FeedTimeout`), or the actuator never reached its target (`ActuatorTimeout`) |
| `Jam` | The load position sensor did not clear after the raise started; the plate is obstructed |
| `PelletLost` | The pellet was missing and this dispense already used its 3 reloads. The base station auto-recovers and dispenses again. A motor jam or a very low pellet store does not auto-recover |

## Heartbeat snapshot

Node → base on CAN ID `0x200 + nodeId`, sent every `kDefaultHeartbeatIntervalMs` (runtime-configurable via
`SetConfig`).

| Byte | Content |
| ---- | ------- |
| 0 | Dispense state |
| 1–2 | Pellets presented (LE16) |
| 3 | Animal presence (touch) |
| 4 | Sensor bits: `bit0` pellet present, `bit1` at load position, `bit2` dome open |
| 5 | Fault code |
| 6–7 | Pellets taken (LE16) |

Carrying both counts means a node that reconnects mid-session reports delivery and consumption together, with
no need to replay the event log. Those bytes are the node's power-on counters. The session totals below are
what the GUI and the CSV call a pellet.

## Session pellet counts

The base station keeps a per-node ledger for the open session (`pellet_ledger.py`):

- **Presented** advances on `Loaded`.
- **Taken** advances on `PelletTaken`.

Each CAN frame and heartbeat is a witness of the node's counter. The session total moves by the **delta**,
so a dropped `Loaded` is filled in by the next frame or heartbeat that still carries the count. A backward
jump (node power-cycled) re-baselines and leaves the session total where it was.

The GUI shows both numbers on the experiment panel (`presented · taken`) and on each node tile
(`presented / taken`).

Restarting the GUI and starting the **same session name** continues those totals, and the trial counter,
from the existing CSV. The new run logs `session_resumed` with the offsets it carried in. A different
session name starts at zero. The node's own counter still runs from power-on either way.

## What the base station can conclude

The pellet sensor turns several inferences into measurements:

- **Consumption, not delivery.** `Loaded` counts what the hardware offered; `PelletTaken` counts what
  the animal actually took. The gap between the two counts is the untouched pellets.
- **Unrewarded openings.** A dome bout that closes with the pellet sensor still asserted is an access that did
  not end in retrieval — a distinct behavioral event from a successful one.
- **Retrieval latency.** The interval from `Loaded` to `PelletTaken` is a per-pellet measure of how
  quickly the animal engaged.
- **Consumption-paced reloading.** An experiment can wait for `PelletTaken` before dispensing again, so the
  session follows the animal rather than a fixed schedule. The built-in free-feeding template works this way.
- **Abandonment.** A pellet presented and never taken is visible directly, which lets a task stop reloading
  in front of a disengaged animal instead of accumulating stale pellets.
