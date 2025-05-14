# Gong Music Intervention Analyses Using Psychometric Measures and Heart Rate Variability
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0) <!-- Adjust license if different -->

## Overview

This repository contains R Markdown scripts and their rendered outputs for analyzing the effects of a gong music intervention. The analyses focus on psychometric variables, interoceptive awareness, and physiological stress markers. Two main sets of analyses are presented, corresponding to different datasets or stages of the research.

## File Structure

This repository includes the following main analysis files:

1.  **`thesis_analysis_Irisa.Rmd`**:
    *   Performs descriptive statistics and parametric ANOVAs for several psychometric variables.
    *   Focuses on between-group comparisons at Time 1 of the gong intervention.
    *   Also includes non-parametric analyses for PANAS (Positive and Negative Affect Schedule) data.
    *   **Rendered Outputs:** `thesis_analysis_Irisa.pdf` and `thesis_analysis_Irisa.html` provide the full report from this Rmd file.

2.  **`thesis_analysis_Pedro.Rmd`**:
    *   Conducts a comprehensive pre-post analysis of the gong music intervention.
    *   Includes descriptive statistics and group comparisons at baseline (Time 1).
    *   Employs Generalized Estimating Equations (GEE) to analyze intervention effects (Group, Time, Group\*Time interaction) on various outcome variables.
    *   Performs post-hoc analyses and effect size calculations for significant GEE findings.
    *   Explores correlations between changes (Deltas T2-T1) in variables within the Experimental Group.
    *   Conducts exploratory multiple linear regression to identify predictors of significant changes in depression, stress, self-regulation, and attentional regulation within the Experimental Group.
    *   **Rendered Output:** `thesis_analysis_Pedro.pdf` provides the full report from this Rmd file.

## Prerequisites

To run the R Markdown scripts (`.Rmd` files) successfully, you will need:

1.  **R:** A recent version of R installed (analyses were developed with R version 4.4).
2.  **RStudio (Recommended):** An IDE like RStudio facilitates working with R Markdown files.
3.  **R Packages:** Install the following R packages. The specific packages required by each script are loaded within the respective `.Rmd` files. Key packages include:
    ```R
    # Run these lines in your R console if you don't have the packages
    # This is a combined list; each script loads what it needs.
    install.packages(c(
      "readxl",         # For reading Excel files
      "dplyr",          # For data manipulation
      "tidyr",          # For data tidying (e.g., pivot_longer)
      "stringr",        # For string manipulation
      "gtsummary",      # For descriptive and regression tables
      "geepack",        # For Generalized Estimating Equations (GEE)
      "emmeans",        # For post-hoc analyses of GEEs
      "effectsize",     # For calculating effect sizes
      "rstatix",        # For statistical tests (t-test, Wilcoxon, ANOVA)
      "nparLD",         # For non-parametric ANOVA-type analyses (used in Irisa)
      "kableExtra",     # For enhancing HTML/LaTeX tables
      "knitr",          # For creating dynamic reports
      "ggplot2",        # For graphics
      "corrplot",       # For visualizing correlation matrices
      "correlation",    # For detailed correlation analysis
      "olsrr",          # For regression diagnostics (e.g., stepwise, Breusch-Pagan)
      "lm.beta",        # For standardized beta coefficients
      "car",            # For regression diagnostics (e.g., Durbin-Watson)
      "conflicted",     # For managing namespace conflicts
      "MASS"            # Often used for statistical functions
      # Add any other specific packages loaded in your scripts
    ))
    ```

## Data

*   This repository **does not include** the raw data files (e.g., `dados_Irisa.xlsx`, `dados_Pedro.xlsx`) due to privacy and data sharing restrictions.
*   You must obtain these files separately and ensure they are accessible by the R scripts.
*   **Crucially, you MUST update the file paths** inside each `.Rmd` script to point to the correct location of your respective data files.
    *   In `thesis_analysis_Irisa.Rmd`, look for lines like `irisa <- read_excel(...)`.
    *   In `thesis_analysis_Pedro.Rmd`, look for lines like `data_original <- read_excel(...)`.

## Usage

1.  **Install Prerequisites:** Ensure R and all required R packages are installed.
2.  **Prepare Data:** Place your data Excel files (e.g., `dados_Irisa.xlsx`, `dados_Pedro.xlsx`) in an accessible directory.
3.  **Update File Paths:** Open each `.Rmd` file in RStudio or a text editor and modify the file path(s) within the `read_excel()` function(s) to match the location of your data file(s).
4.  **Run Analysis:** Open the desired `.Rmd` file in RStudio. You can:
    *   Run individual code chunks sequentially.
    *   Use the "Run" -> "Run All" command.
    *   Knit the document (e.g., to HTML or PDF using the "Knit" button) which will execute all code chunks and generate a full report.

