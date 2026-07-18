# Orchestra

Orchestra is a public, longitudinal intelligence engine. Each cycle asks five AI systems eight differentiated questions, preserves every capture, gives the same corpus to two synthesizers, compares their readings, and tracks falsifiable signals until they are confirmed, invalidated, decayed, or left unresolved.

The repository is the remote-first record: prompts, raw captures, failures, declared synthesis context, disagreement, tracker movement, and specification history all remain inspectable.

## Inspiration

The first spark was P.A.M., the robot in *Fallout 4*. Jaron wanted to build a real system that could gather scattered information, detect patterns, and preserve uncertainty instead of pretending prediction was certainty. What began as one possible system widened when the same machinery proved useful across markets, research, technology, energy, geopolitics, business, demographics, and physical supply chains. That wider instrument became Orchestra.

## Governing documents

- [SPEC.md](SPEC.md) is the consolidated, live specification, currently v1.1.
- [SPEC-ADDENDUM-v1.1.md](SPEC-ADDENDUM-v1.1.md) is the verbatim synthesis-and-tracking addendum produced by Claude Opus 4.8 with Jaron Kyler Bragg.
- [CHANGELOG.md](CHANGELOG.md) records repository-level specification and structure changes.

The addendum changes an important design assumption: synthesis context is recorded, not hidden. Both synthesizers may read any public artifact in the repository, but each must declare what it read so agreement and disagreement can be labeled as independent or informed.

## Current state

| Stage | Status |
|---|---|
| Acquisition: five sources × eight domains | Built and observed |
| Public archive, versioned prompts, metadata, and manifest | Built |
| Eastern refusal-recovery prompt redesign | Prepared as `2026-07-18-v2` |
| Attempt-level refusal versus availability classification | Added for known multi-attempt exceptions |
| Shared synthesis prompt and machine-readable contracts | Built; first synthesis run pending |
| Longitudinal signal tracker | Scaffolded; no signals entered before synthesis |
| Completeness verification | Procedural; automated enforcement remains next |
| Comparison, nine-section reading, and tracker updates | Next execution stage |
| Public website consumption layer | Planned |

## First complete cycle

- Cycle date: 2026-07-18
- Sources: Grok, Perplexity, Gemini, DeepSeek, Kimi
- Domains per source: 8
- Complete final captures: 40/40
- Substantive final captures: 38/40
- Refused final captures: 2/40
- Empty final captures: 0
- Total attempts: 45
- Known unavailable attempts before a final capture: 4
- Wall clock: approximately 38 minutes

DeepSeek’s final Geopolitics / Conflict capture was a refusal. Kimi’s AI / Tech sequence was different: two unavailable K3 attempts were followed by one complete K2.6 refusal. Availability failures describe account or provider state; refusals describe the information environment. Orchestra records them separately.

Browse the [logs index](logs/README.md) or the [2026-07-18 cycle manifest](manifest/2026-07-18.md).

## Architecture

Orchestra has three layers with hard seams:

| Layer | Responsibility | Boundary |
|---|---|---|
| **Acquisition** | Open fresh AI sessions, send prompts, verify, classify, and store raw captures | Exercises no judgment about meaning |
| **Convergence** | Synthesize twice, declare context, compare modes, update signals, and produce the reading | Does not control acquisition surfaces |
| **Consumption** | Financial, research, content, monitoring, and the future public website | Receives attributable artifacts |

The core provenance chain is:

```text
prompt → capture → synthesis item → comparison state → tracked signal → lifecycle event → reading
```

Every link has a stable identifier or path. The append-only event ledger makes tracker history replayable; the Markdown tracker remains the human-readable current view.

## Four levels of convergence

| Level | Name | What agrees |
|---|---|---|
| Collection | Flag hit | The world and a pre-registered prompt hypothesis |
| Synthesis | Source convergence | Two or more collection sources |
| Reading | Synthesis convergence | The two synthesis outputs |
| Time | Re-surface | A current item and a tracked signal from an earlier cycle |

Persistence is not confirmation. A signal can re-surface repeatedly without satisfying its pre-declared confirmation condition.

## Consumer contract

Orchestra promises:

1. **Provenance** — every claim traces to captures, prompts, model versions, context declarations, and lifecycle events.
2. **Preserved disagreement** — independent and informed disagreement remain visible rather than averaged away.
3. **Honest failure** — refusals, unavailability, truncation, retries, degraded cycles, and unresolved signals remain part of the record.

It does not promise that a reading is correct. It promises that the reading is an honest, inspectable function of the available corpus and declared context.

## Repository map

```text
orchestra/
├── SPEC.md
├── SPEC-ADDENDUM-v1.1.md
├── CHANGELOG.md
├── logs/<source>/
│   ├── prompts.md
│   └── YYYY-MM-DD/01-crypto.md ... 08-supply-chain.md
├── manifest/YYYY-MM-DD.md
├── synthesis/PROMPT.md
├── synthesis/YYYY-MM-DD/
├── convergence/YYYY-MM-DD/
├── tracking/
│   ├── signals.md
│   ├── events.jsonl
│   ├── cycles/
│   └── archive/
├── schemas/
└── readings/YYYY-MM-DD.md
```

- [Raw logs and standing prompts](logs/)
- [Cycle manifests](manifest/)
- [Shared synthesis contract](synthesis/)
- [Comparison and preserved dissent](convergence/)
- [Longitudinal signal tracking](tracking/)
- [Machine-readable artifact contracts](schemas/)
- [Human-readable daily readings](readings/)

## Archive discipline

- Raw prompts and responses remain verbatim.
- Corrections are additive and identify the record they amend.
- Prompt and specification changes are versioned, never silently overwritten.
- Final capture outcome and individual attempt outcome are separate.
- Context exposure is declared rather than concealed.
- Signal state is projected from an append-only lifecycle ledger.
- Every cycle commits, including degraded cycles.
- The corpus is pushed remotely before synthesis begins.

The 2026-07-18 metadata was backfilled during the v1.0 migration. Unknown timestamps and memory settings remain `unknown`; Git history preserves the earlier archive.

## Forward-declared financial change

The v1.1 addendum documents a boundary that is true now and intentionally scheduled to change: financial state currently lives outside this repository, but may later become an internal directory and published view. That migration has not happened. No account credential or account state is present here today. When it happens, context declarations, directional data flow, tracker archival, and public/private field classification must change together; [Part H of the addendum](SPEC-ADDENDUM-v1.1.md#part-h--known-forward-contradiction) is the migration record.

## Website direction

The planned public website is a consumption view over the same artifacts: synthesis first, then tracked-signal history, research and financial views, source evidence, disagreement, cycle health, and archive history. Structured schemas and the event ledger are included now so the website can be generated from the public record instead of becoming a second source of truth.

## License

Released under the [MIT License](LICENSE).
