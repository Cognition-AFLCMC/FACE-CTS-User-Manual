# Reporting and Test Results

> Summary of FACE CTS User Manual, Section 7.10 and Appendices (Pages 126-149)

## Overview

The CTS generates a **PDF conformance report** as its primary output. This report provides complete evidence of the conformance test, including all configuration details, test results, and diagnostic information.

## PDF Report Contents

The conformance report includes:

| Section | Contents |
|---------|----------|
| **Configuration Summary** | Toolchain and project configuration details used for the test |
| **Test Results** | Pass/fail status for each requirement tested |
| **Source Code** | Relevant source code excerpts from the test |
| **Log Results** | Detailed log output from each test step |
| **Stack Traces** | Full stack traces for any failures (for debugging) |
| **Output File Location** | Path to the generated PDF report |

## Report Output Location

The PDF report is written to the directory of the project configuration file (`.pcfg`). The exact path is displayed in the **"Output File Location"** field of the Conformance Test Results page.

## Interpreting Results

### Pass
A successful conformance test indicates that the UoC correctly links against all required FACE interfaces for the specified segment and profile. The report confirms conformance with respect to the requirements tested.

### Fail
A failed conformance test provides:
- The specific test(s) that failed
- Source code showing the expected vs. actual interface definitions
- Log files with compiler/linker error messages
- Stack traces pointing to the source of non-conformance

The user can use this diagnostic information to modify the UoC and re-test.

## Appendices Summary

### Appendix A: References
Key references cited in the manual:
- FACE Technical Standard, Editions 3.0 (C17C), 3.1 (C207), 3.2 (C232)
- Open UDDL, Editions 1.0 (C198), 1.1 (C231)
- OMG Interface Definition Language specification

### Appendix C: Glossary

| Acronym | Meaning |
|---------|---------|
| CTS | Conformance Test Suite |
| CVM | Conformance Verification Matrix |
| DMVT | Data Model Validation Tool |
| GSL | Gold Standard Library |
| IDL | Interface Definition Language |
| IOSS | I/O Services Segment |
| LCM | Life Cycle Management |
| OCL | Object Constraint Language |
| OSS | Operating System Segment |
| PCS | Portable Components Segment |
| PSSS | Platform Specific Services Segment |
| SDM | Shared Data Model |
| TSS | Transport Services Segment |
| UDDL | Open Universal Domain Description Language |
| UoC | Unit of Conformance |
| UoP | Unit of Portability |
| USM | UoP Supplied Model |
| VA | Verification Authorities |

### Appendix D: Constraints
Known constraints and limitations of the CTS.

### Appendix E: Known Issues
Known issues organized by:
- **E.1**: Issues common to CTS 3.0.9, 3.1.8, and 3.2.2
- **E.2**: Issues specific to individual CTS versions in this release

### Appendix F: Acknowledgments
Credits to FACE Consortium members and contributing organizations including Georgia Tech Research Institute (GTRI) and Institute for Software Integrated Systems (ISIS).

### Appendix G: Approved Corrections to Technical Standard by CTS Version
Documents corrections applied to the CTS testing logic that differ from the published Technical Standard text, organized by CTS version.

### Appendix H: CTS Treatment of Default Values
Details how the CTS handles default values for element/composition attributes in the data model, including default values for model element attributes.

## SABRE Relevance

SABRE's reporting was directly influenced by the CTS report model:

| CTS Report Feature | SABRE Report Feature |
|-------------------|---------------------|
| PDF conformance report | PDF + JSON report output |
| Pass/fail per requirement | Pass/fail per rule check |
| Source code excerpts | Code snippets with violations highlighted |
| Log files and stack traces | Detailed evidence for each finding |
| Configuration summary | MPU metadata and rule set summary |
| Single-UoC report | Single-MPU report + fleet dashboard |
| No machine-readable output | JSON output for CI/CD integration |
| No remediation guidance | Change package generation (auto-fix PRs) |

SABRE extended the CTS reporting model by adding:
- **JSON output** for machine consumption and CI/CD gates
- **Fleet dashboard** for multi-MPU visualization
- **Change packages** with automated remediation suggestions
- **Tiered scoring** (80/85/90% thresholds) beyond binary pass/fail

---

*Source: FACE CTS User Manual v1.0, Section 7.10 and Appendices A-H (Pages 126-149)*
