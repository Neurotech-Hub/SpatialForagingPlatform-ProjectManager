# Failure Modes

> Status: stub. Drafted in Phase 0 (NTH), expanded in Phase 4 (ABC).
>
> Catalog of how the platform can fail, how each failure is detected, and how
> it is reported to the user.

## Failure mode catalog

| ID | Failure mode | Detection | User signal | Recovery |
| -- | ------------ | --------- | ----------- | -------- |
| F-001 | _example: pellet feeder jam_ | _interaction sensor without pellet sense_ | _LED red blink + base station alert_ | _user clears jam, runs function check_ |

_Add rows as failure modes are identified during architecture and field
testing._

## Detection categories

- Automatically detected by firmware.
- Detectable from base station / host software.
- Requires manual inspection.

## Reporting paths

_How a detected fault propagates: module LED → base station → host UI → user
notification._

## Cross-references

- [`function-checks.md`](function-checks.md) — procedures to confirm a module
  is healthy.
- [`maintenance.md`](maintenance.md) — preventive measures and scheduled
  inspections.
