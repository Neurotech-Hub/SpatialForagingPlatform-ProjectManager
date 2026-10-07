# Meeting: HLAB — two-arm learning and raise faults

- **Date:** 2026-10-05
- **Time / duration:**
- **Attendees:** Jemin (NTH), Ravi Chopra (HLAB)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Ravi’s two-armed bandit analysis
2. `PelletLost` during raise, and a node that does not fully rise
3. Presence-calibration default

## Discussion

- Ravi shared analysis from the HLAB two-module setup. Mice learn the
  two-arm task and bias toward the rich arm. After the rich arm shifts,
  they take a few trials and learn the new rich arm.
- Two hardware issues:
  1. `PelletLost` during raise.
  2. One node does not raise all the way to the top. Cause not identified
     in the meeting.
- Ravi suggested touch factor **80** as the default for calibration.
- Follow-up after the meeting (Jemin, from the logs): `PelletLost` during
  raise is not a pellet falling off the plate. A mouse sitting at an open
  dome grabs the pellet while the plate is still rising, the pellet sensor
  goes empty, and the module reports `PelletLost`.

## Decisions

- Next firmware safeguard, not built yet: do not let a mouse take a pellet
  while the dome is open during the raise.
  - If the dome is open, keep the pellet at the loading position.
  - If the dome opens while the pellet is rising, lower the actuator
    immediately so the mouse cannot take the early pellet.

## Action items

| Owner | Action |
| ----- | ------ |
| NTH | Dome-open safeguard: hold at load if the dome is already open; abort the raise and lower if the dome opens mid-travel |
| NTH | Find why one HLAB node does not reach the top |
| NTH | Consider touch factor 80 as the calibration default |

## Open questions / parking lot

- Which node fails to finish the raise, and whether that is a stall,
  a sensor, or a mechanical stop.
- Touch factor 80 is Ravi’s recommendation; it is not yet the shipped
  default.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md)
  (`PelletLost`), [`docs/dispense-cycle.md`](../docs/dispense-cycle.md)
- Related PROJECT.md phase / goals:
  [Phase 3A — ABC + HLAB Feedback + Maintainability](../PROJECT.md#phase-3a--abc--hlab-feedback--maintainability)
- HLAB delivery:
  [2026-08-14](20260814_meeting.md)
