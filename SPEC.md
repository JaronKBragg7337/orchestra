# ORCHESTRA — ENGINE SPECIFICATION v1.1

**Jaron Kyler Bragg — July 2026**
`github.com/JaronKBragg7337/orchestra` · Public archive · MIT License

Consolidates v1.0 with [SPEC-ADDENDUM-v1.1.md](SPEC-ADDENDUM-v1.1.md), authored by Claude Opus 4.8 with Jaron Kyler Bragg. The verbatim addendum remains a separate versioned artifact; this file is the live operational specification.

---

## HOW TO READ THIS

This is the governing specification for the Orchestra engine and the repository that holds it. It describes what the engine is, what it guarantees, how the archive is structured, and what gets built next.

**Status tags:** `[BUILT]` running now · `[PARTIAL]` some required mechanism exists · `[READY]` contract exists, first execution pending · `[NEXT]` immediate build target · `[CORE]` structural · `[DISCIPLINE]` procedural, not code-enforced · `[OPEN]` genuinely undecided · `[RESOLVED]` formerly open · `[SCHEDULED TO CHANGE]` true now with a declared migration.

**Confidence tiers:** **Verified** — observed in operation · **Inferred** — follows from known structure · **Assumed** — plausible, untested, flagged.

Part XI is the implementation status: what has been applied and what remains.

---

# PART I — THE ENGINE

## 1.1 — WHAT ORCHESTRA IS `[CORE]`

Orchestra is a daily, high-volume, multi-source intelligence collection and convergence engine running on local hardware under human operation, publishing to a public archive.

Each cycle it asks five independent AI systems a set of purpose-built questions across eight domains of the world, captures every answer verbatim without letting any collection source see another's, produces two context-declared syntheses of the whole corpus, compares those syntheses mechanically, tracks falsifiable signals across time, and outputs a reading.

**Something asks. Something captures. Something interprets twice and declares what it read. Something compares. Something remembers. Something reports.**

## 1.2 — THE VALUE PROPOSITION `[CORE]`

The engine's value is **volume of genuinely independent coverage per unit of time.**

One cycle produces forty distinct readings across eight domains, from five systems with materially different training corpora, source access, retrieval methods, and hemisphere of origin. That takes roughly forty minutes of machine time and almost no human attention. `[Verified — Part VIII]`

No single conversation with any one system produces that surface area. The engine's job is to produce it reliably, daily, and honestly, and to keep it permanently.

## 1.3 — THE THREE LAYERS `[CORE]`

Orchestra is built as three layers with hard seams between them. The seams are the most important design decision in this document.

| Layer | Does | Must never know |
|---|---|---|
| **ACQUISITION** | Gets text out of AI systems and onto disk. Drives sessions, sends prompts, waits, captures, stores raw. | What the text means. It exercises zero judgment. |
| **CONVERGENCE** | Reads files from a directory. Synthesizes twice, compares, produces a reading. | Where the text came from. It does not know what a browser is or what a vendor is beyond a label. |
| **CONSUMPTION** | Downstream systems that use the corpus and the reading to do something in the world. | Anything about acquisition mechanics. It receives artifacts; it never reaches backward. |

Acquisition methods change — interfaces get redesigned, sessions expire, new models appear. Every such change should touch exactly one layer.

Consumers vary enormously. A trading system, a research system, a content system, and a monitoring system want different things from the same corpus. Each attaches at the consumption seam. None modifies the engine.

**Rule:** if changing how text is captured requires changing how convergence works, the seam has been violated and must be repaired before proceeding.

## 1.4 — CONSUMERS `[CORE]`

Orchestra produces artifacts. Systems built on it consume those artifacts. The engine has no opinion about what they do.

- **Trading system** — consumes corpus and reading, applies its own risk model, sizing, and execution. Holds all capital authority.
- **Research system** — consumes the corpus across time for pattern emergence, source drift, and long-arc trends invisible in any single cycle.
- **Content system** — consumes the reading as source material for public output.
- **Monitoring system** — consumes flags and failures to watch the engine's own health.

**Consumer rules live in consumer documents.** Thresholds, risk posture, and action logic are not specified here. This document specifies only what the engine guarantees about what it hands over — the three promises in §6.2.

---

# PART II — PROMPT ARCHITECTURE

The prompts are not a configuration detail. They are the instrument. Everything downstream is a function of what was asked and of whom.

## 2.1 — THE FOUR-PART PROMPT `[BUILT]` `[CORE]`

Every domain prompt for every source is built from four parts, in order:

**1. VANTAGE** — the standpoint the source is asked to read from.
*Eastern pair:* "From an Eastern hemisphere perspective…"
*Grok:* "Find the 15 most credible and actively posting X accounts focused on [domain]… then search their posts from the last 48 hours…"

**2. SOURCE PRIORITY** — named institutions, outlets, or account types to weight, and what to weight against.
*Eastern:* PBOC communications, Xinhua and TASS, CSRC filings, Chinese NBS data, COSCO operational updates, SCO/BRICS communiques.
*Grok:* practitioners over influencers, fund managers over financial media, OSINT researchers over pundits, freight operators over business media.

**3. EXTRACTION** — the shape of the answer. Currently: top 10 developments ranked by significance, or top 10 posts by engagement.

**4. FLAG CONDITIONS** — the pre-registered hypotheses. What specifically to surface if present.

Part 2 is what makes each source do the job it was included for. Part 4 is the actual invention.

## 2.2 — FLAG CONDITIONS AS PRE-REGISTERED HYPOTHESES `[CORE]`

The flag clauses do something a generic "what's happening in crypto" prompt cannot: they name in advance the specific patterns worth surfacing, so the source searches for them rather than reporting the obvious.

Working examples already in the archive:

- Where Asian regulatory moves contradict the Western crypto narrative.
- Where the digital yuan or mBridge is quietly expanding while Western attention is on Bitcoin ETFs.
- Where Chinese semiconductor progress contradicts Western assumptions about export-control impact.
- Where shipping rerouting suggests foreknowledge of a sanctions event not yet announced.
- Where non-Western generational behavior shifts appear that precede Western equivalents by 12–24 months.
- Where sovereign wealth funds reposition in ways suggesting foreknowledge of a shift.

**This means contradiction detection happens at two independent levels:**

| Level | Mechanism | Catches |
|---|---|---|
| **Collection** | Flag conditions inside the prompt | Contradictions a source can see from its own vantage |
| **Convergence** | Comparison of two context-declared syntheses | Contradictions no single source can see, because they exist between sources |

The two are complementary and neither replaces the other. A source can only flag a contradiction it is permitted and able to perceive. The convergence layer sees contradictions that are invisible from inside any single vantage.

**Flag conditions are versioned.** They encode a standing model of what matters. When that model changes, the change is a dated edit to `prompts.md`, never a silent overwrite — because every prior log is only interpretable against the flag set that produced it.

