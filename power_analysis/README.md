# power-analysis

**REP Grant Proposal -- Power Analysis Technical Appendix**  
*Resilience Education Program (REP) | NIMH R01 Submission*

This repository contains the full reproducible power analysis submitted as a
technical appendix to a NIMH grant proposal for the REP randomized controlled
trial. The analysis uses **Cohen's d = 0.55** as the benchmark effect size,
derived from pilot REP data.

---

## Study Design

The REP RCT tests a school-based intervention targeting student resilience and
mental health outcomes. The design is a **clustered RCT** with students nested
within schools, requiring design-effect adjustments to standard sample size
calculations.

| Parameter | Value | Source |
|-----------|-------|--------|
| Benchmark effect size | d = 0.55 | REP pilot data |
| ICC | 0.07 | Prior school-based intervention literature |
| Average cluster size | 9 students | Anticipated enrollment |
| Design effect (DE) | 1.56 | ICC × cluster size formula |
| Planned N | 163 | Achieves ≥80% power at d = 0.55, R² = 0.30 |
| Target power | 80% | NIH standard |
| Alpha | 0.05 | Two-tailed |

---

## Repository Structure

```
power-analysis/
├── REP_Power_Analysis.Rmd          # Main analysis notebook
├── data/
│   ├── PowerCurve_Schools.csv           # Power curve data: schools
│   ├── PowerCurve_Within.csv            # Power curve data: within-cell
│   └── PowerCurve_Schools_LRT.csv       # Power curve data: LRT
├── figures/
│   ├── PowerCurve_Schools_Workshop_White.png   # Power vs number of schools
│   ├── PowerCurve_Within_Workshop_White.png    # Power vs students per cell
│   ├── Table_B0_Helper_Functions.png
│   ├── Table_B1_RequiredN_Benchmark.png
│   ├── Table_B2_Sensitivity_Analysis.png
│   ├── Table_B3_Moderation_Power.png
│   └── Table_B4_Mediation_Power.png
├── tables/
│   ├── REP_Power_Tables.docx     # All tables in Word (APA formatted)
│   ├── REP_Power_Tables.xlsx     # All tables in Excel
│   └── Table_B2_MDE.docx         # MDE table (standalone)
└── README.md
```

> **Data:** All analyses use **simulated data** generated within R. No external
> participant data are required. The notebook is fully self-contained and
> reproducible from scratch.

---

## Analyses

### Table B0 -- Helper Functions
Documents the five core functions used throughout:
`design_effect()`, `d_adjust()`, `required_N()`, `power_at_N()`, `MDE_at_N()`

### Table B1 -- Required Sample Sizes
Total N to achieve 80% power at d = 0.55, across R² ∈ {0, 0.30, 0.50} and
two clustering scenarios (no DE vs. DE = 1.56).

### Table B2 -- Sensitivity Analysis for N = 163
Achieved power and MDE for the planned N = 163 under the DE = 1.56 design,
across R² levels. Also includes MDE by sample size (N = 81 vs. 162).

### Table B3 -- Moderation Power
Monte Carlo simulation (n = 200) estimating power to detect a
**Treatment × Moderator interaction** (β = 0.55) in a multilevel model with
random school intercepts. Uses `simr::powerSim()`.

Power curves show how the interaction effect's detectability scales with:
- Number of schools (8 to 32)
- Students per school × treatment × moderator cell (5 to 20)

### Table B4 -- Mediation Power
Bootstrap-based simulation (300 replications × 1,000 bootstraps) estimating
power to detect the **indirect effect** (a × b = 0.105) via the `mediation`
package. Power = proportion of runs where the 95% bootstrap CI excludes zero.

### Analytic Power Curve
`pwr::pwr.t.test()` power curve across d = 0.20 to 0.90 for the effective N
after design-effect adjustment, with the d = 0.55 benchmark annotated.

---

## Key Findings

| Analysis | Result |
|----------|--------|
| Required N (d = 0.55, R² = 0.30, DE = 1.56) | N = 104 |
| Required N (d = 0.55, R² = 0.00, DE = 1.56) | N = 148 |
| Power at N = 163, R² = 0.30 | ~88% |
| MDE at N = 163, R² = 0.30 | d ≈ 0.48 |
| Moderation power (β_interaction = 0.55) | ~80% |
| Mediation power (a × b = 0.105) | ~75% |

---

## Reproducibility

The notebook uses `renv` for package management. To reproduce:

```r
# Install renv if needed
install.packages("renv")

# Restore the exact package versions used
renv::restore()

# Open and knit the notebook
rmarkdown::render("REP_Power_Analysis.Rmd")
```

**R packages required:** `pwr`, `dplyr`, `tibble`, `tidyr`, `purrr`, `ggplot2`,
`scales`, `knitr`, `lme4`, `simr`, `mediation`, `officer`, `flextable`,
`writexl`, `gt`, `webshot2`

```r
install.packages(c("pwr", "dplyr", "tibble", "tidyr", "purrr", "ggplot2",
                   "scales", "knitr", "lme4", "simr", "mediation",
                   "officer", "flextable", "writexl", "gt", "webshot2"))
```

> **Note on simulation runtime:** The moderation power curves (`simr::powerCurve`,
> 3 × 200 simulations) and mediation simulation (300 × 1,000 bootstraps) take
> approximately 15--30 minutes on a standard laptop. Results are stochastic but
> reproducible via `set.seed(1234)`.

---

## Files Removed from Original Archive

| File | Reason |
|------|--------|
| `NIMH Grant Proposal Power Analysis.Rmd` (Oct 6) | Superseded by Effect Size 0.55 version |
| Both `.nb.html` rendered outputs | Not source files |
| `NIMH REP - Research Strategy FINAL_JL.docx` (Oct 6) | Superseded by Oct 13 version |
| `Power Curve.png` (Oct 6, 641 KB) | Superseded by Workshop White versions |
| `PowerCurve_Schools.png`, `PowerCurve_Within.png` (Oct 6) | Superseded by Workshop White versions |
| `Table_B1_YSR_RequiredN.png` through `Table_B6_Mediation_Power.png` (Oct 6) | Old naming scheme, superseded by Oct 9 versions |

---

## Related Repositories

- [`rep`](https://github.com/jlevchenko) -- REP 3.1 component analysis (outcome analyses)
- [`interim-performance-reports`](https://github.com/jlevchenko) -- IPR GPRA aggregation
- [`metrics`](https://github.com/jlevchenko) -- APR region assignment and variable extraction
