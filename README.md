# ADA-Final-Project: Race/Ethnicity, County-Level Poverty, and Colon Cancer-Specific Survival in SEER, 2010-2020

## Project Description

This repository contains a comprehensive analysis of health disparities in colon cancer survival using data from the Surveillance, Epidemiology, and End Results (SEER) database. The project examines three specific objectives: (1) estimating the association between race/ethnicity and colon cancer-specific survival, (2) estimating the association between county-level poverty and survival, and (3) assessing whether county-level income modifies the race/ethnicity–mortality association. The analysis includes 181,264 colon cancer patients diagnosed between 2010 and 2020, employing Kaplan-Meier survival curves, Cox proportional hazards models, Restricted Mean Survival Time (RMST), and effect modification testing to characterize health equity disparities in cancer outcomes.

This project is part of graduate-level coursework in Advanced Data Analytics in Public Health at Washington University in St. Louis, taught by Dr. Kim Johnson.

## Files Included

- `seerfinal_export.csv`: SEER registry data containing 182,113 colon cancer patients with clinical, demographic, and socioeconomic variables (Not included in repository—obtained directly from SEER)
- `SEER_Analysis_Final.Rmd`: Complete R Markdown script containing data preparation, descriptive statistics, Kaplan-Meier curves, Cox regression models, RMST analysis, interaction testing, and comprehensive results and discussion
- `README.md`: This file

## What the Code Does

The `SEER_Analysis_Final.Rmd` script performs the following analyses:

### 1. Data Preparation and Cleaning
- Loads and recodes SEER database fields
- Creates categorical variables for race/ethnicity (6 categories: NHW, NHB, Hispanic, API, AIAN, Other)
- Recodes income into 5 ordered categories based on county-level median household income thresholds
- Generates age, sex, stage, and survival outcome variables
- Performs complete case analysis (N = 181,264 after exclusions of missing data)

### 2. Descriptive Statistics
- Summarizes study population by race/ethnicity, income category, and cancer stage
- Reports event rates, median follow-up times, and mean ages across demographic strata
- Creates publication-ready formatted tables

### 3. Kaplan-Meier Survival Analysis (Preliminary)
- Generates survival curves by race/ethnicity with log-rank statistical tests
- Generates survival curves by income category with log-rank tests
- Generates survival curves by cancer stage with log-rank tests
- Provides visual representation of survival disparities

### 4. Cox Proportional Hazards Models
- **Main Effects Model:** Tests associations with race/ethnicity and income, adjusted for age, sex, stage, and year of diagnosis
- **Tests Proportional Hazards Assumption:** Uses Schoenfeld residuals test; identifies assumption violation
- **Interaction Model:** Tests race × income interactions to assess effect modification
- **Likelihood Ratio Test:** Compares main effects vs. interaction model fit
- Reports hazard ratios with 95% confidence intervals

### 5. Restricted Mean Survival Time (RMST) Analysis — Primary Method

#### a. Unadjusted RMST
- RMST estimated for each race/ethnicity and income group over a **60-month window**.
- Demonstrates absolute survival time differences (months lived on average).
- Calculated using `survRM2::rmst2()` function.

#### b. Adjusted RMST
- RMST model adjusted for age, sex, stage, and year of diagnosis.
- Conducted using ANCOVA-style modeling with RMST regression.
- Evaluates differences in mean survival time while controlling for covariates.
- Tables 2 and 3 report RMST estimates with 95% confidence intervals.

### 6. Interaction Testing
- Statistical testing for Race × Income interactions.
- Small but significant interaction detected; race and income effects largely independent.

### 7. Output
- Publication-ready tables (Tables 1–3).
- KM curves and model diagnostics.
- Model performance: C-index, PH violations, survival differences.
## How to Run the Code

### Prerequisites
- R version 4.4.2 or later
- RStudio (recommended)
- Required R packages: survival, survminer, tidyverse, dplyr, janitor, ggplot2, gridExtra, kableExtra

### Installation of Required Packages
```r
install.packages(c("survival", "survminer", "tidyverse", "dplyr", "janitor", "ggplot2", "gridExtra", "kableExtra"))
```

### Steps to Run

1. **Download or clone this repository** to your computer
   ```bash
   git clone [repository URL]
   ```

2. **Obtain SEER Data**
   - Request SEER data from https://seer.cancer.gov/data/
   - Save as `seerfinal_export.csv` in the project directory
   - Ensure the file contains all required variables:
     - race_and_origin_recode_nhw_nhb_nhaian_nhapi_hispanic
     - median_household_income_inflation_adj_to_2021
     - combined_summary_stage_2004
     - age_recode_with_1_year_olds
     - sex
     - survival_months
     - seer_cause_specific_death_classification
     - year_of_diagnosis

3. **Update File Path**
   - Open `SEER_Analysis_Final.Rmd` in RStudio
   - Update the file path in the `load-data` chunk:
   ```r
   seer <- read.csv("YOUR_PATH_HERE/seerfinal_export.csv")
   ```

