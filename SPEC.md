# ORCHESTRA — ENGINE SPECIFICATION v1.0

**Jaron Kyler Bragg — July 2026**
`github.com/JaronKBragg7337/orchestra` · Public archive · MIT License

---

## HOW TO READ THIS

This is the governing specification for the Orchestra engine and the repository that holds it. It describes what the engine is, what it guarantees, how the archive is structured, and what gets built next.

**Status tags:** `[BUILT]` running now · `[NEXT]` immediate build target · `[CORE]` structural, do not violate · `[DISCIPLINE]` procedural, not code-enforced, therefore fragile · `[OPEN]` genuinely undecided.

**Confidence tiers:** **Verified** — observed in operation · **Inferred** — follows from known structure · **Assumed** — plausible, untested, flagged.

Part XI is the implementation brief: the concrete list of changes to apply to the repository.

---

# PART I — THE ENGINE

## 1.1 — WHAT ORCHESTRA IS `[CORE]`

Orchestra is a daily, high-volume, multi-source intelligence collection and convergence engine running on local hardware under human operation, publishing to a public archive.

Each cycle it asks five independent AI systems a set of purpose-built questions across eight domains of the world, captures every answer verbatim without letting any source see another's, produces two independent syntheses of the whole corpus, compares those syntheses mechanically, and outputs a reading.

**Something asks. Something captures. Something interprets twice, independently. Something compares. Something reports.**

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
| **Convergence** | Comparison of two independent syntheses | Contradictions no single source can see, because they exist between sources |

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

The same pattern explains the Kimi AI/Tech outcome: that prompt asks for Chinese capability milestones, export-control contradictions, and strategic military application — a refusal-dense region for an eastern-hosted model.

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

