# FACE-to-AMS GRA Concept Mapping

> How FACE Technical Standard concepts map to AMS GRA (Army Modular Software Generic Reference Architecture) equivalents used by SABRE.

## Overview

The FACE Technical Standard and AMS GRA both define modular software architectures for defense systems, but they target different domains. FACE focuses on **airborne computing environments**, while AMS GRA focuses on **mission software architecture**. Understanding how concepts map between them is critical for SABRE, which applies FACE CTS verification patterns to AMS GRA conformance.

## Core Concept Mapping

| FACE Concept | AMS GRA Equivalent | Relationship |
|-------------|-------------------|-------------|
| **Unit of Conformance (UoC)** | **Mission Processing Unit (MPU)** | The software component under test |
| **FACE Segment** | **MPU Category** | Architectural classification of the component |
| **Gold Standard Library (GSL)** | **Reference Interface** | The "known-good" baseline for verification |
| **Conformance Test Suite (CTS)** | **SABRE Conformance Tool** | The verification engine |
| **Project Configuration (.pcfg)** | **MPU Manifest (manifest.yaml)** | Component metadata and test configuration |
| **Toolchain Configuration (.tcfg)** | **Rule Set Selection (--rules)** | Verification environment configuration |
| **Conformance Verification Matrix (CVM)** | **YAML Rule Files** | The requirements to verify against |
| **Verification Authority (VA)** | **Verification Authority (VA)** | Same role — the human reviewer |

## Segment-to-Category Mapping

| FACE Segment | Abbreviation | AMS GRA Category | SABRE Tier |
|-------------|-------------|------------------|-----------|
| Operating System Segment | OSS | Platform | Tier 1 (90%) |
| Platform Specific Services Segment | PSSS | Platform | Tier 1 (90%) |
| Transport Services Segment | TSS | Subsystem | Tier 2 (85%) |
| I/O Services Segment | IOSS | Subsystem | Tier 2 (85%) |
| Portable Components Segment | PCS | Application | Tier 3 (80%) |

## Architecture Layer Mapping

```
FACE Architecture                    AMS GRA Architecture
─────────────────                    ────────────────────
┌─────────────────────┐              ┌─────────────────────┐
│  PCS (Portable      │              │  Application MPUs   │
│  Components)        │              │  (Mission Logic)    │
├─────────────────────┤              ├─────────────────────┤
│  TSS (Transport     │              │  Service MPUs       │
│  Services)          │              │  (Integration)      │
├─────────────────────┤              ├─────────────────────┤
│  IOSS (I/O          │              │  Infrastructure     │
│  Services)          │              │  MPUs (I/O, Comms)  │
├─────────────────────┤              ├─────────────────────┤
│  PSSS (Platform     │              │  Platform MPUs      │
│  Specific)          │              │  (OS Services)      │
├─────────────────────┤              ├─────────────────────┤
│  OSS (Operating     │              │  Operating          │
│  System)            │              │  Environment        │
└─────────────────────┘              └─────────────────────┘
```

## Interface Mapping

| FACE Interface Concept | AMS GRA Interface Concept |
|----------------------|--------------------------|
| FACE API (defined per segment) | CAL API (Critical Abstraction Layer) |
| Injectable Interface | Service Interface (OMS Service Interface) |
| Factory Function | Not applicable (static analysis, no runtime adapters) |
| USM (UoP Supplied Model) | MPU Manifest + Interface Control Document |
| SDM (Shared Data Model) | UCI XSD Schema (message type definitions) |

## Verification Approach Mapping

| FACE CTS Verification | SABRE Verification |
|----------------------|-------------------|
| **Link-level testing** — compile UoC against GSLs | **AST analysis** — parse MPU source against rules |
| **Binary interface verification** — does it link? | **Source pattern verification** — does it follow patterns? |
| **Segment-scoped** — test one segment at a time | **Rule-scoped** — test one rule category at a time |
| **Single-UoC focus** — one test run per UoC | **Fleet capability** — verify multiple MPUs in one run |
| **Pass/fail binary** — conformant or not | **Tiered scoring** — percentage-based with thresholds |

## Data Model Mapping

| FACE Data Concept | AMS GRA Data Concept |
|------------------|---------------------|
| Shared Data Model (SDM) | UCI XSD Schema |
| UoP Supplied Model (USM) | MPU Interface Definition |
| UDDL (Domain Description Language) | UCI Message Set Definition |
| Platform Data Types | OMS Data Types |
| Data Architecture Specification | UCI Normalized Interface Specification |

## Report Mapping

| FACE CTS Report Element | SABRE Report Element |
|------------------------|---------------------|
| PDF conformance report | PDF + JSON conformance report |
| Pass/fail per requirement | Pass/fail per rule check with severity |
| Toolchain configuration summary | Rule set and MPU metadata summary |
| Source code excerpts | Code snippets with violation context |
| Compiler/linker error logs | AST analysis findings with evidence |
| No fleet view | Fleet dashboard with aggregate scores |
| No remediation suggestions | Change packages with fix recommendations |

## Key Differences

| Aspect | FACE CTS | SABRE |
|--------|---------|-------|
| **Domain** | Airborne computing | Mission software architecture |
| **Standard** | FACE Technical Standard | OMS v2.5 / UCI v2.5 |
| **Verification Method** | Compile and link | Static analysis (AST + regex) |
| **Requires Compilation** | Yes (compiler + linker required) | No (source-only analysis) |
| **Languages Tested** | C, C++, Ada, Java | Python (extensible) |
| **Scope** | Single UoC per test | Single MPU or fleet |
| **Output** | Binary pass/fail | Percentage score with tiers |

---

*This mapping is based on analysis of the publicly available FACE CTS User Manual and AMS GRA documentation.*
