# Meeting: ABC monthly — 44-hour FED3 and SFM run

- **Date:** 2026-10-28
- **Time / duration:**
- **Attendees:** Matt Gaidica (NTH), Jemin (NTH), Katie (ABC), Carla (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Results of the multi-day experiment
2. Errors Katie saw during the run, including the hopper
3. Presence sensor still missing detections

## Discussion

- The experiment ran **44 hours**. FED3 ran a two-armed bandit at **20/80**,
which makes the pellet harder to get. SFM ran `free_feeding`.
- After about **5 hours** the mouse started taking pellets from SFM more often. Making  
FED3 harder is what moved the mouse onto SFM.
- Next run: two-armed bandit on **both** FED3 and SFM, using the same
trained mouse.
- Katie hit several errors during the run. She will send Jemin a report of
what she saw, including the hopper issue.
- Presence is still wrong in this data. The plan is to take the **24-hour**
camera recording, line its timestamps up with dome status, and find what
the missing presence detections correspond to.

## Decisions

- Next ABC experiment is a matched two-armed bandit on FED3 and SFM with  
the mouse that already learned with last experiment.
- Presence is not closed. Camera-to-dome alignment is the next diagnosis,
not another guess at the cause.

## Action items

| Owner       | Action                                                                                               | Due             |
| ----------- | ---------------------------------------------------------------------------------------------------- | --------------- |
| Katie (ABC) | Send Jemin the error report, including the hopper                                                    | TBD             |
| ABC / NTH   | Align the 24-hour camera recording with dome-status timestamps and trace the missing presence events | TBD             |
| ABC / NTH   | Run two-armed bandit on both FED3 and SFM with the same trained mouse                                | Next experiment |

## Open questions / parking lot

- What the hopper error was. It is in Katie’s report, not diagnosed here.
- Whether missing presence is a calibration miss, a sensor fault, or the
animal sitting where the pad does not see it.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md),
[`docs/user-api.md`](../docs/user-api.md)
- Prior findings (no presence in the log):
[2026-10-19](20261019_abc_meeting.md)
- Follow-up (meeting 2026-11-02; plate and base done 2026-11-03): next
  run is an 80/20 bandit with FED3 and SFM in the same cage.
  [Note](20261102_abc_meeting.md).
