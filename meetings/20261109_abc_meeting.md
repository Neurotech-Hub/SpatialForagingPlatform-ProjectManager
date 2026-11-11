# Meeting: ABC weekly — old firmware jam and fixed-delay idle

- **Date:** 2026-11-09
- **Time / duration:**
- **Attendees:** Jemin (NTH), Katie (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Pellet jam still occurring
2. Experiment going idle during a run
3. Mobile error alerts
4. November experiment window

## Discussion

- Weekly meeting with Katie.
- Pellet jamming was still happening. One node was still on older
firmware, and that is what caused it. The 2026-11-03 plate adjustment
did not cover a node that had not been updated.
- The experiment goes idle during a run. ABC advances on a fixed delay.
Hengen Lab advances on presence clearance, so this path was not seen
there. With a fixed delay, a software bug misses the CAN trigger
because of the fixed cutoff time. A firmware update can add a flag
for that.
- An error alert on a phone would be much more convenient during a
multi-day run. Jemin will implement it.
- This is the last week ABC runs an experiment in November. After this
week’s run, Katie can start again in December.

## Decisions

- Match firmware across ABC nodes before treating pellet jam as a
mechanical problem.
- Fixed-delay idle is a missed CAN trigger at the cutoff. The fix is a
firmware flag, not a change to the presence-clearance path.
- November animal runs stop after this week and resume in December.

## Action items

| Owner       | Action                                                                               | Due                               |
| ----------- | ------------------------------------------------------------------------------------ | --------------------------------- |
| NTH         | Put the lagging ABC node on the current firmware                                     | Completed 11-10                   |
| NTH (Jemin) | Add a firmware flag so a fixed-delay run does not miss the CAN trigger at the cutoff | Completed 11-10                   |
| NTH (Jemin) | Implement a mobile error alert                                                       | Completed 11-10                   |
| Katie (ABC) | This week’s run is the last in November; next runs start in December                 | Week of 2026-11-09, then December |

## Open questions / parking lot

- Katie’s earlier error report, including the hopper, was not closed here.
- Presence misses and the 24-hour camera alignment are unchanged.
- Which node was on the old firmware, once it is confirmed flashed.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md),
[`docs/user-api.md`](../docs/user-api.md)
- Plate adjustment that did not cover the old-firmware node:
[2026-11-02](20261102_abc_meeting.md)
- Earlier idle-after-pause bug (different cause, fixed 2026-10-19):
[2026-10-19](20261019_abc_meeting.md)