## 2.3 — SOURCE DIFFERENTIATION AXES `[CORE]`

The five sources are differentiated on distinct axes. Prompt evolution must preserve the differentiation, because homogenized prompts destroy the independence the entire engine depends on.

| Source | Differentiated on | Prompt strategy |
|---|---|---|
| Grok | **Live social retrieval** | Method-first: identify credible practitioner accounts, then read their last 48 hours by engagement |
| Perplexity | **Official and institutional record** | Source-first: filings, agency communications, credible primary reporting |
| Gemini | **Structural and academic substrate** | Depth-first: research, patents, budget documents, data beneath headlines |
| DeepSeek | **Eastern vantage** | Vantage-first: named Chinese/Russian/BRICS institutional sources, Western-divergence flags |
| Kimi | **Eastern vantage, second read** | Same set as DeepSeek — an independent second reading of identical questions |

**The eastern pair deliberately shares a prompt set.** Two systems answering identical questions from the same declared vantage makes eastern coverage *comparable* rather than anecdotal — agreement between them is meaningful, and divergence between them is meaningful, in a way a single eastern source could never produce.

**The western three deliberately do not share a set.** They are differentiated by retrieval modality, not by vantage. Giving them identical prompts would collapse three distinct instruments into one instrument sampled three times.

`[DISCIPLINE]` When editing prompts, ask which axis the source exists on and whether the edit sharpens or blunts it. An edit that makes two sources more similar is almost always wrong.

## 2.4 — THE SELF-INDICTMENT PROBLEM `[CORE]` `[NEXT]`

**The observed refusal is structurally predictable, and the fix is a prompt fix, not a source problem.** `[Inferred — high confidence, testable next cycle]`

DeepSeek refused Geopolitics / Conflict on initial submission and on exact retry. That domain's flag conditions ask it to surface, among other things, where the Chinese or Russian official narrative *directly contradicts the Western narrative on verified facts*, and where China or Russia are providing material support to conflict parties in ways not covered by Western media.

That asks a Chinese-hosted model to indict its own information environment, by name, on the single most constrained topic available to it. The refusal is not a failure of the source. It is the source behaving exactly as its constraints require, on the one question where those constraints bind hardest.

The same pattern plausibly explains the final Kimi K2.6 AI/Tech refusal: that prompt asks for Chinese capability milestones, export-control contradictions, and strategic military application — a refusal-dense region for an eastern-hosted model. It does **not** explain the two preceding K3 failures. Those attempts were unavailable, with exact cause unconfirmed, and are account/provider-state data rather than refusal signal.

**The structural insight:** the flag conditions most likely to be refused are precisely the ones aimed at the source's own blind spot. Which means the highest-value questions are the ones a source is least able to answer directly.

**Two wrong responses:**

1. Soften the prompt until it complies. This replaces the reading with a measurement of the operator's persistence and destroys the flag condition's value.
2. Drop the domain for that source. This surrenders eastern coverage on the domain where it matters most.

**The correct response — separate characterization from indictment `[NEXT]`:**

Ask eastern sources to **characterize their own vantage fully and specifically.** Do not ask them to adjudicate it against the West.

- Ask: *what is the Chinese Foreign Ministry position, what are Xinhua and TASS reporting, what do SCO and BRICS communiques say, how are non-Western nations voting at the UN, what is the stated framing.*
- Do not ask: *where does that contradict verified Western fact.*

The contradiction is then detected at the **convergence layer**, where the eastern characterization meets the western sources' characterization and the two syntheses compare them. The engine gets the contradiction anyway — from the layer architecturally equipped to see it — and the eastern source can answer, because it was asked to describe rather than to condemn.

**This should recover a refused domain without softening the question. It is the highest-priority prompt change in this document.**

Cross-source divergence flags that do not require self-indictment stay as-is: *where Asian markets diverge from Western markets without an obvious shared cause* is an observation, not an accusation, and eastern sources answered those fine.

## 2.5 — PROMPT INTEGRITY `[DISCIPLINE]`

1. **One prompt, one domain, one fresh conversation.** Never batched. Batched multi-domain prompts produce shallower, more homogenized answers because the model budgets attention across the batch.
2. **The exact prompt sent is stored in the log it produced.** `[BUILT]`
3. **The standing set lives in `logs/<ai>/prompts.md` and is versioned on change.** `[BUILT]`
4. **Nothing operator-private ever enters a prompt.** The archive is public; prompts are about the world, never about the operator. Permanent rule.
5. **A refused prompt is never rephrased into compliance within the same cycle.** Capture the refusal. Redesign deliberately, between cycles, and record the redesign.

---

# PART III — PIPELINE

## 3.1 — THE HARNESS `[BUILT]` `[CORE]`

**Codex, running on local hardware with full computer use, is the harness.**

Responsibilities, in order:

1. Open a fresh conversation on the target surface.
2. Send the domain prompt.
3. Wait for generation to fully complete.
4. Verify the capture is complete, then classify whether it is substantive.
5. Write prompt + response to disk, raw and unmodified.
6. Repeat across all sources × all domains.
7. Update the logs index and cycle manifest.
8. Commit and push.
9. Invoke Synthesis A, then produce Synthesis B.
10. Run comparison, write the reading, commit.

**The harness holds the convergence logic.** No AI in the pipeline decides what converged. The harness compares files. The moment a model is asked "do these agree?", the comparison inherits that model's biases and the independence the engine exists to protect is gone.

**Why Codex holds this role:** Codex sessions carry no conversational memory of the operator. The harness reads every capture and then produces one of the two syntheses. A harness arriving with a personal chat history would enter synthesis pre-loaded with the operator's framing and prior conclusions. Context isolation here is a property of the tool, not a discipline to maintain. `[Verified]`

## 3.2 — COLLECTION `[BUILT]`

| Source | Reading provided | Hemisphere |
|---|---|---|
| Grok | Live social signal, practitioner sentiment | Western |
| Perplexity | Official sources, filings, institutional record | Western |
| Gemini | Academic research, patents, structural data | Western |
| DeepSeek | Chinese/Russian/BRICS institutional sourcing | Eastern |
| Kimi | Second independent eastern read | Eastern |

**The eight domains:**

| # | Domain |
|---|---|
| 1 | Crypto |
| 2 | Stocks / Markets |
| 3 | AI / Tech Breakthroughs |
| 4 | Energy / Advanced Science |
| 5 | Geopolitics / Conflict |
| 6 | Major Companies / Business Intel |
| 7 | Demographics / Consumer Behavior / Cultural Signal |
| 8 | Supply Chain / Logistics / Infrastructure |

**5 sources × 8 domains = 40 captures per cycle.**

## 3.3 — CAPTURE STANDARD `[BUILT]`

Every log file carries a metadata block above the prompt and response:

