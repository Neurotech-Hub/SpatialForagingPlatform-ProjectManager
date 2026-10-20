# Meeting: ABC — first-run findings and failure points

- **Date:** 2026-10-19
- **Time / duration:**
- **Attendees:** Jemin (NTH), Katie (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Findings from last week’s experiment
2. Failure points: nozzle jam, a node that stopped feeding, missing
   presence data, GUI freeze

## Discussion

- Two pellets tried to feed into the nozzle at once and jammed there.
- One node went idle and never fed again. Jemin confirmed this is a
  software bug in `free_feeding`: after a failure pauses the whole
  experiment, nothing triggers the next pellet load. That path is fixed.
- Last week’s log has no presence data. Presence has to be calibrated
  before an experiment starts.
- The GUI froze after about 2–3 days. The on-screen event log was
  growing enough to slow the Raspberry Pi, most likely by using RAM.
  Heartbeats are no longer drawn in that log. They are still written
  to the SD card, and the GUI log is lighter.
- One mouse took **195** pellets from the FED and **17** from SFM. The
  log shows the delivery arm retracted twice because the dome opened
  before the pellet was delivered.
- Katie suggested leaving a clamshell in the cage, filled with pellets,
  so a mouse can spend hours getting used to the shape. A mouse may
  chew the PLA. Jemin can print a clamshell without the auto-close
  spring for that. The raise-retract rule can be made optional in
  firmware if a later run still needs it.

## Decisions

- `free_feeding` reloads the next pellet after an experiment-wide pause.
- GUI event log omits heartbeats. The SD card log still keeps them.
- Calibrate presence before the next experiment.
- In-cage training dome and an optional retract are not built yet.

## Action items

| Owner | Action | Due |
| ----- | ------ | --- |
| NTH | Calibrate presence before the next ABC run | Before next experiment |
| NTH | Print a clamshell dome without auto-close for in-cage training | TBD |
| NTH | Make raise-retract optional in firmware only if a later run needs it | If needed |

## Open questions / parking lot

- What let two pellets enter the nozzle together.
- Whether an in-cage PLA dome survives chewing long enough to be useful.
- HLAB node that does not finish the raise was not part of this visit.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md),
  [`docs/user-api.md`](../docs/user-api.md),
  [`docs/calibration.md`](../docs/calibration.md)
- Prior upgrade:
  [2026-10-12](20261012_abc_meeting.md)
