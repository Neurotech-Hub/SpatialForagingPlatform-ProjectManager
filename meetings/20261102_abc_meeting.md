# Meeting: ABC weekly — pellet jam and 80/20 bandit

- **Date:** 2026-11-02
- **Time / duration:**
- **Attendees:** Jemin (NTH), Katie (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. Pellet jamming
2. Next experiment: FED3 and SFM in the same cage
3. Behavior-box base

## Discussion

- Weekly meeting with Katie.
- Pellet jamming is fixed by adjusting the plate position and with minor
  design updates.
- The base of the behavior box needed a shock absorber.
- The next experiment is a two-armed bandit at **80/20**, with FED3 and
  SFM in the same cage.

## Decisions

- Plate position is adjusted, with the minor design updates, as the fix
  for pellet jamming.
- Replace the behavior-box base so it has a shock absorber.
- Next ABC run: 80/20 two-armed bandit on FED3 and SFM together in one cage.

## Action items

| Owner | Action | Due |
| ----- | ------ | --- |
| NTH | Adjust the plate position and apply the minor design updates for pellet jamming | Done 2026-11-03 |
| NTH | Replace the behavior-box base with one that has a shock absorber | Done 2026-11-03 |
| ABC / NTH | Run the 80/20 bandit with FED3 and SFM in the same cage | Next experiment |

## Open questions / parking lot

- Katie’s error report, including the hopper, was not closed in this meeting.
- Presence misses and the 24-hour camera alignment are unchanged.

## References

- Related ADRs:
- Related docs: [`docs/failure-modes.md`](../docs/failure-modes.md)
- Prior jam:
  [2026-10-19](20261019_abc_meeting.md)
- Prior next-step (matched bandit):
  [2026-10-28](20261028_abc_meeting.md)
