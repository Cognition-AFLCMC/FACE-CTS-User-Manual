# Chapter 7: Test Execution

> Summary of FACE CTS User Manual, Chapter 7 Overview and Appendix B (Pages 71, 126-132)

## Overview

Chapter 7 provides the complete test execution procedures for all FACE UoC types. The high-level workflow applies to all segments, with segment-specific variations documented in [09-Segment-Procedures.md](09-Segment-Procedures.md).

## High-Level Test Workflow

1. **Create the data model** (USM) for the UoC — if it uses or provides any type-specific interfaces (TSS Type Specific or LCM Stateful)
2. **Create a toolchain** (`.tcfg`) for the specific compiler/linker/archiver tools
3. **Create a project** (`.pcfg`) specifying profile, segment, interfaces, and data model
4. **Generate GSLs** — click "Generate GSLs/Interface" to produce Gold Standard Libraries
5. **Write Factory Functions** — implement the generated interface headers
6. **Compile UoC code** — using generated headers and include paths
7. **Add object code** and run "Test UoC Conformance"
8. **Review results** — PDF report generated with pass/fail and detailed logs

## GUI-Based Testing

The primary testing method uses the CTS GUI:

1. Launch CTS: `./run_CTS_GUI.py`
2. Import or create a Project Configuration
3. Configure all tabs (General, Data Model, GSL, Objects/Libraries)
4. Click **"Validate Project"** (checkmark icon) to verify configuration
5. Click **"Test UoC Conformance"** (play icon) to execute tests
6. Results are written to a PDF file in the project configuration directory

## CLI-Based Testing (Appendix B)

The CTS also supports command-line execution via `conformance_test.py`:

```bash
# Run a single project configuration
./conformance_test.py project.pcfg

# Run multiple configurations
./conformance_test.py project1.pcfg project2.pcfg

# Generate GSLs only
./conformance_test.py -a project.pcfg

# Generate GSLs for specific profile
./conformance_test.py -a -o general project.pcfg

# Generate interfaces only (C/C++ only)
./conformance_test.py -i project.pcfg
```

### CLI Switches

| Switch | Description |
|--------|------------|
| `project.pcfg` | Path to project configuration file |
| `-a` | Generate GSLs for the specified project |
| `-o FACE_PROFILE` | Profile for GSL generation (`general`, `safetyExt`, `safetyBase`, `security`) |
| `-i` | Create folder of interfaces only (C/C++ only) |

### CLI Return Codes

| Code | Meaning |
|------|---------|
| 0 | Segment passed verification |
| 1 | Fatal error or exception |
| 2 | Invalid command line options |
| 3 | Invalid toolchain configuration (`.tcfg`) file |
| 4 | Invalid project configuration (`.pcfg`) file |
| 5 | Segment did not pass verification |
| 6 | Data model was invalid |
| 7 | Result requires user to examine log file |
| 8 | Non-critical test failure (e.g., missing file) |

## Viewing Test Results

The CTS generates a **PDF conformance report** containing:
- Toolchain and project configuration information
- Source code excerpts relevant to test results
- Log results for each test
- Stack traces for failures
- Pass/fail status for each requirement tested

The PDF report path is displayed in the "Output File Location" section of the Conformance Test Results page.

## SABRE Relevance

SABRE's CLI design was directly modeled on the CTS CLI:

| CTS CLI | SABRE CLI |
|---------|----------|
| `./conformance_test.py project.pcfg` | `sabre verify ./mpu_path --rules oms-v25` |
| Return code 0 = pass | Return code 0 = pass |
| Return code 5 = fail | Non-zero = violations found |
| `-a` for GSL generation | Not needed (no compilation) |
| Multiple `.pcfg` files | Multiple MPU paths or `--fleet` mode |
| PDF report output | PDF + JSON + dashboard output |

Key CTS CLI patterns adopted by SABRE:
- **Non-interactive batch mode** for CI/CD integration
- **Structured return codes** for automation
- **Configuration file as primary input** (`.pcfg` → `manifest.yaml`)
- **Report output** with detailed logs and evidence

---

*Source: FACE CTS User Manual v1.0, Chapter 7 and Appendix B (Pages 71, 126-132)*