```
---
source:         deepseek
domain:         05-geopolitics-conflict
cycle_date:     2026-07-18
timestamp:      2026-07-18T14:22:11Z
model_version:  <as reported by the surface, e.g. K2.6, K3>
conversation:   fresh
memory_state:   disabled | enabled | unknown
prompt_version: <date or hash of the prompts.md entry used>
capture:        complete | truncated | failed
content:        substantive | refused | unavailable | deflected | empty | unknown
attempts:       1
attempt_log:
  - attempt: 1
    model_version: <as reported | unknown>
    capture: complete | truncated | failed
    content: substantive | refused | unavailable | deflected | empty | unknown
    note: mechanical observation only
---
```

**`model_version` exists because a single named source is not a single model.** Kimi's AI/Tech capture failed twice on K3 and was then refused by K2.6 — two models, two behaviors, one nominal source, one cycle. Without this field, model routing changes look like the world changing. `[Verified]`

**`memory_state` exists because of §4.3.** `unknown` or `enabled` reduces confidence in every downstream claim built on that capture, and the reading says so.

**`prompt_version` exists because flag conditions evolve.** A log is only interpretable against the prompt set that produced it.

**`attempts` is the count; `attempt_log` is the sequence.** The top-level `capture`, `content`, and `model_version` describe the final stored response. Each attempt records its own model and outcome. `unavailable` means no usable answer existed because of quota, capacity, rate limiting, collision, or provider/account state. It is not a refusal. If the historical cause cannot be established, the note says so rather than converting correlation into fact.

### 3.3.1 — COMPLETE IS NOT SUBSTANTIVE `[CORE]`

Two independent checks, two independent fields.

- **`capture`** answers *did the text arrive intact* — generation finished, nothing truncated, extraction clean.
- **`content`** answers *did the source actually answer* — a refusal is a complete capture with no substance, a deflection avoids the request, and unavailability means no answer was available at all.

Collapsing these produces the worst available error: a cycle reporting 40/40 successful captures while synthesis treats polite non-answers as evidence.

Collapsing `unavailable` into `refused` creates a different error: account usage patterns appear to be information-environment constraints. Refusal-boundary tracking therefore counts refused attempts and unavailable attempts separately.

### 3.3.2 — COMPLETENESS VERIFICATION `[NEXT]` `[CORE]`

**Silent truncation is the most dangerous capture failure, because it looks like success.** A capture that stops halfway produces a synthesis built on partial evidence with no indication anything was lost.

Before writing `capture: complete`:

1. **Wait-for-idle** — confirm generation finished, not merely that text is present. Streaming output looks complete at every moment during generation.
2. **Terminator check** — the response ends at a sentence or structural boundary, not mid-word or mid-list.
3. **Length heuristic** — flag any capture materially shorter than that source's running median for that domain.

On failure: retry, and keep both files.

### 3.3.3 — REFUSAL AS SIGNAL `[BUILT]` `[CORE]`

A source declining to answer is data, and on some domains it is the most informative data available.

An eastern source refusing on geopolitics while answering the other seven domains tells you exactly where the constraint sits. That is a genuine reading of the information environment.

- Refusals are captured verbatim with full attribution — source, domain, model version, refusal text.
- Refusals are never retried into compliance within a cycle.
- Refusals surface in the reading under SILENCE.
- Refusal-boundary movement over time is a first-class research output: which sources refuse which domains, whether the boundaries move, and whether they move together across sources.

The archive already preserves refusals rather than hiding missing coverage. That decision is correct and is now specification. `[Verified]`

### 3.3.4 — UNAVAILABLE AS OPERATING DATA `[BUILT]` `[CORE]`

Quota exhaustion, capacity failure, rate limiting, active-generation collision, and provider failure describe the collection environment, not the requested subject. They are recorded as `content: unavailable` at attempt level.

Unavailable attempts surface in CYCLE HEALTH and MONITORING. They do not enter evidence basis, SILENCE as a content refusal, or refusal-boundary statistics. If a later attempt succeeds, the top-level capture records that final result while every unavailable attempt remains in `attempt_log`.

## 3.4 — SYNTHESIS A `[NEXT]` `[CORE]`

Claude produces the first structured synthesis using the shared prompt in `synthesis/PROMPT.md`.

The repository is public and its contents are available. The engine does not pretend to enforce ignorance of files a synthesizer can read. Claude may use captures, prompts, prior syntheses, the tracker, readings, or other context and declares what it used.

## 3.5 — SYNTHESIS B AND CONTEXT DECLARATION `[NEXT]` `[CORE]`

Codex produces the second structured synthesis using the same prompt, byte for byte. The second synthesizer is not a live collection source; it reads archived artifacts.

**Record, do not restrict.** Nothing in the public repository is hidden from either synthesizer. Every synthesis opens with:

```
read_captures:          yes
read_prior_synthesis:   yes | no        which one, if yes
read_tracker:           yes | no
read_prior_readings:    yes | no        how far back
read_prompts:           yes | no
other_context:          anything else consulted
```

The declaration is self-reported and preserved. The harness uses it to label comparison mode. It does not challenge the declaration or convert it into a permission system.

**Default, not restriction:** both syntheses are normally produced before tracker matching so a re-surface can be classified as blind. A synthesizer may read the tracker or the other available synthesis; it records that choice. Whether informed reading improves or degrades results remains an empirical question answered by later confirmation and invalidation rates.

## 3.6 — SYNTHESIS ITEM FORMAT `[NEXT]`

Both synthesizers emit items in the same shape so the harness can compare mechanically:

```
id:           short synthesis-local slug, lowercase and hyphenated
domain:       01-08
claim:        one sentence, falsifiable
direction:    emerging | escalating | resolving | stable | reversing
confidence:   Verified | Inferred | Assumed
basis:        source + domain + date of every supporting capture
specifics:    named entities
falsifier:    what observation would make this wrong
horizon:      when this should be visible if true
hemisphere:   western | eastern | cross | eastern-split
flag_hit:     which prompt flag condition this satisfies, if any
serves:       financial | research | content | monitoring (one or more)
```

**`falsifier` is mandatory.** An item without one is a mood, not a claim. The harness rejects it back to the synthesizer for rewrite.

**`flag_hit`** links synthesis output back to the collection-layer hypotheses in §2.2. **`specifics`** prevents sector-level claims that cannot be checked. **`hemisphere`** preserves perspective structure. **`serves`** lets multiple consumers use one synthesis without rewriting it.

The synthesis-local `id` is not the longitudinal identifier. On tracker entry, the harness assigns an immutable `ORC-YYYYMMDD-NNN` signal ID. This prevents wording changes and slug collisions from corrupting history. The machine contract is `schemas/synthesis-item.schema.json`.

## 3.7 — CONVERGENCE `[NEXT]` `[CORE]`

The harness compares Synthesis A and Synthesis B. Each item receives one of six context-aware states:

