# Logs index

The raw corpus is organized by source and cycle date. Each daily file contains metadata, the exact prompt sent, the verbatim response, and mechanical capture notes.

| Source | Differentiated reading | Standing prompts | Current prompt version | 2026-07-18 | Substantive |
|---|---|---|---|---|---:|
| Grok | Live social signal and practitioner activity | [prompts](grok/prompts.md) | `2026-07-18-v1` | [8 captures](grok/2026-07-18/) | 8/8 |
| Perplexity | Official sources, filings, and institutional record | [prompts](perplexity/prompts.md) | `2026-07-18-v1` | [8 captures](perplexity/2026-07-18/) | 8/8 |
| Gemini | Academic research, patents, budgets, and structural data | [prompts](gemini/prompts.md) | `2026-07-18-v1` | [8 captures](gemini/2026-07-18/) | 8/8 |
| DeepSeek | Eastern institutional vantage | [prompts](deepseek/prompts.md) | `2026-07-18-v2`; cycle used v1 | [8 captures](deepseek/2026-07-18/) | 7/8 |
| Kimi | Independent second Eastern reading | [prompts](kimi/prompts.md) | `2026-07-18-v2`; cycle used v1 | [8 captures](kimi/2026-07-18/) | 7/8 |

## Stable domain filenames

1. `01-crypto.md`
2. `02-stocks-markets.md`
3. `03-ai-tech.md`
4. `04-energy-science.md`
5. `05-geopolitics-conflict.md`
6. `06-companies-business.md`
7. `07-demographics-culture.md`
8. `08-supply-chain.md`

The numeric prefix is the cross-source join key. The same number always identifies the same domain.

## Status and provenance

The [cycle manifest](../manifest/2026-07-18.md) records capture status, substance, model information, attempts, and known failures. The [governing specification](../SPEC.md) defines the archive contract and future pipeline.
