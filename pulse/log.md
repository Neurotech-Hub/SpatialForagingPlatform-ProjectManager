# Pulse log

Newest week first. Project weeks start Monday 2026-06-08. Entries below are
reconstructed from this repo, linked SFM work, and meeting notes — fill gaps
where memory is better than the paper trail.

---

# Week 16 — 2026-09-28 to 2026-10-04

- **Phase:** Phase 3A — ABC + HLAB feedback and maintainability
- **HW / SW:** Alpha / Alpha
- **Meetings this week:**
  [2026-09-30 ABC monthly — box setup and Katie training](../meetings/20260930_meeting.md)

## Shipped

- ABC two-node behavior box set up on site (2026-09-30).
- Katie (ABC) trained on basic operations and system handling.
- Matt walked the monthly ABC meeting through updates since the last ABC
  meeting: sensors, logging, and the reporting tool are behaving.

## In progress

- Susan Maloney (ABC) protocol approval. Clarification question is in;
  hope is approval by Friday 2026-10-02.
- HLAB remains on the Aug 14 two-module setup.

## Blocked / risks

- ABC animal experiments cannot start until the behavior protocol is
  approved.

## Next week

- If approval lands, Katie leads the first ABC experiments (week of
  2026-10-05).
- NTH supports those first runs.

## Notes

- Phase 3A window is weeks 15–17 (through 2026-10-11).

---

# Week 15 — 2026-09-21 to 2026-09-27

- **Phase:** Phase 3A — ABC + HLAB feedback and maintainability
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded

## Shipped

- No meeting recorded. ABC box was not yet installed (setup landed
  2026-09-30).

## In progress

- ABC two-node behavior box, carried from the Week 15 delivery target
  into the Week 16 install.

## Blocked / risks

-

## Next week

- Install the box at ABC and train an operator.

## Notes

- Rebaseline had targeted Week 15 for delivery. Install and training
  happened at the start of Week 16.

---

# Week 14 — 2026-09-14 to 2026-09-20

- **Phase:** Alpha deployments closing; Phase 3A starts Week 15
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none yet (three earlier events filled in:
[2026-07-21](../meetings/20260721_meeting.md),
[2026-08-14](../meetings/20260814_meeting.md),
[2026-08-31](../meetings/20260831_meeting.md))

## Shipped

- Meeting notes for the three major cross-lab events to date.
- Rebaselined `[PROJECT.md](../PROJECT.md)`: Phases 3A–9 no longer treated
as done/active. Production PCBA and n=9 field tests have not started.



## In progress

- ABC first two-node behavior box (HLAB assembly recipe), due next week.
- HLAB continues on the Aug 14 two-module two-armed-bandit setup.



## Blocked / risks

- Phase 5 field tests cannot start until 9-node PCBAs and the enclosure
platform exist (Phase 4, Nov 2026). Calling this week “Phase 5” was
the old plan, not the bench.



## Next week

- Deliver ABC two-node behavior box (Week 15).
- Start Phase 3A: customize experiments; collect ABC + HLAB feedback and
maintainability notes.



## Notes

- Timeline rebaseline 2026-09-16. Original plan had Phase 5 in weeks
12–16; actual sequence is 3A (feedback) → 3B (custom MVP) → 4 (9-node
PCBA + enclosure, 3–4 week fab) → 5 (field tests).

---



# Week 13 — 2026-09-07 to 2026-09-13

- **Phase:** Alpha deployment — ABC two-node box in build
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded

## In progress

- ABC two-node behavior box (HLAB assembly recipe).
- HLAB two-module two-armed bandit still in use.

## Next week

- Finish ABC two-node box for Week 15 delivery.



## Notes

- Later rebaseline (Week 14): n=9 field tests are Phase 5 (Nov–Dec), not
this window.

---



# Week 12 — 2026-08-31 to 2026-09-06

