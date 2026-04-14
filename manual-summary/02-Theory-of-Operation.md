# Chapter 3: Theory of Operation

> Summary of FACE CTS User Manual, Chapter 3 (Pages 37-39)
>
> **Note**: The PDF's Chapter 3 covers theory of operation. Chapter 2 covers installation (see [03-Installation.md](03-Installation.md)).

## Link-Test Methodology

The CTS determines conformance through a **link-test methodology**: integrating targeted testing code with corresponding conformant test code. This is fundamentally a compile-and-link verification — not a runtime test.

### How It Works

For C, C++, and Ada code:

1. **User applications** are linked with FACE test interfaces
2. **Customer interface libraries** are linked against by FACE test applications
3. Test interfaces provide **all possible function calls, data types, and constants** available to the customer code
4. Test applications utilize **all possible function calls, data types, and constants** that should exist in the customer code
5. Test applications are compiled using the customer's header/spec files, then linked against:
   - The customer's code
   - Test libraries containing allowed function calls, data types, and constants

### Pass/Fail Criteria

| Result | Meaning |
|--------|---------|
| Compile + Link **pass** | Customer code is conformant with respect to the requirements tested |
| Compile or Link **fail** | Customer code is **not** conformant; errors are included in test output |

### Important Limitations

- The test determines conformance with respect to **function signature only**
- The test **neither proves nor disproves correctness of functionality**
- For abstract interfaces, the test only verifies that the abstract interface is **defined correctly** — not that the customer code implements it

## Two Linker Methods

### Target Linker Method

The user provides details about their build tools (compiler, linker, archiver paths and flags).

| Aspect | Detail |
|--------|--------|
| **Advantage** | Existing build infrastructure can be reused; conditionally compiled code based on hardware architecture is included |
| **Disadvantage** | Conformance testing authority must know target linker details |

### Host Linker Method

The user modifies their build system to use the host's build tools and recompiles.

| Aspect | Detail |
|--------|--------|
| **Advantage** | Usage details are preselected in the conformance tool |
| **Disadvantage** | Conditionally compiled code based on hardware architecture may not be included; build infrastructure must be modified |

## Special Cases

### OSS Testing Methodology
Unlike other segments, testing the OSS requires system libraries and include files. The user must specify the language standard but should **not** disable headers, built-in functions, and libraries.

### Java Testing Methodology
Java testing uses a different approach from C/C++/Ada since Java does not use a traditional compile-and-link model. The CTS uses class loading and reflection to verify conformance.

## SABRE Relevance

SABRE adopted the **verification-without-execution** concept from CTS's link-test methodology, but adapted it for software architecture verification:

| CTS Approach | SABRE Approach |
|-------------|---------------|
| Compile and link against test interfaces | Parse AST and match against rule patterns |
| Verify function signatures exist | Verify architectural patterns exist |
| Link-test (never executes) | Static analysis (never executes) |
| Binary-level verification | Source-level verification |

Both approaches share the key principle: **verify structure and interfaces without requiring runtime execution**.

---

*Source: FACE CTS User Manual v1.0, Chapter 3 (Pages 37-39)*
