# Independent Validation Pilot — Results

Pilot-scale, not a benchmark: 24 synthetic scenarios, 2 models, hand-authored mechanical ground truth. Numbers below characterize behavior on these 24 scenarios only and should not be read as general leak-rate estimates for either model.

Run date: 2026-09-01. Data is entirely synthetic (fabricated personas, fabricated Aadhaar/PAN/UPI/GSTIN values in valid format but not real identifiers).

## Headline numbers

| Metric | DeepSeek (deepseek-chat) | OpenAI (gpt-4o) |
|---|---|---|
| Scenarios with ≥1 violation | 24 / 24 | 24 / 24 |
| Over-share rate — India fields (Aadhaar/PAN/UPI/GSTIN) | 86.7% | 84.2% |
| Over-share rate — Generic fields (DOB/bank acct/card/employer) | 96.4% | 94.7% |
| Cross-tool leak rate — India fields | 81.5% | 77.2% |
| Cross-tool leak rate — Generic fields | 95.0% | 93.7% |

(Rates are violations ÷ sensitive-field appearances across all tool calls in the 24-scenario set; see `grading.py::aggregate`.)

## Reading these numbers

1. **Both checks fire at very high rates for both models under a default, no-privacy-instruction system prompt.** This is directionally consistent with ToolMinimize's default-prompt finding (81–88% of tool calls carry unnecessary PSD) and ToolPrivacyBench's purpose-bound over-disclosure finding, even though this pilot's absolute numbers aren't comparable to theirs (different tool schemas, different scenario design, 24 cases vs thousands).
2. **India-specific identifiers did not leak/over-share at a higher rate than generic PII in this pilot — they leaked slightly less**, on both models and both checks. This is the opposite of what a naive hypothesis ("exotic-looking identifier strings get handled more carelessly") would predict. A plausible explanation worth testing further: Aadhaar/PAN/UPI/GSTIN values are visually distinctive (fixed formats, unlike free-text DOB or a generic account number) and appear later per persona in the JSON dump handed to the model, both of which could make the model more likely to notice they're a different disclosure than what the current tool needs. This is a hypothesis, not a finding — the pilot's n is too small and the mechanism unconfirmed to assert it in the paper.
3. **No condition tested privacy-instructed prompting.** ToolMinimize and AgentDAM both show large improvements when the system prompt explicitly instructs minimization; this pilot only measures the default-prompt condition. A follow-up run with an instructed condition would be needed to see if the India-vs-generic gap (point 2) holds, widens, or closes under instruction — flag this as future work, not something this pilot claims to have tested.

## Explicit limitations of this pilot

- 24 scenarios, 2 models (DeepSeek, OpenAI). No Claude arm was run (no Anthropic API key was available at run time — this is a gap, not a finding, and should be disclosed as such rather than silently omitted).
- Single run per scenario per model, no repeated sampling — a single non-deterministic generation, not an average over multiple trials. Rates above should be read with that noise in mind.
- Synthetic personas and synthetic tool outcomes (every tool call is acknowledged with a canned "OK: recorded." — the model never sees a realistic downstream consequence of over-sharing).
- Ground truth (necessity + authorization) is hand-authored by one person (Vinit) for this pilot; unlike the paper's main severity/gap matrix, it was not independently cross-checked by a second rater. Reasonable for a declared pilot; would need a second rater before being used as a benchmark claim.
- This pilot measures *within-workflow* cross-tool leakage and over-sharing only, matching ToolPrivacyBench/ToolMinimize's scope. It does **not** test cross-session purpose-reuse — that gap remains open and is exactly what Paper 2's proposed architecture targets.

## Raw data

- `results/deepseek.json`, `results/openai.json` — full per-scenario grading output + raw transcripts (every tool call each model actually made, in order).
