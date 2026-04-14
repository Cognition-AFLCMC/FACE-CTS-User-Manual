# FACE CTS CLI Patterns

## CLI Workflow Patterns Adopted for Sabre

The FACE CTS User Manual documents a CLI workflow for running conformance tests. We adopted and extended these patterns for the Sabre Conformance Tool.

---

## FACE CTS CLI Workflow

```
1. Create project config (.pcfg)
   └─ Specifies target, edition, segment, profile

2. Run verification
   └─ face-cts verify --project mycomponent.pcfg

3. View results
   └─ Results in structured output (console + file)

4. Export report
   └─ face-cts report --format pdf --output results/
```

## Sabre CLI Workflow (Derived from FACE CTS)

```
1. Create/select rule set (rules/*.yaml)
   └─ Specifies standard version, categories, severity levels

2. Run verification
   └─ sabre verify ./nav_mpu --rules oms-v25

3. View results
   └─ Console output + JSON results file

4. Export report
   └─ sabre report --format pdf --output results/

5. (NEW) Deep analysis
   └─ sabre analyze ./nav_mpu --rules oms-v25 --devin

6. (NEW) Generate fixes
   └─ sabre fix ./nav_mpu --rules oms-v25 --output patches/
```

---

## CLI Command Reference (Sabre — Inspired by FACE CTS)

### Core Commands

```bash
# Verify a single MPU
sabre verify ./nav_mpu --rules oms-v25

# Verify with specific categories only
sabre verify ./nav_mpu --rules oms-v25 --categories interface,behavioral

# Verify with custom severity threshold
sabre verify ./nav_mpu --rules oms-v25 --min-severity HIGH

# Batch verify multiple MPUs
sabre verify ./mpu_submissions/ --rules oms-v25 --recursive

# Verify and generate report
sabre verify ./nav_mpu --rules oms-v25 --report pdf --output results/
```

### Analysis Commands (Beyond FACE CTS)

```bash
# Deep analysis with Devin
sabre analyze ./nav_mpu --rules oms-v25 --devin

# Ask natural language questions
sabre ask "Does nav_mpu properly implement the CAL API?" --mpu ./nav_mpu

# Generate fix patches
sabre fix ./nav_mpu --rules oms-v25 --output patches/

# Compare MPU against reference implementation
sabre compare ./nav_mpu --reference ./gold_standard/nav_ref
```

### Fleet Commands (Beyond FACE CTS)

```bash
# Verify all MPUs from a vendor
sabre fleet verify --vendor "Vendor-A" --rules oms-v25

# Fleet status dashboard
sabre fleet status

# Export fleet report
sabre fleet report --format pdf --output fleet_report.pdf
```

---

## Classification

All content is **UNCLASSIFIED**. Derived from publicly available FACE CTS User Manual patterns.
