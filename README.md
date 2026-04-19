# NMAT Analysis

This repository contains the reproducible pipelines used to clean, match, and analyse NMAT (National Medical Admission Test) and PLE (Physician Licensure Exam) datasets. The work is organized as three core Jupyter notebooks representing sequential pipeline stages: data cleaning, PLE matching, and analysis.

## Repository structure (high level)

- `1_Data_Cleaning_Pipeline.ipynb` — data ingestion and QA, canonicalization of NMAT records.
- `2_PLE_Matching_Pipeline.ipynb` — deterministic + fuzzy matching of PLE passers to NMAT records; produces `NMAT_Ultima.csv` and PLE match artifacts.
- `3_NMAT_PLE_Analysis.ipynb` — statistical analyses, non-interactive visualizations, and summary CSV exports used for reporting.
- `dataset/` — source CSVs and generated outputs. Key subfolders: `dataset/output/` (QA & cleaning artifacts) and `dataset/analysis_output/` (analysis tables and static plots saved as HTML/CSV).

## Quick overview of each pipeline

### 1. Data cleaning (`1_Data_Cleaning_Pipeline.ipynb`)

Purpose: create a clean, analysis-ready NMAT table. Main steps and outputs:

- Load raw data files from `dataset/`.
- Normalize names (`normalize_name`) and applicant numbers (`clean_appno`).
- Parse numeric score columns and standardize date/year fields.
- Create person-level keys (`PERSON_KEY`) and flag best records for person-level analyses.
- College and course-group verification & QA (duplicate keys, unmatched colleges, verification status).
- Produce QA artifacts in `dataset/output/` (examples: `03_unique_college_dim.csv`, `04_unmatched_colleges.csv`, `NMAT_Ultima_profile.csv`).

Notes: this pipeline focuses on data hygiene (formats, types, deduping, college normalization) so that downstream matching and analysis steps operate on consistent inputs.

### 2. PLE matching (`2_PLE_Matching_Pipeline.ipynb`)

Purpose: match PLE passers to NMAT records, apply match quality heuristics, and flag rows for clean analysis.

Core steps:

- Load `NMAT_FINAL.csv`, `PLE_DATA.csv`, and `PLE_UNMATCHED.csv` from `dataset/`.
- Apply name normalization and pre-cleaning utilities (same helpers used across pipelines).
- Matching stages:
  1. Stage 0 — manual AppNo join for records with pre-filled applicant numbers.
  2. Stage 1 — exact name matches (with year-gap checks).
  3. Stage 2 — fuzzy name matching (uses `rapidfuzz.token_sort_ratio`) with tie-breaking and disambiguation rules.
- Disambiguation logic includes: `YEAR_GAP_MIN` (default 5 years), `PERCENTILE_FLOOR` (default 40), `FUZZY_ACCEPT_SCORE` (default 92), `FUZZY_REVIEW_SCORE` (default 85), and `FUZZY_GAP_MIN`.
- Build a `master_match` table consolidating best matches and confidence tiers (FINAL_MATCH, FUZZY_MATCH, AMBIGUOUS, etc.).
- Apply match results back to NMAT to produce `NMAT_Ultima.csv` and flags:
  - `IS_PLE_PASSER` — matched PLE passers
  - `IS_PLE_ANALYSIS_SAFE` — clean, high-confidence passers for survival-style analyses
  - `IS_BEST_NMAT_RECORD` — selected NMAT row used for person-level summaries

Key outputs (in `dataset/output/`): `PLE_MATCH_MASTER.csv`, `PLE_PASSERS_IN_NMAT.csv`, `PLE_STILL_UNMATCHED.csv`, `PLE_AMBIGUOUS_REVIEW.csv`, plus the principal output `NMAT_Ultima.csv` (saved to `dataset/`).

### 3. Analysis (`3_NMAT_PLE_Analysis.ipynb`)

Purpose: run descriptive and inferential analyses, export static visualizations and CSV tables for reporting.

Major analyses included:

- Decile and percentile summaries by year, university type, course group, and college.
- College-level decile distribution per university type: for each university type (Public, Private, Foreign, etc.), percentage distribution across deciles (D1–D10) is computed for colleges with at least 10 examinees. Top decile (D10) percentages are emphasized in the exported tables.
- Chi-square test of independence between university type and percentile decile (chi-square statistic, p-value, degrees of freedom, interpretation).
- Data integrity verification: ensure each `NMA_College` maps to exactly one university type (uses original `School Type_rec2_FINAL`); reports colleges with multiple types.
- Course-group distribution pie chart (static) and CSV summary.
- Top 10 pathways to top deciles (D8–D10): frequent flows for `university type → decile` and `course group → decile` as CSV tables.
- Survival rate to top deciles by course group: total examinees, number in D8–D10, and survival percentage.
- PLE non-match rates by decile: for each decile show total examinees, not matched to PLE (`IS_PLE_ANALYSIS_SAFE == False`), and non-match percentage.

