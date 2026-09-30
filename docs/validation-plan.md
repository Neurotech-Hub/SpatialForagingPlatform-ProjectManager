# Validation Plan

Defines what "validated" means for the Spatial Foraging Platform and how we
demonstrate it.

## Validation goals

Three pillars, mirroring the grant proposal:

1. **Engineering** — hardware reliability, communication integrity,
   reproducible build.
2. **Behavior** — animals engage robustly and self-initiate; reward delivery
   is reliable; common task structures run end-to-end.
3. **Recording** — compatibility with chronic electrophysiology and video;
   recovery of canonical signals (e.g. hippocampal spatial tuning,
   striatal/ACC decision-variable encoding).

## MVP validation (Phase 3B)

_One custom experiment running end-to-end with at least one animal._

## Field-test validation (Phase 5)

Phase 5 is planned for weeks 25–28 (2026-11-30 to 2026-12-27), after the
9-node PCBA and enclosure platform in Phase 4. It is **not** the current
window: ABC’s first two-node behavior box is due Week 15; n=9 field tests
follow once those boards and the enclosure are in hand.

### Reliability metrics

_Pellet delivery success rate, interaction sensing accuracy, mechanical
failures per module-day, communication errors per hour._

### Behavior metrics

_Engagement, session structure, task acquisition, exploration pattern._

### Recording metrics

_Sync precision, recording stability, recovery of canonical signals._

## Reference datasets

_Datasets that will be released alongside the manuscript. Format, hosting,
DOI plan._

## Analysis examples

Session analysis and printable HTML reports live in
[`packages/sfm-analysis`](https://github.com/Neurotech-Hub/SFM/tree/main/packages/sfm-analysis)
(`pip install sfm-analysis`, CLI `sfm-report`). Domain vocabulary, log columns,
and derived metrics:
[ANALYSIS_GUIDE.md](https://github.com/Neurotech-Hub/SFM/blob/main/packages/sfm-analysis/docs/ANALYSIS_GUIDE.md).
Operator commands: [README — Operations](../README.md#operations).

## Statistical and reporting plan

Session CSVs are the source of truth. HTML behavior reports
([`sfm-report`](https://github.com/Neurotech-Hub/SFM/tree/main/packages/sfm-analysis);
Pi wrapper [`run_report.py`](https://github.com/Neurotech-Hub/SFM/blob/main/packages/dev_gui/run_report.py))
are the operator-facing summary for ABC/HLAB: pellet accounting, retrieval
latency, presence, interaction funnel, faults, plus task-specific metrics
(bandit choice / WSLS / reversal). Combined reports support cohort tables and
learning curves. Pre-registered n, effect sizes, and success thresholds for
the manuscript are still open.

## Open questions
