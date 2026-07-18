# Convergence artifacts

For each synthesized cycle this directory contains:

- `comparison.json` — comparison items validated against [comparison.schema.json](../schemas/comparison.schema.json).
- `dissent.md` — disagreement preserved verbatim and unresolved.

Each comparison item uses one of six context-aware states:

- `INDEPENDENT_CONVERGENT`
- `INFORMED_CONVERGENT`
- `INDEPENDENT_DIVERGENT`
- `INFORMED_DIVERGENT`
- `ASYMMETRIC`
- `SUSPECT`

The state is derived from the two synthesis context declarations plus the comparison result. Comparison belongs to the harness, not to a collector or synthesizer. See [SPEC.md](../SPEC.md).
