# Project Configuration Pattern

## From FACE CTS `.pcfg` to Sabre YAML Rules

---

## The Evolution

FACE CTS uses `.pcfg` (Project Configuration) files to define verification parameters. We evolved this concept into YAML rule files that are more flexible, version-controllable, and human-readable.

### FACE CTS Project Config (.pcfg)

```
# FACE CTS .pcfg format (simplified)
[project]
name = MyNavigationComponent
edition = 3.1
segment = PSS
profile = General_Purpose
target_path = /path/to/uop/source
output_path = /path/to/results
```

### Sabre Rule Set (YAML)

```yaml
# rules/oms-v25.yaml
metadata:
  name: OMS v2.5 Conformance Rules
  version: 2.5.0
  source: OMS Compliance Checklists (OMSC-CHK-001 through OMSC-CHK-006)
  generated_from: 
    - OMS_Compliance_Checklist_Core.xlsx
    - OMS_Compliance_Checklist_Messaging.xlsx
    - UCI_Schema_v2_5.xsd

categories:
  interface:
    description: "Interface compliance checks (CAL API, messaging)"
    severity_default: CRITICAL
    
  behavioral:
    description: "Behavioral compliance checks (lifecycle, state machine)"
    severity_default: HIGH
    
  documentation:
    description: "Documentation compliance checks (service contracts)"
    severity_default: MEDIUM
    
  structural:
    description: "Structural compliance checks (modularity, dependencies)"
    severity_default: HIGH

tier_thresholds:
  1: { min_score: 90, critical_allowed: 0 }
  2: { min_score: 85, critical_allowed: 0 }
  3: { min_score: 80, critical_allowed: 0 }

rules:
  - id: OMS-CAL-001
    category: interface
    severity: CRITICAL
    description: "All messaging must use CAL API"
    check_type: ast_pattern
    pattern: "import.*cal_api|from.*cal.*import"
    standard_ref: "OMS v2.5 §5.3"
    
  - id: OMS-LCM-001
    category: behavioral
    severity: CRITICAL
    description: "MPU must implement all 5 lifecycle states"
    check_type: state_machine
    required_states: [DORMANT, STARTING, RUNNING, STOPPING, STOPPED]
    standard_ref: "OMS v2.5 §6.1"
    
  # ... 102 more rules derived from OMS checklists
```

### Key Improvements Over .pcfg

```
┌─────────────────┬────────────────────────┬──────────────────────┐
│ Aspect           │ FACE CTS (.pcfg)       │ Sabre (YAML)         │
├─────────────────┼────────────────────────┼──────────────────────┤
│ Format           │ INI-style              │ YAML (human-readable)│
│ Version Control  │ Not designed for VCS   │ Git-native           │
│ Rule Definition  │ Hardcoded in tool      │ Declarative in YAML  │
│ Extensibility    │ New edition = new tool │ New YAML = new rules │
│ Swappability     │ One edition at a time  │ Swap rules at runtime│
│ Transparency     │ Rules inside binary    │ Rules are inspectable│
│ Customization    │ Limited               │ Custom rules welcome │
│ Diff-ability     │ Hard to diff          │ Standard YAML diff   │
└─────────────────┴────────────────────────┴──────────────────────┘
```

### Demo Moment: "Swap the Rules"

The YAML rule architecture enables our killer demo moment:

```
Step 1: sabre verify ./nav_mpu --rules oms-v25
        → 95/95 checks pass (OMS v2.5 conformant)

Step 2: sabre verify ./nav_mpu --rules ams-gra-stub
        → 87/102 checks pass (AMS GRA has additional requirements)

Step 3: "Same engine, different rules, different results.
         When the reference architecture evolves, our tool adapts."
```

This is impossible with FACE CTS's hardcoded approach.

---

## Classification

All content is **UNCLASSIFIED**.
