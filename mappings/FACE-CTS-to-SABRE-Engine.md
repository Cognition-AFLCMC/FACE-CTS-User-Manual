# FACE CTS-to-SABRE Engine Mapping

> How FACE CTS patterns informed the design of SABRE's 3-engine verification pipeline.

## Overview

SABRE's conformance verification engine was designed by studying the FACE CTS User Manual (149 pages) and extracting the architectural patterns, test methodologies, and tool design decisions that could be adapted for AMS GRA software architecture verification. This document traces the specific CTS patterns that shaped each component of SABRE.

## The Core Design Insight

FACE CTS verifies **binary interface conformance** through compile-and-link testing. SABRE needed to verify **software architecture conformance** through source code analysis. The key insight was:

> **CTS proves "your code has the right interfaces" by linking against reference implementations. SABRE proves "your code has the right architecture" by analyzing against reference rules.**

Both approaches share the same fundamental pattern: **define what "correct" looks like, then test real artifacts against that definition**.

## CTS Pattern → SABRE Engine Mapping

### 1. Interface Verification Engine

| CTS Pattern | SABRE Implementation |
|------------|---------------------|
| **GSL link test** — link UoC against Gold Standard Libraries to verify interface signatures | **AST pattern matching** — parse MPU source to verify CAL API usage patterns |
| **What it checks**: Function signatures, data types, constants exist | **What it checks**: Import statements, API calls, interface definitions |
| **How it fails**: Linker error (symbol not found) | **How it fails**: Rule violation (pattern not found or prohibited pattern found) |
| **CTS segments**: IOSS, TSS | **SABRE rules**: OMS §5.3 (CAL API), interface compliance rules |

**Specific CTS patterns adopted**:
- CTS verifies that UoCs don't call restricted functions → SABRE verifies MPUs don't bypass CAL (direct ASB access)
- CTS checks interface completeness → SABRE checks that all required OMS Service Interfaces are implemented
- CTS validates type definitions → SABRE validates UCI message schema compliance

### 2. Behavioral Verification Engine

| CTS Pattern | SABRE Implementation |
|------------|---------------------|
| **Factory Functions** — user-written adapters that prove UoC implements required behaviors | **Lifecycle analysis** — AST analysis that verifies MPU implements required state transitions |
| **What it checks**: Interface implementation exists | **What it checks**: State machine compliance, lifecycle management |
| **How it fails**: Factory Function won't compile | **How it fails**: Missing states, invalid transitions |
| **CTS segments**: PCS, PSSS | **SABRE rules**: OMS §6.1 (Lifecycle), behavioral compliance rules |

**Specific CTS patterns adopted**:
- CTS requires Factory Functions for each provided interface → SABRE requires lifecycle states for each service
- CTS tests derived class implementations → SABRE tests state machine completeness
- CTS verifies injectable interfaces → SABRE verifies dependency injection patterns

### 3. Documentation Verification Engine

| CTS Pattern | SABRE Implementation |
|------------|---------------------|
| **DMVT validation** — validate data model (USM) against reference model (SDM) | **Template matching** — validate MPU documentation against OMSC-TMP-003 template |
| **What it checks**: Data model structure and completeness | **What it checks**: Service contract completeness and structure |
| **How it fails**: USM doesn't match SDM | **How it fails**: Missing sections, incomplete documentation |
| **CTS tool**: DMVT (Data Model Validation Tool) | **SABRE engine**: Documentation engine |

**Specific CTS patterns adopted**:
- CTS validates USM against SDM before testing → SABRE validates manifest before analysis
- CTS checks data architecture compliance → SABRE checks documentation completeness
- DMVT is a prerequisite gate → Documentation checks are a prerequisite for full scoring

## Tool Architecture Mapping

