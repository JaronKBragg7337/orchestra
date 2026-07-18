# Current signals

No signal has entered tracking yet. The first entries are created only after two syntheses and comparison exist for a cycle.

## Record template

```markdown
## SIGNAL ORC-YYYYMMDD-NNN — [name]

slug:                synthesis-local-slug
first_seen:          YYYY-MM-DD
first_seen_state:    INDEPENDENT_CONVERGENT | INFORMED_CONVERGENT |
                     INDEPENDENT_DIVERGENT | INFORMED_DIVERGENT |
                     ASYMMETRIC | SUSPECT
domain:              01-08
hemisphere:          western | eastern | cross | eastern-split
serves:              financial, research, content, monitoring

thesis:              one sentence, as first stated
falsifier:           the observation that would make this wrong
confirmation:        the observation that would confirm it
horizon:             when this should be visible if true
specifics:           named entities

status:              ACTIVE | CONFIRMED | INVALIDATED | DECAYED | UNRESOLVED
confidence_now:      Verified | Inferred | Assumed
confidence_first:    Verified | Inferred | Assumed

resurface_count:     0
blind_resurfaces:    0
informed_resurfaces: 0
blind_streak:        0
resurface_dates:     —
decay_since:         —

basis_history:
  YYYY-MM-DD  blind | informed  source/domain paths  hemisphere

### Cycle log
YYYY-MM-DD  event and context mode.

### Resolution
resolved_on:         —
resolution:          —
postmortem:          —
```
