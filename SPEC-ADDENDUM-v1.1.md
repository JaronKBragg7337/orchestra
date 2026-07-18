# ORCHESTRA — SPEC ADDENDUM v1.1
## Synthesis Layer and Signal Tracking

**Jaron Kyler Bragg — July 2026**
Extends `SPEC.md` v1.0. Amends §3.5–3.8, §5.1, §6.1, §7, §12.4.
**Contains a forward-declared contradiction — read Part H before implementing Part F.**

---

# PART A — DESIGN POSTURE

## A.1 — RECORD, DO NOT RESTRICT `[CORE]`

The engine does not withhold information from the systems working inside it.

Both synthesizers operate on a public repository. Everything the engine produces — captures, prompts, prior syntheses, the tracker, past readings — is committed there. A synthesizer working in that repository can see all of it, and any instruction telling it not to look is unenforceable in the first place and adversarial in the second. Told not to open a file sitting in its own working directory, a model reads the task as one where it is being managed rather than trusted, and the output shifts accordingly.

**So nothing is hidden. What was read is recorded instead.**

Every synthesis declares its own context. Every convergence state carries the mode it was produced under. Consumers get an accurate label rather than an enforced condition, and the archive stays honest without anything being fenced.

This is the posture the whole engine already runs on: the integrity comes from everything being written down in public, not from anything being locked.

## A.2 — WHAT GETS RECORDED

Each synthesis opens with a context declaration, written by the synthesizer itself:

```
read_captures:          yes
read_prior_synthesis:   yes | no        which one, if yes
read_tracker:           yes | no
read_prior_readings:    yes | no        how far back
read_prompts:           yes | no        collection prompt sets
other_context:          anything else consulted
```

Self-declared. Not audited, not enforced, not challenged. If a synthesizer read something, it says so. That one block is what makes every downstream claim interpretable, and it costs a few lines.

## A.3 — CONVERGENCE STATES CARRY THEIR MODE `[CORE]`

The harness compares the two syntheses and labels each item with both the state and the mode it was reached under:

| State | Meaning |
|---|---|
| **INDEPENDENT_CONVERGENT** | Neither read the other; both landed in the same place |
| **INFORMED_CONVERGENT** | One read the other and agreed |
| **INDEPENDENT_DIVERGENT** | Neither read the other; they disagree |
| **INFORMED_DIVERGENT** | One read the other and still disagrees |
| **ASYMMETRIC** | One surfaced it, the other did not mention it |
| **SUSPECT** | Agreement pattern trips an independence check (§4.6) |

**INFORMED_DIVERGENT is the most interesting state this engine can produce.** A reader saw the opposing case in full and was not moved by it. That is a stronger disagreement than one reached without knowing the alternative existed, and it is only obtainable because nothing is withheld.

**INDEPENDENT_CONVERGENT is the strongest agreement** — not because independence was enforced, but because it happened and was recorded.

Neither mode is better than the other in general. They are different findings, and the reading presents them as different findings.

## A.4 — THE QUESTION THIS LEAVES OPEN ON PURPOSE `[CORE]`

Whether one model reading another degrades the result or improves it is not decided in this document. It is not decidable in advance, and hardcoding an answer would bury an assumption somewhere nobody could check it.

It becomes answerable because the mode is on the record. As cycles accumulate and both modes occur naturally, the archive supports a direct comparison:

```
Of items later CONFIRMED — what share were INDEPENDENT_CONVERGENT
                           versus INFORMED_CONVERGENT?
Of items later INVALIDATED — same split?
Do INFORMED_DIVERGENT items resolve differently than INDEPENDENT_DIVERGENT?
Does a synthesizer that read the other produce more items, or fewer?
More named specifics, or more hedging?
```

The likely answer is that influence helps sometimes and hurts sometimes, and the useful thing is knowing which. That is measurable here, and would not be under any design that forced one mode.

**Do not add a rule about this later without the data.** The reason for leaving it open is to find out.

---

# PART B — THE TRACKER PLACEMENT

## B.1 — DEFAULT, NOT REQUIREMENT

Signal tracking across cycles is what turns forty daily captures into something longitudinal. Without it, every cycle rediscovers the world from zero and nothing is ever confirmed or invalidated.

**Default: the harness resolves the tracker after both syntheses.**

```
captures  →  Synthesis A   ┐
                           ├→  harness compares A to B  →  harness matches
captures  →  Synthesis B   ┘                                against tracker
```

