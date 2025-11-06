# Delhi Air Quality Pollution Trends - Project Documentation

## Overview
This project analyzes Delhi's air quality data to understand pollution trends, identify key contributing factors, and build predictive models. The analysis helps us understand how air quality changes over time, what causes pollution spikes, and how different pollutants affect overall air quality.

---

## Table of Contents
1. [Setup and Installation](#setup-and-installation)
2. [Data Loading](#data-loading)
3. [Data Exploration](#data-exploration)
4. [Data Preprocessing](#data-preprocessing)
5. [Outlier Detection and Treatment](#outlier-detection-and-treatment)
6. [Feature Engineering](#feature-engineering)
7. [Data Scaling](#data-scaling)
8. [Analysis 1: Yearly Air Quality Trends](#analysis-1-yearly-air-quality-trends)
9. [Analysis 2: Seasonal Pollution Patterns](#analysis-2-seasonal-pollution-patterns)
10. [Analysis 3: Pollutant Contribution Analysis](#analysis-3-pollutant-contribution-analysis)
11. [Analysis 4: Correlation Between Pollutants](#analysis-4-correlation-between-pollutants)
12. [Analysis 5: Impact of Major Events](#analysis-5-impact-of-major-events)
13. [Analysis 6: WHO Safety Standards Exceedance](#analysis-6-who-safety-standards-exceedance)
14. [Analysis 7: Monthly Distribution Visualization](#analysis-7-monthly-distribution-visualization)
15. [Analysis 8: Time Series Analysis and Prediction](#analysis-8-time-series-analysis-and-prediction)
16. [Results and Conclusions](#results-and-conclusions)
17. [Sources and References](#sources-and-references)

---

## Setup and Installation

### Why we did this:
We need specific Python libraries to work with data, create visualizations, and build machine learning models.

**Libraries installed:**
- `pandas`: For reading and manipulating data
- `numpy`: For numerical operations
- `matplotlib` & `seaborn`: For creating charts and graphs
- `scikit-learn`: For machine learning models
- `plotly`: For interactive visualizations
- `statsmodels`: For time series analysis

---

## Data Loading

### Why we did this:
We need to load our dataset into the workspace to start analyzing it.

**Steps:**
1. Loaded the `final_dataset.csv` file containing 1461 days of air quality data
2. Checked the shape of the dataset: 1461 rows (days) and 12 columns (features)

**Columns in our dataset:**
- Date: The day of recording
- Month, Year: Temporal information
- Holidays_Count: Number of holidays in that period
- Days: Day number
- PM2.5, PM10, NO2, SO2, CO, Ozone: Different pollutant measurements
- AQI: Air Quality Index (overall air quality score)

---

## Data Exploration

### Why we did this:
Before processing data, we need to understand its structure, quality, and basic statistics. This helps us identify potential issues early.

**Steps taken:**
1. **Basic Information (`df.info()`)**: 
   - Checked data types and non-null counts
   - Found: All columns have complete data (no missing values)
   - Confirmed: Mixed data types (dates, integers, floating-point numbers)

2. **Statistical Summary (`df.describe()`)**:
   - Got mean, median, min, max values for all numerical columns
   - Helps understand the range and distribution of pollution values

3. **Null Value Check**:
   - Confirmed no missing values in the dataset
   - Important because missing data can break our analysis and models

---

## Data Preprocessing

### Why we did this:
Raw data often needs cleaning and transformation to make it useful for analysis. We need proper date formats for time-based analysis.

**Steps taken:**

1. **Date Conversion**:
   - Converted the Date column to proper datetime format
   - **Why:** This allows us to do time-based operations like finding trends, comparing years, and detecting seasonal patterns

2. **Creating Season Feature**:
   - Added a new column called "Season" based on months
   - **Categories:** Winter (Dec-Feb), Summer (Mar-May), Monsoon (Jun-Aug), Post-Monsoon (Sep-Nov)
   - **Why:** Seasons significantly affect air quality (e.g., winter has more pollution). This helps us analyze seasonal patterns.

3. **Duplicate Check**:
   - Checked for duplicate rows
   - **Result:** No duplicates found
   - **Why:** Duplicates can skew our analysis and give incorrect results

---

## Outlier Detection and Treatment

### Why we did this:
Outliers are extreme values that might be errors or rare events. We need to identify them to ensure our analysis is accurate and not skewed by these extreme cases.

**Steps taken:**

1. **Visual Detection**:
   - Created boxplots for all pollutants (PM2.5, PM10, NO2, SO2, CO, Ozone, AQI)
   - **Observation:** PM2.5 and PM10 showed many high outliers
   - **Why use boxplots:** They visually show where most values lie and highlight extreme points

2. **Outlier Treatment**:
   - Used IQR (Interquartile Range) method to identify outliers
   - Capped extreme values at reasonable limits (1.5 × IQR beyond quartiles)
   - **Why:** These outliers are likely real pollution events (not errors), but capping them prevents them from dominating our models

---

## Feature Engineering

### Why we did this:
Adding meaningful new features helps our models understand patterns better. Feature engineering improves analysis and prediction accuracy.

**Steps taken:**

1. **AQI Category Creation**:
   - Created categories based on AQI values (Good, Satisfactory, Moderate, Poor, Very Poor, Severe)
   - Used India's Central Pollution Control Board (CPCB) standards
   - **Why:** Categories are easier to interpret than numbers and help identify health risk levels

---

## Data Scaling

### Why we did this:
Different pollutants have different measurement scales (PM2.5 might be 100, while CO might be 2). Scaling standardizes all values to the same range.

**Steps taken:**
- Used StandardScaler to normalize pollutant values
- Converts all values to have mean=0 and standard deviation=1
- **Why:** Machine learning models work better when all features are on the same scale. It ensures no single pollutant dominates the analysis simply because it has larger numbers.

---

## Analysis 1: Yearly Air Quality Trends

### What we did:
Analyzed how Delhi's air quality changed from year to year.

**Method:**
- Calculated yearly averages for AQI and all pollutants
- Plotted trends over time (2020-2024)

**Key Findings:**
- **AQI improved:** Values decreased from 2021 to 2023, showing overall air quality improvement
- **PM2.5 decreased:** Sharp drop between 2022 and 2023, indicating successful pollution control measures
- **2024 uptick:** Slight increase, possibly due to post-COVID industrial recovery

**Why this matters:**
Shows whether pollution control measures are working and helps policymakers understand long-term trends.

---

## Analysis 2: Seasonal Pollution Patterns

### What we did:
Examined how pollution varies by season and month.

**Methods:**
- Created boxplots comparing PM2.5 across seasons
- Plotted monthly averages as a line chart

**Key Findings:**
- **Winter worst:** Highest PM2.5 levels (temperature inversion traps pollutants)
- **Post-Monsoon high:** October-November also show elevated pollution (stubble burning)
- **Summer/Monsoon cleanest:** Lowest pollution levels, especially July-August
- **Peak months:** January and November-December show worst air quality

**Why this matters:**
Understanding seasonal patterns helps predict pollution spikes and plan interventions (like odd-even schemes during peak months).

**Root causes:**
- **Winter:** Temperature inversion, stubble burning, increased fuel consumption, stagnant winds
- **Monsoon:** Rainfall washes out pollutants, explaining cleaner air

---

## Analysis 3: Pollutant Contribution Analysis

### What we did:
Determined which pollutants contribute most to poor air quality.

**Method:**
- Used Linear Regression to predict AQI from all pollutant levels
- Analyzed regression coefficients to see relative importance

**Key Findings:**
- **PM10:** Most influential (coefficient: 60.65) - primary contributor to AQI
- **PM2.5:** Second most influential (coefficient: 44.66) - also very important
- **Other pollutants:** CO, NO2, Ozone, SO2 have much smaller contributions

**Why this matters:**
Focusing pollution control efforts on PM2.5 and PM10 will have the biggest impact on improving air quality.

**Sources:**
- Vehicular traffic
- Industrial emissions
- Construction dust
- Stubble burning

---

## Analysis 4: Correlation Between Pollutants

### What we did:
Examined how different pollutants relate to each other and to overall AQI.

**Method:**
- Created a correlation heatmap showing relationships between all pollutants
- Correlation values range from -1 to +1 (closer to +1 or -1 = stronger relationship)

**Key Findings:**
- **PM2.5 & PM10:** Strong positive correlation (0.78) - rise together
- **PM2.5 & AQI:** Very strong correlation (0.88) - major driver of air quality
- **PM10 & AQI:** Very strong correlation (0.91) - primary contributor
- **CO & AQI:** Moderate-strong correlation (0.73)
- **NO2 & AQI:** Weak-moderate correlation (0.31)
- **SO2 & Ozone:** Almost no correlation with AQI

**Why this matters:**
Understanding relationships helps us:
- Predict one pollutant from another
- Understand common pollution sources
- Focus monitoring efforts on most impactful pollutants

---

## Analysis 5: Impact of Major Events

### What we did:
Analyzed how major events (COVID lockdown, odd-even scheme, stubble burning season) affected air quality.

**Method:**
- Marked specific date ranges for each event
- Compared average PM2.5 before, during, and after each event
- Calculated percentage changes

**Events analyzed:**
1. **COVID Lockdown (Mar-May 2020)**
2. **Odd-Even Scheme (Nov 2023)**
3. **Stubble Burning Season (Oct-Nov 2021)**

**Key Findings:**

1. **Lockdown Effect:**
   - **Before:** 140.87 µg/m³
   - **During:** 85.48 µg/m³ (-39.32% improvement)
   - **Why:** Reduced traffic and industrial activity significantly improved air quality

2. **Odd-Even Scheme:**
   - **Before:** 104.38 µg/m³
   - **During:** 170.68 µg/m³ (+63.52% worse)
   - **Why:** Limited success; overshadowed by stubble burning and winter conditions

3. **Stubble Burning:**
   - **Before:** 43.21 µg/m³
   - **During:** 127.5 µg/m³ (+195.06% worse)
   - **Why:** Massive increase - stubble burning in nearby states causes severe pollution spikes

**Why this matters:**
Shows which interventions work and which external factors (like stubble burning) have the biggest impact.

---

## Analysis 6: WHO Safety Standards Exceedance

### What we did:
Compared Delhi's pollutant levels against WHO (World Health Organization) safety guidelines to assess health risks.

**WHO Standards Used (24-hour averages):**
- PM2.5: 15 µg/m³
- PM10: 45 µg/m³
- NO2: 25 µg/m³
- Ozone: 100 µg/m³
- SO2: 40 µg/m³
- CO: 4 mg/m³

**Method:**
- Counted how many days each pollutant exceeded WHO limits
- Calculated percentage of exceedance days

**Key Findings:**
- **PM2.5:** Exceeds WHO limits on 95%+ of days - severe health risk
- **PM10:** Exceeds limits on 95%+ of days - severe health risk
- **NO2:** Exceeds limits on ~62% of days - moderate concern
- **Ozone & CO:** Within safe limits - not major concerns
- **SO2:** Exceeds limits on ~11% of days - localized industrial events

**Why this matters:**
- Highlights chronic exposure to unsafe air quality
- Shows urgent need for multi-sectoral pollution control
- Identifies which pollutants need immediate attention

**Health Implications:**
Prolonged exposure to PM2.5 and PM10 causes respiratory and cardiovascular diseases.

---

## Analysis 7: Monthly Distribution Visualization

### What we did:
Created interactive boxplots showing PM2.5 distribution by month for different years.

**Method:**
- Used Plotly to create interactive visualizations
- Compared monthly patterns across years (2020-2022)

**Key Findings:**
- **Consistent pattern:** Winter months (Nov-Jan) consistently show highest PM2.5
- **2023 outliers:** Notable extreme pollution events in certain months
- **Seasonal confirmation:** Validates that pollution follows predictable seasonal cycles

**Why this matters:**
- Confirms seasonal patterns are consistent across years
- Identifies years with exceptional pollution events
- Helps forecast pollution for future months

---

## Analysis 8: Time Series Analysis and Prediction

### Part 8.1: Seasonal Decomposition

**What we did:**
Decomposed daily PM2.5 data into components: trend, seasonal, and residual.

**Method:**
- Created 7-day rolling average to smooth daily variations
- Applied seasonal decomposition to identify patterns

**Components Identified:**
1. **Trend:** Long-term direction (shows cyclical pollution behavior)
2. **Seasonal:** 7-day weekly cycles (possibly linked to weekday/weekend traffic patterns)
3. **Residual:** Random fluctuations from irregular events (rain, local emissions)

**Why this matters:**
- Separates systematic patterns from random noise
- Helps understand underlying pollution dynamics
- Useful for forecasting future pollution levels

### Part 8.2: Predictive Modeling

**What we did:**
Built a machine learning model to predict next-day PM2.5 levels.

**Features Used:**
- PM2.5 from yesterday (`pm2p1`)
- PM2.5 from 7 days ago (`pm2p7`)
- Day of week (`dow`)

**Model Used:**
- Random Forest Regressor (100 trees)

**Baseline Model:**
- Simple prediction: tomorrow's PM2.5 = today's PM2.5
- Performance: MAE = 17.83 µg/m³

**Random Forest Model:**
- Performance: MAE = 18.59 µg/m³, RMSE = 27.47 µg/m³
- Comparable to baseline but slightly worse

**Why this matters:**
- Shows PM2.5 follows strong temporal persistence (today predicts tomorrow well)
- Baseline already works well because pollution doesn't change drastically overnight
- Suggests adding weather features (wind, humidity, temperature) could improve predictions

**Future Improvements:**
- Add meteorological data (wind speed, temperature, humidity)
- Include emission-related features (traffic data, industrial activity)
- Try more advanced time series models (ARIMA, LSTM)

---

## Results and Conclusions

### Key Takeaways:

1. **Air Quality Trends:**
   - Overall improvement from 2021-2023, but still far above safe levels
   - 2024 shows slight deterioration, needs monitoring

2. **Seasonal Patterns:**
   - Winter and Post-Monsoon are worst seasons
   - Monsoon offers cleanest air due to rainfall
   - Strong, predictable seasonal cycles

3. **Pollutant Priorities:**
   - PM2.5 and PM10 are primary contributors to poor air quality
   - Focus control measures on particulate matter sources

4. **Event Impact:**
   - Lockdown significantly improved air quality (proves human activity impact)
   - Stubble burning causes massive pollution spikes (needs regional solutions)
   - Odd-even scheme limited success during adverse conditions

5. **Health Concerns:**
   - PM2.5 and PM10 exceed WHO limits 95%+ of days
   - Chronic exposure poses serious health risks
   - Urgent multi-sectoral intervention needed

6. **Predictive Capability:**
   - Current day's pollution strongly predicts next day
   - Adding weather data can improve forecasts
   - Foundation set for operational air quality prediction systems

### Recommendations:

1. **Immediate Actions:**
   - Target PM2.5 and PM10 reduction through vehicle emission controls
   - Address stubble burning in surrounding states (regional coordination needed)
   - Increase monitoring and public awareness during peak months

2. **Long-term Strategy:**
   - Promote cleaner transportation (electric vehicles, public transport)
   - Regulate industrial emissions and construction dust
   - Implement air quality forecasting system using weather data

3. **Policy Interventions:**
   - Coordinate with neighboring states on stubble burning
   - Time traffic restrictions during predicted high-pollution days
   - Create early warning systems for citizens

---

## Project Files

- `model.ipynb`: Complete analysis notebook with all code and visualizations
- `final_dataset.csv`: Original input dataset
- `Cleaned_Delhi_Air_Quality.csv`: Processed dataset with added features (Season, AQI_Category)
- `README.md`: This documentation file

---

## Technologies Used

- **Python 3**: Programming language
- **Pandas**: Data manipulation and analysis
- **NumPy**: Numerical computations
- **Matplotlib & Seaborn**: Static visualizations
- **Plotly**: Interactive visualizations
- **Scikit-learn**: Machine learning models and preprocessing
- **Statsmodels**: Time series analysis and decomposition

---

## Note

This project provides a comprehensive analysis of Delhi's air quality using data science techniques. All visualizations, statistical analyses, and models are documented step-by-step to ensure transparency and reproducibility. The findings support evidence-based decision-making for pollution control and public health protection.

---

## Sources and References

- WHO Global Air Quality Guidelines (2021): Defines recommended 24‑hour thresholds for PM2.5, PM10, NO₂, O₃, SO₂ and CO used in our exceedance analysis. [Link](https://www.who.int/publications/i/item/9789240034228?utm_source=chatgpt.com)

- Decadal Analysis of Delhi’s Air Pollution Crisis (2025): Synthesizes 10+ years of evidence on evolving sources and the real‑world effectiveness of mitigation measures up to 2024; useful context for interpreting trends and interventions. [arXiv](https://arxiv.org/abs/2506.24087)

- NeurIPS 2023 Datasets & Benchmarks paper: Curates resources such as AIRDELHI PM dataset and event‑impact studies (crop burning, Diwali, lockdown), highlighting data complexity and evaluation practices relevant to our analyses. [Proceedings](https://proceedings.neurips.cc/paper_files/paper/2023/file/ee799aff607fcf39c01df6391e96f92c-Paper-Datasets_and_Benchmarks.pdf)

- Delhi Odd‑Even Scheme details (Economic Times): Official dates, timings, exemptions and fines; referenced to contextualize policy periods marked in the event analysis. [Article](https://economictimes.indiatimes.com/news/india/delhi-odd-even-dates-restriction-timings-exemptions-fine-heres-all-you-should-know/articleshow/105114124.cms?from=mdr)

- IIT Delhi/Consortium report on Delhi air pollution (policy roadmap): Emphasizes that only long‑term structural changes (clean fuels, green transport, regulation of industrial/agricultural emissions) can deliver sustained improvement—supports our recommendations. [Report](https://cerca.iitd.ac.in/uploads/Reports/1576211826iitk.pdf)
