# ADA-final-project

# Race/Ethnicity, County-Level Poverty, and Colon Cancer-Specific Survival in SEER, 2010-2020

## Project Description

This repository contains a comprehensive analysis of health disparities in colon cancer survival using data from the Surveillance, Epidemiology, and End Results (SEER) database. The project examines the relationship between race/ethnicity, county-level income, and cancer-specific survival outcomes among 181,264 colon cancer patients diagnosed between 2010 and 2020. The analysis employs multiple statistical methods including Kaplan-Meier survival curves, Cox proportional hazards models, Restricted Mean Survival Time (RMST), and stratified temporal analyses to identify and characterize health equity disparities in cancer outcomes.

This project is part of graduate-level coursework in Public Health at Washington University in St. Louis, conducted under the mentorship of Dr. Melissa Jonson-Reid (Brown School) and Dr. John N. Constantino (School of Medicine) within the CICM DataSMART research team.

## Files Included

- `seerfinal_export.csv`: SEER registry data containing 182,113 colon cancer patients with clinical, demographic, and socioeconomic variables (Not included in repository - obtained from SEER directly)
- `SEER_Analysis_Final.Rmd`: Complete R Markdown script containing data preparation, descriptive analysis, Kaplan-Meier curves, Cox regression models, RMST analysis, stratified analyses, and comprehensive results synthesis
- `README.md`: This file

## What the Code Does

The `SEER_Analysis_Final.Rmd` script performs the following analyses:

1. **Data Preparation and Cleaning**
   - Loads and recodes SEER database fields
   - Creates categorical variables for race/ethnicity (6 categories: NHW, NHB, Hispanic, API, AIAN, Other)
   - Recodes income into 5 ordered categories based on county-level median household income
   - Generates age, sex, stage, and survival outcome variables
   - Performs complete case analysis (N = 181,264 after exclusions)

2. **Descriptive Statistics**
   - Summarizes study population by race/ethnicity, income category, and cancer stage
   - Reports event rates, median follow-up times, and mean ages across strata
   - Creates publication-ready formatted tables

3. **Kaplan-Meier Survival Analysis**
   - Generates survival curves by race/ethnicity with log-rank tests
   - Generates survival curves by income category with log-rank tests
   - Generates survival curves by cancer stage with log-rank tests
   - Provides visualization of survival disparities over time

4. **Cox Proportional Hazards Regression**
   - Fits main effects model: Surv(time, event) ~ race_ethnicity + income_cat + age_numeric + sex_binary + stage + year_of_diagnosis
   - Fits interaction model: race_ethnicity × income_cat interactions
   - Performs Likelihood Ratio Test to compare model fit
   - Tests the proportional hazards assumption using Schoenfeld residuals
   - Reports hazard ratios with 95% confidence intervals

5. **Restricted Mean Survival Time (RMST) Analysis**
   - Calculates RMST within a 60-month window for each race/ethnicity group
   - Calculates RMST for each income category
   - Computes differences vs. reference groups
   - Provides clinically interpretable months of survival estimates

6. **Stratified Temporal Analyses**
   - Fits separate Cox models for the early period (0-12 months)
   - Fits separate Cox models for the intermediate period (12-36 months)
   - Fits separate Cox models for the late period (36+ months)
   - Examines how disparities change across the disease trajectory

7. **Statistical Output**
   - Generates formatted tables for all results
   - Produces publication-ready figures
   - Calculates concordance indices for model performance
   - Provides comprehensive statistical inference

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
   - Ensure the file contains all required variables: race_and_origin_recode_nhw_nhb_nhaian_nhapi_hispanic, median_household_income_inflation_adj_to_2021, combined_summary_stage_2004, age_recode_with_1_year_olds, sex, survival_months, seer_cause_specific_death_classification, year_of_diagnosis

3. **Update File Path**
   - Open `SEER_Analysis_Simplified.Rmd` in RStudio
   - Update the file path in the `load-data` chunk to match your local directory:
   ```r
   seer <- read.csv("YOUR_PATH_HERE/seerfinal_export.csv")
   ```

4. **Set Working Directory**
   - In RStudio: Session → Set Working Directory → Choose Directory
   - Or use: `setwd("YOUR_PATH_HERE")`

5. **Run the Analysis**
   - Click "Knit" button in RStudio to generate complete HTML report
   - Alternatively, run line-by-line using Ctrl+Enter
   - Or execute entire script: `rmarkdown::render("SEER_Analysis_Simplified.Rmd")`

6. **View Results**
   - HTML output file will be generated: `SEER_Analysis_Simplified.html`
   - Open in web browser for formatted tables, figures, and complete analysis

## Key Findings

- **Non-Hispanic Black patients** experience 18% excess hazard of cancer-specific death (HR = 1.18, p < 0.0001)
- **Racial disparities are dynamic**, emerging in intermediate follow-up (HR = 1.09 at 12-36 months) and maximizing in late follow-up (HR = 1.36 at 36+ months), with NO significant disparity in early period (0-12 months)
- **Income gradient evident**: High-income patients gain 2.57 months of survival (6% longer life) compared to low-income patients
- **Cancer stage is strongest predictor** (HR = 9.57 for localized vs. distant)
- **Model performance excellent**: Concordance Index = 0.824

## Study Limitations

1. Proportional hazards assumption violated (χ² = 2363.36, p < 0.0001)
2. County-level vs. individual-level income (ecological fallacy)
3. No treatment data available in SEER
4. Missing comorbidity information
5. SEER represents ~35% of US population (regional limitations)
6. Cancer-specific vs. all-cause mortality only
7. No data beyond 2020
8. Unmeasured confounding possible

## Author

- **Name:** Gagan Vijay
- **Program:** Master of Public Health (MPH)
- **Institution:** Washington University in St. Louis
- **School:**  School of Public Health
- **Faculty:** Dr. Kim Jhonson
- **Date:** December 2025

## Contact

For questions or inquiries regarding this analysis, please contact:
- Gagan Vijay: [g.vijay@wustl.edu]



## Citation

Vijay, G., (2025). Race/ethnicity, county-level poverty, and colon cancer-specific survival in SEER, 2010-2020. *Washington University in St. Louis, School of Public Health.*

## License

This project is provided for educational and research purposes. Use of SEER data is subject to SEER data use agreements and terms.

## Acknowledgments

- SEER Program of the National Cancer Institute for providing the cancer registry data
- Washington University in St. Louis for research support
- Dr. Kim Jhonson for mentorship and guidance

---

**Last Updated:** December 2025  
**Analysis Date:** December 1, 2025  
**Status:** Complete

