# Where Memory Breaks — Linear Attention vs. Softmax Attention

**DataForge 2026 · Pathway Track** · Topic: *Linear Attention* (with a *Comparing Linear Attention Variants* thread) · connected to **BDH-CQ**

**Open the artifact:** `linear_attention_story.html` — a single static HTML file. Double-click it, or open it in any browser. No build step, no server, no sign-in, no network calls except loading KaTeX from a CDN (the artifact still runs if that fails; only equation typesetting degrades).

### What's in this repository

| File | What it is |
|---|---|
| `linear_attention_story.html` | **The artifact.** Self-contained interactive explainer, 11 modules. |
| `concept_summary.pdf` | The required one-page concept summary. ~1,400 words, two-column A4, single page. This is above the track's "approximately 500–950 words" recommendation — kept to one page as required, with a "why this matters now" section and a methodology section added for density; trim if a strict word cap turns out to matter more than page count. |
| `concept_summary.html` | Source for the PDF above, kept so the PDF is reproducible. |
| `README.md` | This file: claim, audience, objectives, architecture, evidence labelling, reproduction. |
| `SOURCES_AND_LICENSES.md` | Source and license record for all code, data, graphics, fonts, and reused components. |
| `AI_DISCLOSURE.md` | AI assistance, code, data, and asset disclosure, plus citation-verification results. |
| `LICENSE` | MIT license for the original work in this repository. |

---

## The one-sentence claim

> A fixed-size additive memory can compress an unbounded stream of key→value facts into one small state, but because every fact is superposed into the same state, retrieval accuracy degrades as more facts (or more similar keys) are packed in — while a growing key–value cache avoids that interference by never merging facts together, at the cost of memory that grows with sequence length.

This is falsifiable inside the artifact: Module 7 (`#liveSandbox`) runs both mechanisms live, in the browser, on the same random facts, and shows you the accuracy numbers. If additive linear attention *didn't* degrade under load, the "Overload" preset would show it staying at ~100% — it doesn't.

## Who this is for

