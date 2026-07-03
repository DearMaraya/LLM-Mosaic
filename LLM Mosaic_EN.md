---
title: "LLM Mosaic — A Multi-Source Triangulation Pattern for LLM-Powered Knowledge Bases"
type: article
tags: [LLM, wiki, knowledge-base, multi-source-triangulation, methodology]
source: "Su Dongpo research multi-source assembly practice + LLM Wiki concept"
created: 2026-07-03
---

# LLM Mosaic

> A knowledge organization pattern that uses LLMs to **juxtapose multiple independent sources on the same subject, annotate their discrepancies, and trace every claim back to its origin**.
>
> LLM Wiki answers: "What do I know about X?" LLM Mosaic answers: "What does Source A say about X? Source B? Source C? And where do they disagree?"
>
> This is a concept document designed to be copy-pasted into your LLM Agent. It communicates the core ideas and operating model — your Agent collaborates with you to instantiate the implementation.

---

## The Core Idea

### What LLM Wiki Can't Do

LLM Wiki's default mode is *synthesis* — the LLM reads multiple documents, digests, merges, and outputs a coherent summary. For most use cases, this is sufficient. A textbook, an annual report, a set of meeting notes — you just need a clear, accurate answer.

But in some domains, **synthesis itself is the problem.**

**Historical biography**: Lin Yutang portrays Wang Anshi as a paranoid crank. Li Yibing portrays him as an idealistic reformer. Whom should you believe? The LLM gives you "On balance, Wang Anshi was a complex historical figure…" — technically not wrong, but it erases the very tension that matters most.

**Legal cases**: The prosecution, the defense, and the judge each narrate the same facts differently. The LLM synthesizes this into "This case involves…" — and you lose the fractures between perspectives.

**Competitive intelligence**: Five industry reports reach diametrically opposed conclusions about the same market. The LLM synthesizes: "The market outlook is broadly positive, though uncertainties remain." You lose the grip you needed to make a differentiated judgment.

**Literary criticism**: Five critics offer five distinct readings of the same work. The LLM synthesizes: "The work is thematically rich and has inspired diverse critical engagement." You lose the spark of intellectual collision.

**The common pattern: when multiple independent sources offer independent judgments on the same subject, the *disagreements between them* are the most valuable information. Yet conventional LLM workflows are designed precisely to eliminate those disagreements.**

### What LLM Mosaic Does Instead

LLM Mosaic has one core principle: **Assemble, don't synthesize.**

Lay out what each source says about an entity, side by side. Preserve the original form. Don't merge. Don't judge on the reader's behalf. Every claim carries a link — one click back to the exact spot in the source material.

```
❌ "On balance, the Crow Terrace Poetry Trial was a product of Northern Song
   factional politics…"                                 ← LLM judges for the reader
✅ "Li Yibing says: Shen Kuo was the instigator. …📖"
   "Lin Yutang says: Wang Anshi's cabal of petty men orchestrated it. …📖"
   "Zhou Wenhan says: A cultural celebrity attacked in the age of print. …📖"
   "Zhu Yong says: Suffering as the prelude to artistic transcendence. …📖"
                                                        ← Reader sees the originals
                                                           and judges for themselves
```

This isn't "not trusting AI" — it's recognizing that **some questions derive their value from the coexistence of perspectives, not from the unification of answers.**

When an LLM puts four biographers' accounts of the same event side by side on your screen, each traced back to its source — the discernment you gain as a human reader far exceeds anything any "synthesized conclusion" could give you.

---

## Architecture

Three layers — but the middle layer's role is fundamentally different from LLM Wiki:

```
┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
│   Raw Sources     │  →   │   Mosaic Pages    │  →   │   Output Layer    │
│   (Source library)│      │   (Multi-source   │      │   (Optional)       │
│   Read-only        │      │    juxtaposition)  │      │   Articles/scripts │
└──────────────────┘      └──────────────────┘      └──────────────────┘
        ↑                         ↑                         ↑
        └── Every citation clicks back to an exact source location ──┘
```

### Layer 1: Raw Sources

Your curated collection of independent sources. Like LLM Wiki — the LLM reads but never modifies. The critical difference: your sources must be **independent perspectives on the same subject**, not sequential or complementary textbook chapters.

What counts as an "independent source":