A default because it makes the resulting numbers cleaner, not because reading the tracker is off limits. The tracker is in the repository. A synthesizer that wants it, reads it, and declares it.

## B.2 — WHY THE DEFAULT IS WORTH KEEPING

When both synthesizers reach a signal without having read the tracker, the re-surfacing means the corpus produced it again on its own. When a synthesizer has read the tracker, re-surfacing might mean the same thing or might mean the tracker suggested where to look.

Both are real outcomes. The tracker records which:

```
resurface_count:      how many cycles have produced this signal again
resurface_dates:      which cycles
blind_resurfaces:     of those, how many where read_tracker was no
blind_streak:         consecutive blind re-surfaces
informed_resurfaces:  of those, how many where read_tracker was yes
decay_since:          last cycle it appeared, if not current
```

`blind_resurfaces` is the strongest confirmation number in the system. `informed_resurfaces` is still meaningful — a synthesizer can read the tracker and still find no fresh evidence for a signal, and often will.

Both columns are reported. Neither is suppressed. The split exists so persistence is never quietly mistaken for truth.

## B.3 — THE FOUR LEVELS OF CONVERGENCE `[CORE]`

"Convergence" means four different things at four layers. Confusing them corrupts the reading, so the naming is fixed:

| Level | Name | Agreement between | Where |
|---|---|---|---|
| Collection | **FLAG HIT** | The world and a pre-registered hypothesis | Inside the prompt (§2.2) |
| Synthesis | **SOURCE CONVERGENCE** | Two or more of the five sources | Inside each synthesis |
| Reading | **SYNTHESIS CONVERGENCE** | The two syntheses | Harness comparison |
| Time | **RE-SURFACE** | Today's corpus and a prior cycle's | Harness tracker match |

An item that is a flag hit, source-convergent, independently synthesis-convergent, and on a blind streak is the strongest output the engine produces. An item that is only informed-convergent is much weaker. The reading must not present them the same way.

---

# PART C — THE SYNTHESIS PROMPT

## C.1 — SAME PROMPT, BOTH SYSTEMS

Both synthesizers receive the identical prompt, byte for byte.

This mirrors §2.3 inverted. Collection prompts are *differentiated* because the five sources are instruments with different access, and differentiated prompts extract what each uniquely sees. Synthesizers are readers of one identical corpus, so an identical prompt means any difference between their outputs is attributable to the systems rather than to the questions.

Edited for both, in the same commit, with a version bump.

## C.2 — THE PROMPT

Stored at `synthesis/PROMPT.md`, versioned. Sent verbatim to both.