## Analysis Details

### `thesis_analysis_Irisa.Rmd`
*   **Descriptive Statistics:** Calculates and presents descriptive statistics (mean, SD for continuous; N, % for categorical) for key variables, grouped by intervention group.
*   **Normality Testing:** Uses Shapiro-Wilk tests to assess the normality of continuous variables within each group.
*   **Group Comparisons (Baseline):**
    *   Performs Fisher's Exact Test for categorical variables (e.g., Sex).
    *   Uses t-tests (Welch's for unequal variances) or Mann-Whitney U tests (Wilcoxon rank-sum) for continuous variables based on normality assessment to compare groups at baseline.
*   **Repeated Measures ANOVA (Parametric - for IDATE):**
    *   Prepares data for long format.
    *   Conducts a two-way mixed ANOVA (Time \* Group) using `rstatix::anova_test`.
    *   Performs post-hoc paired t-tests within groups if appropriate.
*   **Non-Parametric ANOVA-type Analysis (for PANAS):**
    *   Prepares data for long format.
    *   Uses `nparLD::nparLD()` for non-parametric analysis of Time \* Group effects.
    *   Performs post-hoc non-parametric paired tests (e.g., `nparLD::npar.t.test.paired`) within groups if appropriate.

### `thesis_analysis_Pedro.Rmd`
*   **Data Preparation:** Imports, cleans, and renames variables. Converts relevant columns to appropriate data types. Creates a long-format dataset for repeated measures analyses.
*   **Descriptive Statistics (Baseline - T1):** Generates a comprehensive baseline characteristics table using `gtsummary`, comparing Experimental and Control groups.
*   **Analysis of Intervention Effects (GEE):**
    *   For each outcome variable, fits a Generalized Estimating Equation (GEE) model with Time, Group, and Time\*Group interaction terms, using an AR1 correlation structure.
    *   Presents ANOVA-like Wald test results for the GEE models.
*   **Post-Hoc Analyses (for GEE):**
    *   If a significant Time\*Group interaction is found (p < 0.10), performs post-hoc comparisons of T1 vs. T2 changes within each group using `emmeans`.
    *   Calculates and interprets Hedges' g effect sizes for these within-group changes.
*   **Correlations of Change Scores (Experimental Group):**
    *   Calculates delta scores (T2-T1) for all relevant variables in the Experimental group.
    *   Computes a Spearman correlation matrix of these delta scores.
    *   Visualizes the correlation matrix using `corrplot`.
    *   Optionally, provides a detailed table of correlations using the `correlation` package.
*   **Exploratory Predictors of Significant Changes (Experimental Group):**
    *   Focuses on outcomes that showed significant improvement (Depression, Stress, MAIA Self-Regulation, MAIA Attentional Regulation).
    *   For each, fits multiple linear regression models with relevant delta scores (selected based on prior correlation analysis, |rho| > 0.40) as predictors.
    *   Presents final model summaries, standardized beta coefficients, and checks regression assumptions (normality, homoscedasticity, autocorrelation of residuals).
*   **Session Information:** Includes output from `sessionInfo()` for reproducibility.

## Outputs

Each `.Rmd` script, when knitted, produces a comprehensive report (PDF and/or HTML) containing:

*   All executed R code (if `echo=TRUE` for specific chunks).
*   Narrative explanations of the methods and results.
*   Formatted tables for descriptive statistics, model summaries, ANOVA results, GEE results, post-hoc tests, and correlations.
*   Plots for data visualization, model diagnostics, and network analysis (if applicable).

## Important Notes

*   **File Paths:** The most critical step for reproducibility is updating the paths to your data files within each `.Rmd` script.
*   **Assumptions:** The scripts include checks for statistical assumptions (e.g., normality for t-tests/ANOVAs, regression diagnostics). Interpretations should consider whether these assumptions are met.
*   **Exploratory Nature:** Some analyses, particularly the predictor analyses in `thesis_analysis_Pedro.Rmd`, are explicitly exploratory due to sample size and should be interpreted as hypothesis-generating.

## How to Cite

If you use or adapt this code or analysis structure, please consider citing this repository or the associated research.

### Citing this Repository/Code (Example):
Pedrosa, F. (2025). *Analyses for Gong Music Intervention Study*. GitHub Repository. https://github.com/FredPedrosa/Anxiety_HRV

## Author

*   **Frederico Pedrosa**
*   fredericopedrosa@ufmg.br

## License

This project is licensed under the GPL v3 License - see the [LICENSE.md](LICENSE.md) file for details. Commercial use may require explicit permission.
