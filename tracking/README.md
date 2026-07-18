# Signal tracking

Tracking turns daily synthesis into a longitudinal instrument. A signal is not presumed true because it entered this directory: it must re-surface, meet its confirmation condition, hit its falsifier, decay, or pass its horizon unresolved.

## Files

- [signals.md](signals.md) — human-readable current projection of every live signal.
- [events.jsonl](events.jsonl) — append-only machine history. One JSON object per lifecycle event.
- `cycles/YYYY-MM-DD.md` — cycle measurements, declared synthesis context, health, and tracker movement.
- `archive/YYYY-QN.md` — resolved or structurally superseded tracker snapshots.

`events.jsonl` is the replayable history; `signals.md` is its readable current view. The two must agree. A future harness should rebuild `signals.md` from the event stream and fail the cycle if the result differs from the committed projection.

## Identity

The `id` emitted by a synthesizer is a local, human-readable slug. When an item first enters tracking, the harness assigns an immutable global identifier in the form `ORC-YYYYMMDD-NNN`. Later wording changes do not change that ID. Merges and splits are lifecycle events, never silent renames.

This distinction prevents a renamed thesis from appearing new and prevents two unrelated claims with similar slugs from collapsing into one signal. It also gives the future website, financial joins, citations, and post-mortems a durable address.

## Lifecycle

- **ACTIVE** — entered and still within horizon.
- **CONFIRMED** — the pre-declared confirmation observation occurred.
- **INVALIDATED** — the pre-declared falsifier occurred; triggers a post-mortem.
- **DECAYED** — stopped appearing for the configured absent-cycle interval.
- **UNRESOLVED** — horizon passed without observable confirmation or falsification.

Persistence and validity are separate. `resurface_count` measures recurrence; `status` records resolution.

## Entry and movement

Convergent items in either context mode may enter. A divergent item may enter when the disagreement itself is the signal. Asymmetric items enter provisionally. Each later event records whether re-surfacing was blind or informed, its hemisphere pattern, basis paths, and the synthesis context that produced it.

The machine contract is [tracking-event.schema.json](../schemas/tracking-event.schema.json).