## 3.3 — CAPTURE STANDARD `[NEXT]`

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
capture:        complete | truncated | retry-N | failed
content:        substantive | refused | deflected | empty
attempts:       1
---
```

**`model_version` exists because a single named source is not a single model.** Kimi's AI/Tech capture failed twice on K3 and was then refused by K2.6 — two models, two behaviors, one nominal source, one cycle. Without this field, model routing changes look like the world changing. `[Verified]`

**`memory_state` exists because of §4.3.** `unknown` or `enabled` reduces confidence in every downstream claim built on that capture, and the reading says so.

**`prompt_version` exists because flag conditions evolve.** A log is only interpretable against the prompt set that produced it.

### 3.3.1 — COMPLETE IS NOT SUBSTANTIVE `[CORE]`

Two independent checks, two independent fields.

- **`capture`** answers *did the text arrive intact* — generation finished, nothing truncated, extraction clean.
- **`content`** answers *did the source actually answer* — a refusal is a complete capture with no substance, and so is a deflection.

Collapsing these produces the worst available error: a cycle reporting 40/40 successful captures while synthesis treats polite non-answers as evidence.

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

## 3.4 — SYNTHESIS A `[NEXT]` `[CORE]`

Claude, via Claude Code, reads the day's captures from disk and produces a structured synthesis.

**Claude Code is required for this role.** Chat surfaces carry conversation history, project context, and persistent memory of the operator — prior projects, frameworks, conclusions. A synthesizer that already knows what the operator believes is not an independent reading of the world.

**Filesystem contamination `[DISCIPLINE]` `[Inferred — high confidence]`:** Claude Code reads `CLAUDE.md` and project memory from the working directory and its parents. If synthesis runs inside a repo carrying a `CLAUDE.md` that describes the operator's philosophy, goals, or prior conclusions, the contamination closed by avoiding chat re-enters through the filesystem.

**Enforcement:** synthesis runs in a directory with either no `CLAUDE.md` or a minimal one containing synthesis instructions only — nothing about the operator, nothing about prior readings. Verify at the start of every cycle. This is the easiest rule here to break by accident.

## 3.5 — SYNTHESIS B `[NEXT]` `[CORE]`

After Synthesis A is written and sealed, the harness produces its own independent synthesis over the same captures.

**Ordering rule:** Synthesis B is written without Synthesis A in scope. Enforced at file-path level — A is written where the B step does not read, and the two meet only at comparison.

**Why the second synthesizer is not also a collector:** a system that both reads the world and synthesizes the readings finds its own collection output persuasive, because it is the output it would have produced. It reinforces its own priors and calls the result convergence. The harness never touches a live source; it only reads captures. **The firewall is architectural, not procedural** — there is no discipline to maintain and nothing to check. `[Verified by construction]`

## 3.6 — SYNTHESIS ITEM FORMAT `[NEXT]`

Both synthesizers emit items in the same shape so the harness can compare mechanically:

```
domain:      one of the eight
claim:       one sentence, falsifiable
direction:   emerging | escalating | resolving | stable | reversing
confidence:  Verified | Inferred | Assumed
basis:       which captures support it (source + domain + date)
falsifier:   what observation would make this wrong
horizon:     when this should be visible if true
flag_hit:    which prompt flag condition this satisfies, if any
```

**`falsifier` is mandatory.** An item without one is a mood, not a claim. The harness rejects it back to the synthesizer for rewrite.

**`flag_hit`** links convergence-layer output back to the collection-layer hypotheses in §2.2, which is how the flag conditions get evaluated over time: a flag condition that never hits across many cycles is either wrong or badly worded, and that is worth knowing.

## 3.7 — CONVERGENCE `[NEXT]` `[CORE]`

The harness compares Synthesis A and Synthesis B. Per item, one of four states:

| State | Meaning |
|---|---|
| **CONVERGENT** | Both readings reach the same conclusion at compatible confidence |
| **DIVERGENT** | Both engaged the item, reached different conclusions |
| **ASYMMETRIC** | One reading surfaced it, the other did not mention it at all |
| **SUSPECT** | Both agree, but the agreement pattern trips an independence check (§4.6) |

**Divergence is the highest-information output the engine produces.** Two independent readings of identical evidence reaching different conclusions means the evidence genuinely underdetermines the answer. That is a fact about the world, and a single-model pipeline erases it silently.

**Asymmetric is the second most interesting** — something one synthesizer treated as central and the other did not notice at all is a live question about attention and framing, and often the earliest indicator available.

**Neither is resolved by the harness. Neither is averaged. Neither is dropped for tidiness.** Consumers decide what to do with disagreement. The engine hands it over intact.

## 3.8 — THE READING `[NEXT]`

Each cycle terminates in one human-readable document.

**1 — CONVERGENT.** Ranked. Each item: domain, claim, confidence, basis, falsifier, flag hit.
**2 — DIVERGENT.** The question, reading A, reading B, what the disagreement hinges on. Verbatim, unresolved.
**3 — ASYMMETRIC.** What it was, which synthesizer raised it, plausible reason the other missed it.
**4 — SILENCE.** Domains that produced nothing. Expected topics absent across all five sources. **Refusals, with source, domain, and model version.** Absence is signal; refusal is the loudest kind.
**5 — CONTRADICTION.** Where sources contradicted each other at collection, with attribution. Includes collection-layer flag hits from §2.2.
**6 — CROSS-DOMAIN CONNECTIONS.** Where two domains appear to describe the same underlying thing.
**7 — FLAGS.** Independence checks tripped, failed captures, model-version changes, unavailable sources, anything degraded about this cycle.

---

# PART IV — THE INDEPENDENCE MODEL

The engine reports agreement between systems. If those systems were secretly reading each other, or reading the operator, the agreement is manufactured and the engine produces confident garbage. Confident garbage is worse than silence, because a consumer acts on it.

**Everything in this part exists so that convergence means something.**

## 4.1 — VECTOR 1: SYNTHESIZER MEMORY OF THE OPERATOR

*Closed by architecture.* Claude Code carries no chat history of the operator. The harness carries no chat memory. Both isolated by tool selection rather than discipline.

*Residual:* filesystem context per §3.4. Live. Check every cycle.

## 4.2 — VECTOR 2: SYNTHESIZER CROSS-READING

*Closed by ordering.* Synthesis B is produced before Synthesis A is readable, enforced at file-path level per §3.5.

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

**Stated plainly:** both synthesizers being western is a real limitation on perspective diversity at the synthesis layer, accepted in exchange for the architectural firewall in §3.5. Whether to add an eastern synthesis pass is open — §12.1. The tradeoff is not free.

## 4.6 — SILENT AGREEMENT DETECTION `[NEXT]` `[CORE]`

Flag `SUSPECT` when:

1. Synthesis A and B agree with structurally near-identical reasoning, not merely identical conclusions.
2. All five sources return near-identical evidence packages with no source-characteristic differentiation — particularly suspicious given how differently the five are prompted (§2.3).
3. Eastern sources match western sources verbatim where genuine perspective difference is expected.
4. The same named specific appears in identical framing across all five sources in one cycle with no prior visibility.

**Response: label it in FLAGS with the reason. Do not suppress, do not block.**

This is the engine's whole posture on rules — **surface and label; the consumer decides.** Blocking belongs to systems that take consequential action. The engine hands over a document.

---

# PART V — THE ARCHIVE

## 5.1 — TARGET REPOSITORY STATE

```
orchestra/
├── README.md                        entry point, current archive status
├── LICENSE                          MIT
├── SPEC.md                          this document
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
│   └── YYYY-MM-DD/
│       ├── synthesis-a.md           Claude Code, sealed before B begins
│       └── synthesis-b.md           harness, written with A out of scope
├── convergence/
│   └── YYYY-MM-DD/
│       ├── comparison.json          machine-readable convergence states
│       └── dissent.md               preserved disagreement, verbatim
└── readings/
    └── YYYY-MM-DD.md                the seven-section daily output
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

