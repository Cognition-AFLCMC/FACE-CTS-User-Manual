[![FACE CTS](https://img.shields.io/badge/FACE%20CTS-Conformance%20Test%20Suite-2f5597)]() [![The Open Group](https://img.shields.io/badge/Source-The%20Open%20Group-1e3a8a)]() [![Classification](https://img.shields.io/badge/Classification-UNCLASSIFIED-green)]() [![Content](https://img.shields.io/badge/Content-Analysis%20%26%20Summary-blue)]()

# FACE CTS User Manual — Analysis & Summary

Reference documentation analyzing the **FACE Conformance Test Suite (CTS) User Manual** (149 pages) and how its patterns informed the design of Project SABRE's conformance verification tool.

> [!NOTE]
> This repository contains **analysis and summaries** of the FACE CTS User Manual — not the manual itself. The original 149-page PDF is published by The Open Group and available for download at [opengroup.org](https://www.opengroup.org/sites/default/files/FACE_CTS_UserManual_v10.pdf). All content here is UNCLASSIFIED and derived from publicly available sources.

## Overview

The FACE Conformance Test Suite (CTS) is The Open Group's official tool for verifying software conformance to the FACE Technical Standard. The CTS provided critical design patterns and implementation lessons that directly shaped how SABRE was built.

```
┌─────────────────────────────────────────────────────────────┐
│              WHAT SABRE LEARNED FROM FACE CTS                │
│                                                              │
│  FACE CTS Pattern              SABRE Adoption                │
│  ──────────────────            ──────────────                │
│  .pcfg project config    →    YAML rule files                │
│  .tcfg toolchain config  →    CLI --rules flag               │
│  Gold Standard Libraries →    Synthetic MPU portfolio         │
│  Segment-based testing   →    3-engine verification pipeline │
│  CLI batch mode          →    sabre verify command           │
│  PDF conformance report  →    PDF + JSON + dashboard output  │
│  Link-level verification →    AST + schema verification      │
│                                                              │
│  CTS tests hardware interfaces; SABRE tests software arch.  │
└─────────────────────────────────────────────────────────────┘
```

## Original PDF

- **Title**: Conformance Test Suite User Manual
- **Versions Covered**: CTS 3.0.9, 3.1.8, and 3.2.2
- **Standards Supported**: FACE Technical Standard, Editions 3.0, 3.1, and 3.2
- **Pages**: 149
- **Publisher**: The Open Group
- **Download**: [FACE CTS User Manual v1.0 (PDF)](https://www.opengroup.org/sites/default/files/FACE_CTS_UserManual_v10.pdf)

## Why FACE CTS?

FACE CTS provides the **"how tests work"** — the test patterns and tool design that SABRE's conformance engine was modeled after:

- **Proven Test Architecture**: CTS has been used for years to verify FACE conformance. Its architecture is battle-tested
- **CLI + GUI Design**: CTS supports both GUI and CLI workflows. SABRE adopted the CLI-first approach for automation
- **Project Configuration**: CTS's `.pcfg` file format directly inspired SABRE's YAML rule file design
- **Gold Standard Libraries**: CTS's concept of "known-good reference code" became SABRE's synthetic MPU portfolio
- **Segment-Based Testing**: CTS organizes tests by FACE segments (PCS, PSSS, TSS, IOSS, OSS). SABRE organizes by check category (interface, behavioral, documentation)

## What's Included

| Directory | Contents |
|-----------|----------|
| [`analysis/`](analysis/) | Architecture analysis, CLI patterns, test procedures, FACE-to-SABRE mapping |
| [`patterns/`](patterns/) | Gold Standard Library pattern, Project Configuration pattern |
| [`manual-summary/`](manual-summary/) | Chapter-by-chapter summaries of the 149-page PDF |
| [`mappings/`](mappings/) | FACE-to-AMS GRA concept mapping, FACE CTS-to-SABRE engine mapping |

## CTS Version Coverage

| CTS Version | FACE Standard Edition | Status |
|-------------|----------------------|--------|
| 3.0.9 | Edition 3.0 | Covered in this analysis |
| 3.1.8 | Edition 3.1 | Covered in this analysis |
| 3.2.2 | Edition 3.2 | Covered in this analysis |

## Relationship to Other Repos

```
Cognition-AFLCMC/
├── MOSA                       (policy framework — the "WHY")
├── SOSA                       (verification process — the "HOW")
├── FACE-CTS-User-Manual       ◄── THIS REPO (test patterns — the "HOW tests work")
├── AF-OMS                     (conformance rules source — the "WHAT to verify")
├── AF-UCI                     (message schemas — the "WHAT messages look like")
├── SABRE-Conformance-Application-  (the conformance tool itself)
└── Synthetic-MPU-s            (test fixtures — compliant & non-compliant MPUs)
```

## Classification

All materials in this repository are **UNCLASSIFIED**. Content is derived from the publicly available FACE CTS User Manual PDF and our own analysis. No proprietary or member-restricted Open Group content is reproduced — only publicly available information and our interpretations.

**Copyright Notice**: The FACE CTS User Manual is copyright © The Open Group. The summaries and analysis in this repository are original work that references the publicly available PDF. The original PDF should be consulted for authoritative content.
