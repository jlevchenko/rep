# rep

**REP 3.1 Component Analysis -- Fidelity, Descriptive Statistics, and Outcome Models**

This repository contains R notebooks for the REP (Resilience Education Program)
3.1 component analysis study. The study examines the effects of three intervention
components -- CICO, CBI, and Resilient Families (RF) -- on student mental health
and social outcomes across five participating schools.

---

## Study Overview

**Design:** Randomized component analysis with three conditions:

| Condition | Components |
|-----------|-----------|
| Condition 1 | CBI + CICO |
| Condition 2 | CBI + RF |
| Condition 3 | CBI + CICO + RF |

**Primary outcomes:** BESST (teacher/student empathy), BFS (behavior and feelings),
YSR (youth self-report internalizing), PCOISS (parental confidence), CASSS
(social support from classmates and teachers), SRCS (school readiness composite)

**Fidelity components:** CICO (AM, Daytime, PM sessions), CBI (curriculum-based
instruction), RF (Resilient Families meetings 1-3)

---

## Repository Structure

```
rep/
├── 01_Fidelity_Measures.Rmd         # CICO, CBI, and RF fidelity + exposure analysis
├── 02_REP_Analyses.Rmd              # Descriptives, imputation, ANCOVA, effect sizes
├── data/
│   ├── REP2_Synthetic.xlsx          # Anonymized fidelity observation data
│   └── rep_exposure.xlsx            # Fidelity exposure output (generated)
├── outputs/
│   ├── cohens_d_all_coefficients.csv          # Cohen's d for all ANCOVA coefficients
│   ├── REP_Fidelity_Measures.xlsx             # Fidelity metrics output (generated)
│   ├── Descriptive_Statistics_Report.docx     # Overall descriptive statistics
│   ├── Descriptive_Statistics_Report_Condition.docx  # Descriptives by condition
│   └── Descriptive_Statistics_Report_Condition_Post.docx
├── figures/
│   └── mean_scores_by_time_with_error_bars.png
└── README.md
```

> **Note on source data:** The primary student-level analysis dataset
> (`REP_3.1_Data_Spreadsheet.xlsx`) is not included in this repository due to
> IRB restrictions and data use agreements. Update the `excel_file` path in
> `02_REP_Analyses.Rmd` to point to your local copy. The fidelity observation
> data (`REP2_Synthetic.xlsx`) is included with all identifying information
> removed.

---

## Anonymization

All personally identifiable information has been removed or replaced:

| Original | Anonymized |
|----------|-----------|
| Black Hawk, Gompers, Olson, Schenk, Spring Harbor | School_A through School_E |
| ~70 implementer names | Implementer_A through Implementer_AJ |
| ~30 observer names | Observer_A through Observer_L |
| Student names | Student initials were already pseudonymous; retained as-is |

The same mapping dictionary is applied consistently across both the Excel data
file and the Rmd code, so school labels match across all analyses.

---

## Notebooks

### `01_Fidelity_Measures.Rmd`

Processes direct observation fidelity data from `REP2_Synthetic.xlsx` and computes
adherence and quality metrics for each intervention component.

**Key improvements over original:**

- A single `calculate_fidelity()` function replaces the copy-pasted `calculate_metrics()` 
  that was redefined separately in every section (AM, DT, PM, CBI, RF). The new
  version accepts an `adj_cols` argument for column-level opportunity adjustments
  instead of hardcoded index ranges.
- A single `compute_exposure()` function replaces six separate exposure calculation
  blocks.
- Sheet names updated to match anonymized school codes in `REP2_Synthetic.xlsx`.
- School name recoding applied to the CBI sheet at load time.
- `|>` native pipe used throughout; `%>%` removed.
- The RF Meeting 3 trailing-column drop is now explicit (`select(-last_col())`).

### `02_REP_Analyses.Rmd`

Primary outcome analyses from the student-level dataset.

**Key improvements over original:**

- School name anonymization applied immediately after import via a named
  `school_map` vector and `recode()` -- no real school names appear anywhere
  in the code or outputs.
- The file path is clearly marked with a comment directing users to update it.
- The `split_srcs()` SRCS parser is a clean function applied once rather than
  duplicated for pre and post.
- Multiple imputation is refactored: `build_sets()` generates predictor combinations,
  `run_rmse()` evaluates them, and `impute_var()` wraps the full pipeline. This
  replaces ~500 lines of repeated per-variable boilerplate.
- ANCOVA models are fit via `run_ancova()` using `lapply()` over `dv_list`,
  with a `tryCatch()` wrapper so a single model failure does not abort the notebook.
- Cohen's d computation is consolidated into the model-fitting step; no separate
  loop needed.
- Plots use `geom_errorbar()` with SE rather than the earlier version without
  error bars; saved directly to `figures/`.
- Word document exports go to `outputs/` with relative paths.

---

## Files Removed from Original Archive

| File | Reason |
|------|--------|
| `REP Analyses.Rmd` (Oct 2024) | Superseded by Jan 2026 version |
| `REP Analyses Updated.Rmd` (Jun 2025) | Superseded by Jan 2026 version |
| `Fidelity Code 2`, `Fidelity Code 2.0` | Exact duplicates of `Fidelity Code.Rmd` |
| `Fidelity Code.Rmd` | Superseded by `temp.Rmd` (Jan 2025) |
| `REP2.xlsx` | Superseded by `REP2_updated.xlsx` (Jan 2025) |
| `REP 3.1 Fidelity Notes_11.12.24.xlsx` | Superseded by Nov 13 version |
| `mean_scores_plot.png` | Older figure without error bars |
| `mean_scores_by_time.png` | Superseded by version with error bars |
| All `.nb.html` / `.html` rendered outputs | Not source files |
| `Test_Document.docx` | Test file |
| `REP_Descriptive_Statistics.docx` (Sep 2024) | Superseded by Jan 2026 version |
| `Descriptive_Statistics_Report_Condition_Post.docx` (Oct 2024) | Superseded |
| `3.1 Fidelity Measures` (no extension) | Unknown file type |
| `regression_results_kable.html` | Rendered output |
| `temp.Rmd` | Renamed to `01_Fidelity_Measures.Rmd` |

---

## Usage

1. Clone the repository
2. For `02_REP_Analyses.Rmd`: update the `excel_file` path in the Setup chunk
   to point to your local copy of the primary dataset
3. Open either `.Rmd` file in RStudio and run all chunks or knit to HTML

**R packages required:**
```r
install.packages(c("readxl", "tidyverse", "ggplot2", "stringr", "purrr",
                   "mice", "corrr", "knitr", "broom", "kableExtra",
                   "lubridate", "survey", "officer", "emmeans", "writexl"))
```

---

## Related Repositories

- [`interim-performance-reports`](https://github.com/jlevchenko) -- IPR data aggregation and GPRA tier assignment
- [`metrics`](https://github.com/jlevchenko) -- APR region assignment and raw variable extraction
- [`ipr-registration`](https://github.com/jlevchenko) -- IPR webinar registrant matching pipeline