## 5.3 — ARCHIVE DISCIPLINE `[CORE]`

1. **Nothing is deleted.** Failures, refusals, empties, and retries all persist.
2. **Nothing is edited after commit.** Corrections are new files referencing the original.
3. **Prompts are versioned beside their logs.** A log without its exact prompt set is uninterpretable later.
4. **Every cycle commits, including degraded ones.** Git history is the tamper-evident record; no separate immutability layer is needed.
5. **The manifest reflects reality including failure.** A manifest recording only successes lies by omission.
6. **Remote-first.** The corpus exists on GitHub before synthesis begins. The operator works primarily from a phone; a corpus that only exists on one machine is unavailable most of the time.
7. **Public and MIT by design.** Legible to collaborators with no access-granting mechanism, citable, and independently verifiable — anyone can check that the reading follows from the logs.

## 5.4 — THE CORPUS AS INSTRUMENT `[CORE]`

The daily reading is one product. **The corpus is the other, and over time it is the larger one.**

Forty logs per day is roughly 1,200 a month and 14,600 a year — each timestamped, prompt-linked, source-attributed, version-stamped, never edited.

Questions only the corpus can answer:

- Which sources called which developments early, and by how much.
- How a source's confidence on a topic moved before the world resolved it.
- Which flag conditions actually hit, and which never fire and should be rewritten.
- When refusal boundaries shifted, in which direction, and whether across sources simultaneously.
- Whether model version changes altered a source's read of the same question.
- **Whether the five sources are converging over time** — which would be Vector 3 or Vector 4 becoming measurable.

That last one matters most. **The corpus is the only instrument that can measure the engine's own core assumption.**

---

# PART VI — CONSUMER INTERFACE

## 6.1 — ARTIFACTS

| Artifact | Path | Contents |
|---|---|---|
| Raw corpus | `logs/<source>/<date>/` | Every capture with metadata, prompt, and response |
| Prompt sets | `logs/<source>/prompts.md` | Standing prompts, versioned |
| Manifest | `manifest/<date>.md` | Per-capture status including every failure |
| Synthesis A | `synthesis/<date>/synthesis-a.md` | First independent reading |
| Synthesis B | `synthesis/<date>/synthesis-b.md` | Second independent reading |
| Comparison | `convergence/<date>/comparison.json` | Machine-readable convergence states |
| Dissent | `convergence/<date>/dissent.md` | Preserved disagreement, verbatim |
| Reading | `readings/<date>.md` | Seven-section human document |