- Multiple biographies of the same person (different authors, different eras, different stances)
- Multiple news outlets covering the same event (different media, different political leanings)
- Multiple legal documents for the same case (prosecution, defense, judgment)
- Multiple analyst reports on the same market (different firms, different times, different assumptions)
- Multiple critical essays on the same work (different schools, different frameworks)
- Multiple national archives covering the same historical period (different state perspectives)
- Multiple papers on the same technique (different teams, different methods)

A source is a **complete perspective** — not a puzzle piece, but an independent rendering of the entire picture. Put multiple sources together, and you see multiple *versions* of the same picture.

### Layer 2: Mosaic Pages

This is the core of LLM Mosaic. One page per entity (person, event, concept, case, product). Page structure:

```markdown
## Timeline (if applicable)
Objective facts in chronological order, each event traced to annals/archives

## Source Perspectives
### Source A: Core judgment + quoted passages + 📖
### Source B: Core judgment + quoted passages + 📖
### Source C: Core judgment + quoted passages + 📖
### Source D: (if absent, honestly note "Not specifically addressed")

## Key Discrepancies
A side-by-side comparison table showing where sources contradict and where they corroborate
```

Key differences from LLM Wiki:

| | LLM Wiki | LLM Mosaic |
|---|---|---|
| **Source nature** | Typically one primary source per domain | Multiple independent sources on the same subject |
| **Page structure** | Synthesized summary + conceptual exposition | Sources juxtaposed + discrepancies annotated |
| **LLM's role** | Digest → synthesize → output | Locate → extract → arrange → link |
| **Reader's role** | Receive distilled knowledge | Read originals and form own judgment |
| **"Consensus"** | A desired output | Not pursued — divergence is the value |

### Layer 3: Output Layer (Optional)

The same set of Mosaic pages can feed multiple output formats — articles, scripts, reports, calendars, visualizations. Core principle: **output may take a stance, but sourcing remains transparent**. A reader may disagree with your interpretation, but every assertion traces back to its origin.

### Supporting Files

Two special files underpin Mosaic operations:

**entity-index.md**: The entity × source matrix. This is the Mosaic "control panel" — before compiling any entity, the LLM consults this table to know which sources mention the entity and in which chapters.

```
| Entity      | Source A  | Source B  | Source C | Source D  | Annals    |
|-------------|-----------|-----------|----------|-----------|-----------|
| Shen Kuo    | Extensive | Multiple  | --       | Brief     | 7 entries |
| Wang Anshi  | Extensive | Extensive | Brief    | Extensive | 23 entries|
| Crow Terrace| Core event| Core event| Brief    | Core event| 15 entries|
```

The matrix's value isn't precise counting — it's rapid triage: **Which sources have something to say about this entity? Which are completely silent? Which rows concentrate the cross-source tensions?**

**log.md**: Append-only operation log. Records every ingest (new source intake scan), every compile (entity page built), every lint (cross-page consistency audit).

---

## Operations

Four core operations: Ingest (intake scan), Compile (build entity page), Query (multi-source inquiry), Lint (discrepancy audit).

### Ingest (Intake Scan)

When a new source arrives, don't directly "read → understand → write into wiki pages." Instead:

1. **Add to the raw source library**. Format conversion (EPUB→MD / PDF→MD / web→MD). Mark as read-only.
2. **Full entity scan**. The LLM scans the new source, extracts all mentioned entities (people, events, concepts), and records which chapter each appears in.
3. **Update entity-index.md**. Add a new column (the new source) to the matrix. For each existing entity, annotate: present/absent/extensive/brief. Discover new entities → add rows.
4. **Selective backfill**. For entities in entity-index with multi-source coverage, supplement existing Mosaic pages with the new source's perspective. Don't require full backfill — prioritize high-value entities.
5. **Update log.md**.

Key design insight: **One scan, matrix records positions, backfill on demand**. You don't need to read, comprehend, and write into every affected page for every new source. The scan phase only locates. The compile phase does the deep reading of specific passages.

### Compile (Build Entity Page)

For entities in entity-index that show multi-source coverage, launch the compile workflow:

1. **Consult the matrix**. Locate which sources mention this entity and in which chapters.
2. **Read only the relevant passages**. Don't read entire books — follow the matrix's location data and read only the relevant sections. A 100,000-character biography might have only 2,000 characters about Shen Kuo. Read those 2,000 characters.
3. **Assemble by source**. For each source, extract key passages, annotate the core judgment, and attach 📖 links back to the originals.
4. **Write the discrepancy table**. Side-by-side comparison of how sources converge and diverge on key points.
5. **Honestly mark silent sources**. If a source says nothing about this entity, write "Not specifically addressed." A blank isn't neutral — a source choosing *not* to write about someone is itself a piece of information.
6. **Update index.md and log.md**.