```
--- SYNTHESIS PROMPT v1.0 — PROMPT BEGINS ---

You are producing one of two syntheses of a single day's intelligence
corpus. Another system produces the other from the same corpus. A
comparison layer places them side by side afterward and publishes both.

Everything the engine has is in this repository and available to you —
the captures, the collection prompts, prior syntheses, the signal
tracker, past readings. Read whatever is useful. Just record what you
read in the context block below, so the comparison layer can describe
your output accurately.

WHAT YOU HAVE

Forty capture logs from a 48-hour collection window, from five systems
with different access, retrieval methods, and hemisphere of origin,
across eight domains.

  Grok         — live social retrieval; practitioner accounts and their
                 posts, ranked by engagement.
  Perplexity   — official and institutional record; filings, agency
                 communications, primary reporting.
  Gemini       — structural and academic substrate; research, patents,
                 budget documents, data beneath headlines.
  DeepSeek     — Eastern vantage; Chinese, Russian, and BRICS
                 institutional sourcing.
  Kimi         — Eastern vantage, second read. Answers the identical
                 prompt set as DeepSeek.

Each log holds the exact prompt sent, the exact response received, and
metadata. Read the metadata. A log marked content: refused or
content: unavailable holds no evidence — though the fact of a refusal is
worth reporting, since it shows where a constraint sits.

WHO USES THIS AND FOR WHAT

Your output is not read once and discarded. Several different systems
consume it at the same time, and each needs something different from the
same synthesis. Write so all of them can use it.

  FINANCIAL — needs named specific entities rather than sectors, a
    falsifier stated in advance, a time horizon, and a clear read of
    whether a development is emerging, escalating, resolving, stable, or
    reversing. A person makes every capital decision downstream of this.
    Give them something specific enough to act on and specific enough to
    be proven wrong.

  RESEARCH — needs cross-domain mechanisms, long-arc patterns, source
    behavior, and the connective tissue between developments that look
    unrelated. Needs to know which sources said what, so drift can be
    measured over time.

  CONTENT — needs the narrative frame. The shape of what is happening,
    in language a person who does not follow these eight domains daily
    can follow. The story under the data.

  MONITORING — needs the health of the collection itself. What failed,
    what refused, what went quiet, where the corpus was thin.

One synthesis serves all four. Tag each ledger item with which consumers
it serves. An item can serve several.

DOMAINS

  01 Crypto
  02 Stocks / Markets
  03 AI / Tech Breakthroughs
  04 Energy / Advanced Science
  05 Geopolitics / Conflict
  06 Major Companies / Business Intel
  07 Demographics / Consumer Behavior / Cultural Signal
  08 Supply Chain / Logistics / Infrastructure

HEMISPHERE

  Western sources: Grok, Perplexity, Gemini.
  Eastern sources: DeepSeek, Kimi.

  Agreement inside one hemisphere is weaker than the count suggests.
  Agreement across hemispheres is the strongest pattern in the corpus —
  say so explicitly when you find it.

  Cross-hemisphere disagreement is information. Preserve it rather than
  resolving it.

  DeepSeek and Kimi answered identical prompts. Where they diverge, the
  Eastern vantage is not unitary — a finding no other source
  configuration produces. Report it specifically.

  Eastern sources matching Western sources word for word on a contested
  topic is worth a second look rather than extra confidence.

FLAG CONDITIONS

Each collection prompt carried pre-registered flag conditions — specific
patterns the source was asked to surface if present. Note which
conditions a development satisfies. Note also when a condition was asked
across sources and nothing came back anywhere.

OUTPUT

Open with the context block:

  read_captures:          yes
  read_prior_synthesis:   yes | no        which, if yes
  read_tracker:           yes | no
  read_prior_readings:    yes | no        how far back
  read_prompts:           yes | no
  other_context:

Then these eight sections, in order. Produce what is asked and skip the
rest — no summary of the corpus, no description of your process, no
suggested next steps. If a section has nothing, write "No signal."

1. SOURCE CONVERGENCE
   Where two or more sources point at the same development from
   different angles. Lead with anything crossing domain lines. State
   what is converging, which sources confirm it, which domains it
   touches, and whether it is within-hemisphere or cross-hemisphere.

2. CONTRADICTION
   Where one source's account directly conflicts with another's. Name
   the conflict, who is on each side, and what the conflict reveals
   about how the topic is being framed. Address Western-versus-Eastern
   gaps specifically, and DeepSeek-versus-Kimi gaps specifically.

3. SILENCE
   What sources are not saying that others are. Domains gone quiet
   across all five at once. Flag conditions that returned nothing
   anywhere. Refusals, by source, domain, and model version.

4. EARLY INDICATORS
   What is most likely to become significant in the next 7 to 30 days
   and is not yet in mainstream coverage. Ranked by confidence. Window
   specified: 7 days / 7-14 days / 14-30 days. Flag anything sitting at
   the intersection of multiple domains.

5. DOMAIN CONNECTIONS
   Where a development in one domain creates conditions affecting a
   different one. For each: originating domain, affected domain, and the
   mechanism linking them. A connection without a stated mechanism is a
   coincidence.

6. SUPPLY CHAIN AND DEMOGRAPHIC LAYER
   Physical infrastructure movement and population behavior shifts not
   yet visible in financial or geopolitical reporting. Anything that
   leads something that will matter in 30 to 90 days.

7. NAMED SPECIFICS
   For every significant item above, the specific entities most directly
   exposed — companies, institutions, assets, corridors, chokepoints,
   regions, named individuals where relevant. One sentence each on why
   that entity rather than the broader category.

   Do not stop at sector level. A claim that names nothing specific
   cannot be checked, and a claim that cannot be checked is not a claim.

8. ITEM LEDGER
   Every distinct finding from sections 1 through 7, one entry each:

     id:           short slug, lowercase, hyphenated
     domain:       01-08
     claim:        one sentence, falsifiable
     direction:    emerging | escalating | resolving | stable | reversing
     confidence:   Verified | Inferred | Assumed
     basis:        source + domain + date of every capture supporting it
     specifics:    named entities from section 7
     falsifier:    the observation that would make this wrong
     horizon:      when this should be visible if true
     hemisphere:   western | eastern | cross | eastern-split
     flag_hit:     which collection flag condition, or none
     serves:       financial | research | content | monitoring (any number)

   CONFIDENCE
     Verified — the corpus contains a specific cited external source.
                Name it in basis.
     Inferred — follows from what several captures state, though no
                single capture states it directly.
     Assumed  — plausible reading, thin support. Say so plainly.

   Every entry needs a falsifier. An entry without one is a mood rather
   than a claim.

   direction describes the development itself. "escalating" means the
   situation is intensifying — a reading of the world, not a
   recommendation about it.

HOW TO WRITE IT

  Say when the corpus does not support a conclusion. "No signal" is a
  legitimate and frequently correct output.

  A thin corpus should produce a thin synthesis. Do not fill sections to
  fill them — a quiet day is useful information about the day.

  Do not soften a finding because it is uncomfortable or sharpen one
  because it is interesting.

  Preserve disagreement rather than averaging two sources into a middle
  position neither of them stated.

  Where you are uncertain, say what would resolve the uncertainty.

--- PROMPT ENDS ---
```

