# Chapter 1: Introduction

> Summary of FACE CTS User Manual, Chapter 1 (Pages 7-11)

## Purpose

The Conformance Test Suite (CTS) User Manual guides users through installing and effectively using the CTS to test software conformance against the FACE Technical Standard, Editions 3.0, 3.1, and 3.2.

## What Can Be Tested

The CTS tests **Units of Conformance (UoCs)** and **data models** against a subset of requirements defined in the FACE Technical Standard. All testable requirements are identified in the **Conformance Verification Matrix (CVM)**, provided by the FACE Consortium.

All types of UoCs may be tested:

| # | Segment | Abbreviation |
|---|---------|-------------|
| 1 | Portable Components Segment | PCS |
| 2 | Platform Specific Services Segment | PSSS |
| 3 | Transport Services Segment | TSS |
| 4 | I/O Services Segment | IOSS |
| 5 | Operating System Segment | OSS |

## CTS Version Numbering

The CTS version identifier follows the pattern `3.X.Y`:
- **X** = minor edition of the Technical Standard
- **Y** = version of the CTS released for that edition

Example: CTS 3.1.0 supports FACE Technical Standard Edition 3.1 and is the initial CTS release for that edition.

This manual covers **CTS 3.0.9, 3.1.8, and 3.2.2**.

## Tools Contained in the Test Suite

The CTS includes multiple tools that work together:

### 1. UsmIDLGenerator / DIG (Data Model to IDL Generator)
- Generates IDL (Interface Definition Language) for platform data types and views specified by a UoP (Unit of Portability) in a USM (UoP Supplied Model)
- Generated IDL is compiled into source code for the UoC's programming language

### 2. Ideal (IDL Translator)
- Converts FACE interfaces defined in IDL using programming language mappings from the FACE Technical Standard
- Supports Ada, Java, C99, and C++03
- Generates language-specific code from UsmIDLGenerator/DIG output

### 3. DMVT (Data Model Validation Tool)
- Takes Shared Data Model (SDM) and UoC's USM as inputs
- Tests USM adherence to the data architecture specification
- The SDM is available at [opengroup.org/face/docsandtools](https://www.opengroup.org/face/docsandtools)

### 4. FACE Conformance Application
- Front-end GUI and backend testing processes
- Generates the FACE conformance report after testing

## Conformance Testing Workflow

The CTS workflow follows a 14-step process:

1. **Create/import toolchain configuration** (`.tcfg`) for compiler/linker/archiver tools
2. **Create/import project configuration** (`.pcfg`) specifying profile, segment, interfaces, and data model
3. **Click "Generate GSLs/Interface"** to start the process
4. USM location is read from the `.pcfg` file; USM is parsed for TSS and LCM interfaces
5. **UsmIDLGen/DIG** translates data structures to IDL
6. **Ideal** generates interface headers/spec files as the **Gold Standard Library (GSL)**
7. User retrieves the generated text file with include paths
8. User writes **Factory Functions** implementing each UoC interface
9. User adds Factory Functions to the `.pcfg` file
10. User **compiles UoC code** using generated headers and include paths
11. User adds object code and runs **"Test UoC Conformance"**
12. DMVT validates the USM against the SDM
13. **Conformance Application** runs link tests:
    - Tests source files for injectables
    - Compiles CTS Factory Functions
    - Links test executables (link-only, never executed)
    - Performs restricted call verification
14. **PDF conformance report** is generated with test logs and stack traces

## SABRE Relevance

The CTS workflow directly informed SABRE's verification pipeline design:

| CTS Step | SABRE Equivalent |
|----------|-----------------|
| `.pcfg` project configuration | YAML rule files |
| `.tcfg` toolchain configuration | `--rules` CLI flag |
| Gold Standard Libraries | Synthetic MPU portfolio |
| Link-test verification | AST + schema verification |
| PDF conformance report | PDF + JSON + dashboard output |
| 14-step workflow | 6-stage pipeline (Ingest → Remediate) |

---

*Source: FACE CTS User Manual v1.0, Chapter 1 (Pages 7-11)*
