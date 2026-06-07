# User API

> Status: stub. Owned by Phase 2 (HLAB) with NTH support.
>
> Defines how specialized experiments are authored, executed, synchronized,
> and analyzed.

## Experiment language / API

_Decision: scripting language, config DSL, GUI-only, or hybrid. Justification._

## Python scripting

_Allowed, required, or optional. If allowed, what does the API surface look
like?_

## Device functions exposed to users

_Reward delivery, LED control, sensor read, sync output, event logging,
module discovery, etc. List each function with a one-line signature and
expected timing guarantees._

## Boundary: what users can do vs what NTH must support

| Capability | User-implementable | Requires NTH support |
| ---------- | ------------------ | -------------------- |

## External hardware requirements

_Cables, adapters, sync boxes, cameras, recording-system I/O. Each item
should appear in the support-tool BOM._

## Minimum viable custom experiment

_Description of the MVP experiment HLAB will implement in Phase 5._

## Output format

_Behavioral output files, event/timestamp schema, file naming conventions._

## Cross-references

- Sync wiring and timing: [`sync-and-recording.md`](sync-and-recording.md).
- Function checks before running: [`function-checks.md`](function-checks.md).

## Open questions
