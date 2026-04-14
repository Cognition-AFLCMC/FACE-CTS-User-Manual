# FACE CTS Architecture Analysis

## How the Conformance Test Suite is Structured

This document analyzes the architecture of the FACE CTS (Conformance Test Suite) based on the publicly available User Manual (Edition 3.1, 149 pages) and extracts patterns applicable to the Sabre Conformance Tool.

---

## CTS Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    FACE CTS ARCHITECTURE                     │
│                                                              │
│   ┌──────────────────┐                                      │
│   │  Project Config   │  .pcfg file defines:                │
│   │  (.pcfg)          │  - Target UoP (Unit of Portability) │
│   │                   │  - FACE Edition (2.1, 3.0, 3.1)     │
│   │                   │  - Segment (IOSS, PCS, PSS, TSS)    │
│   │                   │  - Profile (Security, Safety, GP)   │
│   └────────┬─────────┘                                      │
│            │                                                 │
│            ▼                                                 │
│   ┌──────────────────┐                                      │
│   │  Test Selection   │  Filters test procedures by:        │
│   │                   │  - Segment applicability             │
│   │                   │  - Profile requirements              │
│   │                   │  - Edition version                   │
│   └────────┬─────────┘                                      │
│            │                                                 │
│            ▼                                                 │
│   ┌──────────────────┐                                      │
│   │  Test Execution   │  Runs each applicable test:         │
│   │                   │  - Static analysis checks            │
│   │                   │  - Interface signature validation    │
│   │                   │  - API usage verification            │
│   │                   │  - Documentation completeness        │
│   └────────┬─────────┘                                      │
│            │                                                 │
│            ▼                                                 │
│   ┌──────────────────┐                                      │
│   │  Results & Report │  Produces:                           │
│   │                   │  - Per-test pass/fail/warning        │
│   │                   │  - Evidence references               │
│   │                   │  - Conformance determination         │
│   │                   │  - PDF/HTML report                   │
│   └──────────────────┘                                      │
│                                                              │
│   ★ Our Sabre tool follows this same 4-stage pattern ★     │
└─────────────────────────────────────────────────────────────┘
```

---

## Key Architectural Patterns We Adopted

### 1. Project Configuration Pattern

**FACE CTS**: Uses `.pcfg` project configuration files that define what to test and how.

**Sabre Adaptation**: We use YAML rule files (`rules/*.yaml`) that define what conformance rules to check and against which standard version.

```
FACE CTS (.pcfg)                    SABRE (rules.yaml)
─────────────────                    ──────────────────
target_uop: MyComponent              target: nav_mpu
face_edition: 3.1                    standard: oms-v25
segment: PSS                         tier: 2
profile: General_Purpose             categories: [interface, behavioral, docs]
```

### 2. Segment-Specific Test Organization

**FACE CTS** organizes tests by FACE Technical Standard segments:
- **IOSS** (I/O Services Segment)
- **PCS** (Platform Computing Segment)
- **PSS** (Platform-Specific Services Segment)
- **TSS** (Transport Services Segment)

**Sabre Adaptation**: We organize rules by AMS GRA / OMS categories:
- **Interface Compliance** (CAL API, messaging)
- **Behavioral Compliance** (lifecycle, state machine)
- **Documentation Compliance** (service contracts, checklists)
- **Structural Compliance** (modularity, dependencies)

```
FACE CTS Segments              SABRE Categories
──────────────────              ─────────────────
IOSS (I/O Services)     →     Interface Compliance
PCS (Platform Comp.)    →     Structural Compliance
PSS (Platform-Specific) →     Behavioral Compliance
TSS (Transport)         →     Documentation Compliance
```

### 3. Gold Standard Library

**FACE CTS** ships with known-compliant reference implementations that serve as verification baselines.

**Sabre Adaptation**: Our synthetic MPU portfolio serves the same purpose:
- **Compliant MPUs** (nav_mpu, comms_mpu, sensor_fusion_mpu, ew_countermeasure_mpu) — known-good references
- **Non-compliant MPUs** (missing_cal_api, incomplete_docs, wrong_state_machine, timing_violation_mpu) — known-bad references that prove the tool catches real violations

```
FACE CTS GOLD STANDARD                SABRE SYNTHETIC MPUs
──────────────────────                 ────────────────────
Known-compliant UoPs                   Compliant MPUs (4)
  └─ Verify tool correctly passes       ├─ nav_mpu (open-source)
                                         ├─ comms_mpu (open-source)
                                         ├─ sensor_fusion_mpu (custom)
                                         └─ ew_countermeasure_mpu (custom)

Known-non-compliant UoPs               Non-compliant MPUs (4)
  └─ Verify tool correctly fails         ├─ missing_cal_api (open-source)
                                         ├─ incomplete_docs (open-source)
                                         ├─ wrong_state_machine (custom)
                                         └─ timing_violation_mpu (custom)
```

### 4. CLI-First Design

**FACE CTS**: Provides a command-line interface as the primary interaction method, with GUI as optional.

**Sabre Adaptation**: Same approach — CLI-first (`sabre verify`) with web dashboard as optional.

```
FACE CTS CLI:
  face-cts verify --project mycomponent.pcfg --output results/

SABRE CLI:
  sabre verify ./nav_mpu --rules oms-v25 --output results/
```

---

## What FACE CTS Does Well (That We Keep)

1. **Deterministic Results**: Same input → same output, every time. No AI randomness in core checks.
2. **Evidence-Based**: Every finding links to a specific code location and standard reference.
3. **Tiered Severity**: Not all failures are equal — CRITICAL vs WARNING vs INFO.
4. **Offline-Capable**: The tool runs without internet connection (important for classified environments).
5. **Version-Aware**: Supports multiple editions of the FACE standard simultaneously.

## Where FACE CTS Falls Short (That We Improve)

1. **No AI Analysis**: FACE CTS is purely static. Our tool adds Devin-powered deep analysis.
2. **No Fix Generation**: FACE CTS reports problems. Our tool generates change packages (PRs) that fix them.
3. **No Natural Language**: FACE CTS requires expertise to interpret results. Our "Ask Devin" panel explains in plain English.
4. **No Fleet View**: FACE CTS checks one UoP at a time. Our dashboard shows all MPUs across all vendors.
5. **No CI/CD Integration**: FACE CTS is a standalone tool. Our tool integrates with GitHub Actions and Windsurf.

---

## Classification

All content is **UNCLASSIFIED**. Analysis based on publicly available FACE CTS User Manual.