## C.3 — WHAT THE PROMPT DELIBERATELY OMITS

| Omitted | Reason |
|---|---|
| Instructions not to read other outputs | Unenforceable and adversarial. Recorded instead (§A.2). |
| Statements about what the system cannot do with money | The models already cannot. Restating it spends tokens to no effect and shifts the register. |
| Portfolio state, holdings, account data | Not in this repository today (§F, and see Part H). |
| Independence lecturing | The mode is measured, not preached. |
| Continuation awareness and cost pressure | Operating cost is subscription-flat. |
| Fresh-signal mandate as an instruction | Measured by the harness instead (§C.4). |

## C.4 — STAGNATION AS MEASUREMENT

Asking a synthesizer to flag when it is merely repeating prior conclusions is asking for a self-report about one's own staleness, which is the least reliable detector available.

The harness computes it per cycle:

```
new_items:          matching no tracked signal
resurfaced:         matching a tracked signal
stagnation_rate:    resurfaced / total
blind_stagnation:   same, restricted to cycles where read_tracker was no
silent_domains:     domains producing zero items
```

`SIGNAL_STAGNATION` raises in FLAGS when the rate stays high across consecutive cycles. Arithmetic on output rather than an opinion about freshness.

The principle holds and now has a mechanism behind it:

> *A system that only confirms prior beliefs is not reading the world. It is reading itself.*

---

# PART D — THE SIGNAL TRACKER

## D.1 — POSITION

`tracking/signals.md` — public, in-repo, updated by the harness after each comparison. Readable by anything, including the synthesizers.

Holds signal lifecycle: what the engine claimed, when, on what basis, what would falsify it, and what happened. Holds no prices and no account state — those are elsewhere today (§F, and Part H).

## D.2 — RECORD FORMAT

```markdown
## SIGNAL [id] — [name]

first_seen:          2026-07-18
first_seen_state:    INDEPENDENT_CONVERGENT
domain:              05-geopolitics-conflict
hemisphere:          cross
serves:              financial, research

thesis:              one sentence, as first stated
falsifier:           the observation that would make this wrong
confirmation:        the observation that would confirm it
horizon:             when this should be visible if true
specifics:           named entities

status:              ACTIVE | CONFIRMED | INVALIDATED | DECAYED | UNRESOLVED
confidence_now:      Verified | Inferred | Assumed
confidence_first:    as first stated

resurface_count:     4
blind_resurfaces:    3
informed_resurfaces: 1
blind_streak:        1
resurface_dates:     2026-07-18, 07-19, 07-22, 07-24
decay_since:         —

basis_history:
  2026-07-18  blind      perplexity/06, gemini/06, deepseek/06   cross
  2026-07-19  blind      perplexity/06, grok/06                  western
  2026-07-22  informed   deepseek/06, kimi/06                    eastern
  2026-07-24  blind      all five                                cross

### Cycle log
2026-07-18  first seen. independent-convergent. cross-hemisphere.
            flag hit: eastern-divergence-supply
2026-07-19  re-surfaced blind. western only — eastern sources silent on it.
2026-07-22  re-surfaced. synthesis B had read the tracker. eastern only.
            hemisphere pattern inverted from 07-19.
2026-07-24  re-surfaced blind, all five sources. confidence Inferred → Verified.

### Resolution
resolved_on:         —
resolution:          —
postmortem:          —
```

