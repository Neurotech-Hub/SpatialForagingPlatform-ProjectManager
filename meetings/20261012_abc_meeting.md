# Meeting: ABC — firmware and GUI upgrade, 48-hour plan

- **Date:** 2026-10-12
- **Time / duration:**
- **Attendees:** Jemin (NTH), Katie (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Upgrade firmware and GUI on the ABC box
2. Pellet counter and auto-recover
3. How the first animal experiments will run

## Discussion

- Jemin met Katie and upgraded all module firmware and the GUI on her box.
- The GUI pellet counter from the 2026-10-05 request is on this build.
- Auto-recover is in for faults such as `PelletLost`. The failures that
  still need a person are a motor jam and a very low pellet store.
- GUI errors that were blocking a run were fixed. The box is ready to
  start a new experiment.
- For training, the clamshell dome is left open so mice can learn the
  port.
- Two FED devices sit in the same box as the SFM modules.
- Each experiment runs **48 hours**. Cohort size is **8–10 adult**
  animals, split evenly between males and females.
- Katie wrote the procedure for review:
  [ABC procedure (Google Doc)](https://docs.google.com/document/d/1izdzfEGDUeT35xuk4L2V7V4M0QKjw6MNcU3AjcuHySA/edit?tab=t.3mnl8xp5jlwk).

## Decisions

- ABC’s first runs use an open clamshell dome, two FED devices in the
  same box, and a 48-hour session.
- Animals: 8–10 adults per experiment, half male and half female.
- Auto-recover handles faults such as a lost pellet. Motor jam and a
  very low pellet store do not.

## Action items

| Owner | Action | Due |
| ----- | ------ | --- |
| Katie (ABC) | Start the next experiment on the upgraded box | Next run |
| NTH | Review Katie’s procedure doc | Before the first 48-hour run |
| NTH | Support the 48-hour runs if jam or low-pellet faults stop a module | During runs |

## Open questions / parking lot

- HLAB node that does not finish the raise is unchanged by this visit.
- Touch factor 80 is still only a suggested calibration default.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md),
  [`docs/user-api.md`](../docs/user-api.md)
- Procedure:
  [Google Doc](https://docs.google.com/document/d/1izdzfEGDUeT35xuk4L2V7V4M0QKjw6MNcU3AjcuHySA/edit?tab=t.3mnl8xp5jlwk)
- Related PROJECT.md phase / goals:
  [Phase 3A — ABC + HLAB Feedback + Maintainability](../PROJECT.md#phase-3a--abc--hlab-feedback--maintainability)
- Prior ABC startup:
  [2026-10-05](20261005_abc_meeting.md)
- Follow-up (2026-10-19): first-run findings, including the nozzle jam
  and the free-feeding pause bug.
  [Note](20261019_abc_meeting.md).
