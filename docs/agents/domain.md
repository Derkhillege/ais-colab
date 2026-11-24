# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

This repo is **single-context**: one glossary and one ADR directory, both at the root.

## Before exploring, read these

- **`GLOSSARY.md`** at the repo root.
- **`docs/adr/`**: read the ADRs that touch the area you're about to work in.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## File structure

```
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-two-layer-reference-frame-for-lock-kpis.md
│   └── 0002-evaluate-lock-kpis-by-verdict-ambiguity.md
└── src/
    └── ais_shader/
```

If this repo ever splits into genuinely separate contexts, the multi-context layout is a root `GLOSSARY-MAP.md` pointing at one `GLOSSARY.md` per context, with context-scoped decisions under `src/<context>/docs/adr/`. Until that file exists, treat the repo as single-context.

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `GLOSSARY.md`. Don't drift to synonyms the glossary explicitly avoids — it pins known term collisions deliberately, so picking a synonym reintroduces the ambiguity the entry exists to remove.

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0002 (evaluate lock KPIs by verdict ambiguity), but worth reopening because…_
