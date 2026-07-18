# Orchestra

Orchestra is a public archive of domain-specific intelligence collection prompts and AI responses. It currently contains the first manual collection cycle across Grok, Perplexity, Gemini, DeepSeek, and Kimi.

## Browse the archive

Start with the [logs index](logs/README.md), then choose an AI and collection date.

```text
logs/
├── grok/
│   ├── prompts.md
│   └── YYYY-MM-DD/
├── perplexity/
│   ├── prompts.md
│   └── YYYY-MM-DD/
├── gemini/
│   ├── prompts.md
│   └── YYYY-MM-DD/
├── deepseek/
│   ├── prompts.md
│   └── YYYY-MM-DD/
└── kimi/
    ├── prompts.md
    └── YYYY-MM-DD/
```

Each dated directory contains one Markdown file per collection domain. Every daily log preserves both the exact prompt and the AI response.

## Current archive

- Collection date: 2026-07-18
- AI layers: 5
- Domains per AI: 8
- Response logs: 40
- Empty logs: 0

Two saved responses are refusals rather than substantive signal collections:

- DeepSeek refused the Geopolitics / Conflict prompt on the initial submission and an exact retry.
- Kimi's K3 agent failed twice on AI / Tech Breakthroughs; K2.6 then returned a refusal.

The refusals remain in the archive so the collection record is complete and does not silently hide missing coverage.

## Domains

1. Crypto
2. Stocks / Markets
3. AI / Tech Breakthroughs
4. Energy / Advanced Science
5. Geopolitics / Conflict
6. Major Companies / Business Intel
7. Demographics / Consumer Behavior / Cultural Signal
8. Supply Chain / Logistics / Infrastructure

## Adding another day

Create a new ISO-date directory under each participating AI:

```text
logs/<ai>/YYYY-MM-DD/
```

Use the same domain filenames where possible. Update that AI's `prompts.md` whenever its standing prompt set changes.

## License

Released under the [MIT License](LICENSE).