**Audience:** someone comfortable with vectors and dot products (e.g., an early ML/CS undergraduate, or a data scientist who hasn't specifically studied linear attention) who wants to understand *why* linear attention trades recall for a fixed memory footprint, and how that trade-off shows up in BDH-CQ's contextual memory.

**Prerequisites:**
- Vectors, dot products, and matrix–vector multiplication.
- A rough sense of what "attention" does in a Transformer (query/key/value) is helpful but not required — Module 3 builds it from scratch with a plain-language key–value analogy.
- No prior exposure to linear attention, fast weights, or BDH is assumed.

## Learning objectives

By the end, a learner should be able to:
1. Explain why standard (softmax) attention's memory grows with context length, while linear attention's does not.
2. State the mechanism behind linear attention's failure mode (superposition / cross-talk in a fixed-size state) and predict when it gets worse (more facts, more similar keys, smaller state).
3. Read the failure equation `r(kᵢ) = (kᵢᵀkᵢ)vᵢ + Σⱼ≠ᵢ(kᵢᵀkⱼ)vⱼ` and identify the self-signal term versus the cross-talk term.
4. Name where this shows up in BDH-CQ's own vocabulary (contextual memory, `Sₜ ← Uθ(Sₜ₋₁, Dₜ)`, and the neuron–synapse/Hebbian framing) and state the one important way BDH-CQ's published mechanism *differs* from this lesson's toy (a learned, general update function evaluated once per demonstration, not a fixed per-token additive rule).
5. Recognize at least one concrete misconception ("just make the state bigger" doesn't remove interference as a structural property) and one open research direction that responds to it (Gated DeltaNet's selective erase/write; Variational Linear Attention's bounded-norm update).

## The sixty-second test

Module 10 (Transfer) and Module 11 (Explain it back) are the check: a learner who has only played with the sliders should be able to pick the correct answer to a novel trade-off question, then write 2–3 sentences that get scored (client-side, keyword/idea coverage, not graded prose) against three required ideas — mechanism, failure, and the BDH-CQ boundary.

---

## Architecture of the artifact — what's live, precomputed, illustrative

Everything numeric in this artifact is **computed in your browser at load time and on every slider move** — there are no server calls, no precomputed lookup tables, and no recorded video passed off as a live run. Concretely:

| Component | Status | Detail |
|---|---|---|
| Retrieval accuracy chart, query table, heatmap, Frobenius-norm readout, memory-size bars (Module 7) | **Live** | Real Gaussian random key/value vectors, generated with a seeded PRNG (`mulberry32`), rerun on every slider/preset change. The "Reshuffle" button redraws a fresh seed so a learner can confirm it isn't a replayed recording. |
| Step-through write/read walkthrough, per-token heatmap and KV-cache list (Module 7) | **Live** | Same underlying facts and math, stepped one token at a time. |
| Module 8 write-granularity toggle and its accuracy/table | **Live**, but explicitly a **toy reimplementation** | A synthetic additive-memory simulation on small vectors — not BDH-CQ's trained weights or its general learned update `Uθ`. Labeled as such in the panel. |
| Module 1 memory-game animation (card stack vs. state cells) | **Teaching illustration** | Deterministic scripted animation to build intuition before any computation is shown. Labeled "Play 4 facts," not claimed as a live model run. |
| Module 2 compute-flow diagrams (arc mesh vs. converging arrows) | **Illustration of a compute pattern, driven by real arithmetic** | The arc/arrow layout is generated from the slider value `n` via a deterministic layout function (not random/animated for its own sake), and the two counters (`n(n-1)/2` comparisons vs. `n` updates) are computed live from that same `n`. Clicking a token reports how many comparisons involve it — also computed, not scripted. It does not replay real attention weights; it depicts *which pairs get compared*, which the live sandbox in Module 7 then computes for real. |
| Module 3 key–query–value diagram | **Teaching illustration** | Hand-authored SVG of the retrieval idea: a query compared against stored keys, the closest key returning its paired value. The interactive fact-lab beside it is a scripted teaching example; the live numeric-vector version is Module 7. |
| Module 9 architecture family tree | **Illustrative / structural** | Hand-authored SVG. Branch structure reflects design intent, not a citation graph or a chronology. Carries the explicit caveats that BDH is not an SSM in the Mamba sense, and that BDH-GPU is a separate ReLU-low-rank formulation using linear attention. |
| Module 2 compute-flow diagrams (arc mesh vs. converging arrows) | **Illustration of a compute pattern, driven by real arithmetic** | The arc/arrow layout is generated from the slider value `n` via a deterministic layout function (not random/animated for its own sake), and the two counters (`n(n-1)/2` comparisons vs. `n` updates) are computed live from that same `n`. It does not replay real attention weights — it depicts *which pairs get compared*, which the live sandbox in Module 7 then computes for real. |
| Module 9 trade-off landscape map (SVG scatter of approaches) | **Qualitative / illustrative — explicitly labeled** | Ordinal positioning based on each cited paper's stated design goal and mechanism. It is **not** a benchmark plot with measured numbers, and the panel says so directly. BDH-CQ is deliberately left off this plot (with a note explaining why) rather than forcing an apples-to-oranges comparison. |
| Module 8 neuron–synapse Hebbian diagram | **Conceptual illustration, explicitly labeled** | A hand-built, simplified rendering of BDH's neuron–synapse framing (sparse activity, Hebbian strengthening) to make the outer-product write concrete. It is not a visualization of activations from a trained checkpoint. The panel states the real reported sparsity (~5% active) versus the diagram's simplified 2-of-20 (10%). |
| KaTeX equation rendering | **Live**, third-party library | KaTeX 0.16.9 loaded from cdnjs, MIT-licensed. A small script (bottom of the file) calls `katex.render` on every `.imath[data-tex]` element and on the two equation cards. |
| Ambient background network animation | **Purely decorative** | Explicitly commented in the source as non-representational atmosphere; it is not a rendering of any real computation and is disabled under `prefers-reduced-motion`. |

**Stated limits (also disclosed in-page under "Stated limits of this toy setup"):**
- The synthetic vocabulary has only 10 possible values — collisions are more likely here than with a realistic vocabulary.
- The live heatmap only renders the first 48 state dimensions even when the state size slider is set higher.
- The accuracy curve samples up to N=180 for rendering speed, not because that is a hard capacity limit.
- The Module 8 toggle and Module 9 diagrams are simplifications explicitly called out as such; none are presented as official BDH or BDH-CQ output.

## The BDH / BDH-CQ module

The BDH connection is woven into Module 8 (not appended at the end) with two concrete anchors:

1. **The equation**: BDH-CQ's published contextual memory update, `Sₜ ← Uθ(Sₜ₋₁, Dₜ)` — a *learned, general* function evaluated once per demonstration — is placed directly next to this lesson's per-token additive update `S ← S + φ(k)⊗v`. The additive case is the special case the BDH-CQ paper itself connects to attention, fast-weight memory, and linear-attention views of contextual association; the artifact is explicit that BDH-CQ's general update is *not assumed to be additive*, and that the toggle only explores that special case.
2. **The diagram**: an interactive neuron–synapse illustration grounds BDH's own vocabulary for the same write — a sparse set of neurons firing together strengthens the synapse between them, and that synapse *is* the memory write. This maps the abstract `k⊗v` operation onto BDH's brain-inspired framing (sparse non-negative activation, Hebbian plasticity) described in the Dragon Hatchling paper.

What's changing, stated plainly: in this lesson's toy, it's a fixed-size numeric matrix (`S`, shape `d × VOCAB`). In BDH-CQ's published form, it's a recurrent contextual state updated by a learned neural function once per demonstration, not once per token. The artifact does not claim to run or reproduce BDH-CQ's trained model; it reimplements a small, clearly labeled additive special case for teaching, per the track's own guidance on precomputed/illustrative work.

## Primary sources (2022–2026), cited beside the claims they support

1. **Arora et al., "Zoology: Measuring and Improving Recall in Efficient Language Models"** (arXiv:2312.04927, ICLR 2024) — introduces multi-query associative recall (MQAR), the exact task this artifact's live sandbox runs.
2. **Arora et al., "Simple linear attention language models balance the recall-throughput tradeoff"** (BASED, arXiv:2402.18668, ICML 2024) — the accuracy-vs-state-size relationship the main chart reproduces on a small synthetic version of the same task.
3. **Zhong, Xu, Ao & Shi, "Understanding Transformer from the Perspective of Associative Memory"** (arXiv:2505.19488, 2025) — the retrieval signal-to-noise ratio (SNR) framing used to describe the heatmap's degradation.
4. **Pandey & Singh, "Variational Linear Attention: Stable Associative Memory for Long-Context Transformers"** (arXiv:2605.11196, May 2026) — the Frobenius-norm growth result behind the live `‖S‖_F` readout, and the source for the "bounds state growth directly" comparison point.
5. **Yang, Kautz & Hatamizadeh, "Gated Delta Networks: Improving Mamba2 with Delta Rule"** (arXiv:2412.06464, ICLR 2025) — cited as a deployed production example (Qwen3-Next) and as the "selective erase + write" comparison point.

All five are reproduced with full citations in-page under "Primary sources" at the end of the artifact, and referenced inline next to the specific claims they support (chart, norm readout, comparison table/map).

**BDH primary sources referenced conceptually** (used for the BDH module's framing — sparse activation, Hebbian synaptic writes, contextual-memory update form): the Dragon Hatchling paper and the BDH-CQ technical report, per the track brief. This repository does not include local copies of those two papers; the module's equations and diagram are built from the mechanisms as described in the DataForge Pathway track brief and public secondary description, and are explicitly labeled as a conceptual illustration rather than a rendering of primary-source figures. **This is a gap to close before final submission** — see "Known gaps" below.

## Sources, licenses, and provenance of components

| Component | Source | License |
|---|---|---|
| KaTeX 0.16.9 (CSS + JS) | `cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/` | MIT |
| Fonts | System font stack only (`-apple-system, Segoe UI, Helvetica, Arial` for body/UI; `Iowan Old Style, Palatino Linotype, Palatino, Georgia` serif for headings; `ui-monospace, SFMono-Regular, JetBrains Mono, Menlo, Consolas` for code/math) | No embedded font files; relies on fonts already present on the reader's system, with generic fallbacks. No separate license needed. |
| All charts, diagrams, animations, SVG illustrations | Hand-authored in this file (canvas + inline SVG + vanilla JS) | Original to this project |
| Synthetic data (keys, values, facts) | Generated at runtime by a seeded PRNG (`mulberry32`), not sourced from any dataset | Original / synthetic |
| Text, equations, and narrative structure | Original to this project, grounded in the cited papers above | — |

No third-party images, icons, illustrations, or trained model weights are used anywhere in this artifact.

## AI assistance disclosure

Significant portions of this artifact's code, copy, and interaction design were produced with AI assistance (Claude, via Claude Code), working from the primary sources listed above and from the DataForge Pathway track brief. Specifically, AI assistance was used for:
- Drafting and iterating the JavaScript for the live retrieval simulation, chart/heatmap rendering, and the new interactive diagrams (compute-flow comparison, trade-off landscape map, neuron–synapse illustration).
- Drafting prose and equation labels, which were then checked against the cited papers for accuracy.
- Structural/CSS refactoring and accessibility passes (glossary tooltips, scroll progress, reduced-motion handling).

The registered team is responsible for understanding, defending, and being able to modify every component and claim in this artifact, per the track's AI-assistance and technical-ownership requirements. Anyone extending this file should verify new or modified numeric claims against the cited primary sources before publishing them.

## How to reproduce the results

There is nothing to install. Everything runs client-side:

1. Open `linear_attention_story.html` in a modern desktop or mobile browser (Chrome, Firefox, Safari, Edge).
2. Scroll or use the "Learning path" nav to move through the eleven modules.
3. In Module 7, move the **N** (facts), **d** (state size), and **key similarity** sliders, or click a preset — the accuracy chart, table, heatmap, and Frobenius-norm readout recompute immediately (same seed unless you click "Reshuffle").
4. To verify a specific accuracy number by hand: the toy's linear-attention state is `S = Σ φ(kⱼ) ⊗ one_hot(vⱼ)` (no feature map is applied beyond the raw Gaussian key, i.e. `φ = identity` in this artifact) and retrieval is `argmax(qᵀS)`; softmax retrieval selects the value of whichever stored key has the largest dot product with the query. Both are implemented in the `<script>` block (`buildLinearMemory`, `linearRetrieve`, `softmaxRetrieve`, `evalAccuracy`).

No GPU, no dataset download, no API keys.

## Known gaps (in progress, disclosed rather than hidden)

This submission package is not yet complete against the full Pathway track checklist. Specifically, still outstanding:
- **One-page concept summary PDF** (500–950 words, required deliverable) — not yet written.
- **Local primary-source copies / direct citations for the Dragon Hatchling paper and BDH-CQ technical report** — the BDH module currently relies on the track brief's description of BDH's mechanisms rather than a direct page/equation citation into those two papers; this should be tightened before final submission.
- **A separate source-and-license record file** consolidating the table above, and a standalone AI-assistance/data/asset disclosure file (currently folded into this README) — the track brief asks for these as identifiable items in the submission package.
- **Public hosting** — this file currently lives in a local git repository; it still needs to be pushed to a public repo and deployed to a URL that opens without sign-in.

Flagging these explicitly rather than presenting the package as finished.
