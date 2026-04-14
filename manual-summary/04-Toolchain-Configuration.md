# Chapter 4: Toolchain Configuration File

> Summary of FACE CTS User Manual, Chapter 4 (Pages 40-52)

## Overview

A **Toolchain Configuration File** (`.tcfg`) contains information to configure and compile CTS test objects. It specifies how to link with a user-supplied UoC given its target environment. Toolchain files use the `.tcfg` extension and are generated with [StringTemplate](http://www.stringtemplate.org/), a freely available template library.

## Managing Toolchain Files

The Toolchain Files List is accessed via **File > Toolchains** in the CTS navigation bar.

| Action | Description |
|--------|------------|
| **New** | Opens the toolchain editor for creation from scratch |
| **Import** | File browser to copy existing `.tcfg` file(s) to the working directory |
| **Open** | Opens the selected toolchain in the editor |
| **Remove** | Removes from list view (does not delete the file) |
| **Clone** | Creates a copy of the selected toolchain |
| **Change** | Changes the directory to search for toolchain files |

## Building a Toolchain Configuration File

The Toolchain Configuration Builder has five tabs:

### 1. General Tab

| Setting | Description |
|---------|------------|
| **Programming Language** | Must match the UoC's language (C, C++, Ada, Java) |
| **Segment Type** | Either "IOSS/PCS/PSSS/TSS" or "OSS" (OSS testing is different) |
| **OSS Profile(s)** | Select all applicable profiles the UoC satisfies |
| **PATH Addition** | Include library paths (e.g., `/usr/bin` for GCC) |
| **Environment Variables** | Define name-value pairs for the build environment |

### 2. File Extensions Tab

Defines file extensions for source, header, object, and library files used by the toolchain. The CTS uses these extensions to identify and process files correctly during testing.

### 3. Tools Tab

Configures the actual build tools:

| Tool | Configuration |
|------|--------------|
| **Compiler** | Path, executable name, and flags |
| **Linker** | Path, executable name, and flags (target or host) |
| **Archiver** | Path, executable name, and flags |

The Tools tab also includes the **Target vs. Host Linker** selection:
- **Target Linker**: Uses the embedded system's linker (reuses existing build infrastructure)
- **Host Linker**: Uses the development system's linker (preselected details)

### 4. Compiler Specific Tab

Handles compiler-specific functionality and configuration:

| Feature | Description |
|---------|------------|
| **Compiler-specific flags** | Flags unique to the chosen compiler |
| **Include path handling** | How the compiler resolves include directives |
| **Library path handling** | How the linker resolves library dependencies |
| **Architecture flags** | Target architecture specification |

### 5. Notes Tab

Free-text field for user notes about the toolchain configuration. This information is stored in the `.tcfg` file but does not affect testing behavior.

## SABRE Relevance

The CTS toolchain configuration concept directly influenced SABRE's approach to verification configuration:

| CTS `.tcfg` Concept | SABRE Equivalent |
|---------------------|-----------------|
| Toolchain file per compiler environment | Rule set per standard version |
| Compiler/linker/archiver paths | Not needed (static analysis, no compilation) |
| Target vs. host linker choice | Not applicable (source-level analysis) |
| File extension configuration | File pattern matching in rule definitions |
| Environment variables | CLI flags and environment config |

SABRE eliminated the need for toolchain configuration entirely because it performs AST-based static analysis rather than compile-and-link testing. This is one of the key simplifications SABRE achieved over the CTS model.

---

*Source: FACE CTS User Manual v1.0, Chapter 4 (Pages 40-52)*