```
FACE CTS Architecture              SABRE Architecture
────────────────────                ──────────────────
┌────────────────────┐              ┌────────────────────┐
│  CTS GUI           │              │  Web Dashboard     │
│  (Java/JavaFX)     │              │  (Flask)           │
├────────────────────┤              ├────────────────────┤
│  CLI               │              │  CLI               │
│  (conformance_     │              │  (sabre verify)    │
│   test.py)         │              │                    │
├────────────────────┤              ├────────────────────┤
│  Conformance App   │              │  3-Engine Pipeline │
│  (test + link)     │              │  (parse + check)   │
├────────────────────┤              ├────────────────────┤
│  UsmIDLGen/DIG     │              │  Rule Generator    │
│  (data → IDL)      │              │  (Excel → YAML)    │
├────────────────────┤              ├────────────────────┤
│  Ideal             │              │  Rule Selector     │
│  (IDL → code)      │              │  (filter rules)    │
├────────────────────┤              ├────────────────────┤
│  DMVT              │              │  Doc Engine        │
│  (validate model)  │              │  (validate docs)   │
├────────────────────┤              ├────────────────────┤
│  PDF Report        │              │  Report Generator  │
│                    │              │  (PDF + JSON)      │
└────────────────────┘              └────────────────────┘
```

## Configuration File Evolution

```
CTS .pcfg File                      SABRE YAML Rule File
──────────────                      ────────────────────
<project>                           rule:
  <segment>PCS</segment>              id: OMS-CAL-001
  <language>C++</language>             category: interface
  <profile>general</profile>           severity: CRITICAL
  <toolchain>gcc.tcfg</toolchain>      standard: OMS v2.5
  <objects>                            section: "§5.3"
    <file>myuoc.o</file>               check:
  </objects>                             pattern: "import.*oms.cal"
  <usm>model.usm</usm>                  description: "CAL API import"
</project>                              expected: true
```

The evolution from XML-based `.pcfg` to YAML rule files was driven by:
1. **Version control**: YAML diffs cleanly in git; XML does not
2. **Human readability**: YAML is easier for VAs to review and modify
3. **Composability**: YAML rules can be filtered and combined; `.pcfg` is monolithic
4. **Automation**: YAML can be auto-generated from Excel checklists

## CLI Design Evolution

```
CTS CLI:
  ./conformance_test.py project.pcfg
  ./conformance_test.py -a project.pcfg          # Generate GSLs
  ./conformance_test.py -o general project.pcfg   # Profile-specific

SABRE CLI:
  sabre verify ./mpu_path --rules oms-v25
  sabre verify ./mpu_path --rules oms-v25 --tier 1    # Tier-specific
  sabre verify --fleet ./fleet_dir --rules oms-v25     # Fleet mode
  sabre change-packages ./mpu_path                      # Remediation
```

Key CLI patterns borrowed from CTS:
- **Configuration file as primary input** (`.pcfg` → `manifest.yaml`)
- **Non-interactive mode** for automation
- **Structured return codes** (0 = pass, non-zero = issues)
- **Profile/tier filtering** (`-o general` → `--tier 1`)

Key CLI patterns SABRE added beyond CTS:
- **Fleet mode** — verify multiple MPUs in one command
- **Change packages** — auto-generate remediation suggestions
- **JSON output** — machine-readable results for CI/CD
- **Dashboard mode** — launch web visualization

## What SABRE Didn't Adopt from CTS

| CTS Feature | Why SABRE Didn't Adopt It |
|------------|--------------------------|
| Compile-and-link testing | SABRE does source analysis; no compilation needed |
| Toolchain configuration | No compiler/linker tools to configure |
| Factory Functions | No runtime adapters needed for static analysis |
| Java/JavaFX GUI | SABRE uses Flask web dashboard instead |
| Language-specific testing | SABRE currently targets Python only (extensible) |
| Binary artifact input | SABRE works with source code directly |

## Summary

SABRE's 3-engine architecture is a **domain adaptation** of CTS's verification approach:

| Dimension | CTS | SABRE |
|-----------|-----|-------|
| **Domain** | Hardware interface conformance | Software architecture conformance |
| **Method** | Compile and link | Parse and match |
| **Reference** | Gold Standard Libraries | YAML rule definitions |
| **Input** | Binary objects + headers | Source code + manifest |
| **Output** | PDF report (pass/fail) | PDF + JSON + dashboard (scored) |
| **Scale** | One UoC at a time | One MPU or entire fleet |

---

*This mapping is based on analysis of the publicly available FACE CTS User Manual v1.0 and the SABRE conformance tool architecture.*
