# philippines-imf-llm

A Google Colab notebook that reads the IMF's 2025 Article IV report on the Philippines (IMF Country Report No. 25/333, December 2025, 85 pages) and turns it into section summaries, a checked indicator JSON, theme counts, grounded answers to test questions and a dashboard.

Kota Venkata Raghavendra Kalyan, SRN 24134836. Data Analysis with AI, LLM coding case study, University of Hertfordshire.

## Quick start

1. Open `Philippines_Article_IV_LLM.ipynb` in Colab.
2. Put `data/philippines_cr25333.pdf` next to it. To repeat the recorded run without calling any model, also upload `cache/`.
3. Runtime > Run all.

Paths are relative, and the notebook never prints or stores a key.

## Which model answers?

The `mode` line in the `[run]` block of the settings cell (step 1b) decides, together with the key: in `auto` mode, if a Colab secret called `GEMINI_API_KEY` is available, calls go to the Gemini API through `google-genai`; without it the notebook runs in replay mode and only reads `cache/replies/`, stopping with a message if a reply is missing.

- writer: `gemini-3.7-flash`
- judge: `gemini-3.6-flash`
- embeddings: `sentence-transformers/multi-qa-distilbert-cos-v1`, searched with FAISS `IndexFlatL2`

Each reply is saved as one JSON file under `cache/replies/`, named by the SHA-256 of model, system text, prompt and schema. Replies are checked against a JSON Schema with `jsonschema`; a reply that fails gets one repair request.

The recorded run used the Gemini API on 4 November 2026. The notebook in this repository is a Restart and run all from that cache: 120 calls, all cached, 0 live.

## What each step produces (`artefacts/json/`)

| Step | Output |
|---|---|
| reading, cleaning, sections, chunks | `cleaning.json`, `sections.json`, `chunks.json` |
| section summaries, judge, rewrites | `section_summaries.json` |
| executive summary | `executive_summary.json` |
| themes, strengths and vulnerabilities | `themes.json`, `insights.json` |
| indicator JSON, evidence, judge audit | `indicators.json`, `indicator_evidence.json`, `indicator_fallbacks.json`, `indicator_audit.json` |
| time series from the statistical tables | `timeseries.json` |
| retrieval and graded answers | `rag_eval.json` |
| extension: multi-query retrieval with query variants written in Python | `extension_multiquery.json` |
| long tables for plotting | `long_tables.json` |
| every prompt, every call | `run_record.json`, `call_log.json` |

Figures (300 dpi): `artefacts/figures/dashboard.png`, `artefacts/figures/extension_multiquery.png`.

## Folder

```
Philippines_Article_IV_LLM.ipynb   notebook, executed in Colab
data/                               the IMF report (PDF)
cache/replies/                      stored model replies
cache/                              embedding vectors
artefacts/                          JSON outputs and figures
requirements.txt                    package versions of the Colab run
LICENSE                             MIT
```

## Source

International Monetary Fund (2025) *Philippines: 2025 Article IV Consultation-Press Release; Staff Report; and Statement by the Executive Director for the Philippines*. IMF Country Report No. 25/333. Washington, DC: IMF.
