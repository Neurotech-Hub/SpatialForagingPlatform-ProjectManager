# Budget and BOM Tracking

This folder tracks the project budget against the McDonnell NRP award and
points to the canonical Bills of Materials (BOMs), which live in the
hardware-electronics sub-repository (linked from the
[main README](../README.md#related-repositories)).

This file is for planning visibility, not engineering: it should answer "are
we on budget?" at a glance.

## Award

**Total: $40,000** over 12 months. Source:
[`../references/McDonnell-NRP_GAIDICA.pdf`](../references/McDonnell-NRP_GAIDICA.pdf).

## Budget allocation

| Recipient | Category                                                      | Awarded   | Committed | Spent | Remaining |
| --------- | ------------------------------------------------------------- | --------- | --------- | ----- | --------- |
| NTH       | Engineering salary (electronics, firmware, UI, integration)   | $15,000   | $0        | $0    | $15,000   |
| NTH       | Fabrication & prototyping equipment                           | $5,000    | $0        | $0    | $5,000    |
| ABC       | Benchmarking module array (n=9)                               | $3,000    | $0        | $0    | $3,000    |
| ABC       | Behavioral testing, animals, technician effort                | $7,000    | $0        | $0    | $7,000    |
| HLAB      | Validation module array (n=9)                                 | $3,000    | $0        | $0    | $3,000    |
| HLAB      | Electrophysiology validation, animals, recording support      | $7,000    | $0        | $0    | $7,000    |
| **Total** |                                                               | **$40,000** | **$0**  | **$0** | **$40,000** |

> Lab abbreviations are defined in the [main README — Abbreviations](../README.md#abbreviations).

## Canonical BOMs

The engineering BOMs themselves (per-component part numbers, vendors,
quantities, prices) live in the hardware sub-repositories and are linked
here once those repos exist.

| BOM                | Canonical location                | Last reviewed |
| ------------------ | --------------------------------- | ------------- |
| Module BOM         | TBD (link to hardware-electronics) | —             |
| Base station BOM   | TBD (link to hardware-electronics) | —             |
| Support-tool BOM   | TBD (link to hardware-electronics) | —             |

## Unit economics

Per-module and per-base-station unit economics are summarized here once the
production-intent BOMs are finalized in Phase 6, confirming that the
n=9 + n=9 build fits the budget.

| Item                | Estimated cost | Quantity | Subtotal |
| ------------------- | -------------- | -------- | -------- |
| Module (production) | TBD            | TBD      | TBD      |
| Base station        | TBD            | TBD      | TBD      |
| Support tools       | TBD            | TBD      | TBD      |
