# rep

**Resilience Education Program (REP) -- Fidelity, Outcome, and Power Analyses**

This repository contains the analysis pipelines for the Resilience Education
Program (REP), a school-based Tier 2 intervention for students at risk of
internalizing problems, developed and studied through the School Mental
Health Collaborative at WCER, UW-Madison. It covers two study phases (REP
3.1 and REP 4.1) plus the power analysis submitted as a technical appendix
to a subsequent NIMH grant proposal.

---

## Repository Structure

```
rep/
├── 3.1/                  # REP 3.1 component analysis (fidelity, descriptives, outcome models)
│   ├── 01_Fidelity_Measures.Rmd
│   ├── 02_REP_Analyses.Rmd
│   ├── data/
│   ├── figures/
│   ├── output/
│   └── README.md
├── 4.1/                  # REP 4.1 outcome analysis (multiple imputation, ANCOVA, robust SEs)
│   ├── REP 4.1 Analyses Updated.Rmd
│   ├── REP 4.1 Analyses Updated.nb.html
│   ├── outputs/
│   └── README.md
├── power_analysis/       # NIMH R01 power analysis technical appendix
│   ├── REP_Power_Analysis.Rmd
│   ├── data/
│   ├── figures/
│   ├── tables/
│   └── README.md
└── README.md
```

Each subfolder is self-contained with its own data, outputs, and README.
See the sub-folder READMEs for full analytic detail; this top-level README
gives the overview and how the pieces relate.

---

## Study Phases

### `3.1/` -- REP 3.1 Component Analysis

Examines the effects of three intervention components -- CICO, CBI, and
Resilient Families (RF) -- on student mental health and social outcomes
across five participating schools, using a randomized component analysis
design (three conditions combining the components). Outcomes include BESST,
BFS, YSR, PCOISS, CASSS, and SRCS. Includes fidelity measurement (CICO, CBI,
RF sessions) alongside the outcome models.

### `4.1/` -- REP 4.1 Outcome Analysis

A two-condition randomized trial (REP vs. Control) comparing student,
teacher, and caregiver reported outcomes (BFS, YSR, SRCS, PCOISS, CASSS,
BESS) at pre- and post-test. Primary analyses are ANCOVA models on
multiply imputed data, re-fit with HC3 robust standard errors, plus a set
of manuscript review analyses (missingness, MAR evidence, MI predictor
audits, Table 2 descriptives) and a path (mediation) analysis.

### `power_analysis/` -- NIMH R01 Power Analysis

A fully reproducible power analysis technical appendix supporting a NIMH
grant proposal for a REP randomized controlled trial, using Cohen's d =
0.55 as the benchmark effect size from pilot REP data. Covers required
sample size, sensitivity analysis, moderation power (simulation-based), and
mediation power (bootstrap-based), all built on simulated data with no
external participant data required.

---

## Usage

Each subfolder can be run independently:

1. Clone the repository
2. Navigate to the subfolder for the analysis you need (`3.1/`, `4.1/`, or `power_analysis/`)
3. Follow that subfolder's README for data paths and required R packages
4. Knit the relevant `.Rmd` file, or open it in RStudio and run all chunks

Raw participant data for `3.1/` and `4.1/` are not included due to IRB
restrictions and data use agreements; `power_analysis/` is fully
self-contained and uses simulated data only.

---

## Related Repositories

- [`cico`](https://github.com/jlevchenko) -- Check-In/Check-Out multilevel outcome analysis
- [`epic`](https://github.com/jlevchenko) -- MMSD school climate factor analysis for Project EPIC
- [`metrics`](https://github.com/jlevchenko) -- METRICS national TA center data pipelines
