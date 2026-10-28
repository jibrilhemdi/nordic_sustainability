# Task A – Summarization and Tagging

**Objective.** Help a sustainability consultant screen a small excerpt of Ørsted's
2024 Annual Report through traceable AI summaries and sustainability tags.

**Built.** `orsted_sustainability_analysis.ipynb` and `results.csv`. The notebook
extracts nine logical passages from six printed pages (66, 67, 69–72), applies the
eight-category case taxonomy, and displays results, excerpt-only category counts,
evidence checks, and two generated summaries for human review when available.
It covers Task A only.

**Workflow and tools.** Python, PyMuPDF, pandas, the OpenAI-compatible SDK, and
python-dotenv. Visually chosen regions keep columns and topic tables separate.
Printed pages are recorded alongside one-based PDF pages and zero-based indices.
OpenRouter (`https://openrouter.ai/api/v1`) uses
`dots-studio/dots-3-note-preview:free` for both summarisation and tagging. Requests
try a strict JSON schema, fall back to prompted JSON if needed, and validate
locally, with at most three attempts per chunk. CSV evidence retains page numbers
and quotation-match flags. JSON `iro_types` supports multiple labels per chunk;
the CSV's `iro_type` column separates them with ` | `. “Not specified” is used
alone only when no type is supported. Failed/unrun rows have blank AI fields and a status.

**Run.** Use Python 3.10+ and open the notebook in Jupyter or VS Code, from this
folder with `Orsted Annual Report 2024.pdf` beside it. Install dependencies in the
selected notebook environment:

```sh
python -m pip install "pymupdf>=1.24,<2" "openai>=1.30,<3" "pandas>=2,<4" python-dotenv ipykernel
```

Set `OPENROUTER_API_KEY` in the environment or a local `.env` file, then restart
the kernel and **Run All**. Do not submit `.env` or share the key. Without a key,
extraction and local checks still run, but no AI findings are created. New runs
make API calls and overwrite the CSV; saved notebook outputs can be reviewed
without rerunning. API failures remain visible; the model is never substituted.

**Verification.** All nine chunks returned structured API results with multiple
IRO labels supported. Label-list checks passed, and every supporting quotation
matched the selected text. The notebook includes reviewer checks for materiality
and impact qualifiers; factual accuracy still needs human review.

**Limitations.** This is a selected excerpt, not a full-report assessment. The
PDF-specific regions need rechecking for other layouts. Exact evidence matches
do not establish semantic support or factual accuracy; summaries, qualifiers,
table labels, and all flagged tags need human review. Company disclosures are
self-reported. Free endpoint availability and rate limits can prevent completion.

**Future improvements at Nordic Sustainability.** Add batch processing of multiple
annual reports with layout-aware extraction; evaluate against manually labelled
examples; route uncertain classifications to human reviewers; develop consistent
ESRS-based company comparisons. Before deployment, use secret management and
access controls, and assess API providers' retention, routing, and data-handling
terms, particularly for confidential client documents. These are proposed next
steps and are not implemented here.