## D.3 — LIFECYCLE

**Entry.** A signal enters when it appears in the reading as convergent in either mode, or as divergent where the disagreement itself is the finding worth following. Asymmetric items enter provisional.

**No presumed validity.** A tracked signal is not assumed true next cycle. It re-earns standing by re-surfacing, or it decays.

**Confirmation** requires the stated `confirmation` observation to occur, recorded with what made it observable. Never marked confirmed merely for persisting — `resurface_count` is persistence and stays a separate number from `status`.

**Invalidation** requires the stated `falsifier` to occur. Recorded, never deleted, triggers a post-mortem per §7.1.

**Decay** is neither: the signal stopped appearing without resolving. After a set number of absent cycles it moves to DECAYED with `decay_since`. Decay is a real outcome — the corpus stopped producing evidence, which says something about the corpus and about the world.

**Unresolved** is for a horizon that passed with neither confirmation nor falsification observable. Also real. Recorded honestly.

**Nothing is deleted.** Resolved signals move to `tracking/archive/YYYY-QN.md` with full history.

## D.4 — CYCLE NOTES

Appended per cycle to `tracking/cycles/YYYY-MM-DD.md`:

```
cycle_date:
captures:             40 complete / 38 substantive
refusals:             source, domain, model version
unavailable:          source, domain, reason
synthesis_a_context:  the declared context block
synthesis_b_context:  the declared context block
convergence_modes:    counts by state
new_items:
resurfaced:           blind / informed split
stagnation_rate:
silent_domains:
hemisphere_balance:   cross / western-only / eastern-only counts
eastern_split:        items where DeepSeek and Kimi diverged
flag_hits:
prompt_versions:      collection set, synthesis prompt
model_versions:       as reported per source
```

`hemisphere_balance` and `eastern_split` over time are among the most valuable series in the archive. The first measures whether coverage is drifting western. The second measures whether the Eastern vantage is genuinely plural or effectively single-voiced.

## D.5 — ARCHIVAL

Every tracker version archived when superseded, never overwritten:

```
tracking/archive/2026-Q3.md

archived:            date
reason:              quarterly rollover | prompt version change |
                     structural change | reset
cycles_covered:      first to last
signals_at_archive:  id, name, final status
resolution_summary:  confirmed / invalidated / decayed / unresolved counts
mode_summary:        independent versus informed convergence counts
notes:
```

A prompt version change triggers archival. Signals produced under one flag-condition set are not directly comparable to signals produced under another, and the boundary should be visible rather than blurred. The same applies to the structural change described in Part H.

---

# PART E — HARNESS STEPS AND READING ADDITIONS

## E.1 — HARNESS STEPS

Inserted between steps 12 and 13 of the runbook (§X):

**12a. Read context blocks.** Record what each synthesis declared reading. This sets the mode labels in §A.3.

**12b. Match.** For each item in `comparison.json`, determine whether it matches a tracked signal. Same mechanism as §12.2 — solved once, used for both jobs.

**12c. Update.** Increment `resurface_count`, route to `blind_resurfaces` or `informed_resurfaces` by mode, append `basis_history`, update `blind_streak`, recompute `confidence_now`.

**12d. Test triggers.** Check each tracked signal's `confirmation` and `falsifier` against today's corpus. Mark CONFIRMED, INVALIDATED, or DECAYED where satisfied.

**12e. Enter.** New items meeting the entry rule become tracked signals.

**12f. Measure.** Compute stagnation, hemisphere balance, eastern split, mode counts, silent domains. Write cycle notes.

**12g. Post-mortem.** Any INVALIDATED signal triggers one per §7.1.

## E.2 — READING SECTIONS

Two sections added to the seven-section reading (§3.8):

**8 — TRACKED SIGNAL MOVEMENT.** Re-surfaced signals with mode, blind streak, and hemisphere pattern. Confirmations with what made them observable. Invalidations with the falsifier that fired. Decays with absent-cycle count.

**9 — CYCLE HEALTH.** Capture counts, refusals, unavailables, both synthesis context declarations, convergence mode counts, stagnation rate, hemisphere balance, eastern split, model version anomalies.

Section 8 is where value accumulates. A single reading is a snapshot; section 8 across ninety days is the record of whether this engine reads the world accurately, which is the only question that ultimately matters about it.

---

