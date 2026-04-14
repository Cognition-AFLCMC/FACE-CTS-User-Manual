# Chapter 5: Project Configuration File

> Summary of FACE CTS User Manual, Chapter 5 (Pages 53-66)

## Overview

The **Project Configuration File** (`.pcfg`) contains all UoC object paths, dependencies, and settings needed to accurately test a UoC for conformance. One `.pcfg` file is defined for every FACE UoC.

## Managing Project Files

The Project Files List is accessed via **File > Projects** in the CTS navigation bar.

| Action | Description |
|--------|------------|
| **New** | Opens a new file dialog to create an empty project |
| **Import** | File browser to find and add an existing `.pcfg` file |
| **Open** | Opens the selected project |
| **Remove** | Removes from list view (does not delete the file) |
| **Clone** | Creates a copy of the selected project |
| **Test Project** | Executes the test procedure on the selected project |

## Building a Project Configuration File

The Project Configuration Builder has six tabs:

### 1. General Tab

| Setting | Description |
|---------|------------|
| **Base Directory** | Root directory for all file paths in the project |
| **Segment** | PCS, PSS, TSS, IOS, or OSS |
| **Language** | Programming language of the UoC |
| **OSS Profile** | Profile the UoC reflects |
| **Operating Environment** | POSIX, ARINC653, or POSIX ARINC653 |
| **Toolchain Path** | Path to the `.tcfg` file |
| **UoC Name** | Name of the Unit of Conformance |

#### Operating Environment Options
- **POSIX**: Standard POSIX APIs
- **ARINC 653**: For UoCs providing or using ARINC 653 APIs
- **POSIX ARINC653**: Combined environment
- **Native Language**: Available for C++11/C++14 UoCs

### 2. Data Model Tab

| Setting | Description |
|---------|------------|
| **USM Path** | Path to the UoP Supplied Model file |
| **SDM Path** | Path to the Shared Data Model file |
| **UDDL Version** | Version of the Universal Domain Description Language |

The SDM is available for download from The Open Group's website.

### 3. Gold Standard Libraries Tab

| Setting | Description |
|---------|------------|
| **GSL Output Directory** | Where generated Gold Standard Libraries are placed |
| **Profile Selection** | Which profiles to generate GSLs for |

### 4. Objects/Libraries Tab

This is the most detailed tab, containing:

| Setting | Description |
|---------|------------|
| **UoC Object Files** | Compiled object files (.o, .obj) or source files |
| **UoC Header Files** | Header files (.h, .hpp) or spec files (.ads) |
| **Library Files** | Additional libraries the UoC depends on |
| **Factory Functions** | User-written implementation of FACE interfaces |
| **Include Paths** | Compiler include paths for the UoC |
| **Library Paths** | Linker library paths |

#### Configuration Subtab
- Defines specific test configurations
- Sets up injectable interfaces
- Configures inter-UoC dependency handling

### 5. Notes Tab

Free-text field for user notes about the project configuration. Stored in the `.pcfg` file but does not affect testing.

### 6. Project Info Tab

| Setting | Description |
|---------|------------|
| **Vendor Name** | Name of the UoC vendor |
| **Product Name** | Name of the product |
| **Version** | UoC version number |
| **Contact Information** | Vendor contact details |

This information appears in the generated conformance report.

## SABRE Relevance

The CTS `.pcfg` file was the primary inspiration for SABRE's rule and configuration design:

| CTS `.pcfg` Concept | SABRE Equivalent |
|---------------------|-----------------|
| One `.pcfg` per UoC | One `manifest.yaml` per MPU |
| Segment selection (PCS/PSSS/TSS/IOSS/OSS) | MPU category (Platform/Subsystem/Application) |
| Profile selection | Tier selection (Tier 1/2/3) |
| Object/library paths | Source code directory path |
| Factory Functions | Not needed (static analysis) |
| Vendor/product info in Project Info tab | MPU metadata in manifest |
| Base Directory for relative paths | `--mpu-path` CLI argument |

SABRE's YAML-based configuration is significantly simpler because static analysis doesn't require compilation artifacts, toolchain details, or Factory Functions.

---

*Source: FACE CTS User Manual v1.0, Chapter 5 (Pages 53-66)*
