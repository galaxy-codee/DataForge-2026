# Research papers used

Submission: **Where Memory Breaks — Linear Attention vs. Softmax Attention** · DataForge 2026, Pathway Track

Seven papers ground the claims made in the artifact and concept summary. Five are recent primary sources (2022–2026) cited directly beside the technical claims they support; two are Pathway's own BDH/BDH-CQ publications, cited for the module that connects this lesson to BDH-CQ's contextual memory. No text, figures, or tables from any of these are reproduced — all claims are restated in original wording with the source attached.

---

### 1. Zoology: Measuring and Improving Recall in Efficient Language Models
**Arora, S. et al.** · arXiv:2312.04927 · ICLR 2024
**Used for:** defines multi-query associative recall (MQAR) — the exact task the artifact's live sandbox (Module 7) runs, and the reason the artifact tests retrieval under many simultaneous key→value facts rather than a single one.
**Verification status:** cited from secondary knowledge; **not yet checked against a local copy of the PDF.** Flagged as an open item in the repo's own AI disclosure — confirm the arXiv ID and claim wording against the primary PDF before final submission.

### 2. Simple linear attention language models balance the recall-throughput tradeoff (BASED)
**Arora, S. et al.** · arXiv:2402.18668 · ICML 2024
**Used for:** the accuracy-vs-state-size relationship that the artifact's main chart reproduces (in miniature, on synthetic data) — i.e., why shrinking the fixed-size state degrades recall.
**Verification status:** **confirmed** — title, authors, and arXiv ID checked directly against a local copy of the PDF.

### 3. Understanding Transformer from the Perspective of Associative Memory
**Zhong, Xu, Ao & Shi (ByteDance Seed)** · arXiv:2505.19488 · 2025
**Used for:** the retrieval signal-to-noise ratio (SNR) framing used to describe why the state heatmap visibly degrades as more facts are packed in.
**Verification status:** **confirmed** — title, authors, and arXiv ID checked directly against a local copy of the PDF.

### 4. Variational Linear Attention: Stable Associative Memory for Long-Context Transformers
**Pandey, R. & Singh, A.** · arXiv:2605.11196 · May 2026
**Used for:** the Frobenius-norm growth result behind the live `‖S‖_F` readout in Module 7, and the specific quantitative comparison points quoted in the concept summary (O(T) norm growth, a 109× norm reduction at T=1,000, and 62% accuracy at the per-head capacity boundary).
**Verification status:** **confirmed**, including the specific quantitative claims — checked directly against a local copy of the PDF.

### 5. Gated Delta Networks: Improving Mamba2 with Delta Rule
**Yang, S., Kautz, J. & Hatamizadeh, A.** · arXiv:2412.06464 · ICLR 2025
**Used for:** cited as a deployed production example (Qwen3-Next) and as the "selective erase + write" comparison point against the artifact's plain additive-memory toy.
**Verification status:** **confirmed**, including the ICLR 2025 acceptance line — checked directly against a local copy of the PDF.

### 6. The Dragon Hatchling
**Pathway** · Pathway publication
**Used for:** grounds BDH's neuron–synapse vocabulary (sparse activation, Hebbian strengthening) used in Module 8's diagram and the artifact's mapping of the abstract `k⊗v` write onto that framing. The diagram explicitly states it is a simplified original drawing, not a reproduction of a paper figure, and notes the real reported sparsity (~5% active) against the diagram's simplified 2-of-20 (10%).
**Verification status:** **not verified against a local copy.** The artifact's BDH characterizations currently follow the DataForge Pathway track brief's description rather than a direct page/equation citation into this paper — the single largest open item before final submission.

### 7. BDH-CQ technical report
**Pathway** · Pathway publication
**Used for:** the published contextual-memory update `Sₜ ← Uθ(Sₜ₋₁, Dₜ)`, placed directly beside this lesson's toy additive update `S ← S + φ(k)⊗v` in Module 8, with an explicit statement that BDH-CQ's real update is a *learned, general* function evaluated once per demonstration — not assumed to be additive, and not the same as the toy.
**Verification status:** **not verified against a local copy.** Same gap as #6 — description follows the track brief rather than a direct citation into the technical report.

---

## Summary table

| # | Paper | Year | Identifier | Supports | Verified vs. local copy |
|---|---|---|---|---|---|
| 1 | Zoology | 2024 | arXiv:2312.04927 | MQAR task definition (live sandbox) | No — secondary knowledge |
| 2 | BASED | 2024 | arXiv:2402.18668 | Accuracy vs. state-size chart | Yes |
| 3 | Associative-memory view of Transformers | 2025 | arXiv:2505.19488 | Retrieval SNR / heatmap degradation | Yes |
| 4 | Variational Linear Attention | 2026 | arXiv:2605.11196 | Frobenius-norm readout, quoted stats | Yes |
| 5 | Gated Delta Networks | 2025 (ICLR) | arXiv:2412.06464 | Production example, erase+write comparison | Yes |
| 6 | The Dragon Hatchling | — | Pathway pub. | BDH neuron–synapse framing (Module 8 diagram) | No — via track brief |
| 7 | BDH-CQ technical report | — | Pathway pub. | Contextual memory update equation (Module 8) | No — via track brief |

**Before final submission:** papers 1, 6, and 7 should be checked against their own primary PDFs and cited with page/equation-level specificity, the same way papers 2–5 already are. This is called out rather than smoothed over because the AI-assistance disclosure in this repo commits the team to defending every claim against its primary source.
