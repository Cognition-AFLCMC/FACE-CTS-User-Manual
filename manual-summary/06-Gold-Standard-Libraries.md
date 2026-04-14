# Chapter 6: Sample Projects and Gold Standard Libraries

> Summary of FACE CTS User Manual, Chapters 6 and relevant sections of Chapter 7 (Pages 67-70, 79-81)

## Overview

**Gold Standard Libraries (GSLs)** are the cornerstone of the CTS verification methodology. They are auto-generated reference implementations of FACE interfaces that serve as the "known-good" baseline against which UoC code is tested.

## What Are Gold Standard Libraries?

GSLs are generated code artifacts that contain:
- All possible function calls allowed by the FACE Technical Standard for a given segment and profile
- All data types defined in the standard
- All constants specified in the standard

When a UoC is linked against GSLs, the linker verifies that the UoC's interfaces match exactly what the standard requires — no more, no less.

## GSL Generation Process

The GSL generation pipeline uses two tools:

### Step 1: UsmIDLGenerator / DIG
1. Reads the USM (UoP Supplied Model) from the project configuration
2. Parses for TSS Typed interfaces and LCM Stateful interfaces
3. Translates data structures and typed interfaces to IDL (Interface Definition Language)

### Step 2: Ideal (IDL Translator)
1. Takes the IDL output from Step 1
2. Generates language-specific interface files:
   - **C/C++**: Header files (`.h`, `.hpp`)
   - **Ada**: Spec files (`.ads`)
   - **Java**: Java interface files (`.java`)
3. Places generated files in the GSL output directory (`include/FACE/`)
4. Generates a text file listing all required include paths

## Sample Project and Toolchain Configuration Files

The CTS includes sample projects for testing and validation:

### Build Flags
Sample projects include predefined build flags for supported compilers and target architectures.

### Linux Generation
```bash
# Generate sample projects on Linux
# The CTS provides scripts to auto-generate sample .pcfg and .tcfg files
```

### Windows Generation
Windows generation has specific considerations:
- MSYS2/MinGW toolchain paths differ from Linux
- Cygwin compatibility requires specific flags
- **Known issue**: Failing test results may occur with Shared Data Model configurations on Windows

## Factory Functions

After GSL generation, the user must write **Factory Functions**:

1. For each FACE interface the UoC **provides**:
   - C++/Java: Create a derived class from the generated interface class
   - C/Ada: Implement the functions/procedures defined in the generated interface

2. For each FACE interface the UoC **uses** (accesses):
   - Implement the Injectable interface for that interface

Factory Functions are the bridge between the UoC's actual implementation and the CTS test framework.

## SABRE Relevance

The GSL concept was one of the most influential CTS patterns for SABRE's design:

| CTS GSL Concept | SABRE Equivalent |
|----------------|-----------------|
| Gold Standard Libraries (auto-generated reference code) | **Synthetic MPU portfolio** (reference architectures) |
| GSLs represent "known-good" interfaces | Compliant synthetic MPUs represent "known-good" architectures |
| UoC linked against GSLs to verify conformance | MPU analyzed against YAML rules to verify conformance |
| Factory Functions (user-written test adapters) | Not needed (SABRE analyzes source directly) |
| `include/FACE/` output directory | Rule files in `rules/oms-v25/`, `rules/uci-v25/` |
| USM → IDL → GSL generation pipeline | OMS Excel → YAML rule generation pipeline |

The key insight SABRE borrowed: **define "known-good" reference artifacts, then test real code against them**.

---

*Source: FACE CTS User Manual v1.0, Chapters 6-7 (Pages 67-70, 79-81)*