### Query (Multi-Source Inquiry)

You query the Mosaic. The LLM doesn't "synthesize an answer" — it "reports by source":

- "What role did Shen Kuo play in the Crow Terrace Poetry Trial?" → The LLM answers: "Li Yibing holds that… Lin Yutang holds that… Zhou Wenhan holds that… Zhu Yong holds that… The key divergence is… See each source's passages for details."
- "How does each biography assess Wang Anshi's reforms overall?" → The LLM reads the Wang Anshi passages across all biographies, generates a comparison table, and marks consensus and divergence.

Answers always carry traceable links. You can verify any claim by clicking back to the source at any time.

### Lint (Discrepancy Audit)

Periodically have the LLM audit across pages. LLM Mosaic-specific checks:

- **Compilation completeness**: Are all entities in entity-index with multi-source coverage compiled into Mosaic pages?
- **Link validity**: Do 📖 links still point to the correct chapters? Have source files been moved or renamed?
- **Honest silence**: Sources marked "Not specifically addressed" — do they need re-scanning to confirm? (Could be a scan false negative.)
- **Contradiction flagging**: When two sources flatly contradict on the same fact, is this explicitly annotated in the discrepancy table? Or are they listed separately with no indication the reader should notice?
- **Backfill progress**: For recently ingested sources, what's the backfill status on high-value entities? Which key entities still lack the new source's perspective?

### Full Workflow: Adding a New Source

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ New book  │ →  │ Entity   │ →  │ Update   │ →  │ Selective │
│ EPUB→MD  │    │ scan     │    │ matrix   │    │ backfill  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
                                                     ↓
                                               ┌──────────┐
                                               │ Update   │
                                               │ index    │
                                               │ Append   │
                                               │ log      │
                                               └──────────┘