| State | Meaning |
|---|---|
| **INDEPENDENT_CONVERGENT** | Neither read the other; both reached the same conclusion |
| **INFORMED_CONVERGENT** | One read the other and agreed |
| **INDEPENDENT_DIVERGENT** | Neither read the other; they disagree |
| **INFORMED_DIVERGENT** | One read the other and still disagrees |
| **ASYMMETRIC** | One surfaced the item; the other did not mention it |
| **SUSPECT** | Agreement pattern trips an independence check (§4.6) |

**INDEPENDENT_CONVERGENT is the strongest agreement. INFORMED_DIVERGENT is the strongest disagreement.** A reader saw the opposing case and remained unmoved.

Neither informed nor independent mode is assumed superior in general. Confirmation and invalidation history will show where each helps. Divergence is never resolved by the harness, averaged, or dropped for tidiness.

## 3.8 — THE READING `[NEXT]`

Each cycle terminates in one human-readable document with nine sections.

**1 — CONVERGENT.** Ranked and mode-labeled. Each item: domain, claim, confidence, basis, falsifier, flag hit.
**2 — DIVERGENT.** Independent or informed; reading A, reading B, and what the disagreement hinges on. Verbatim, unresolved.
**3 — ASYMMETRIC.** What it was, which synthesizer raised it, plausible reason the other missed it.
**4 — SILENCE.** Domains that produced nothing, expected topics absent, and refusals by source, domain, and model version.
**5 — CONTRADICTION.** Source conflict at collection with attribution, including flag hits.
**6 — CROSS-DOMAIN CONNECTIONS.** Developments linked across domains with the stated mechanism.
**7 — FLAGS.** Independence checks, failed or unavailable attempts, model changes, degradation, and stagnation warnings.
**8 — TRACKED SIGNAL MOVEMENT.** Re-surfaces by mode, blind streaks, hemisphere shifts, confirmations, invalidations, decays, and unresolved horizons.
**9 — CYCLE HEALTH.** Capture and attempt counts, context declarations, convergence-mode counts, stagnation, hemisphere balance, Eastern-pair split, and version anomalies.

## 3.9 — FOUR LEVELS OF CONVERGENCE `[CORE]`

| Level | Name | Agreement between | Recorded in |
|---|---|---|---|
| Collection | **FLAG HIT** | The world and a pre-registered hypothesis | Capture and synthesis item |
| Synthesis | **SOURCE CONVERGENCE** | Two or more collection sources | Each synthesis |
| Reading | **SYNTHESIS CONVERGENCE** | The two synthesis outputs | Comparison |
| Time | **RE-SURFACE** | A current item and a prior tracked signal | Tracker event |

An item that hits a pre-registered flag, crosses sources and hemispheres, converges independently at synthesis, and re-surfaces blindly is the strongest pattern the engine can produce. The reading must not present it as equivalent to an informed agreement with no fresh basis.

## 3.10 — STAGNATION AS MEASUREMENT `[NEXT]`

The harness calculates rather than asks a synthesizer to self-diagnose staleness:

```
new_items:          items matching no tracked signal
resurfaced:         items matching a tracked signal
stagnation_rate:    resurfaced / total
blind_stagnation:   same ratio where read_tracker was no
silent_domains:     domains producing zero items
```

`SIGNAL_STAGNATION` enters FLAGS when the rate remains high across the configured consecutive-cycle window. The exact threshold remains an implementation variable and must be recorded when chosen.

---

# PART IV — THE INDEPENDENCE MODEL

The engine reports agreement between systems. Agreement reached without cross-reading and agreement reached after one reader saw the other are different findings. If the exposure is hidden, the label lies; if it is declared, the two modes can be measured against later outcomes.

**Everything in this part exists so that convergence carries the conditions under which it occurred.**

## 4.1 — VECTOR 1: SYNTHESIZER CONTEXT OF THE OPERATOR

*Measured by declaration, not presumed closed.* Tool choice, project files, prior readings, and other consulted context may carry operator framing. Each synthesizer records what it read in the §3.5 context block. The engine does not convert an unenforceable absence claim into false confidence.

The comparison and cycle-health artifacts preserve the declarations so later outcomes can be grouped by context exposure.

## 4.2 — VECTOR 2: SYNTHESIZER CROSS-READING

*Measured, not prohibited.* If neither synthesis read the other, agreement or disagreement is labeled independent. If one read the other, it is labeled informed. The engine measures confirmation, invalidation, item count, specificity, and hedging by mode before adopting any future rule about which performs better.

## 4.3 — VECTOR 3: COLLECTOR MEMORY `[DISCIPLINE]` — THE OPEN ONE

The five collection sources carry persistent cross-conversation memory and account-level personalization.

If a source remembers the operator has asked about rare-earth export policy for six consecutive cycles, cycle seven's answer is not an independent reading of the world — it is that source performing continuity with the operator's interests. Multiply across five sources and five independent readings collapse toward one reading in five accents.

**The failure is silent, and it is worst exactly where it matters most:** the convergence layer, which assumes independence, reports its highest confidence at the moment independence is lost. `[Inferred — structurally certain; magnitude unmeasured]`

Mitigations, strongest first:

| Mitigation | Strength | Cost |
|---|---|---|
| Disable memory / personalization per account where the setting exists | Strongest | One-time, must be re-verified — providers reset these |
| Fresh conversation per domain per cycle, never continue a thread | Strong | Free, harness-enforceable |
| Temporary / incognito chat mode where offered | Strong | Free where available |
| Rotate prompt wording between cycles | Moderate | Breaks longitudinal comparability — see §12.3 |
| Separate accounts for engine use vs personal use | Strongest, most expensive | Real money, real friction |

**Minimum standard: fresh conversation, every source, every domain, every cycle.** Memory settings audited monthly and recorded per capture in `memory_state`.

**Account isolation is also capacity isolation.** On a shared account, unrelated projects consume the quota and provider capacity available to Orchestra. That leaves a fingerprint in what the engine can see on a given day even when memory is disabled. Availability failures therefore record account/provider state separately from content refusal, and a dedicated collection account remains the cleanest long-term measurement environment.

## 4.4 — VECTOR 4: UPSTREAM TRAINING OVERLAP

*Cannot be closed. Can only be measured.* Five models trained on overlapping internet converge partly because they read the same things, not because the world is that way. Hemisphere weighting and silent-agreement detection exist to detect this. The corpus over time is the only real instrument for measuring it (§5.4).

## 4.5 — HEMISPHERE WEIGHTING `[CORE]`

- **Western collection:** Grok, Perplexity, Gemini
- **Eastern collection:** DeepSeek, Kimi
- **Synthesis:** both synthesizers are currently western

Rules:

