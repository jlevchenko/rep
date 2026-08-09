# rep-4.1

**REP 4.1 Outcome Analysis -- Multiple Imputation, ANCOVA, Robust SEs, and Manuscript Review Items**

This repository contains the R notebook and derived outputs for the REP
(Resilience Education Program) 4.1 randomized trial outcome analysis. The
study compares REP and Control conditions on student, teacher, and caregiver
reported mental health and social outcomes at pre- and post-test, using
multiply imputed data.

---

## Study Overview

**Design:** Two-condition randomized trial (REP vs. Control), pre/post
measurement, with primary analyses run as ANCOVA (post-test outcome ~
pre-test covariate + condition) on multiply imputed variables.

**Outcome domains:**

| Reporter | Measures |
|----------|----------|
| Student | BFS (internalizing, externalizing, total), YSR (anxious/depressed, withdrawn/depressed, somatic complaints, social problems, internalizing problems), SRCS, PCOISS |
| Caregiver | CASSS (caregiver) |
| Teacher | CASSS (teacher), BESS (ERI, IRI, ARI, BERI) |

**Analytic steps covered in the notebook:**

1. Data read-in, type coercion, and SRCS scoring
2. Descriptive summary of raw analysis variables
3. Variable-by-variable multiple imputation (custom `mice`-based pipeline with deterministic predictor-set selection and RMSE-based model choice)
4. ANCOVA models for each outcome, with Cohen's d
5. Regression assumption checks
6. Re-fit of all ANCOVA models with HC3 robust standard errors
7. Manuscript review items requested by Steve and Kierin: teacher/caregiver missingness, MAR evidence, MI demographic predictor audit, and Table 2 descriptives from the final imputed dataset
8. Supplemental checks: BESS missingness pattern, natural (non-imputed) BFS data by week, percent missing by site, and entries per child
9. Path (mediation) analysis: condition -> mediator (CASSS caregiver/teacher, PCOISS) -> outcome, with bootstrapped indirect effects

---

## Repository Structure

```
rep-4.1/
├── REP_4_1_Analyses_Updated.Rmd          # Full analysis notebook (source)
├── REP_4_1_Analyses_Updated_nb.html      # Rendered HTML notebook (knit output)
├── data/
│   └── Master_REP4.1.xlsx                # Primary trial dataset (not included, see note)
└── outputs/
    ├── REP4.1_missingness_by_variable.csv
    ├── REP4.1_missingness_by_reporter_time.csv
    ├── REP4.1_teacher_missingness_prepost.csv
    ├── REP4.1_caregiver_parent_missingness_candidates.csv
    ├── REP4.1_missingness_association_tests.csv
    ├── REP4.1_mar_evidence_summary.csv
    ├── REP4.1_mi_demographic_predictor_audit.csv
    ├── REP4.1_Table2_imputed_M_SD_long.csv
    └── REP4.1_Table2_imputed_M_SD_wide.csv
```

> **Note on source data:** The primary trial dataset (`Master_REP4.1.xlsx`) is
> not included in this repository due to IRB restrictions and data use
> agreements. Update the `read_excel()` path in the first data-read chunk of
> `REP_4_1_Analyses_Updated.Rmd` to point to your local copy.

---

## Notebook: `REP_4_1_Analyses_Updated.Rmd`

**Author:** Original author Kierin Barnett, modified by Juliana Levchenko.

A single knit-to-HTML notebook (also configured for Word and PDF output) that
runs the full outcome analysis pipeline top to bottom. Section headers match
the table of contents produced on knit:

