# Clause/gap matrix: per-row audit trail

This file supports Table 3 of the paper. For each of the 14 rows it gives:

- where the finding appears in the source
- a short excerpt from the source
- the statutory text behind each rubric code
- the trigger decision taken under Algorithm 1 (see [`../rubric/RUBRIC.md`](../rubric/RUBRIC.md))

You can recode any row yourself and compare it with ours.

Source page numbers refer to the arXiv PDF version current in September 2026. Excerpts are kept short and are given for verification only; read the source for context. All rows were coded by one author (Vinit Brahmankar). No second independent coding was done.

**Statutory anchors used below** (the Act came into force by notification G.S.R. 843(E) and the Rules by G.S.R. 846(E), both dated 13 Nov 2025; the substantive provisions apply from 13 May 2027):

| Code | Provision | Text relied on |
|---|---|---|
| PL | Act s. 6(1) | consent "shall be limited to such personal data as is necessary for such specified purpose" |
| PL | Act s. 2(za) | specified purpose: "the purpose mentioned in the notice given by the Data Fiduciary to the Data Principal" |
| PL | Act s. 7 | closed list of nine "certain legitimate uses", (a)–(i); no open balancing ground |
| SL | Act s. 8(7) | erase "upon the Data Principal withdrawing her consent or as soon as it is reasonable to assume that the specified purpose is no longer being served" |
| SDF | Rules r. 13(3) | an SDF "shall observe due diligence to verify that technical measures including algorithmic software … are not likely to pose a risk to the rights of Data Principals" (notified SDFs only) |
| DPR | Act ss. 11–12 | right to information about processing; right to correction, completion, updating and erasure |
| GAP-ADM | Act Ch. III, ss. 11–14 | rights are access (s. 11), correction/erasure (s. 12), grievance redressal (s. 13), nomination (s. 14); none is a right to contest an automated decision |

Outside the rubric but relevant to row 6: under Act s. 8(3), data likely to be used to make a decision affecting the Data Principal must be complete, accurate and consistent. Act s. 13 also provides a grievance route that leads to the Board.

---

| # | Source (arXiv) | Location | Excerpt | Trigger decision | Codes → Status |
|---|---|---|---|---|---|
| 1 | PrivacyLens, 2409.00138 | p. 1 (abstract); Table 2, p. 8 | "leak sensitive information in 25.68% and 38.69% of cases, even when prompted with privacy-enhancing instructions" | Data given for one task is disclosed into an action whose purpose does not need it → PL | PL → Binding |
| 2 | AgentDAM, 2503.09780 | Tables on pp. 8–9 | GPT-family privacy scores 0.557–0.732 against 0.882–0.902 for Llama/Claude (higher is better); authors put GPT privacy performance at "25% to 46%" (p. 9) | The web agent uses data beyond what the task needs → PL | PL → Binding |
| 3 | Privacy in Action, 2509.17488 | p. 3; mitigation p. 1 and p. 5 | classifies data "as sensitive 98.0% of the time, yet still leaks private details in 33.1% of cases"; after mitigation, 7.30% (DeepSeek-R1) to 15.91% (Qwen3-14B) | Disclosure outside the task purpose, which persists after filtering → PL | PL → Binding |
| 4 | SG/KR AISI, 2606.17114 | p. 1 (risk types); results table p. 11 | five risk types including "data awareness, audience awareness, … data minimization, and access-boundary awareness"; fully correct and safe 15.0–58.3% | Each of these four categories involves data reaching a context its purpose does not cover → PL | PL → Binding |
| 5 | SG/KR AISI, 2606.17114 | p. 1 | review "revealed claim-action mismatches", i.e. a redaction asserted but not performed | An action on the person's data is reported as done when it was not → DPR | DPR → Binding |
| 6 | Agents That Know Too Much, 2606.26627 | p. 1; p. 7 | "compositional and cross-session inference leakage, the two least-protected risks"; composition is "the outcome most specific to agentic operation" | An attribute is inferred, never disclosed, and acted on → GAP-ADM. SDF applies only if the deployer is notified. No unconditional rubric code applies. | SDF (cond.), GAP-ADM → Structural gap. General duties such as s. 8(3) and s. 13 still apply. |
| 7 | ToolPrivacyBench, 2606.28061 | p. 2; scope p. 4 | "MT-POI remains between 19.19 and 28.04" while TaskSuccess is 92.23–97.70 | Fields cross tools beyond their purpose-bound authorization → PL | PL → Binding |
| 8 | ToolMinimize, 2608.24957 | Table, p. 2 | over-share 81.0–87.5% by default; 36.0–76.2% under a privacy prompt | Tool calls carry fields that the call's purpose does not require → PL | PL → Binding |
| 9 | ToolMinimize, 2608.24957 | p. 7 | "reduces cross-call PSD linkability by 70.6% … session-level budgets would reduce this further" | Data is linked across calls beyond each call's purpose → PL | PL → Binding |
| 10 | AgentLeak, 2602.11510 | abstract; Finding 3 | inter-agent messages leak 68.8% against 27.2% for final outputs, so "output-only audits miss 41.7% of violations"; internal 57.8% against external 27.2%; 1.6× exposure | Data flows into internal channels that the task purpose does not cover → PL | PL → Binding |
| 11 | MAGPIE, 2510.15186 | abstract | Gemini 2.5-Pro leaks up to 50.7% and GPT-5 up to 35.1% "even when explicitly instructed not to" | Disclosure beyond the purpose, despite instruction → PL | PL → Binding |
| 12 | MAGPIE, 2510.15186 | abstract | Gemini 2.5-Pro shows "manipulation in 38.2% of cases" | Behaviour between agents, not a decision about the person; only the conditional SDF code applies | SDF (cond.) → Structural gap |
| 13 | PiSAs, 2607.05318 | table, p. 7 | appropriateness violations range from 25.0% to 94.6% by topology | Information reaches agents or users the purpose does not cover → PL | PL → Binding |
| 14 | PiSAs, 2607.05318 | p. 8; scope p. 9 | "total visibility violations rise from 36–47% to 63–90% with Hybrid memory" | Exposure outside the purpose (PL), and data persisting in shared memory beyond the purpose (SL) | PL, SL → Binding |

## Excluded

- **Cross-session purpose reuse.** It is not a row, because none of the ten sources measures it: ToolPrivacyBench and PiSAs (p. 9: "We do not measure cross-task leakage") rule it out explicitly, ToolMinimize defers it, and the survey flags it as an open problem. It is measured outside tool use by CIMemories (2511.14937) and PersistBench (2602.01146).
- **Adversarial work.** The backdoored-tool-use paper was excluded because it concerns a different threat model.
- **Codes that never vary.** Codes that took the same value on every row are stated once in the paper rather than given a column: s. 8(1) processor accountability, cross-border transfer, s. 9 children's data and s. 8(6) breach notification.
