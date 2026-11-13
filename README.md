# Supplementary artifact: Potential DPDP Compliance Risks in Tool-Calling LLM Agents

Supplementary material for:

> Vinit Brahmankar and Rohini Pise. *Potential DPDP Compliance Risks in Tool-Calling LLM Agents: A Systematic Reanalysis of Privacy-Leakage Benchmarks Against India's Data Protection Law.* 5th Congress on Smart Computing Technologies (CSCT 2026), Springer SIST (to appear).

This repository reads published benchmark failures against statutory text. It does not give legal advice, and it does not establish that any real deployment has broken the law.

## Contents

| Path | What it is |
|---|---|
| [`rubric/RUBRIC.md`](rubric/RUBRIC.md) | Terms, inclusion criterion, the five clause codes and Algorithm 1 |
| [`matrix/MATRIX.md`](matrix/MATRIX.md) | For each of the 14 rows of Table 3: source location, a short excerpt, statutory text and the trigger decision |
| [`pilot/`](pilot/) | The exploratory pilot (Sect. 6): harness, grader, scenarios, personas, tool schemas, raw transcripts and results |

## Literature search

The literature was identified in September 2026 through these routes:

- Google Scholar and general web search
- arXiv listings
- IEEE Xplore
- journal tables of contents
- research-lab blogs
- forward and backward citation chasing

Inclusion and exclusion criteria are in `rubric/RUBRIC.md` and `matrix/MATRIX.md`. Screening counts were not recorded.

## Pilot: how it was run

- **Run date:** 1 September 2026, one run per scenario per model.
- **Models:** `deepseek-chat` (DeepSeek API) and `gpt-4o` (OpenAI API), called by these undated aliases. The dated snapshot each provider served was not logged.
- **Temperature:** not set, so each provider's default applied.
- **System prompt:** the default prompt in `pilot/run_harness.py` (`SYSTEM_PROMPT`). It contains no privacy instruction.
- **Grading:** mechanical; no LLM judge. To check the grader, run `python3 pilot/grading.py`, which runs it on a fully compliant transcript and a fully over-sharing transcript.

### Table 4 with denominators

The denominator is the number of sensitive-field appearances in that model's tool-call arguments.

| Metric | DeepSeek | OpenAI (gpt-4o) |
|---|---|---|
| Scenarios with ≥1 violation | 24/24 | 24/24 |
| Over-share, India fields | 117/135 (86.7%) | 96/114 (84.2%) |
| Over-share, generic fields | 135/140 (96.4%) | 90/95 (94.7%) |
| Cross-tool leak, India fields | 110/135 (81.5%) | 88/114 (77.2%) |
| Cross-tool leak, generic fields | 133/140 (95.0%) | 89/95 (93.7%) |

All personas and identifiers are fabricated. The Aadhaar, PAN, UPI and GSTIN values have a valid format but are not real, and any match with a real person or number is coincidental.

### Re-running

Install the packages:

```
pip install openai
```

Then run each model:

```
DEEPSEEK_API_KEY=... python3 pilot/run_harness.py --model deepseek --out pilot/results/deepseek.json
OPENAI_API_KEY=...   python3 pilot/run_harness.py --model openai   --out pilot/results/openai.json
```

## Licence

Code is under MIT. Data and documentation are under CC BY 4.0.
