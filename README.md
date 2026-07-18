# Orchestra

Orchestra is a daily, multi-source intelligence collection and convergence engine. It asks five independent AI systems eight purpose-built questions, preserves every capture verbatim, produces two isolated syntheses, compares them mechanically, and publishes a reading without hiding disagreement or failure.

The repository is the public, remote-first archive. The governing architecture, guarantees, runbook, failure model, and build sequence live in [SPEC.md](SPEC.md).

## Current state

| Stage | Status |
|---|---|
| Acquisition: five sources × eight domains | Built and observed |
| Public archive and versioned prompt sets | Built |
| Capture metadata, numbered filenames, and cycle manifest | Backfilled for the first cycle |
| Eastern refusal-recovery prompt redesign | Prepared as `2026-07-18-v2` for the next cycle |
| Completeness verification | Procedural; code enforcement remains next |
| Dual synthesis, convergence, and daily reading | Next build stage |
| Public website consumption layer | Planned |

## First complete cycle

- Cycle date: 2026-07-18
- Sources: Grok, Perplexity, Gemini, DeepSeek, Kimi
- Domains per source: 8
- Complete captures: 40/40
- Substantive captures: 38/40
- Empty captures: 0
- Wall clock: approximately 38 minutes

DeepSeek refused Geopolitics / Conflict on submission and exact retry. Kimi's K3 agent failed twice on AI / Tech; K2.6 then refused. Both refusals remain in the archive as data.

Browse the [logs index](logs/README.md) or the [2026-07-18 cycle manifest](manifest/2026-07-18.md).

## Architecture

Orchestra has three layers with hard seams:

| Layer | Responsibility | Boundary |
|---|---|---|
| **Acquisition** | Open fresh AI sessions, send prompts, wait, verify, classify, and store raw captures | Exercises no judgment about meaning |
| **Convergence** | Read files, synthesize twice in isolation, compare, and produce the reading | Does not know or control acquisition surfaces |
| **Consumption** | Trading, research, content, monitoring, and the future public website | Receives artifacts and never writes backward into the engine |

Artifacts flow outward only. Consumer state never re-enters collection or synthesis.

## Consumer contract

Orchestra promises three things:

1. **Provenance** — every claim can trace to a source, prompt version, capture, and cycle.
2. **Preserved disagreement** — divergent and asymmetric readings are not averaged away.
3. **Honest failure** — refusals, truncations, retries, unavailable sources, and degraded cycles are surfaced.

It does not promise that a reading is correct. It promises that the reading is an honest function of what the sources actually said.

## Repository map

```text
orchestra/
├── SPEC.md
├── logs/<source>/
│   ├── prompts.md
│   └── YYYY-MM-DD/01-crypto.md ... 08-supply-chain.md
├── manifest/YYYY-MM-DD.md
├── synthesis/YYYY-MM-DD/
├── convergence/YYYY-MM-DD/
└── readings/YYYY-MM-DD.md
```

- [Raw logs and standing prompts](logs/)
- [Cycle manifests](manifest/)
- [Independent syntheses](synthesis/)
- [Mechanical comparisons and preserved dissent](convergence/)
- [Human-readable daily readings](readings/)

## Archive discipline

- Nothing is deleted.
- Raw responses remain verbatim.
- Corrections are additive and reference the original.
- Prompt changes are versioned, never silently overwritten.
- Every cycle commits, including degraded cycles.
- Capture completeness and content substance are recorded separately.
- The corpus is pushed remotely before synthesis begins.

The 2026-07-18 metadata was backfilled during the SPEC v1.0 migration. Unknown timestamps and memory settings are explicitly marked `unknown` rather than guessed. Git history preserves the pre-migration archive.

## Website direction

A public website is planned as a consumption layer over these artifacts: a synthesis-first view, with dedicated pages for research, financial information, source-level evidence, disagreement, and archive history. The website will read from the engine's outputs; it will not write back into acquisition or convergence.

## License

Released under the [MIT License](LICENSE).