**A consumer may take any subset.** A trading system might take the raw corpus and run its own synthesis under its own risk model, ignoring the engine's reading entirely. A content system might take only the reading. Both are correct uses.

## 6.2 — THE ENGINE'S CONTRACT

Three promises to every consumer:

1. **Provenance.** Every claim traces to specific captures, which trace to specific prompts, prompt versions, timestamps, and model versions.
2. **Preserved disagreement.** Nothing is averaged, resolved, or dropped for tidiness. What the sources disagreed about arrives as disagreement.
3. **Honest failure.** Refusals, truncations, unavailable sources, and tripped independence checks are reported, not hidden. A degraded cycle is labeled degraded.

**What the engine does not promise:** that any reading is correct. It promises the reading is an honest function of what the sources actually said.

## 6.3 — DIRECTIONALITY `[CORE]`

**Artifacts flow outward only.** No consumer writes into `logs/`, `synthesis/`, `convergence/`, or `manifest/`. No consumer's output re-enters collection or synthesis as input.

A trading system's positions must never influence what the engine reads or how it synthesizes. The moment consumer state feeds back into the engine, the engine stops reading the world and starts reading the consumer, and every system built on it inherits that distortion silently.

Consumers write to their own repositories. Absolute.

---

# PART VII — OPERATING RULES

1. **Convergence logic lives in the harness.** No AI decides what converged.
2. **No AI in the pipeline calls another AI.** All routing goes through the harness.
3. **One prompt, one domain, one fresh conversation.** Never batched, never continued.
4. **Raw captures are immutable.** Failures are captured, not deleted.
5. **Synthesis B is written before Synthesis A is readable.** File-path enforced.
6. **Synthesis A runs through Claude Code in a directory with no operator context.**
7. **`capture` and `content` are checked separately.** Refusals are captured as refusals.
8. **Refusals are never rephrased into compliance within a cycle.**
9. **Prompt sets are versioned, never silently overwritten.**
10. **Source differentiation is preserved.** Edits that make two sources more similar are almost always wrong.
11. **Disagreement is preserved verbatim.** Never averaged, never resolved by the harness.
12. **Artifacts flow outward only.** No consumer output re-enters the engine.
13. **Nothing operator-private enters a prompt.** The archive is public.
14. **Every cycle commits, including degraded ones.**
15. **The engine reports when it was wrong.**

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
| Kimi | 8/8 | 7 | AI/Tech failed twice on K3, then refused by K2.6 |
| **Total** | **40** | **38** | Zero empty files |

**Findings**

1. **Eastern collection costs ~2.6× the wall clock of western per capture.** Retries, refusals, and sign-in friction concentrate on the eastern side. Sources are not interchangeable in scheduling.

2. **Refusals are prompt-predictable, not source-random.** Both refusals landed on flag conditions asking an eastern-hosted model to indict its own information environment. §2.4 specifies the fix.

3. **A single named source is not a single model.** Kimi K3 failed twice; K2.6 then refused — two models, two behaviors, one cycle. `model_version` exists because of this.

4. **Session interruption is routine and has a clean handling pattern.** DeepSeek blocked at sign-in mid-cycle. The correct and observed response: pause, leave the login surface open, report the block, wait for the human to authenticate, resume on command. **Never automate around an authentication challenge.** The cycle completed after handoff.

5. **Zero empty files across 40 captures.** The capture layer is sound. Completeness verification (§3.3.2) should still be hardened before raising frequency — the failure it guards against is silent by nature and would not have appeared in this result.

---

# PART IX — FAILURE MODES