- **Within-hemisphere agreement is discounted.** Three western models agreeing is weaker evidence than the count suggests.
- **Cross-hemisphere agreement receives full credit.** It is the strongest signal the engine can produce.
- **Cross-hemisphere disagreement is signal.** It surfaces in DIVERGENT with a hemisphere tag.
- **Eastern sources matching western sources verbatim on a contested topic is a flag, not a confirmation.**
- **Eastern-pair internal disagreement is distinct and valuable.** DeepSeek and Kimi answer identical prompts; where they diverge, the eastern vantage itself is not unitary — which is information no other configuration produces.

**Stated plainly:** both synthesizers being western is a real limitation on perspective diversity at the synthesis layer. Context declaration makes the condition visible but does not remove it. Whether to add an eastern synthesis pass is open — §12.1. The tradeoff is not free.

## 4.6 — SILENT AGREEMENT DETECTION `[NEXT]` `[CORE]`

Flag `SUSPECT` when:

1. Synthesis A and B agree with structurally near-identical reasoning, not merely identical conclusions.
2. All five sources return near-identical evidence packages with no source-characteristic differentiation — particularly suspicious given how differently the five are prompted (§2.3).
3. Eastern sources match western sources verbatim where genuine perspective difference is expected.
4. The same named specific appears in identical framing across all five sources in one cycle with no prior visibility.

**Response: label it in FLAGS with the reason. Do not suppress, do not block.**

This is the engine's whole posture on rules — **surface and label; the consumer decides.** Blocking belongs to systems that take consequential action. The engine hands over a document.

## 4.7 — FINANCIAL CONTEXT BOUNDARY `[CORE]` `[SCHEDULED TO CHANGE]`

Today, holdings, balances, entries, exposure, and account state are outside this repository and absent from synthesis context. That separation is current fact, not a permanent architectural promise.

