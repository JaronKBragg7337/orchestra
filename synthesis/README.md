# Synthesis artifacts

Both synthesizers receive [PROMPT.md](PROMPT.md) byte for byte. Collection systems are differentiated instruments; synthesizers are two readers of the same corpus, so their prompt must be identical for their differences to remain interpretable.

Each synthesis begins with a self-declared context block:

```text
read_captures:          yes
read_prior_synthesis:   yes | no        which one, if yes
read_tracker:           yes | no
read_prior_readings:    yes | no        how far back
read_prompts:           yes | no
other_context:          anything else consulted
```

Context is recorded, not restricted. The comparison layer uses these declarations to distinguish independent from informed convergence and divergence.

Per cycle:

- `synthesis-a.md` — Claude’s human-readable reading.
- `synthesis-a.json` — Claude’s context block and item ledger as structured data.
- `synthesis-b.md` — Codex’s human-readable reading.
- `synthesis-b.json` — Codex’s context block and item ledger as structured data.

No synthesis artifacts have been produced yet. The first execution must use prompt version `1.0`; each JSON sidecar validates against [the synthesis-output schema](../schemas/synthesis-output.schema.json), whose ledger entries use the synthesis-item contract.