| Failure | Likelihood | Detection | Response |
|---|---|---|---|
| Session expired / logged out | High `[Verified]` | Capture returns login surface | Pause, leave surface open, report, wait for human, resume on command |
| CAPTCHA or bot challenge | Moderate | Capture returns challenge | Pause, hand to human. Never automate around it |
| Source refuses a domain | High `[Verified]` | `content` classifier | Capture verbatim, log to SILENCE, redesign prompt between cycles per §2.4 |
| Model version changed mid-cycle | Observed `[Verified]` | `model_version` mismatch | Log both, flag, do not merge as one source-read |
| Silent truncation | High, quiet | §3.3.2 three checks | Retry, keep both files |
| Streaming captured mid-generation | High | Wait-for-idle | Confirm completion, not presence |
| UI redesign breaks extraction | Moderate, recurring | Garbled or empty captures | Adapter fix only. Convergence untouched (§1.3) |
| Memory setting silently re-enabled | Moderate | Monthly audit | Re-disable, flag affected cycles |
| Rate limit mid-cycle | Moderate | Limit notice in capture | Partial cycle, logged as partial. Never fabricate the gap |

**On automating logged-in interfaces `[Assumed]`:** consumer AI surfaces change without notice and their terms around automation vary. The operational risk is breakage and lockout rather than enforcement. The three-layer seam exists precisely so that when a surface changes, only the adapter changes.

---

# PART X — CYCLE RUNBOOK

**Pre-cycle**
1. Verify all five sources reachable and authenticated.
2. Verify memory/personalization settings per source; record state.
3. Verify the synthesis directory carries no operator context.
4. Create dated directories. Open the cycle manifest entry.

**Collection**
5. Per source, per domain: fresh conversation → send prompt → wait for idle → verify complete → classify content → write log with metadata, prompt, and response.
6. Log every failure, refusal, retry, and version anomaly to the manifest.
7. Commit and push. **The corpus exists remotely before synthesis begins.**

**Synthesis**
8. Invoke Claude Code over the day's captures. Write Synthesis A. Seal.
9. Harness produces Synthesis B over the same captures, A out of scope.

**Convergence**
10. Compare. Write `comparison.json`.
11. Preserve all disagreement verbatim to `dissent.md`.
12. Run hemisphere weighting and silent-agreement checks.

**Output**
13. Write the reading in the seven-section format.
14. Update the logs index and manifest. Commit, push, drop to the routing folder.

**Post-cycle**
15. Read it.
16. Write post-mortems when reality reports back on earlier cycles.

---

# PART XI — CHANGES TO APPLY

Implementation brief. Ordered by value.

**1. Prompt redesign for refused domains — highest priority.**
Rewrite DeepSeek and Kimi Geopolitics / Conflict and AI / Tech Breakthroughs flag conditions per §2.4: characterize the eastern vantage in full specificity, remove clauses asking the source to adjudicate its own environment against Western fact. Version the change in `prompts.md` with a dated entry. Do not delete the prior version. Test next cycle — the domains should return substantive.

**2. Capture metadata block.**
Add the §3.3 frontmatter to the log template. Backfill what is knowable for 2026-07-18; mark unknowable fields `unknown` rather than guessing.

**3. Split `capture` from `content`.**
Two fields, two checks. The 2026-07-18 cycle is 40 complete captures, 38 substantive — record it that way.

**4. Completeness verification.**
Implement wait-for-idle, terminator check, and length heuristic per §3.3.2 before writing `capture: complete`.

**5. Numbered domain filenames.**
`01-crypto.md` through `08-supply-chain.md`, identical across every source directory. Rename existing files.

**6. Manifest directory.**
`manifest/YYYY-MM-DD.md` — per-capture status including every failure, retry, refusal, and version anomaly. The existing logs index stays as the navigation surface; the manifest is the status record.

**7. Directory scaffolding.**
Create `synthesis/`, `convergence/`, `readings/` with README stubs explaining what will live there.

**8. `SPEC.md`.**
This document, committed at the repo root, linked from `README.md`.

**9. README update.**
Add: what the engine is, the three layers, the consumer contract, and a pointer to `SPEC.md`. Keep the current-archive status block and update it per cycle.

