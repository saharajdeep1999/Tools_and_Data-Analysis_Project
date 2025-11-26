# Tools_and_Data-Analysis_Lab
Respiratory Virus Wastewater & Mortality Analysis
Project Overview
This analysis examines respiratory virus surveillance through two complementary approaches:

Wastewater Surveillance: Pathogenic concentration detection in wastewater samples
Mortality Tracking: Provisional deaths attributed to respiratory illnesses

The integration of these datasets enables correlation analysis between environmental viral presence and population health outcomes.

Datasets:
**1. Respiratory Virus Wastewater Surveillance Dataset**
Source: Respiratory_Virus_Wastewater_Surveillance.csv
Dimensions: 6,645 rows × 14 columns

Key Columns:
mmwr_week: MMWR week identifier
week: Week number (1-52)
week_start / week_end: Week date ranges
season: Respiratory season (e.g., 2024-2025)
pathogen: Type of respiratory virus (Influenza A, Influenza B, SARS-CoV-2, RSV, PMMOV)
siteno / sitename: Wastewater treatment plant location
year: Reporting year (2022-2025)
target_wkavg_concentration: Weekly average viral concentration
conc_toplot: Log-transformed concentration for visualization
wkavg_val: Normalized weekly average values
wkavg_val_category: Categorical severity level (Minimal, Low, Moderate, High, Very High)

Data Quality:
Missing values: ~7.77% in concentration fields, ~27.77% in categorical fields
No duplicate rows, Data spans from 2022 to 2025

**2. Provisional Deaths Due to Respiratory Illnesses Dataset**
Source: Provisional_Deaths_Due_to_Respiratory_Illnesses.csv
Dimensions: 1,107 rows (limited to 1,000 for analysis) × 9 columns

Key Columns:
mmwr_week: MMWR week identifier
week: Week number (1-53)
week_start / week_end: Week date ranges
season: Respiratory season
disease: Type of respiratory disease (COVID-19, Influenza, RSV)
percent: Percentage of deaths attributed to respiratory illness
current_week_ending: Most recent week date
row_id: Unique identifier

Data Quality:

No missing values detected
All columns fully populated
Data spans 2018-2025


Methods
Data Preprocessing
Both datasets underwent standardized cleaning procedures following industry best practices:
**1. Missing Data Handling**
Wastewater Dataset:

target_wkavg_concentration: 516 missing values (7.77%)
conc_toplot: 516 missing values (7.77%)
wkavg_val: 1,845 missing values (27.77%)
wkavg_val_category: 1,845 missing values (27.77%)
Strategy: Retained missing values; downstream analyses use listwise deletion

Respiratory Dataset:

No missing values detected across all 9 columns
All records complete for analysis

**2. Duplicate Records**

Wastewater Dataset: 0 duplicate rows found
Respiratory Dataset: 0 duplicate rows found
Action: No removal necessary; datasets are clean

**3. Data Type Verification & Conversion**
Wastewater Dataset:

pathogen: Converted from categorical text to numeric codes
  Influenza A = 0
  SARS-CoV-2 = 1
  PMMOV = 2
  RSV = 3
  Influenza B = 4


All numeric columns verified as float64 or int64
Date columns remain as object type (string)

**Respiratory Dataset:** disease: Converted from categorical text (COVID-19, Influenza, RSV) to numeric codes
**All numeric columns correctly typed, No conversion needed for temporal columns**

**4. Outlier Detection & Investigation**

**Wastewater Concentration Outliers:**
  Maximum concentration: 6.23 × 10⁹ copies/mL (Little Village site, PMMOV, Feb 2025)
  Minimum concentration: 0 copies/mL (multiple sites, multiple pathogens)
  Assessment: Extreme maximum likely valid (viral load detection); zeros indicate non-detection, not errors
  Action: Retained as valid data; log transformation applied for visualization (conc_toplot)

**Respiratory Mortality Outliers:**
  Maximum mortality percentage: 45% (pandemic peak periods)
  Minimum: 0% (periods with no recorded respiratory deaths)
  Assessment: Valid; reflects actual pandemic impact variation
  Action: Retained; represents true seasonal and epidemic variation

**Merge datasets on week identifiers to enable direct correlation analysis
Implement seasonal decomposition for trend analysis
Develop predictive models using wastewater surveillance as leading indicators
Create interactive dashboards for real-time monitoring
Conduct geographic stratification and regional comparison studies**


