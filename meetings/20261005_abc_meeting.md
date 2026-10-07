# Meeting: ABC weekly — first experiment startup with Katie

- **Date:** 2026-10-05
- **Time / duration:**
- **Attendees:** Jemin (NTH), Katie (ABC)
- **Phase:** Phase 3A — ABC + HLAB Feedback + Maintainability

## Agenda

1. First experiment startup on the ABC behavior box
2. Procedures required before a run, and day-to-day system operation

## Discussion

- Weekly meetup. Jemin met Katie for the first experiment startup.
- Jemin walked through the procedures required before an experiment and
  how to operate the system.
- If an experiment is paused, mice can be left without food until the
  next morning. ABC wants to run experiments overnight.
- It is unclear whether the experiment runs with a FED4 in the same box
  or as a separate run with the mice.
- Katie flagged the data: it may take about 24 hours for mice to find
  food. In that case ABC needs a second feeding option.
- Pellet counts are hard to get from the event log. Katie wants the
  count on the GUI so it takes less effort to read.

## Decisions

- No protocol or software change was locked in this meeting. Overnight
  running, a backup feeding path, and a GUI pellet count are open ABC
  requests.

## Action items

| Owner | Action |
| ----- | ------ |
| NTH | Show pellet counts on the GUI, not only in the event log |
| NTH / ABC | Define overnight running, including what happens if a session is paused and mice still need food |
| NTH / ABC | Decide whether FED4 shares the box with SFM or mice run on SFM alone |
| ABC | Specify the second feeding option if mice have not found food within about 24 hours |

## Open questions / parking lot

- Pause policy: who resumes, and how mice are fed if a run stops overnight.
- FED4 coexistence vs a separate SFM-only run.
- Which backup feeder or manual feed covers the first 24 hours.

## References

- Related ADRs:
- Related docs: [`docs/user-api.md`](../docs/user-api.md),
  [`docs/function-checks.md`](../docs/function-checks.md)
- Related PROJECT.md phase / goals:
  [Phase 3A — ABC + HLAB Feedback + Maintainability](../PROJECT.md#phase-3a--abc--hlab-feedback--maintainability)
- Prior ABC setup:
  [2026-09-30](20260930_meeting.md)