- **Data Manipulation** -- SRCS scoring, type coercion, factor baselines (`condition` reference level `Control`)
- **Data Summary** -- six-number summary for all raw pre/post analysis variables
- **Multiple Imputation** -- one sub-section per outcome variable (BFS, YSR, PCOISS, SRCS, CASSS, BESS). Each uses a deterministic, seeded pipeline: candidate predictors are assembled, expanded into 1-, 2-, and 3-way predictor sets, evaluated by masked-data RMSE, and the best set is used for a final `mice` run. Only NAs in the original variable are replaced with imputed values.
- **Analysis** -- ANCOVA (`lm(post ~ pre + condition)`) for every outcome, with Cohen's d, full model summaries, and a tidy results table
- **Regression Assumption Checks**
- **Robust Standard Errors** -- same ANCOVA models re-fit with HC3 robust SEs (`sandwich` + `lmtest`) for the manuscript
- **Manuscript Review Items** -- responses to specific reviewer (Steve/Kierin) comments, described below
- **BESS Pattern of Missingness (Supplemental)**
- **Natural Data by Week -- BFS**
- **Percentage Missing Data by Site**
- **Number of Entries by Child**
- **Path Analysis** -- `lavaan`-based mediation models (condition -> mediator -> outcome) for combinations of BFS Total, teacher BESS IRI/ERI, BFS Externalizing, and YSR Social Problems as outcomes, with CASSS caregiver, CASSS teacher, and PCOISS as mediators, using a "post, controlling for pre" ANCOVA-style specification and bootstrapped indirect effect CIs

**Reproducibility:** the notebook sets a global seed (`20251113L` for the
analysis chunks; deterministic sub-seeds for each imputation chunk), fixes
RNG algorithms, and locks locale/timezone in the setup chunk, so re-knitting
against the same source data reproduces identical imputed values and results.

---

## Manuscript Review Outputs

The "Manuscript Review Items" section was added specifically to answer
outstanding comments from Steve Kilgus and Kierin Barnett on the REP 4.1
manuscript. Each output below is written directly from that section of the
notebook.

| File | Addresses | Contents |
|------|-----------|----------|
| `REP4.1_missingness_by_variable.csv` | General missingness reporting | N, observed N, missing N, and % missing for every raw pre/post outcome, by reporter (student/teacher) and construct |
| `REP4.1_missingness_by_reporter_time.csv` | Summary of the above | Missing % range, mean, and SD collapsed to reporter x timepoint |
| `REP4.1_teacher_missingness_prepost.csv` | "What about teacher measures?" | Teacher-reported BESS missingness only (pre and post), the specific table requested |
| `REP4.1_caregiver_parent_missingness_candidates.csv` | Caregiver/parent measure missingness | Missingness for all caregiver/parent-pattern columns in the master data, including ones not retained in the final ANCOVA models |
| `REP4.1_missingness_association_tests.csv` | MAR evidence | Logistic regression, chi-square, or Fisher exact tests of whether missingness on each outcome is associated with each observed predictor (gender, condition, other outcomes) |
| `REP4.1_mar_evidence_summary.csv` | MAR evidence, summarized | Per-outcome count of predictors tested, number of significant associations, minimum p-value, and a plain-language interpretation; also includes Little's MCAR test |
| `REP4.1_mi_demographic_predictor_audit.csv` | "Was gender the only demographic predictor in MI?" | Every demographic/design-pattern variable in the data, classified as included demographic predictor, included design predictor, or not included in the MI predictor pools |
| `REP4.1_Table2_imputed_M_SD_long.csv` | Manuscript Table 2 | Long-format N, mean, SD, and formatted M (SD) for every outcome x condition x timepoint, computed from the final imputed dataset |
| `REP4.1_Table2_imputed_M_SD_wide.csv` | Manuscript Table 2 | Same data pivoted to match the manuscript layout: REP Pre/Post and Control Pre/Post M (SD) columns per outcome |

---

## Usage

1. Clone the repository
2. Update the `read_excel()` path in `REP_4_1_Analyses_Updated.Rmd` to point to your local copy of `Master_REP4.1.xlsx`
3. Knit the Rmd to HTML (or open as a notebook in RStudio and run all chunks); imputation chunks are seeded and deterministic, so results should match `REP_4_1_Analyses_Updated_nb.html`
4. CSV outputs are written to `outputs/` automatically by the manuscript review chunks

**R packages required:**
```r
install.packages(c("here", "readxl", "tidyverse", "dplyr", "lubridate",
                   "tidyr", "knitr", "ggplot2", "mice", "gridExtra", "grid",
                   "broom", "sandwich", "lmtest", "naniar", "lavaan",
                   "stringr", "purrr", "tibble"))
```

---

## Related Repositories

- [`rep`](https://github.com/jlevchenko) -- REP 3.1 component analysis (fidelity, descriptives, outcome models)
- [`power-analysis`](https://github.com/jlevchenko) -- NIMH grant power analysis
- [`interim-performance-reports`](https://github.com/jlevchenko) -- IPR data aggregation and GPRA tier assignment