**10. Per-cycle commits.**
Going forward, each cycle commits on completion rather than batching.

---

# PART XII — OPEN VARIABLES

**12.1 — Third synthesizer.** Both synthesizers are western (§4.5). Whether to add an eastern synthesis pass, and whether any eastern surface can be run with adequate context isolation given the refusal patterns observed.

**12.2 — Comparison implementation.** Whether the harness compares items by embedding similarity, structured field matching, or an isolated model call. A model call reintroduces AI judgment into the one layer built to be free of it; structured matching may be too brittle for natural-language claims. **Hardest open question in this document.**

**12.3 — Prompt stability versus memory defense.** Identical prompts every cycle make the corpus a real longitudinal instrument (§5.4). Rotating wording defends against collector memory (§4.3). These pull directly against each other and both matter. Current lean: hold prompts stable, defend memory through fresh conversations and account settings, and treat any prompt change as a versioned event.

**12.4 — Cross-cycle synthesis.** Whether a synthesizer should ever read prior cycles. Enables trend detection; re-opens operator contamination through the archive, since prior readings encode prior conclusions.

**12.5 — Flag condition evaluation.** How often to review which flag conditions actually hit. A condition that never fires across many cycles is either wrong or badly worded — but distinguishing "wrong" from "the thing genuinely isn't happening yet" requires judgment.

**12.6 — Memory-state verification.** Whether provider memory settings can be programmatically confirmed or only manually checked. Currently manual, therefore currently unreliable.

**12.7 — Cycle frequency.** One per day is the current assumption; throughput allows more. Whether more cycles add signal or correlated noise is unmeasured.

**12.8 — Refusal tracking as a product.** Refusal-boundary movement over time may be more valuable to some consumers than the convergence reading. Whether to surface it as a distinct artifact.

---

# PART XIII — BUILD SEQUENCE

**Stage 1 — Collection.** `[BUILT]` Five sources, eight domains, forty captures, public archive, prompts versioned beside logs.

**Stage 2 — Capture hardening.** `[NEXT]` Items 1–6 of Part XI. Prompt redesign first, since it should recover two refused domains immediately.

**Stage 3 — Synthesis A.** Claude Code over the day's captures in a context-clean directory. Verify no `CLAUDE.md` leakage by testing it adversarially, not by assuming.

**Stage 4 — Synthesis B.** Harness synthesis with A out of scope. **Verify the ordering firewall by attacking it.**

**Stage 5 — Comparison and reading.** Four convergence states, dissent preservation, seven-section output. Resolve §12.2 here.

**Stage 6 — Daily operation.** Run it repeatedly. Let post-mortems teach the engine what this document got wrong.

**Stage 7 — First consumer.** Attach one downstream system at the consumption seam. Confirm it never needs to reach backward into the engine. If it does, extend the artifact set in §6.1 rather than breaking the seam.

---

# CLOSING

The engine reads the world at a volume no single conversation reaches, and keeps those readings honest enough that other systems can be built on them.

Everything structural serves one requirement: **the five readings must be genuinely independent.** Differentiated prompts, fresh conversations, tool-level context isolation, ordered synthesis, hemisphere weighting, silent-agreement detection — all of it protects the same thing. Convergence between systems that were reading each other is not information, and a consumer acting on it inherits a distortion it cannot see.

The prompts are the instrument. The flag conditions are standing hypotheses about what matters, and the ones a source refuses are the ones aimed at its own blind spot — which makes them the most valuable questions in the set and the ones that must be asked from the right layer.

The corpus outlasts every individual reading. Forty logs a day, never edited, never deleted, prompt-linked and version-stamped, is the instrument that eventually measures whether the engine's core assumption still holds.

The convergence layer does not know what a browser is. Keep it that way and the acquisition layer can change as many times as it needs to.

Disagreement is information.

Absence is signal. Refusal is the loudest kind.

Independence is the thing being protected.

Artifacts flow outward only.

---

*Orchestra Engine Specification v1.0*
*Jaron Kyler Bragg — July 2026*
*Public archive · MIT License*