- **Phase:** Alpha deployment — ABC install path
- **HW / SW:** Alpha / Alpha
- **Meetings this week:**
[2026-08-31 ABC site tour](../meetings/20260831_meeting.md)



## Shipped

- ABC site tour (2026-08-31): retrofit vs replicate the HLAB behavior-box
assembly; cleaning protocol; ABC first pair will follow the Hengen
recipe.
- Planning hub brought in line with Neurotech-Hub/SFM (VFM→SFM rename,
operator workflow, `sfm-analysis`).



## In progress

- ABC two-node behavior box build (not n=9 field test).
- HLAB continues on the Aug 14 two-module setup.  


## Next week

- Continue ABC two-node box; HLAB remains on delivered hardware.



## Notes

- Later rebaseline (Week 14): this tour was **not** Phase 5 start. Phase 5
is n=9 field tests after the 9-node PCBA and enclosure (weeks 25–28).

---



# Week 11 — 2026-08-24 to 2026-08-30

- **Phase:** Alpha deployment
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded



## Shipped

- Software repo renamed VFM → SFM (2026-08-27) on GitHub
([Neurotech-Hub/SFM](https://github.com/Neurotech-Hub/SFM)).



## In progress

- ABC tour prep (Week 12).
- HLAB continues on the two-module setup.



## Next week

- ABC tour; decide retrofit vs replicate HLAB behavior-box assembly.



## Notes

- No commits in this planning repo this week; rename landed in SFM.
- 9-node PCBA was not placed this week (that is Phase 4, after custom MVP).

---



# Week 10 — 2026-08-17 to 2026-08-23

- **Phase:** Alpha deployment — analysis SDK
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded (SDK follow-up from Aug 14)



## Shipped

- Analysis report SDK delivered (~2026-08-21), as discussed at the Aug 14
HLAB handover: `sfm-analysis` / `sfm-report`.
- Reporting and sync documentation.
- Bandit test HTML report (`docs/Bandit_test_01_report.html`).



## In progress

- HLAB two-module two-armed bandit on delivered hardware.
- ABC still without a box (tour scheduled Week 12).



## Next week

- VFM→SFM alignment; prep ABC tour.



## Notes

- Production 9-node PCBA is Phase 4 (Nov), not this week.

---



# Week 9 — 2026-08-10 to 2026-08-16

- **Phase:** Alpha deployment to HLAB
- **HW / SW:** Alpha / Alpha
- **Meetings this week:**
[2026-08-14 first SFM delivery to HLAB](../meetings/20260814_meeting.md)



## Shipped

- First SFM delivery to Hengen Lab (Ravi Chopra), 2026-08-14: two-module
setup with two-armed bandit.



## In progress

- HLAB bring-up on the two-module pair.
- Analysis report SDK discussed at handover; due about a week later.



## Next week

- Deliver `sfm-analysis` / `sfm-report` (~2026-08-21).
- Support HLAB bench use.



## Notes

- No commits in this planning repo this week. Custom-experiment MVP
(Phase 3B) is still later; this is the HLAB alpha pair.

---



# Week 8 — 2026-08-03 to 2026-08-09

- **Phase:** Alpha deployment prep
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded



## Shipped

- BOM part-number correction and M2 screw updates.



## In progress

- First HLAB two-module delivery scheduled for the following week.
- Production-intent 9-node electronics still open (Phase 4, later).



## Next week

- First HLAB hardware delivery (two-module two-armed bandit).



## Notes

- 

---



# Week 7 — 2026-07-27 to 2026-08-02

- **Phase:** Phase 2 — Engineering sprint (close)
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded



## Shipped

- Project status advanced to Phase 2 / Week 7; July 21 meeting captured.
- Module mounting hardware documented; Fusion design link surfaced.
- Electronics BOM committed.
- Dispense logic for pellet-presence sensing.
- Motor-screw and BOM spreadsheet updates.



## In progress

- Week 8 rollout to ABC/HLAB (from July 21 discussion).
- Bedding offset and pellet-detector priority from that meeting.



## Blocked / risks

- IACUC protocol amendment still outstanding (Matt brief).



## Next week

- HLAB two-module delivery; ABC box still later.



## Notes

- 

---



# Week 6 — 2026-07-20 to 2026-07-26

- **Phase:** Phase 2 — Engineering sprint
- **HW / SW:** Alpha in progress / Alpha software demonstrating connectivity
- **Meetings this week:**
[2026-07-21 initial ABC + HLAB discussion](../meetings/20260721_meeting.md)



## Shipped

- Cross-lab intro: implementation plan; pellet-presence sensing; experiment
tasks. Hardware (Matt) and alpha software (Jemin).



## In progress

- Beam-break sensor PCBs (expected July 28 as of the meeting).
- 3D design finalization; build week and beta distribution prep.
- Home-cage bedding offset; pellet detector brought forward.



## Blocked / risks

- IACUC amendment needed for home-cage intent.



## Next week

- Sensor PCB arrival; lock mechanical design; document mounting.



## Notes

- Decisions: prioritize pellet detector; add bedding offset; pursue IACUC
amendment.

---



# Week 5 — 2026-07-13 to 2026-07-19

- **Phase:** Phase 2 — Engineering sprint
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded



## Shipped

- `docs/failure-modes.md` and `docs/function-checks.md`.



## In progress

- Engineering sprint: bring-up, communications, command/event/sync paths,  
UI mockup for ABC.



## Next week

- ABC + HLAB intro meeting.



## Notes

- 

---



# Week 4 — 2026-07-06 to 2026-07-12

- **Phase:** Phase 1A / 1B close
- **HW / SW:** Alpha / Alpha
- **Meetings this week:** none recorded



## Shipped

- No commits in this planning repo.



## In progress

- ABC UI/UX kickoff and HLAB experiment-kickoff requirements feeding Phase 2.



## Next week

- Phase 2 engineering sprint.



## Notes

- 

---



# Week 3 — 2026-06-29 to 2026-07-05

- **Phase:** Phase 1A / 1B Kickoff
- **HW / SW:** Architecture frozen; alpha in fab / not yet
- **Meetings this week:** none recorded



## Shipped

- No commits in this planning repo.



## In progress

- User stories / task templates (ABC) and experiment API / sync / MVP spec  
(HLAB).



## Next week

- Close kickoff phases; start engineering sprint.



## Notes

- 

---



# Week 2 — 2026-06-22 to 2026-06-28

- **Phase:** Phase 1A / 1B Kickoff
- **HW / SW:** Architecture documented / not yet
- **Meetings this week:** none recorded



## Shipped

- Architecture document: CAN topology, identifier layout, module hardware.



## In progress

- ABC UI/UX kickoff and HLAB implementation kickoff (parallel).



## Next week

- Continue kickoff requirements for the engineering sprint.



## Notes

- 

---



# Week 1 — 2026-06-15 to 2026-06-21

- **Phase:** Phase 0 — Architecture
- **HW / SW:** Architecture / not yet
- **Meetings this week:** none recorded



## Shipped

- No commits in this planning repo.



## In progress

- Hardware, communication, sync, and diagnostic architecture.
- Initial BOM and alpha prototype (n=3 + base station) to fab.



## Next week

- Publish architecture doc; start ABC/HLAB kickoffs.



## Notes

- 

---



# Week 0 — 2026-06-08 to 2026-06-14

- **Phase:** Phase 0 — Architecture
- **HW / SW:** Planning / planning
- **Meetings this week:** none recorded



## Shipped

- Project manager repo initialized.
- `PROJECT.md` timeline anchored: Week 0 = 2026-06-08.
- Collaborator abbreviations and phase structure.



## In progress

- Architectural engineering before user-facing design.



## Next week

- Close architecture decisions into `docs/`.



## Notes

- 

