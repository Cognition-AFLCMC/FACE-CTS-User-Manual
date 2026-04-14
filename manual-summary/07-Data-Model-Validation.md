# Chapter 7.7: Data Model Validation

> Summary of FACE CTS User Manual, Section 7.7 (Pages 122-125)

## Overview

The **Data Model Validation Tool (DMVT)** validates a UoC's data model (USM — UoP Supplied Model) against the Shared Data Model (SDM) and the data architecture specification in the FACE Technical Standard. DMVT validation is a prerequisite step — the CTS invokes DMVT before proceeding with link-level testing for UoCs that require a USM.

## What the User Must Provide

| Input | Description |
|-------|------------|
| **USM** | The UoP Supplied Model — the UoC's data model file |
| **SDM** | The Shared Data Model — available from The Open Group |
| **UDDL Version** | The version of Open Universal Domain Description Language used |

## DMVT Validation Process

### What DMVT Checks

1. **Structural Validity**: USM conforms to the expected XML/data format
2. **SDM Consistency**: Data types in the USM reference valid SDM entities
3. **Data Architecture Compliance**: USM adheres to FACE data architecture rules
4. **Type Definitions**: All type definitions follow FACE Technical Standard conventions
5. **Platform Type Mapping**: Platform-specific types are correctly mapped

### Test Procedures

1. Install the CTS
2. Launch the CTS GUI via `run_CTS_GUI.py`
3. Create or import a Project Configuration file
4. Configure the Data Model tab:
   - Set the USM path
   - Set the SDM path
   - Select the UDDL version
5. Click **"Validate Data Model"** to run DMVT

### Results

| Result | Meaning |
|--------|---------|
| **Valid** | USM conforms to the SDM and FACE data architecture specification |
| **Invalid** | USM has violations; detailed error messages are provided |

DMVT validation errors must be resolved before link-level conformance testing can proceed.

## DMVT vs. DAVT

| Tool | Full Name | Purpose |
|------|-----------|---------|
| **DMVT** | Data Model Validation Tool | Validates USM against SDM |
| **DAVT** | Data Architecture Validation Tool | Validates data architecture constraints (introduced in later editions) |

The CTS invokes both tools during the conformance test workflow (Step 11 in the Chapter 1 workflow).

## SABRE Relevance

SABRE's documentation engine performs a conceptually similar function to DMVT:

| CTS DMVT Concept | SABRE Equivalent |
|-----------------|-----------------|
| Validate USM against SDM | Validate MPU manifest against rule schema |
| Data architecture compliance | Documentation completeness checks |
| Prerequisite before link testing | Prerequisite before behavioral testing |
| XML/data format validation | YAML schema validation |
| SDM as reference data model | OMS/UCI standards as reference specifications |

Both systems implement a **"validate the model before testing the implementation"** pattern.

---

*Source: FACE CTS User Manual v1.0, Section 7.7 (Pages 122-125)*
