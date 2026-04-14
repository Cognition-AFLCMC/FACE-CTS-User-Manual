# Chapter 7: Segment-Specific Test Procedures

> Summary of FACE CTS User Manual, Sections 7.2-7.6, 7.8-7.9 (Pages 71-127)

## Overview

Chapter 7 provides detailed, segment-specific test procedures for each FACE UoC type. While the overall workflow is consistent (configure → generate GSLs → write Factory Functions → test), each segment has unique requirements and configuration options.

## PCS — Portable Components Segment (§7.2)

**Pages 71-81**

### What the User Must Provide
- Project's object files (C/C++/Ada) or class/jar files (Java)
- Project's header files (C/C++) or spec files (Ada)
- Project's USM (UoP Supplied Model)
- Project's toolchain file (`.tcfg`)

### General Tab Configuration
| Setting | Value |
|---------|-------|
| Segment | **PCS** |
| Language | Language of the UoC |
| OSS Profile | Not applicable for PCS |
| Operating Environment | POSIX, ARINC653, or POSIX ARINC653 |

### Key PCS Characteristics
- PCS UoCs are the most portable — they must work across different platform implementations
- Interfaces are defined against the Gold Standard Libraries
- Factory Functions implement each interface the UoC provides

---

## PSSS — Platform Specific Services Segment (§7.3)

**Pages 82-92**

### What the User Must Provide
- Same as PCS, plus:
- Specification of the OSS profile the UoC targets
- Graphics API selection (OpenGL/Vulkan) if applicable

### General Tab Configuration
| Setting | Value |
|---------|-------|
| Segment | **PSS** |
| Language | Language of the UoC |
| OSS Profile | Required — select the target profile |
| Operating Environment | POSIX, ARINC653, or POSIX ARINC653 |
| POSIX Multi-Process APIs | Enable if required |
| ARINC653 Version | Select if ARINC653 environment chosen |
| Graphics API | OpenGL/Vulkan dropdown (if UoC uses graphics) |

### Key PSSS Characteristics
- Platform-specific — tied to a particular OSS profile
- May include graphics API calls (OpenGL/Vulkan)
- If the UoC encodes/decodes video using Vulkan extensions, the toolchain must include the macros from FACE Technical Standard Table 13

---

## TSS — Transport Services Segment (§7.4)

**Pages 93-104**

### What the User Must Provide
- Project's object files or class/jar files
- Project's header files or spec files
- Project's USM
- Project's toolchain file

### General Tab Configuration
| Setting | Value |
|---------|-------|
| Segment | **TSS** |
| Language | Language of the UoC |
| Operating Environment | As appropriate |

### Key TSS Characteristics
- TSS UoCs handle data transport between other segments
- Testing includes **special verification cases** for transport-specific interfaces
- TSS Type Specific and LCM Stateful interfaces require USM/SDM validation

---

## IOSS — I/O Services Segment (§7.5)

**Pages 104-114**

### What the User Must Provide
- Project's object files or class/jar files
- Project's header files or spec files
- Project's USM
- Project's toolchain file

### General Tab Configuration
| Setting | Value |
|---------|-------|
| Segment | **IOS** |
| Language | Language of the UoC |
| Operating Environment | As appropriate |

### Key IOSS Characteristics
- IOSS UoCs provide I/O services (sensor interfaces, hardware abstraction)
- Configuration includes I/O-specific interface selections
- Testing verifies correct I/O interface definitions

---

## OSS — Operating System Segment (§7.6)

**Pages 115-122**

### What the User Must Provide
- The OS's include path
- The target OS object files

> **Note**: OSS testing has significantly different requirements from other segments.

### General Tab Configuration
| Setting | Value |
|---------|-------|
| Segment | **OSS** |
| Language | Language of the UoC |
| Profile | Select applicable OS profiles |

### Key OSS Characteristics
- **Fundamentally different** from other segments:
  - Requires system libraries and include files
  - User should specify the language standard but should NOT disable headers, built-in functions, or libraries
  - Tests verify that the OS provides the required API calls based on the selected profile
- Result is pass/fail based on whether the OS supplies the necessary calls for the profile

### OSS Testing Methodology
Unlike other segments where user code is linked against GSLs, OSS testing verifies that the operating system itself provides the required POSIX/ARINC 653 APIs.

---

## Language-Specific Considerations

### Ada Segments (§7.8)
- Ada Runtime Libraries are allowed but must only use standard POSIX calls
- If the Runtime Library is packaged with the UoC, it must comply with profile restrictions
- Ada Runtime Library must be included as part of object/library files
- The correct Gold Standard POSIX library must be compiled and included

### Java Segments (§7.9)
- Java testing uses a different methodology from C/C++/Ada
- Class loading and reflection are used instead of link-level testing
- Requires Qt 5.15 (not available on Ubuntu 20)
- Ant 1.9.x required for build management

## SABRE Relevance

SABRE's test categories were directly inspired by CTS segment organization:

| CTS Segment | SABRE Category | OMS/UCI Source |
|-------------|---------------|---------------|
| IOSS (I/O Services) | Interface Compliance | OMS §5.3 (CAL API) |
| PCS (Portable Components) | Structural Compliance | OMS §4.1 (Service Model) |
| PSSS (Platform Specific) | Behavioral Compliance | OMS §6.1 (Lifecycle) |
| TSS (Transport) | Documentation Compliance | OMSC-TMP-003 (Service Contract) |
| OSS (Operating System) | Platform Verification | OMS §5.2 (OS Façade) |

While CTS segments map to FACE architecture layers, SABRE categories map to verification concerns — but the organizational principle of **"group tests by domain"** came directly from CTS.

---

*Source: FACE CTS User Manual v1.0, Chapter 7 (Pages 71-127)*