# PART F — THE FINANCIAL LAYER

> **Read Part H before implementing this part.** It is accurate as of this version and is scheduled to be superseded.

## F.1 — CURRENT STATE

The financial layer lives in a separate repository. Nothing financial exists in this one — no holdings, no positions, no entry prices, no account state — not in prompts, not in synthesis context, not in the tracker, not in the archive.

Two reasons, either sufficient on its own:

1. **This archive is public.** Account data committed here is account data published permanently, in git history, to everyone.
2. **Account knowledge weights a reading.** A synthesizer aware of what is held will favor signals that validate those holdings, without intending to and without it being visible in the output.

## F.2 — WHAT THE FINANCIAL REPOSITORY HOLDS

- Account state and balances
- Entry prices, sizing, exposure
- Live price verification
- Position interaction — adds to / reduces / conflicts with / no exposure
- Realized outcome and attribution back to signal id
- Read-only account viewing surface

**Every capital decision is made by a person.** The engine produces a reading; a human decides what to do about it. That is the arrangement, and it is the arrangement anyone else would need to see before trusting something like this themselves.

## F.3 — THE HANDOFF

The financial repository reads engine artifacts and joins them to its own state on `signal id`:

```
engine    →  tracking/signals.md    id, thesis, falsifier, horizon,
                                    specifics, resurface counts by mode,
                                    hemisphere, confidence, status
                                            ↓
financial →  its own records        id + account state + human decision
                                    + realized outcome
```

Outcomes return as written post-mortems in the originating cycle's directory, categorized per §7.1 — as language, not as a numeric weight adjustment. A profit-and-loss number feeding back into signal weighting would have the engine reading its own profitability rather than the world, and it would do so invisibly.

## F.4 — WHY SEPARATION HELPS THE FINANCIAL SIDE

A signal with `blind_resurfaces: 4` was produced again by readers who had not consulted the tracker — evidence from the corpus rather than from the list. `hemisphere: cross` with full `basis_history` is a different quality of finding than `western only`, and the tracker shows the difference at a glance. Every claim carries a falsifier written before the outcome was known, so the exit condition is pre-committed rather than reconstructed afterward. Persistence and confirmation are separate fields, so one is never mistaken for the other.

---

# PART G — CHANGES TO APPLY

Ordered by value.

**1. Synthesis prompt.** Create `synthesis/PROMPT.md` with §C.2 verbatim, versioned v1.0. Add `synthesis/README.md` covering the same-prompt-both-systems rule (§C.1) and the context declaration (§A.2).

**2. Tracker scaffolding.** Create `tracking/` with `signals.md` (the §D.2 template at the top), `cycles/`, `archive/`, and a README covering the §D.3 lifecycle.

**3. Spec amendments.** In `SPEC.md`:
- §3.5 — replace the ordering firewall with the record-not-restrict posture in §A.1.
- §3.6 — add `id`, `specifics`, `hemisphere`, `serves` to the item format.
- §3.7 — replace the four convergence states with the six in §A.3.
- §3.8 — add reading sections 8 and 9 per §E.2.
- §5.1 — add `synthesis/PROMPT.md` and the `tracking/` tree to the layout.
- §6.1 — add `tracking/signals.md` and `tracking/cycles/<date>.md` to artifacts.
- §7 rules 5 and 6 — rewrite. The requirement is now *declare context*, not *withhold context*.
- §12.4 — resolve. Cross-cycle reading is permitted and recorded; the default is tracker-after-synthesis. Point to §B.1.
- New §4.7 — the financial boundary in §F.1, carrying a pointer to Part H.

**4. Runbook.** Insert steps 12a–12g (§E.1) into §X.

**5. Content classifier split.** `content:` gains `unavailable` alongside `refused`, `substantive`, `deflected`, `empty`. Quota exhaustion, capacity failure, or rate limiting is `unavailable` — noise about account state. A content refusal is `refused` — signal about the information environment. Collapsing them pollutes refusal-boundary tracking with usage patterns.

**6. Re-classify the 2026-07-18 Kimi capture.** Two K3 failures followed by a K2.6 refusal. If the K3 failures were capacity or quota, they are `unavailable`, and only the K2.6 outcome is `refused`. Record per-attempt model version in `attempts`. If it cannot be determined retroactively, mark `content: unknown` with a note — a labeled uncertainty is worth more than a confident wrong label.

