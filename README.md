# LLM Mosaic

> **Assemble, don't synthesize.** A knowledge organization pattern that uses LLMs to juxtapose multiple independent sources on the same subject, annotate their discrepancies, and trace every claim back to its origin.

## What is LLM Mosaic?

LLM Wiki answers: "What do I know about X?"  
LLM Mosaic answers: **"What does Source A say about X? Source B? Source C? And where do they disagree?"**

When you have multiple independent accounts of the same subject — biographies by different authors, analyst reports from different firms, legal documents from different sides — the disagreements are the most valuable information. But conventional LLM workflows are designed to *erase* those disagreements through synthesis.

LLM Mosaic does the opposite: **juxtapose, don't merge; trace every claim.**

## Read the Full Document

- [LLM Mosaic（中文版）](LLM%20Mosaic_CN.md) | [中文 README](README_CN.md)
- [LLM Mosaic (English)](LLM%20Mosaic_EN.md)

## Core Principles

1. **Assemble, don't synthesize** — Lay out every source's account side by side. Preserve the original. Don't merge. Don't judge for the reader.
2. **Trace every claim** — Every assertion carries a clickable link back to the exact paragraph in the source material.
3. **One entity, one page** — Each person, event, or concept gets one page that houses all source perspectives.
4. **The matrix is the control panel** — An entity × source matrix (`entity-index.md`) tells the LLM where to look before compiling, so it only reads relevant passages.
5. **Honestly mark silence** — If a source says nothing about an entity, write "Not specifically addressed." What a source chooses *not* to write is itself a perspective.
6. **Add a column, not a rewrite** — A new source means a new column in the matrix and selective backfill, never a full rewrite.

## Architecture

```
Raw Sources (read-only) → Mosaic Pages (juxtaposed, per-source, traced) → Output (optional)
```

Two supporting files: `entity-index.md` (entity × source matrix) and `log.md` (append-only operation log).

## Where It Applies

Any domain where multiple independent perspectives look at the same subject:

- Historical biography (multiple biographers)
- Legal analysis (prosecution, defense, judgment)
- Competitive intelligence (multiple analyst reports)
- Literary criticism (multiple critical schools)
- Multi-national perspectives on historical events

## Origin

LLM Mosaic's methodology emerged from the Su Dongpo research project — 4 biographies + 3 volumes of annals, 4 million characters, a 56-entity × 7-source cross-reference matrix.

## License

MIT
