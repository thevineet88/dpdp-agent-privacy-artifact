# Classification rubric (as in Sect. 4 of the paper)

## Terms

- **Binding obligation:** an unconditional duty or right in the DPDP Act or Rules whose trigger a finding meets.
- **Potential non-compliance:** a finding that meets such a trigger. It becomes a violation only where all of these hold for a real deployment: it falls within Act s. 3, it is operated by a Data Fiduciary, and no valid consent or s. 7 ground covers the use. A synthetic benchmark result never establishes that.
- **Structural gap:** a finding that triggers no unconditional duty among the five rubric codes. It does **not** mean that no obligation of any kind applies.

## Inclusion criterion

A failure mode is included if it meets all of these:

- it is a distinct, named category or a quantified metric reported by an in-scope source
- it concerns non-adversarial leakage
- it either carries a reported rate or is a taxonomy category that the source uses consistently

## Clause codes

| Code | Anchor | Trigger |
|---|---|---|
| PL | Act ss. 4, 6(1), 2(za), 7 | Data used or disclosed in a context other than the specified purpose it was provided for. Bright-line, because s. 7 is a closed list. |
| SL | Act s. 8(7)–(8) | Data persisting after the purpose is served or consent is withdrawn. |
| SDF | Rules r. 13(3) | Algorithmic software not verified as unlikely to risk data-principal rights. Applies only to notified Significant Data Fiduciaries. |
| DPR | Act ss. 11–14 | A right to access, correction or erasure asserted as honoured but not performed. |
| GAP-ADM | Absence in Act Ch. III | An automated decision on an inferred attribute, which no right in ss. 11–14 lets the data principal contest. |

## Algorithm 1

```
C ← { c ∈ {PL, SL, SDF, DPR} : trigger(c) holds for the source description }
if the failure mode is an automated decision on an inferred attribute:
    C ← C ∪ {GAP-ADM}
if C \ {GAP-ADM, SDF} = ∅:   status ← Structural gap   # no unconditional duty
else:                         status ← Binding
return C, status
```

SDF is left out of the status test because r. 13(3) binds only notified entities, and no source establishes that a deployer holds that designation.
