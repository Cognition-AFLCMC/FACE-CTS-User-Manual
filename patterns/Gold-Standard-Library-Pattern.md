# Gold Standard Library Pattern

## How FACE CTS Ships Known-Compliant References and How We Adapted This

---

## The Pattern

FACE CTS includes a **Gold Standard Library** — a set of known-compliant reference implementations that serve as verification baselines. This is a critical pattern because:

1. **Validates the tool itself**: If the tool says a known-good implementation fails, the tool has a bug
2. **Demonstrates capability**: Evaluators can see the tool correctly passing compliant code
3. **Provides templates**: Vendors can study the gold standard to understand what compliance looks like
4. **Enables regression testing**: Any tool changes can be validated against the gold standard

## Our Adaptation: Synthetic MPU Portfolio

We extended the Gold Standard pattern with a **hybrid approach**:

```
┌─────────────────────────────────────────────────────────────┐
│              SYNTHETIC MPU PORTFOLIO (Hybrid)                │
│                                                              │
│   KNOWN-COMPLIANT (Gold Standard equivalent)                │
│   ┌─────────────────────────────────────────────┐           │
│   │ nav_mpu          (OPEN-SOURCE DERIVED)       │           │
│   │   OMS CAL API + UCI nav messages             │           │
│   │   Evaluator can verify against published std │           │
│   ├─────────────────────────────────────────────┤           │
│   │ comms_mpu        (OPEN-SOURCE DERIVED)       │           │
│   │   OMS pub/sub + UCI Command templates        │           │
│   │   Evaluator can verify against published std │           │
│   ├─────────────────────────────────────────────┤           │
│   │ sensor_fusion_mpu (CUSTOM-BUILT BY DEVIN)    │           │
│   │   Real-world radar+IR+EO fusion scenario     │           │
│   │   Shows depth beyond published standards     │           │
│   ├─────────────────────────────────────────────┤           │
│   │ ew_countermeasure_mpu (CUSTOM-BUILT BY DEVIN)│           │
│   │   Multi-domain C2 EW response coordination   │           │
│   │   Shows real CCA operational complexity      │           │
│   └─────────────────────────────────────────────┘           │
│                                                              │
│   KNOWN-NON-COMPLIANT (Violation demonstration)             │
│   ┌─────────────────────────────────────────────┐           │
│   │ missing_cal_api   (OPEN-SOURCE DERIVED)      │           │
│   │   Bypasses CAL → OMS §5.3 violation          │           │
│   │   Evaluator can verify the standard requires │           │
│   │   CAL usage                                  │           │
│   ├─────────────────────────────────────────────┤           │
│   │ incomplete_docs   (OPEN-SOURCE DERIVED)      │           │
│   │   Missing Service Contract sections          │           │
│   │   Evaluator can check OMSC-TMP-003 template  │           │
│   ├─────────────────────────────────────────────┤           │
│   │ wrong_state_machine (CUSTOM-BUILT BY DEVIN)  │           │
│   │   Skips STARTING state → OMS §6.1 violation  │           │
│   │   Shows tool catches subtle behavioral bugs  │           │
│   ├─────────────────────────────────────────────┤           │
│   │ timing_violation_mpu (CUSTOM-BUILT BY DEVIN) │           │
│   │   QoS timing constraint violation            │           │
│   │   Shows tool catches real-time requirements  │           │
│   └─────────────────────────────────────────────┘           │
│                                                              │
│   ★ Open-source MPUs = credibility (verifiable)             │
│   ★ Custom-built MPUs = capability (real-world depth)       │
└─────────────────────────────────────────────────────────────┘
```

---

## Why Hybrid is Better Than FACE CTS's Approach

| Aspect | FACE CTS | Sabre (Hybrid) |
|--------|----------|----------------|
| Reference source | Single vendor (FACE Consortium) | Open-source (verifiable) + custom (capable) |
| Credibility | Trust the vendor | Customer can independently verify |
| Real-world depth | Standard compliance only | Includes operational CCA scenarios |
| Demo impact | "Our tool passes our reference" | "Our tool passes YOUR standards AND catches real bugs" |

## Classification

All content is **UNCLASSIFIED**.