Outputs: static CSVs and non-interactive HTML/SVG where appropriate are written to `dataset/analysis_output/`. Representative files include `04C_college_decile_by_uni_type_pct.csv`, `04D_chi_square_uni_type_vs_decile.csv`, `05D_top10_pathways_uni_to_top_deciles.csv`, `06E_survival_top_decile_by_course.csv`, `06F_ple_nonmatch_by_decile.csv`, and many additional descriptive CSVs.

Important: all visual outputs in the analysis pipeline are static (no interactive Sankey/alluvial visualizations), and all tables are exported as CSV for reproducibility and downstream use.

## Score components (performance measures)

Part I — Aptitude (Cognitive Skills)

Measures reasoning and perceptual abilities. Composed of four subtests (raw scores summed):

- Verbal — word analogies, reading comprehension
- Inductive Reasoning — pattern recognition, logical sequences
- Quantitative — numerical reasoning, basic math
- Perceptual Acuity — visual scanning, attention to detail

Part II — Science (Subject Matter Knowledge)

Measures knowledge in four natural and social science domains:

- Biology
- Physics
- Social Science
- Chemistry

TRUE Raw Score = Part I Raw Score + Part II Raw Score

These components are used throughout the analysis for subtest-level trends and aggregate performance metrics.

## How to run (recommended order)

1. Create and activate a Python environment (Python 3.9+ recommended):

```bash
python -m venv .venv
source .venv/bin/activate   # macOS/Linux
.venv\\Scripts\\activate    # Windows PowerShell
```

2. Install core dependencies (example):

```bash
pip install pandas numpy pyarrow rapidfuzz unidecode tqdm matplotlib seaborn jupyterlab
```

3. Run notebooks sequentially in the following order (open in Jupyter/VSCode and run all cells):

- `1_Data_Cleaning_Pipeline.ipynb`
- `2_PLE_Matching_Pipeline.ipynb`
- `3_NMAT_PLE_Analysis.ipynb`

Optional: use `papermill` or `nbconvert` to execute notebooks non-interactively for automation.

## Configuration knobs

- `YEAR_GAP_MIN` — minimum gap (years) required between NMAT and PLE for a valid match (default: 5).
- `PERCENTILE_FLOOR` — minimum NMAT percentile for candidate selection (default: 40).
- `FUZZY_ACCEPT_SCORE`, `FUZZY_REVIEW_SCORE`, `FUZZY_GAP_MIN` — fuzzy matching thresholds used in `2_PLE_Matching_Pipeline.ipynb`.

These are defined near the top of `2_PLE_Matching_Pipeline.ipynb` and can be adjusted before running matching stages.

## Key outputs and locations

- Cleaned & merged NMAT: `dataset/NMAT_Ultima.csv` (principal merged file used in analysis).
- Matching artifacts: `dataset/output/PLE_MATCH_MASTER.csv`, `dataset/output/PLE_PASSERS_IN_NMAT.csv`.
- Analysis tables: `dataset/analysis_output/` (multiple CSVs; see that folder for a full list). Example files: `04C_college_decile_by_uni_type_pct.csv`, `04D_chi_square_uni_type_vs_decile.csv`, `06E_survival_top_decile_by_course.csv`.

## Notes and reproducibility

- The repo saves intermediate QA outputs so analyses are auditable — do not delete `dataset/output/` and `dataset/analysis_output/` if you want to reproduce results.
- The matching pipeline intentionally uses conservative defaults for analysis-safe flags; review `IS_PLE_ANALYSIS_SAFE` and `IS_PLE_PASSER` if you change matching thresholds.

## Next steps / suggested analyses

- Re-run analysis after updating matching thresholds to measure sensitivity.
- Produce additional static cohort plots stratified by admission year and course group.
- If desired, generate a comprehensive `data_dictionary.md` from `dataset/NMAT_Ultima.csv` (field-level descriptions) — this can be automated by profiling the CSV.

## Contact

If you need changes to the analysis, new exports, or help running the notebooks, open an issue or contact the repository owner.

---

*Generated for reproducible NMAT–PLE analyses.*