4. **Set Working Directory**
   - In RStudio: Session -> Set Working Directory -> Choose Directory
   - Or use: `setwd("YOUR_PATH_HERE")`

5. **Run the Analysis**
   - Click "Knit" button in RStudio to generate complete HTML report
   - Alternatively, run line-by-line using Ctrl+Enter
   - Or execute entire script: `rmarkdown::render("SEER_Analysis_Corrected_Final.Rmd")`

6. **View Results**
   - HTML output file will be generated: `SEER_Analysis_Final.html`
   - Open in web browser for formatted tables, figures, and complete analysis

## Key Findings

**Primary Objective 1 - Race/Ethnicity Association:**
- Non-Hispanic Black patients experience 18% excess hazard of cancer-specific death (HR = 1.18, 95% CI: 1.15-1.21, p < 0.0001)
- RMST: NHB patients lose 2.31 months of survival vs. NHW (42.48 vs. 44.79 months in 60-month window)
- Asian/Pacific Islander patients show favorable outcomes (HR = 0.91, +1.90 months RMST)

**Primary Objective 2 - Income Association:**
- Clear socioeconomic dose-response gradient: High-income patients gain 2.57 months survival vs. low-income
- Linear trend: Each income category elevation = 11% hazard reduction (HR = 0.89, p < 0.0001)
- RMST: Low income 43.31 months → High income 45.88 months

**Secondary Objective - Effect Modification:**
- Race - income interaction model is statistically significant (χ² = 32.94, p = 0.0343)
- However, the effect is modest (20 new parameters yield only χ² gain of 32.94)
- Most individual interaction coefficients are non-significant (p > 0.05)
- **Interpretation:** Race and income effects are largely independent rather than synergistic

**Secondary Objective - Effect of cancer stage
- NHB patients live 3.00 months less than NHW patients over 5 years
(Adjusted RMST difference: −3.00 months; 95% CI: −5.01 to −0.99; P = 0.004)
Decomposition of Disparity
- Total Effect: −3.00 months (full disparity = 100%)
- Direct Effect: −1.63 months (54%) - Treatment quality, care access, biological factors
- Mediated Effect: −1.37 months (46%) - Later-stage diagnosis (screening disparities)



**Overall Model Performance:**
- Concordance Index = 0.824 (excellent discrimination)
- Cancer stage strongest predictor (HR = 9.57 for localized vs. distant)
- Proportional hazards assumption violated globally (χ² = 2363.36, p < 0.0001), justifying RMST as primary analysis

## Study Limitations

1. **Proportional Hazards Assumption Violation:** While RMST addresses this mathematically, results should not be extrapolated beyond 60 months

2. **County-Level vs. Individual-Level Income:** Subject to ecological fallacy; individual patient-level SES would be more precise

3. **No Treatment Data:** SEER does not consistently record chemotherapy, radiation, or surgical approach details

4. **Missing Comorbidity Information:** Unmeasured baseline health differences could confound associations

5. **Complete Case Analysis:** Assumes data missing completely at random (MCAR)

6. **Regional Representation:** SEER covers approximately 35% of U.S. population; findings may not be nationally generalizable

7. **Cancer-Specific Mortality Only:** Competing mortality from other causes not captured

8. **"Other" Race Category:** Small sample size (N = 1,129, 54 events) results in imprecise estimates

9. **Temporal Generalization:** Findings cover 2010-2020; may not reflect current disparities (2024+)

10. **Unmeasured Confounding:** Potential confounding from provider implicit bias, patient health literacy, insurance type/stability, and tumor biology

11. **Interaction Effect Size:** While statistically significant, the practical magnitude of effect modification is small

## Author

- **Name:** Gagan Vijay
- **Program:** Master of Public Health (MPH)
- **Institution:** Washington University in St. Louis
- **School:** Brown School of Social Work / School of Public Health
- **Faculty Advisor:** Dr. Kim Johnson
- **Course:** Advanced Data Analytics in Public Health
- **Date:** December 2025

## Contact

For questions or inquiries regarding this analysis, please contact:
- Gagan Vijay: g.vijay@wustl.edu

## Citation

Vijay, G. (2025). Race/ethnicity, county-level poverty, and colon cancer-specific survival in SEER, 2010-2020. *Advanced Data Analytics Final Project, Washington University in St. Louis, School of Public Health.*

## License

This project is provided for educational and research purposes. Use of SEER data is subject to SEER data use agreements and terms of service. For more information, visit https://seer.cancer.gov/data/

## Acknowledgments

- SEER Program of the National Cancer Institute for providing the cancer registry data
- Washington University in St. Louis for research support and institutional resources
- Dr. Kim Johnson for mentorship, guidance, and feedback throughout the project

---

**Last Updated:** December 3, 2025  
**Analysis Date:** December 3, 2025  
**Status:** Complete and Ready for Submission
