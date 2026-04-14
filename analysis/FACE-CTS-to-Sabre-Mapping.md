# FACE CTS to Sabre Feature Mapping

## Detailed Feature-by-Feature Comparison

| FACE CTS Feature | Sabre Equivalent | Enhancement |
|-----------------|------------------|-------------|
| `.pcfg` project config | `rules/*.yaml` rule sets | YAML is more readable, version-controlled |
| Segment-based test selection | Category-based rule filtering | More granular (6 categories vs 4 segments) |
| Gold Standard Library | Synthetic MPU portfolio | Hybrid: open-source + custom-built |
| CLI verification | `sabre verify` CLI | Same pattern, extended with fleet commands |
| PDF report generation | PDF + JSON + Dashboard | Machine-readable output + live dashboard |
| Single UoP verification | Fleet-wide verification | Multi-vendor, multi-MPU dashboard |
| Static analysis only | Static + AI-powered analysis | Devin deep scan for semantic issues |
| Report-only output | Report + change packages | Auto-generated fix PRs |
| No IDE integration | Windsurf real-time linting | Conformance feedback while coding |
| No CI/CD integration | GitHub Actions workflow | Blocks non-conformant PRs |
| No NL interface | "Ask Devin" panel | Natural language queries about conformance |
| Edition-based versioning | Rule set versioning | Swap between OMS v2.5 / AMS GRA |

---

## Architecture Comparison

```
FACE CTS Architecture              SABRE Architecture
─────────────────────              ──────────────────

┌──────────────┐                   ┌──────────────┐
│ .pcfg Config │                   │ YAML Rules   │
└──────┬───────┘                   └──────┬───────┘
       │                                  │
┌──────▼───────┐                   ┌──────▼───────┐
│ Test Select  │                   │ Rule Select  │
└──────┬───────┘                   └──────┬───────┘
       │                                  │
┌──────▼───────┐                   ┌──────▼───────┐
│ Test Execute │                   │ Verify Engine│
│ (static only)│                   │ (static + AI)│
└──────┬───────┘                   └──────┬───────┘
       │                                  │
┌──────▼───────┐                   ┌──────▼───────┐
│ PDF Report   │                   │ Report +     │
│              │                   │ Dashboard +  │
│              │                   │ Fix Gen +    │
│              │                   │ Ask Devin    │
└──────────────┘                   └──────────────┘
```

## Classification

All content is **UNCLASSIFIED**.