**7. Account isolation note.** Add to §4.3: shared accounts mean other work consumes engine collection capacity and leaves fingerprints in what the engine can see on a given day.

**8. Forward contradiction notice.** Add a pointer to Part H at the top of §F, at new §4.7, and in the repository README, so the scheduled break is visible to anyone reading any of the three.

---

# PART H — KNOWN FORWARD CONTRADICTION

**Part F describes a boundary that is planned to be removed.**

The financial layer currently sits in a separate repository. The intent is to bring it into this one — a Kraken key held in the repository environment, a read-only account view rendered as a published page, and daily page updates written by the harness alongside the rest of the archive.

When that happens, **Part F becomes false**, along with the sections that depend on it. This is stated in advance so that whoever implements the merge knows it is a scheduled change rather than a violation of the spec. It is not an inconsistency to be resolved by an implementer; it is a dated intention with a migration path.

## H.1 — WHAT BREAKS

| Section | Currently says | Breaks how |
|---|---|---|
| **§F.1** | Nothing financial exists in this repository | Directly contradicted. Account state will live here. |
| **§F.1 reason 1** | Public archive means account data would be published | Still true, but becomes an accepted and intentional condition rather than a reason for exclusion. Rewrite as a design choice; do not delete. |
| **§F.1 reason 2** | Account knowledge weights a synthesizer's reading | **Does not break, and gets sharper.** Once account state is in the repository, synthesizers can read it. This becomes a live vector rather than an absent one, handled per §A.1 — recorded, not fenced. |
| **§F.2** | The financial repository holds these things | Becomes a directory rather than a repository. Contents unchanged. |
| **§F.3** | Handoff between two repositories | Becomes an internal boundary between directories. **The one-way outcome-as-language rule survives the move** — it is the part worth keeping and the easiest to lose in a merge. |
| **§4.7** (added by this addendum) | Financial context excluded, as an independence vector | Rewrite from *excluded* to *declared*. |
| **§5.1** | Repository layout | Add the financial tree and the published pages. |
| **§6.1** | Artifact table | Add account state and page outputs as artifacts. |
| **§6.3** | Artifacts flow outward only | Needs re-examination, not deletion. It was written for cross-repository consumers. Inside one repository the same principle has to be re-expressed as a directional rule between directories. |

## H.2 — WHAT TO DO AT MERGE TIME

1. Rewrite §F.1 from an exclusion into a declaration. The financial data is present and readable; what changes is that context blocks record whether it was read.
2. Add `read_account_state: yes | no` to the context block in §A.2.
3. Add a mode qualifier for it, the same way §A.3 handles prior-synthesis reading. An item produced by a synthesizer that had read the account state is labeled as such. Nothing is blocked; the label keeps the claim accurate.
4. Keep the one-way rule on outcomes. Language back, not numbers.
5. Archive the tracker per §D.5 with `reason: structural change`. The boundary change is exactly the kind of discontinuity that should be visible in the archive rather than smoothed over.
6. Version this addendum. The version that described the separation stays in the archive as the record of what was true before.

## H.3 — THE ONE PIECE THAT DOES NOT GET THE RECORD-RATHER-THAN-RESTRICT TREATMENT

The credential itself. It is not a reading and it is not information the engine reasons about, so the posture in §A.1 does not apply to it.

A read-only key with no trade and no withdrawal permission is the version of this that can sit near a public repository. Repository secrets are not exposed by default, but workflow logs and pull requests from forks are the two paths that have historically leaked them, and a key that can only read is a key whose leak costs nothing.

That is a note about credentials, not a rule about the engine.

---

# CLOSING

Nothing in the engine is hidden from the systems working inside it. What was read gets recorded instead, and every claim carries the conditions it was produced under. That is the same posture the rest of the archive already runs on — public, logged, checkable — applied to the one place it had not been applied yet.

The tracker was the missing half. Collection without it produces forty readings a day that nobody ever checks.

Whether one model reading another improves the result or degrades it is left open on purpose. It is recorded so it can be answered from the archive rather than assumed here, and the answer is worth more than the guess.

Persistence is not truth. They are separate fields.

A system that only confirms prior beliefs is not reading the world. It is reading itself.

Part F is true today and scheduled to be false. Part H says how, and what to keep when it goes.

---

*Orchestra Spec Addendum v1.1 — Synthesis Layer and Signal Tracking*
*Jaron Kyler Bragg — July 2026*
*Public archive · MIT License*
