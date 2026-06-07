# Synchronization and Recording

> Status: stub. Owned by Phase 0 (NTH), refined in Phases 2 and 5 (HLAB).
>
> Capture how the platform synchronizes with external recording systems
> (electrophysiology, video) and how timestamps flow through the system.

## Sync ownership

_Modules vs base station vs both — who is the time master, and why._

## Sync outputs

_TTL pulses, 5V logic levels, serial messages, timestamp packets. Voltage,
edge timing, polarity, jitter budget._

## Sync inputs

_What external signals does the platform consume, if any?_

## Timestamp schema

_How events are timestamped, clock source, drift handling, expected
millisecond/microsecond resolution._

## Compatibility targets

_Which recording systems must be supported (e.g. Open Ephys, SpikeGLX, video
systems used by HLAB)?_

## Output files

_Event/timestamp file format, fields, and example records._

## Verification procedure

_How to validate sync end-to-end on the bench (cross-reference
[`function-checks.md`](function-checks.md))._

## Open questions
