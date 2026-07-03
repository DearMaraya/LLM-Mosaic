# LLM Mosaic

> **Assemble, don't synthesize.**
>
> While everyone else is using LLMs to *synthesize*, we do the opposite — lay out what every independent source says about the same subject, side by side. Make the disagreements visible. Trace every claim back to its origin.
>
> This is not a tool. This is a **way of seeing knowledge.**

📖 [中文文档](LLM%20Mosaic_CN.md) | [English Full Document](LLM%20Mosaic_EN.md) | [中文 README](LLM%20Mosaic_README_CN.md)

---

## Standing on the Shoulders of Giants, Seeing What They Didn't

In 2025, Karpathy proposed LLM Wiki — letting LLMs incrementally build and maintain a persistent markdown knowledge base. Three layers (Raw Sources → Wiki → Schema), three operations (Ingest / Query / Lint). A beautiful insight: **knowledge shouldn't be re-derived from scratch with every query. It should compound.**

In the traditional RAG ecosystem — NotebookLM, ChatGPT file upload, every "feed docs to AI" product — the core pattern is "retrieve → synthesize → generate an answer." Fast. Accurate. Effortless.

Both directions share the same underlying assumption: **the LLM's job is to digest information for you and output a clean answer.**

This assumption holds for most use cases. But there's one where it breaks —

**When you have multiple independent sources making different judgments about the same subject, what you need isn't synthesis. You need the full picture.**

---

## The Problem

LLMs are great at synthesis. Feed them five reports, they give you an "overall assessment." Feed them four biographies, they give you a "complex, multifaceted" portrait.

Sounds good. But there's a fundamental issue —

**Synthesis presupposes that an objective truth exists, and your job is just to extract it.**

History doesn't work that way. Case law doesn't. Market analysis doesn't. Literary criticism definitely doesn't.

Lin Yutang says Wang Anshi was a paranoid crank. Li Yibing says he was an idealistic reformer. The LLM synthesizes: "Wang Anshi was a complex historical figure…" — technically not wrong, but it erases every bit of tension that would actually let you understand the man. Divergence gets compressed. Perspectives get averaged. The most valuable information gets discarded as noise.

**Karpathy's LLM Wiki got it right: knowledge should compound. RAG got it right: retrieval accelerates access. But neither answered one question: when multiple sources contradict each other — compound what? Retrieve what?**

---

## The Solution

LLM Mosaic has one core principle: **Assemble, don't synthesize.**

| The Default Way | LLM Mosaic |
|---|---|
| Read five reports → output one "synthesized conclusion" | Read five reports → lay five judgments side by side |
| LLM judges for the reader | Reader judges from the originals |
| Pursue consensus | Preserve divergence — divergence *is* information |
| Re-derive from scratch every query | Assemble once, keep stacking new sources |

```
❌ "On balance, the Crow Terrace Poetry Trial was a product of Northern Song
   factional politics…"                                 ← LLM judges for the reader

✅ "Li Yibing says: Shen Kuo was the instigator. 📖"
   "Lin Yutang says: Wang Anshi's cabal of petty men orchestrated it. 📖"
   "Zhou Wenhan says: A cultural celebrity under siege in the age of print. 📖"
   "Zhu Yong says: Suffering as the prelude to artistic transcendence. 📖"
                                                        ← Reader sees the originals
                                                           and judges for themselves
```

This isn't "not trusting AI." It's recognizing a simple fact: **for some questions, the coexistence of perspectives is worth more than the unification of answers.**

---

## What It Is and Isn't

**LLM Mosaic is not RAG.** RAG is "retrieve relevant chunks → generate an answer." Mosaic is "locate all relevant passages → extract by source → juxtapose." RAG pursues answers. Mosaic pursues the full picture.

**LLM Mosaic is not LLM Wiki.** LLM Wiki is a knowledge base whose default operation is synthesis. LLM Mosaic is a compilation mode whose only operation is assembly. You can use Mosaic's method to compile pages inside LLM Wiki's three-layer architecture — the Su Dongpo project does exactly this.

**LLM Mosaic is a knowledge organization primitive.** Just as MapReduce is a primitive for distributed computing and the Transformer is a primitive for sequence modeling — Mosaic is a primitive for knowledge compilation in multi-source triangulation scenarios.

---

## How It Works

Three moves:

**1. Build the matrix.** When a new source arrives, don't read the whole thing. First, scan for entities — which people / events / concepts does this source mention? In which chapters? Write it into an `entity-index.md` matrix.

```
| Entity       | Li Yibing | Lin Yutang | Zhou Wenhan| Zhu Yong | Annals    |
|--------------|-----------|------------|------------|----------|-----------|
| Shen Kuo     | Extensive | --         | Multiple   | Brief    | 7 entries |
| Wang Anshi   | Extensive | Extensive  | Brief      | Extensive| 23 entries|
```

The matrix's value: know where to look before compiling, read only the relevant passages during compilation, know which entities to backfill when new sources arrive.

**2. Compile the page.** Consult the matrix → read only relevant passages → assemble originals by source → write the discrepancy table → honestly mark silent sources.

**3. Add a column, never rewrite.** A new biography = a new column in the matrix + selective backfill of high-value entities. You never need a full rewrite. As sources grow from 3 to 30, Mosaic's value isn't "a few more data points" — it's one independent dimension after another.

---

## Why Now

This has nothing to do with model capability. GPT-4 can synthesize. GPT-5 will synthesize. GPT-6 will synthesize even better. Synthesis will only improve.

**But what this pattern requires isn't "stronger synthesis" — it's the discipline to suppress synthesis.**

As LLM context windows grow larger and multi-document processing becomes near-lossless, a counterintuitive trend is emerging: when the cost of synthesis approaches zero, **divergence becomes the scarcest resource.** When anyone can make an AI spit out "Wang Anshi was a complex historical figure," the person who can lay four biographies side by side, with every citation pinned to a paragraph, holds something else entirely — **raw material for judgment that cannot be averaged away.**

---

## Origin

LLM Mosaic emerged from the Su Dongpo research project — 4 biographies, 3 volumes of annals, 4 million characters of source material, a 56-entity × 7-source cross-reference matrix. But it isn't limited to history. Any domain where multiple independent perspectives look at the same subject — case law analysis, competitive intelligence, literary criticism, multi-national archives — is Mosaic territory.

---

## License

MIT
