# How to run and view this submission

Submission: **Where Memory Breaks — Linear Attention vs. Softmax Attention** · DataForge 2026, Pathway Track

Nothing in this project needs to be installed, built, or compiled. There is no server, no dependencies to `pip install` or `npm install`, no dataset to download, and no API key. Everything runs client-side in a browser.

---

## 1. Viewing the artifact (`linear_attention_story.html`)

**Fastest — from the web (no download):**
Open [the hosted artifact link](https://galaxy-codee.github.io/DataForge-2026/linear_attention_story.html) from `ACCESS_LINKS.md`. It loads like any webpage.

**From the zip / repo, locally:**
1. Unzip the submission (or clone the repo).
2. Find `linear_attention_story.html` in the top-level folder.
3. Double-click it, or right-click → **Open with** → any modern browser (Chrome, Firefox, Safari, Edge).
4. That's it — no local server needed. The only network call the page makes is to load KaTeX (for equation typesetting) from a CDN; if that fails or you're offline, the page still works, only equation formatting degrades.

**Using it once open:**
- Scroll top to bottom, or use the "Learning path" nav to jump between the 11 modules.
- Module 7 is the live part: move the **N** (number of facts), **d** (state size), and **key similarity** sliders, or click a preset (e.g. "Overload") — the accuracy chart, table, heatmap, and Frobenius-norm readout all recompute instantly in your browser.
- Click **Reshuffle** to redraw with a fresh random seed and confirm the numbers aren't a canned recording.
- Modules 10–11 are a short self-check: a multiple-choice transfer question, then a free-text explanation that gets scored client-side against three required ideas (mechanism, failure mode, BDH-CQ boundary). Nothing you type leaves your browser.

## 2. Viewing the concept summary

Three equivalent ways, pick whichever is easiest:
- **PDF (the required deliverable):** open `concept_summary.pdf` from the repo — GitHub renders PDFs inline, or open it in any PDF reader after unzipping.
- **HTML (source of the PDF, for reference):** open `concept_summary.html` the same way as the artifact above (double-click, or use the hosted link).
- **From the zip:** both files sit at the top level alongside the artifact — no extraction or conversion needed.

## 3. Reading the supporting documents

All of these are plain Markdown — readable in any text editor, or rendered nicely by GitHub/most zip-preview tools:

| File | What you'll find |
|---|---|
| `README.md` | The one-sentence claim, audience, learning objectives, and the "what's live vs. illustrative" architecture table |
| `RESEARCH_PAPERS.md` | All 7 papers used, what each supports, and verification status |
| `SOURCES_AND_LICENSES.md` | Every reused component (code, fonts, graphics, data) with its license |
| `AI_DISCLOSURE.md` | Exactly where and how AI assistance was used |
| `ACCESS_LINKS.md` | Every public link in one place |
| `LICENSE` | MIT license for the original work in this repo |

## 4. Accessing the source repository

The public repo — [github.com/galaxy-codee/DataForge-2026](https://github.com/galaxy-codee/DataForge-2026) — contains the exact same files as the zip. To get a local copy instead of downloading a zip:

```bash
git clone https://github.com/galaxy-codee/DataForge-2026.git
cd DataForge-2026
```

Then follow steps 1–3 above on the cloned files.

## 5. Verifying a specific number by hand (optional, for judges who want to check the math)

The toy linear-attention state is `S = Σ φ(kⱼ) ⊗ one_hot(vⱼ)` (with `φ = identity` in this artifact — no feature map beyond the raw Gaussian key). Retrieval is `argmax(qᵀS)`. Softmax retrieval selects the value of whichever stored key has the largest dot product with the query. Both are implemented in the `<script>` block of `linear_attention_story.html`, in the functions `buildLinearMemory`, `linearRetrieve`, `softmaxRetrieve`, and `evalAccuracy` — searchable directly in the file (Ctrl/Cmd+F) since it's a single self-contained HTML file with no bundler or minification.

## 6. Troubleshooting

- **Hosted link 404s:** GitHub Pages may not be enabled yet, or is still deploying (~1–2 min after enabling). Use the `raw.githack.com` mirror link in `ACCESS_LINKS.md` instead — it works immediately with no setup.
- **Equations look like plain text/Unicode instead of typeset math:** the KaTeX CDN didn't load (offline, or the CDN was unreachable). This is a cosmetic fallback only — no functionality is lost.
- **Sliders in Module 7 feel unresponsive:** the accuracy curve is cached per state size and recomputed on demand; a brief pause on the very first move of a new state size is expected, not a bug.
