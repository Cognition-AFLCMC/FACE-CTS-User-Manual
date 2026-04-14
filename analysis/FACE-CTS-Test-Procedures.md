# FACE CTS Test Procedures

## Test Procedure Patterns Mapped to AMS GRA Conformance

The FACE CTS organizes test procedures by segment and profile. This document maps those patterns to our AMS GRA conformance verification approach.

---

## FACE CTS Test Procedure Categories

The FACE CTS defines test procedures across four segments, each with specific verification focus areas:

```
┌─────────────────────────────────────────────────────────────┐
│              FACE CTS TEST PROCEDURE CATEGORIES              │
├──────────────┬──────────────────────────────────────────────┤
│ IOSS         │ I/O interface conformance                     │
│ (I/O Svcs)   │ - Data format validation                     │
│              │ - Interface signature checking                │
│              │ - I/O abstraction compliance                  │
├──────────────┼──────────────────────────────────────────────┤
│ PCS          │ Platform computing conformance                │
│ (Platform)   │ - OS abstraction layer compliance            │
│              │ - Resource management patterns               │
│              │ - Thread/process model verification           │
├──────────────┼──────────────────────────────────────────────┤
│ PSS          │ Platform-specific services                    │
│ (Specific)   │ - Hardware abstraction verification          │
│              │ - Device driver interface compliance          │
│              │ - Platform-specific API usage                 │
├──────────────┼──────────────────────────────────────────────┤
│ TSS          │ Transport services conformance                │
│ (Transport)  │ - Message transport compliance               │
│              │ - Pub/sub pattern verification                │
│              │ - Data distribution compliance                │
└──────────────┴──────────────────────────────────────────────┘
```

## Mapping to Sabre / OMS Categories

```
FACE CTS Segment    →    Sabre Category         →    OMS/UCI Source
────────────────────────────────────────────────────────────────────
IOSS (I/O)          →    Interface Compliance    →    OMS §5.3 (CAL API)
                                                      UCI XSD schemas
                                                      OMS §5.4 (Transport)

PCS (Platform)      →    Structural Compliance   →    OMS §4.1 (Service Model)
                                                      OMS §4.3 (Tier System)
                                                      Module boundary checks

PSS (Specific)      →    Behavioral Compliance   →    OMS §6.1 (Lifecycle)
                                                      OMS §5.5 (QoS)
                                                      Timing constraints

TSS (Transport)     →    Documentation           →    OMSC-TMP-003 (Service Contract)
                         Compliance                   OMSC-CHK-005 (Checklist)
                                                      API documentation
```

---

## Test Procedure Structure

### FACE CTS Test Procedure Format

Each FACE CTS test procedure follows a standard structure:
1. **Test ID**: Unique identifier
2. **Description**: What is being tested
3. **Preconditions**: Required setup
4. **Test Steps**: Step-by-step execution
5. **Expected Results**: What constitutes pass/fail
6. **Evidence**: What to collect as proof

### Sabre Verification Check Format (Derived)

```yaml
- id: OMS-CAL-001
  category: interface
  severity: CRITICAL
  description: "MPU uses CAL API for all messaging (OMS §5.3)"
  check_type: ast_pattern
  pattern: "import.*cal_api|from.*cal.*import"
  expected: true
  evidence: "File path, line number, code excerpt"
  standard_ref: "OMS v2.5, Section 5.3, Requirement CAL-API-001"
  remediation: "Replace direct messaging with CAL API calls"
```

---

## Test Categories by Verification Type

### Static Analysis Tests (Automated)

| Category | Example Check | FACE CTS Analog |
|----------|--------------|-----------------|
| API Usage | CAL API import verification | IOSS interface checks |
| Schema Validation | UCI message type validation | IOSS data format checks |
| Pattern Matching | Lifecycle state machine implementation | PCS OS abstraction checks |
| Dependency Analysis | No cross-MPU direct imports | PCS resource management |

### Document Analysis Tests (Automated)

| Category | Example Check | FACE CTS Analog |
|----------|--------------|-----------------|
| Service Contract | All required sections populated | TSS documentation |
| Compliance Checklist | OMSC-CHK-005 complete | TSS compliance docs |
| API Documentation | All public methods documented | IOSS interface docs |

### Behavioral Analysis Tests (Devin-Powered)

| Category | Example Check | FACE CTS Analog |
|----------|--------------|-----------------|
| Semantic Analysis | Interface semantics match spec | PSS platform-specific |
| Cross-Reference | Code matches documented behavior | PSS verification |
| Edge Cases | Error handling covers spec requirements | PCS robustness |

---

## Classification

All content is **UNCLASSIFIED**. Derived from publicly available FACE CTS User Manual.