This project also investigates the relationship between viral pathogen concentrations detected in wastewater and mortality rates from respiratory illnesses. By merging wastewater surveillance data with provisional mortality data, we employ rigorous statistical hypothesis testing to understand disease-mortality dynamics across COVID-19, Influenza, and RSV.
Objectives

Examine whether high viral concentrations in wastewater predict higher mortality rates
Identify disease-specific differences in mortality during high viral concentration periods
Determine associations between concentration levels and mortality severity categories
Provide evidence-based insights for public health surveillance and intervention planning

Data Sources
Two primary datasets are used in this analysis:

Respiratory_Virus_Wastewater_Surveillance.csv - Contains weekly average viral pathogen concentrations detected in wastewater samples, including Influenza A, Influenza B, SARS-CoV-2, RSV, and PMMOV
Provisional_Deaths_Due_to_Respiratory_Illnesses.csv - Contains provisional mortality data linked by week (MMWR) and disease type

Methodology
Data Preparation

Pathogen variants (Influenza A & B) are consolidated into single disease categories
Datasets are merged on MMWR week and disease classification
Missing or incomplete records are removed to ensure data quality
PMMOV (not linked to human mortality) is excluded from analysis

**Statistical Approach**
Hypothesis 1: High Viral Concentration Predicts Higher Mortality
Test: Independent samples t-test

  H₀: No difference in mortality between high and low concentration weeks
  H₁: High concentration weeks have significantly higher mortality
  Method: Groups are created based on median concentration; mortality percentages are compared
  Assumptions Verified: Normality (Shapiro-Wilk), Equal variances (Levene's test)
  Effect Size: Cohen's d

Hypothesis 2: Disease-Specific Concentration-Mortality Relationship
Test: One-Way ANOVA

  H₀: Mean mortality is equal across all disease types
  H₁: At least one disease shows different mortality during high concentration
  Method: High concentration weeks are stratified by disease (COVID-19, Influenza, RSV)
  Assumptions Verified: Normality per group, Equal variances across groups

Hypothesis 3: Association Between Concentration and Mortality Severity
  Test: Chi-Squared Test of Independence

  H₀: Concentration level and mortality severity are independent
  H₁: Concentration level and mortality severity are associated
  Method: Mortality categorized as Low (≤1%), Moderate (1-3%), High (>3%)
  Effect Size: Cramér's V
  Cell Requirements: Minimum expected frequency ≥ 5 verified

Requirements
pandas
numpy
matplotlib
seaborn
scipy
Installation
bashpip install pandas numpy matplotlib seaborn scipy
Usage
Run the analysis script:
bashpython analysis_script.py
The script will:

Load both CSV datasets
Merge datasets on week and disease classification
Execute all three hypothesis tests with assumption verification
Display detailed statistical results and interpretations
Generate visualization outputs for each hypothesis

Output Files
The analysis produces the following outputs:

hypothesis1_results.png - Box plot and distribution histogram comparing mortality between high and low viral concentration weeks
hypothesis2_results.png - Box plot and bar chart with error bars showing disease-specific mortality differences
hypothesis3_results.png - Heatmap displaying the association between concentration and mortality severity categories

Console output includes comprehensive statistical reports with p-values, test statistics, effect sizes, and practical interpretations.
Key Findings Structure
For each hypothesis, the analysis provides:

Descriptive Statistics: Group means, standard deviations, and sample sizes
Assumption Verification: Results of normality and homogeneity tests
Statistical Test Results: Test statistics, p-values, degrees of freedom
Effect Sizes: Cohen's d (t-test), Cramér's V (chi-squared)
Statistical Decision: Whether to reject or fail to reject the null hypothesis at α = 0.05
Practical Interpretation: Real-world meaning of findings with numerical context

**Significance Level**
All statistical tests use α = 0.05 as the threshold for significance determination.
Author Notes
This analysis uses standard epidemiological and statistical methods to examine public health surveillance data. Results are intended to support understanding of pathogen-mortality relationships and inform evidence-based public health decision-making.

Disclaimer
This analysis is based on provisional mortality data and wastewater surveillance measurements. Interpretations should consider potential limitations in data collection, reporting delays, and the ecological nature of wastewater-based epidemiology.