```

---

## Core Mechanisms

### Mechanism 1: The Entity × Source Matrix

entity-index.md is the Mosaic control panel. Its value isn't "completeness" — it's that **the LLM knows where to look before compiling, reads only what it should during compilation, and knows which entities to backfill when a new source arrives**.

Without this matrix, the LLM would have to read every source in full for every compilation — 6.6MB of raw text crammed into the context window, costs exploding, quality degrading. With the matrix, compiling Shen Kuo requires reading only 4 relevant passages from the originals, a few hundred characters — the LLM's attention stays focused where judgment is actually needed.

### Mechanism 2: Assemble, Don't Synthesize

This is the soul of LLM Mosaic, the thing that distinguishes it from LLM Wiki.

LLM Wiki's default posture is: "Let me digest this material for you and give you a clean, concise conclusion." In most use cases, this is a feature — you don't need a 30-page summary, you just need the key points.

But in multi-source triangulation, "digestion" means "sanding down the divergences." Four biographers each tell their own story. The LLM digests them and gives you an "overall assessment" — the cost of those words is that you never see the most fascinating tensions between Li Yibing and Zhou Wenhan.

**LLM Mosaic requires the LLM to suppress the urge to digest.** Find the original text. Extract it. Arrange it. Link it back. Don't add "overall," "essentially," "on balance." The reader judges. The LLM's job is to put the raw materials for judgment in front of the reader.

### Mechanism 3: Trace Every Claim

Every claim carries a clickable link back to the exact location in the source material.

Why chapter/paragraph precision, not "see *Su Dongpo: A New Biography*"? Because "see a book" is no citation at all — that's a 300-page search task. "See Chapter 5, Section 2" is a 5-page search task. `[[source#^block-id]]` is a single click.

**Citation granularity determines verification cost.** Finer granularity → lower verification cost → readers are more likely to actually verify. Coarser granularity → "traceability" becomes a posture rather than a function.

### Mechanism 4: Honestly Mark Silent Sources

A source has nothing to say about this entity — don't skip it; explicitly mark "Not specifically addressed."

This matters. Lin Yutang's 400-page biography barely mentions Shen Kuo — this isn't an accidental omission; it's a narrative choice. Lin was writing a romanticized Su Dongpo, and Shen Kuo — the "Crow Terrace informant" — doesn't fit that narrative frame. **What a source chooses not to write is as much a perspective as what it does write.**

### Mechanism 5: Side-by-Side Discrepancy Tables

Don't describe source convergences and divergences in prose paragraphs. Use a table placed side by side:

| Key Point           | Li Yibing                    | Lin Yutang            | Zhou Wenhan                      | Zhu Yong              |
| ------------------- | ---------------------------- | --------------------- | -------------------------------- | --------------------- |
| Primary cause of Crow Terrace Trial | Shen Kuo informant + institutional flaws | Wang Anshi's cabal of petty men | Print-culture celebrity under siege | Suffering as prelude to art |
| Shen Kuo's role     | Instigator                   | Not addressed         | Rising star investigator         | Former friend turned spy |

The table's value: the reader sees, in one glance, where four versions sit relative to each other on a dimension. Discrepancies aren't *described* — they're *seen*.

---

## When to Use LLM Mosaic vs. LLM Wiki

| Signal | → Use LLM Wiki | → Use LLM Mosaic |
|---|---|---|
| **Source nature** | Textbook-style: complementary, progressive, no fundamental disagreements | Biography/criticism/reportage: independent judgments, meaningful disagreements |
| **Your goal** | Master a domain's knowledge structure | See multiple versions of the same subject |
| **"Truth"** | There is a broadly objective answer | No single answer — perspectives are the value |
| **Source count** | 1–2 core sources | 3+ independent sources |
| **Typical domains** | Learning a course, reading a book, organizing internal docs | Historical research, case law analysis, competitive intelligence, literary criticism, multi-national perspectives |

The two are not mutually exclusive. The Su Dongpo project itself uses LLM Wiki's architecture with LLM Mosaic's multi-source assembly method — the three-layer structure comes from LLM Wiki; "assemble, don't synthesize" comes from LLM Mosaic. You can mix both patterns within a single project.

---

## Why This Works

### 1. LLMs are superhuman at finding and arranging — judgment stays human

Scanning four biographies for every mention of one person — humans need hours; LLMs need seconds. But deciding whether Li Yibing or Lin Yutang is more credible — that's the human reader's domain. LLM Mosaic deploys the LLM at its absolute strength (location and arrangement) and reserves final judgment for the human.

### 2. Discrepancy is information, not noise

Traditional knowledge management pursues consistency — a wiki page can't say "Shen Kuo was both a petty schemer and a genius," that would confuse readers. But historical research *needs* that very confusion — Shen Kuo looks radically different through different biographers' eyes, and that discrepancy is itself the most important information, about both Shen Kuo and the biographers. LLM Mosaic's page structure is built to hold that tension.

### 3. The matrix does subtraction

With 3 sources, managing cross-source relationships is manually maintainable. At 7, 15, 30 sources, "who said what where" becomes a combinatorial explosion. The entity-index matrix compresses this into one scan + one table + on-demand backfill. Adding a new source costs O(n) scanning, not O(n²) cross-comparison.

### 4. Compounding comes from "adding a column"

Each new source doesn't just make existing Mosaic pages "a bit richer" — it adds **a whole new dimension**. Two sources on the same subject: two data points. Three sources: triangulation. Ten sources: you start to see a "narrative spectrum" — some entities are remarkably consistent across all sources, others are nearly incompatible across different accounts. This compounding effect only accumulates under "assemble, don't synthesize" — because every act of synthesis collapses the prior multi-dimensional information into a single point.

---

## Notes

This document describes a concept and operating model, not a specific implementation. Exact directory structure, page formats, traceability precision (chapter-level vs. block-level), entity taxonomies — all depend on your domain, your source types, your choice of LLM and reading tools.

Everything mentioned is optional and modular. You might have only 3 sources and not need an entity-index. You might skip the output layer and only browse and think within Mosaic pages. You might need block-level citations because your sources are OCR'd PDFs rather than well-structured ebooks. Your domain has its own entity types and discrepancy dimensions — adapt the template; don't copy it.

The right way to use this is to share this document with your LLM Agent, tell it your domain, your sources, and what you want to see clearly. Collaborate to instantiate a Mosaic that fits your needs. This document's only job is to communicate the pattern: **juxtapose multiple sources; trace every claim.** Your LLM can handle the rest.

---

*LLM Mosaic's methodology emerged from the Su Dongpo research project — 4 biographies + 3 volumes of annals, 4 million characters, a 56-entity × 7-source cross-reference matrix. But its applicability extends far beyond historical biography: any domain where "multiple independent perspectives look at the same thing" can organize knowledge with this pattern.*
