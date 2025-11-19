# Tools_and_Data-Analysis_Lab
This analysis examines respiratory virus surveillance through two complementary approaches:

Wastewater Surveillance: Pathogenic concentration detection in wastewater samples
Mortality Tracking: Provisional deaths attributed to respiratory illnesses

The integration of these datasets enables correlation analysis between environmental viral presence and population health outcomes.

Datasets
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
