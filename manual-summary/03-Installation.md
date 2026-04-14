# Chapter 2: Installation

> Summary of FACE CTS User Manual, Chapter 2 (Pages 12-36)

## Overview

The CTS installation chapter covers system requirements, prerequisites, and installation procedures for both Linux and Windows environments. The CTS is primarily written in Python and Java.

## Supported Platforms

### Linux (Tested Distributions)
- Red Hat Enterprise Linux (RHEL) 9.3
- Ubuntu 22.04
- Ubuntu 20.04

> **Note**: Given the CTS is primarily written in Python and Java, it is reasonable to believe it will perform well on other Linux distributions, though no definitive claims are made.

### Windows
- Windows 10
- Windows 11

## System Requirements

| Requirement | Detail |
|-------------|--------|
| Internet connection | Required for downloading prerequisites |
| Disk space (VM) | At least 15 GB allocated drive |
| Root/admin permission | Required for package installation |

## Required Dependencies

| Dependency | Version |
|-----------|---------|
| Python | 3.8 or greater (with pip) |
| Java | JDK 1.8 (with JavaFX support for GUI) |
| PDF viewer | Any |

## Language-Specific Prerequisites

| UoC Language | Required Tools |
|-------------|---------------|
| C | C compiler/linker (e.g., GCC 11.4) |
| C++ | C++ compiler/linker (e.g., GCC/G++ 11.4) |
| Ada | Ada compiler/linker (e.g., GCC GNAT 11.4) |
| Java | Java JDK 1.8, Ant 1.9.x, Qt 5 (for Java testing) |

### POSIX Libraries Required
- `libarchive-devel` (RHEL) / `libarchive-dev` (Ubuntu)
- `zlib-devel` (RHEL) / `zlib1g-dev` (Ubuntu)

## Installation on Linux

### 1. Install Prerequisites

**GCC/G++** (for C/C++/Ada projects):
```bash
# RHEL 9
sudo dnf install -y gcc gcc-c++ gcc-gnat

# Ubuntu 22
sudo apt install -y gcc g++ gnat
```

**Python 3**:
```bash
# RHEL 9
sudo dnf install -y python3 python3-pip

# Ubuntu 22
sudo apt install -y python3 python3-pip
```

**Java 8 (Azul Zulu OpenJDK with JavaFX)**:
- Standard OpenJDK does **not** include JavaFX, which the CTS GUI requires
- Azul Zulu OpenJDK with FX must be installed from the Azul repository

**Ant 1.9.x** (for Java UoC testing):
```bash
# RHEL 9
sudo dnf install -y ant

# Ubuntu 22
sudo apt install -y ant
```

**Qt 5** (Java testing only):
```bash
# RHEL 9
sudo dnf install qt5-devel

# Ubuntu 22
sudo apt install qtbase5-dev
```

> **Important**: Java testing cannot be performed on Ubuntu 20 because the JavaConformance executable depends on Qt 5.15, and Ubuntu 20's default Qt 5 package is version 5.12.

### 2. Install CTS

Extract the CTS archive (ZIP or tar.gz) to a folder with read/write/executable access.

### 3. Set Environment Variables

```bash
# RHEL 9
export JDK8_HOME=/usr/lib/jvm/zulu-fx-8/
export JAVA_HOME=$JDK8_HOME

# Ubuntu 20/22
export JDK8_HOME=/usr/lib/jvm/zulu-fx-8-amd64/
export JAVA_HOME=$JDK8_HOME
```

Add these to `~/.bashrc` for persistence.

### 4. Launch CTS

```bash
# Navigate to CTS installation directory
./run_CTS_GUI.py

# For verbose output
./run_CTS_GUI.py -v
```

## Installation on Windows

### Key Differences from Linux
- **MSYS2** required for C/C++/Ada projects (provides GCC/MinGW toolchain)
- **Long Paths** must be enabled in Windows 10/11
- Cygwin/GCC toolchains have specific installation variances
- Environment variables set through System Properties

### Windows-Specific Prerequisites
- MSYS2 (for C/C++/Ada samples only)
- Python 3 (via python.org installer)
- Java 8 OpenJDK with FX (Azul Zulu)
- Ant (manual download)

## Accommodating Older CTS Versions

For older CTS versions that require Python 2.7:
1. Create a Python 2.7 virtual environment
2. Configure the CTS to use the virtual environment
3. Set up Python environment variables for CTS usage

## SABRE Relevance

SABRE's installation is significantly simpler than FACE CTS because SABRE is a pure Python tool:

| CTS Installation | SABRE Installation |
|-----------------|-------------------|
| Python + Java + C/C++ compilers + Qt5 | Python 3.11 only |
| Platform-specific linkers required | No compilation tools needed |
| GUI requires JavaFX | Web dashboard (Flask) |
| 15 GB minimum disk space | < 500 MB |
| Complex environment variable setup | `pip install -e .` |

This simplicity was a deliberate design choice: SABRE performs static analysis rather than link-level testing, eliminating the need for language-specific compilers.

---

*Source: FACE CTS User Manual v1.0, Chapter 2 (Pages 12-36)*