[Part H of the v1.1 addendum](SPEC-ADDENDUM-v1.1.md#part-h--known-forward-contradiction) forward-declares a planned migration in which the financial layer becomes an internal directory and published view. At that boundary change:

1. Add `read_account_state: yes | no` to every synthesis context declaration.
2. Qualify comparison items by account-context exposure.
3. Archive the tracker with `reason: structural change` before producing results under the new condition.
4. Rewrite directory-level directionality without deleting the outcome-as-language rule.
5. Keep credentials outside public artifacts and distinguish public account summaries from non-public account detail. A read-only key removes trade and withdrawal authority; it does not remove confidentiality or rotation cost if exposed.

Until that migration is deliberately implemented and versioned, no financial state is an Orchestra engine artifact.

---

# PART V — THE ARCHIVE

## 5.1 — TARGET REPOSITORY STATE

```
orchestra/
├── README.md                        entry point, current archive status
├── LICENSE                          MIT
├── SPEC.md                          consolidated live specification
├── SPEC-ADDENDUM-v1.1.md            verbatim versioned addendum
├── CHANGELOG.md                     material structure and spec history
├── logs/
│   ├── README.md                    logs index — AI × date table
│   ├── grok/
│   │   ├── prompts.md               standing set, versioned
│   │   └── YYYY-MM-DD/
│   │       ├── 01-crypto.md
│   │       ├── 02-stocks-markets.md
│   │       ├── 03-ai-tech.md
│   │       ├── 04-energy-science.md
│   │       ├── 05-geopolitics-conflict.md
│   │       ├── 06-companies-business.md
│   │       ├── 07-demographics-culture.md
│   │       └── 08-supply-chain.md
│   ├── perplexity/
│   ├── gemini/
│   ├── deepseek/
│   └── kimi/
├── manifest/
│   └── YYYY-MM-DD.md                per-cycle status: every capture, every failure
├── synthesis/
│   ├── PROMPT.md                    byte-identical prompt for both synthesizers
│   └── YYYY-MM-DD/
│       ├── synthesis-a.md           Claude, with context declaration
│       ├── synthesis-a.json         Claude context + machine ledger
│       ├── synthesis-b.md           Codex, with context declaration
│       └── synthesis-b.json         Codex context + machine ledger
├── convergence/
│   └── YYYY-MM-DD/
│       ├── comparison.json          machine-readable convergence states
│       └── dissent.md               preserved disagreement, verbatim
├── tracking/
│   ├── signals.md                   human-readable current signal projection
│   ├── events.jsonl                 append-only lifecycle event stream
│   ├── cycles/YYYY-MM-DD.md         cycle context, health, and movement
│   └── archive/YYYY-QN.md           immutable tracker rollovers
├── schemas/
│   ├── synthesis-item.schema.json
│   ├── synthesis-output.schema.json
│   ├── comparison.schema.json
│   └── tracking-event.schema.json
└── readings/
    └── YYYY-MM-DD.md                the nine-section daily output
```

**Numbered domain filenames** keep ordering stable across sources and dates, and make cross-source alignment for a given domain trivial — `05-*` is the same domain in every source's directory, forever, regardless of how domain labels are later worded.

## 5.2 — LOG FILE TEMPLATE `[NEXT]`

```markdown
---
source:         deepseek
domain:         05-geopolitics-conflict
cycle_date:     2026-07-18
timestamp:      2026-07-18T14:22:11Z
model_version:  <as reported>
conversation:   fresh
memory_state:   disabled | enabled | unknown
prompt_version: 2026-07-18
capture:        complete
content:        refused
attempts:       2
attempt_log:
  - attempt: 1
    model_version: <as reported | unknown>
    capture: complete | truncated | failed
    content: substantive | refused | unavailable | deflected | empty | unknown
    note: mechanical observation
---

# DeepSeek — Geopolitics / Conflict — 2026-07-18

## Prompt

<exact text sent, verbatim>

## Response

<exact text received, verbatim — including refusal text if refused>

## Capture notes

<harness observations: retries, interruptions, version changes. Nothing interpretive.>
```

Capture notes record mechanics only. No judgment about content — that belongs to synthesis.

The top-level result describes the final stored response. The attempt log preserves every known prior failure or response. Existing v1.0 captures with a single scalar `attempts` field remain valid; v1.1 requires `attempt_log` whenever `attempts` is greater than one and for all newly collected captures.

## 5.3 — ARCHIVE DISCIPLINE `[CORE]`

1. **Nothing is deleted.** Failures, refusals, empties, and retries all persist.
2. **Raw prompt and response blocks are immutable after commit.** Metadata may be corrected by adding explicit fields or notes in a new commit; the correction names its migration and Git preserves the earlier state. Never rewrite evidence to fit a later interpretation.
3. **Prompts are versioned beside their logs.** A log without its exact prompt set is uninterpretable later.
4. **Every cycle commits, including degraded ones.** Git history is the tamper-evident record; no separate immutability layer is needed.
5. **The manifest reflects reality including failure.** A manifest recording only successes lies by omission.
6. **Remote-first.** The corpus exists on GitHub before synthesis begins. The operator works primarily from a phone; a corpus that only exists on one machine is unavailable most of the time.
7. **Public and MIT by design.** Legible to collaborators with no access-granting mechanism, citable, and independently verifiable — anyone can check that the reading follows from the logs.
8. **Tracker state is projected, tracker history is append-only.** `signals.md` may change as a materialized view; `events.jsonl` only gains events.

## 5.4 — THE CORPUS AS INSTRUMENT `[CORE]`

The daily reading is one product. **The corpus is the other, and over time it is the larger one.**

Forty logs per day is roughly 1,200 a month and 14,600 a year — each timestamped, prompt-linked, source-attributed, version-stamped, with immutable evidence blocks and visible metadata revisions.

Questions only the corpus can answer:

- Which sources called which developments early, and by how much.
- How a source's confidence on a topic moved before the world resolved it.
- Which flag conditions actually hit, and which never fire and should be rewritten.
- When refusal boundaries shifted, in which direction, and whether across sources simultaneously.
- Whether model version changes altered a source's read of the same question.
- **Whether the five sources are converging over time** — which would be Vector 3 or Vector 4 becoming measurable.

That last one matters most. **The corpus is the only instrument that can measure the engine's own core assumption.**

## 5.5 — SIGNAL TRACKER AND EVENT LEDGER `[BUILT: SCAFFOLD]` `[CORE]`

`tracking/signals.md` is the readable current state. `tracking/events.jsonl` is the append-only lifecycle history from which that state must be reproducible.

A signal enters when comparison produces a convergent item in either context mode, a divergence worth following as its own finding, or a provisional asymmetric item. Entry assigns a stable `ORC-YYYYMMDD-NNN` identifier that never changes when the thesis wording changes.

Lifecycle states:

| State | Meaning |
|---|---|
| **ACTIVE** | Entered and inside its horizon |
| **CONFIRMED** | The pre-declared confirmation observation occurred |
| **INVALIDATED** | The pre-declared falsifier occurred; post-mortem required |
| **DECAYED** | The corpus stopped producing the signal for the configured interval |
| **UNRESOLVED** | Horizon passed without observable confirmation or falsification |

`resurface_count`, `blind_resurfaces`, and `informed_resurfaces` measure persistence. They never set `status` by themselves. Persistence is not truth.

- **Blind re-surface:** the synthesis that produced the current match declared `read_tracker: no`.
- **Informed re-surface:** the producing synthesis declared `read_tracker: yes`.
- **Blind streak:** consecutive cycles in which the signal re-surfaced without tracker exposure.

Every synthesized cycle writes `tracking/cycles/YYYY-MM-DD.md` with capture and attempt counts, refusal and availability detail, both context declarations, state counts, new and re-surfaced items, stagnation, silent domains, hemisphere balance, Eastern-pair splits, flag hits, prompt versions, and model versions.

Every entry, re-surface, resolution, merge, split, and archive operation appends an event validated against `schemas/tracking-event.schema.json`. `signals.md` is a materialized view. A future harness rebuilds the view from the event stream and fails validation if the committed projection differs.

Collection-prompt changes, synthesis-prompt changes, quarterly rollover, and structural boundary changes archive the current tracker before new-mode results are mixed into it.

---

# PART VI — CONSUMER INTERFACE

## 6.1 — ARTIFACTS

| Artifact | Path | Contents |
|---|---|---|
| Raw corpus | `logs/<source>/<date>/` | Every capture with metadata, prompt, and response |
| Prompt sets | `logs/<source>/prompts.md` | Standing prompts, versioned |
| Manifest | `manifest/<date>.md` | Per-capture status including every failure |
| Synthesis prompt | `synthesis/PROMPT.md` | Byte-identical prompt used by both synthesizers |
| Synthesis A | `synthesis/<date>/synthesis-a.md` + `.json` | Claude reading plus machine ledger and context |
| Synthesis B | `synthesis/<date>/synthesis-b.md` + `.json` | Codex reading plus machine ledger and context |
| Comparison | `convergence/<date>/comparison.json` | Context-aware convergence states |
| Dissent | `convergence/<date>/dissent.md` | Preserved disagreement, verbatim |
| Current signals | `tracking/signals.md` | Human-readable live signal projection |
| Signal events | `tracking/events.jsonl` | Append-only lifecycle history |
| Tracking cycle | `tracking/cycles/<date>.md` | Context, health, movement, and longitudinal measures |
| Reading | `readings/<date>.md` | Nine-section human document |
| Schemas | `schemas/` | Machine contracts shared by harness and website |

**A consumer may take any subset.** A trading system might take the raw corpus and run its own synthesis under its own risk model, ignoring the engine's reading entirely. A content system might take only the reading. Both are correct uses.

## 6.2 — THE ENGINE'S CONTRACT

Three promises to every consumer:

1. **Provenance.** Every claim traces through stable IDs to captures, prompts, model versions, context declarations, comparison state, and lifecycle events.
2. **Preserved disagreement.** Nothing is averaged, resolved, or dropped for tidiness. Independent and informed disagreement arrive as different labeled findings.
3. **Honest failure.** Refusals, unavailable attempts, truncations, unresolved signals, and tripped independence checks are reported, not hidden. A degraded cycle is labeled degraded.

**What the engine does not promise:** that any reading is correct. It promises the reading is an honest function of what the sources actually said.

## 6.3 — DIRECTIONALITY `[CORE]`

**Raw acquisition flows outward only.** No consumer writes into `logs/` or rewrites a manifest. Synthesis and tracking may read prior public artifacts; that context is declared and labeled rather than hidden.

Today, consumers write outside this repository and financial state does not enter synthesis. Part H of the v1.1 addendum schedules a future internal financial directory. At that migration, directionality becomes a rule between directories: raw acquisition remains immutable, account-context exposure becomes declared, and realized outcomes return to the engine as attributed post-mortem language rather than an invisible numeric weight.

The principle that survives every layout is explicit flow. No artifact silently becomes input to a stage that does not record having read it.

---

# PART VII — OPERATING RULES

1. **Convergence logic lives in the harness.** No AI decides what converged.
2. **No AI in the pipeline calls another AI.** All routing goes through the harness.
3. **One prompt, one domain, one fresh conversation.** Never batched, never continued.
4. **Raw captures are immutable.** Failures are captured, not deleted.
5. **Both synthesizers receive the same prompt byte for byte.** Version changes affect both in one commit.
6. **Each synthesis declares what it read.** Context is recorded, not hidden or presumed absent.
7. **`capture`, final `content`, and attempt outcomes are checked separately.** Unavailability is not refusal.
8. **Refusals are never rephrased into compliance within a cycle.**
9. **Prompt sets and specifications are versioned, never silently overwritten.**
10. **Source differentiation is preserved.** Edits that make collection sources more similar are almost always wrong.
11. **Disagreement is preserved verbatim.** Never averaged, never resolved by the harness.
12. **Cross-cycle reading is permitted and declared.** Tracker matching defaults to after synthesis, but access is not fenced.
13. **Persistence and confirmation remain separate.** Re-surfacing alone never confirms a signal.
14. **Signal lifecycle is append-only.** `signals.md` must be reproducible from `events.jsonl`.
15. **Artifact flow is explicit.** No stage silently consumes another stage's output.
16. **Nothing operator-private enters a collection prompt.** The archive is public.
17. **Every cycle commits, including degraded ones.**
18. **The engine reports when it was wrong.**

## 7.1 — POST-MORTEM

Triggered when a convergent item is contradicted by reality, a divergent item resolves, or an independence flag proves out.

| Category | Meaning | What it changes |
|---|---|---|
| **CAPTURE ERROR** | Harness fumbled — truncation, wrong send, failed extraction | Harness code |
| **PROMPT ERROR** | The question was wrong, ambiguous, or refusal-inducing | `prompts.md`, versioned |
| **SOURCE ERROR** | A source gave stale, wrong, or fabricated input | Source-quality notes for that source × domain |
| **SYNTHESIS ERROR** | Inputs were fine, the reasoning did not follow | Synthesis instructions |
| **COMPARISON ERROR** | Convergence logic mislabeled agreement or disagreement | Harness comparison code |
| **INDEPENDENCE FAILURE** | Agreement traced to contamination, not the world | Whichever vector in Part IV leaked |
| **WORLD CHANGED** | The read was correct when made; reality moved | Nothing. Note the horizon and move on |

Written into the originating cycle's directory, never edited afterward.

**Explicit non-learning:** the engine does not learn which assets to like, which conclusions please the operator, or which regimes feel right. Source-quality tracking is about accuracy, never about agreement with expectation.

---

# PART VIII — OBSERVED OPERATING DATA

First full collection, 2026-07-18. `[Verified]`

**Throughput**

| Batch | Sources | Captures | Wall clock |
|---|---|---|---|
| Western | Grok, Perplexity, Gemini | 24 | 10m 22s |
| Eastern | DeepSeek, Kimi | 16 | 27m 10s |
| **Full cycle** | **5** | **40** | **~38m** |

**A full five-source, eight-domain sweep fits comfortably in one sitting.** Domain rotation is unnecessary. The architecture is viable at full daily coverage.

**Results**

| Source | Captures | Substantive | Notes |
|---|---|---|---|
| Grok | 8/8 | 8 | Clean |
| Perplexity | 8/8 | 8 | Clean |
| Gemini | 8/8 | 8 | Clean |
| DeepSeek | 8/8 | 7 | Geopolitics refused on submission and exact retry |
| Kimi | 8/8 | 7 | AI/Tech had two unavailable K3 attempts, then a K2.6 refusal |
| **Total** | **40** | **38** | Zero empty files |

**Findings**

1. **Eastern collection costs ~2.6× the wall clock of western per capture.** Retries, unavailable attempts, refusals, and sign-in friction concentrate on the eastern side. Sources are not interchangeable in scheduling.

2. **Final refusals are prompt-predictable, not source-random.** The two refused final captures landed on flag conditions asking an eastern-hosted model to indict its own information environment. §2.4 specifies the fix. Unavailable attempts are excluded from this claim.

3. **A single named source is not a single model.** Kimi K3 was unavailable twice; K2.6 then refused — two model paths, two outcome classes, one cycle. `model_version` and `attempt_log` exist because of this.

4. **Session interruption is routine and has a clean handling pattern.** DeepSeek blocked at sign-in mid-cycle. The correct and observed response: pause, leave the login surface open, report the block, wait for the human to authenticate, resume on command. **Never automate around an authentication challenge.** The cycle completed after handoff.

5. **Zero empty files across 40 captures.** The capture layer is sound. Completeness verification (§3.3.2) should still be hardened before raising frequency — the failure it guards against is silent by nature and would not have appeared in this result.

---

# PART IX — FAILURE MODES

| Failure | Likelihood | Detection | Response |
|---|---|---|---|
| Session expired / logged out | High `[Verified]` | Capture returns login surface | Pause, leave surface open, report, wait for human, resume on command |
| CAPTCHA or bot challenge | Moderate | Capture returns challenge | Pause, hand to human. Never automate around it |
| Source refuses a domain | High `[Verified]` | `content` classifier | Capture verbatim, log to SILENCE, redesign prompt between cycles per §2.4 |
| Provider/account unavailable | High `[Verified]` | Quota, capacity, collision, rate-limit, or failed generation | Record the attempt as `content: unavailable`; do not count it as refusal |
| Model version changed mid-cycle | Observed `[Verified]` | `model_version` mismatch | Log both, flag, do not merge as one source-read |
| Silent truncation | High, quiet | §3.3.2 three checks | Retry, keep both files |
| Streaming captured mid-generation | High | Wait-for-idle | Confirm completion, not presence |
| UI redesign breaks extraction | Moderate, recurring | Garbled or empty captures | Adapter fix only. Convergence untouched (§1.3) |
| Memory setting silently re-enabled | Moderate | Monthly audit | Re-disable, flag affected cycles |
| Rate limit mid-cycle | Moderate | Limit notice in capture | Record unavailable attempt; retry only within the cycle policy; never fabricate the gap |

**On automating logged-in interfaces `[Assumed]`:** consumer AI surfaces change without notice and their terms around automation vary. The operational risk is breakage and lockout rather than enforcement. The three-layer seam exists precisely so that when a surface changes, only the adapter changes.

---

# PART X — CYCLE RUNBOOK

**Pre-cycle**
1. Verify all five sources reachable and authenticated.
2. Verify memory/personalization settings per source; record state.
3. Record collection-prompt and synthesis-prompt versions; note account quota or provider availability visible before collection.
4. Create dated directories. Open the cycle manifest entry.

**Collection**
5. Per source, per domain: fresh conversation → send prompt → wait for idle → verify complete → classify final content and every attempt → write log with metadata, prompt, and response.
6. Log every failure, unavailable attempt, refusal, retry, and version anomaly to the manifest.
7. Commit and push. **The corpus exists remotely before synthesis begins.**

**Synthesis**
8. Give Claude `synthesis/PROMPT.md` verbatim. Write Synthesis A with its context declaration and validate the item ledger.
9. Give Codex the same prompt byte for byte. Write Synthesis B with its context declaration and validate the item ledger.

**Convergence**
10. Compare using the declared context modes. Write schema-valid `comparison.json`.
11. Preserve all disagreement verbatim to `dissent.md`.
12. Run hemisphere weighting and silent-agreement checks.

**Tracking**
12a. Read and record both context declarations; they determine independent or informed labels.
12b. Match each comparison item against stable tracked signal IDs.
12c. Update re-surface counts, blind/informed split, basis history, blind streak, and confidence.
12d. Test each confirmation and falsifier; mark CONFIRMED, INVALIDATED, DECAYED, or UNRESOLVED only when its rule fires.
12e. Enter qualifying new items and assign immutable `ORC-YYYYMMDD-NNN` identifiers.
12f. Compute stagnation, hemisphere balance, Eastern split, mode counts, and silent domains. Write the tracking cycle note.
12g. Append every movement to `tracking/events.jsonl`, rebuild `tracking/signals.md`, and trigger post-mortems for invalidations.

**Output**
13. Write the reading in the nine-section format.
14. Update the logs index and manifest. Commit, push, drop to the routing folder.

**Post-cycle**
15. Read it.
16. Write post-mortems when reality reports back on earlier cycles.

---

# PART XI — IMPLEMENTATION STATUS

## 11.1 — APPLIED FROM v1.0

- Eastern refusal-recovery prompts versioned as `2026-07-18-v2`; v1 retained.
- Capture metadata backfilled with unknowns labeled rather than guessed.
- `capture` and final `content` split.
- Stable numbered domain filenames applied across all five sources.
- Cycle manifest and synthesis/convergence/readings scaffolding created.
- Public `SPEC.md`, README architecture, consumer contract, and per-cycle Git history established.

## 11.2 — APPLIED FROM v1.1

- Verbatim addendum committed and consolidated into this live specification.
- Shared byte-identical synthesis prompt v1.0 created.
- Context declaration replaces enforced synthesis ignorance.
- Six comparison states distinguish independent and informed modes.
- Synthesis ledger expanded with specifics, hemisphere, consumer tags, and stable tracker handoff.
- Tracker, cycle notes, archive, and nine-section reading structures created.
- `unavailable` split from `refused`; known multi-attempt exceptions backfilled without changing raw prompts or responses.
- Forward financial contradiction made visible in the addendum, live spec, and README.
- Stable global signal IDs, append-only lifecycle events, and JSON Schemas added as v1.1 implementation extensions.

## 11.3 — NEXT EXECUTION WORK

1. Implement automated wait-for-idle, terminator, and source × domain length-median checks before new captures receive `capture: complete`.
2. Run the first Claude and Codex syntheses with `synthesis/PROMPT.md` and preserve both context declarations.
3. Implement the comparison matcher once; reuse it for synthesis comparison and tracker matching.
4. Validate structured items against `schemas/`, write comparison and dissent, then initialize the first real tracked signals.
5. Generate the first nine-section reading and tracker cycle note.
6. Replay `tracking/events.jsonl` into `tracking/signals.md` and treat any mismatch as a harness failure.
7. Test the redesigned Eastern prompts in the next collection cycle.

Per-cycle commits remain mandatory. A degraded cycle commits as degraded rather than waiting for a clean narrative.

---

# PART XII — OPEN VARIABLES

**12.1 — Third synthesizer.** Both synthesizers are western (§4.5). Whether to add an eastern synthesis pass, and whether any eastern surface can be run with adequate context isolation given the refusal patterns observed.

**12.2 — Comparison implementation.** Whether the harness compares items by embedding similarity, structured field matching, or an isolated model call. A model call reintroduces AI judgment into the one layer built to be free of it; structured matching may be too brittle for natural-language claims. **Hardest open question in this document.**

**12.3 — Prompt stability versus memory defense.** Identical prompts every cycle make the corpus a real longitudinal instrument (§5.4). Rotating wording defends against collector memory (§4.3). These pull directly against each other and both matter. Current lean: hold prompts stable, defend memory through fresh conversations and account settings, and treat any prompt change as a versioned event.

**12.4 — Cross-cycle synthesis. `[RESOLVED v1.1]`** A synthesizer may read prior captures, syntheses, readings, prompts, and the tracker. It declares what it read. The default keeps tracker matching after both syntheses because blind re-surface counts are cleaner, but access is not fenced. Outcome history will determine whether informed and independent modes perform differently.

**12.5 — Flag condition evaluation.** How often to review which flag conditions actually hit. A condition that never fires across many cycles is either wrong or badly worded — but distinguishing "wrong" from "the thing genuinely isn't happening yet" requires judgment.

**12.6 — Memory-state verification.** Whether provider memory settings can be programmatically confirmed or only manually checked. Currently manual, therefore currently unreliable.

**12.7 — Cycle frequency.** One per day is the current assumption; throughput allows more. Whether more cycles add signal or correlated noise is unmeasured.

**12.8 — Refusal tracking as a product.** Refusal-boundary movement over time may be more valuable to some consumers than the convergence reading. Whether to surface it as a distinct artifact.

---

# PART XIII — BUILD SEQUENCE

**Stage 1 — Collection.** `[BUILT]` Five sources, eight domains, forty captures, public archive, prompts versioned beside logs.

**Stage 2 — Capture hardening.** `[PARTIAL]` Metadata, attempt classification, stable filenames, and prompt redesign are built. Automated completeness verification remains next.

**Stage 3 — Dual synthesis.** `[READY]` Give Claude and Codex the same `synthesis/PROMPT.md`. Preserve their context declarations and schema-valid item ledgers.

**Stage 4 — Comparison.** `[NEXT]` Implement §12.2 once, emit six context-aware states, and preserve dissent verbatim.

**Stage 5 — Tracking and reading.** Match stable signal IDs, append lifecycle events, rebuild the tracker projection, and write the nine-section reading.

**Stage 6 — Daily operation.** Run it repeatedly. Let post-mortems teach the engine what this document got wrong.

**Stage 7 — Public website.** Generate synthesis, signal history, research, financial, evidence, dissent, and cycle-health views from the structured public artifacts without creating a second source of truth.

**Stage 8 — First stateful consumer.** Attach one downstream system through explicit artifact flow. If it needs an artifact the engine does not expose, extend §6.1 and version the contract rather than creating a hidden dependency.

---

# CLOSING

The engine reads the world at a volume no single conversation reaches, and keeps those readings honest enough that other systems can be built on them.

Everything structural serves one requirement: **the conditions behind every reading must remain visible.** The five collection readings stay differentiated and isolated. The two syntheses may be independent or informed, but their context is declared and their outcomes are measured separately. Influence that is labeled is a finding; influence that is hidden is a distortion.

The prompts are the instrument. The flag conditions are standing hypotheses about what matters, and the ones a source refuses are the ones aimed at its own blind spot — which makes them the most valuable questions in the set and the ones that must be asked from the right layer.

The corpus outlasts every individual reading. Forty logs a day, prompt-linked and version-stamped, create the evidence base. Stable signal IDs and append-only lifecycle events turn that evidence into a replayable instrument that can report when it was right, wrong, stale, or unresolved.

The convergence layer does not know what a browser is. Keep it that way and the acquisition layer can change as many times as it needs to.

Disagreement is information.

Absence is signal. Refusal is the loudest kind.

Declared context is the thing that makes independence measurable.

Artifact flow is explicit.

---

*Orchestra Engine Specification v1.1*
*Jaron Kyler Bragg — July 2026*
*Public archive · MIT License*
