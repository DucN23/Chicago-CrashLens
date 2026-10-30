# Chicago CrashLens
**Course:** CS 483  
**Project:** Chicago CrashLens  
**Team:** Htet Yan, Hafsa Rasheed, Duc Nguyen  
**Document status:** Version 1 — Data Discovery and Infrastructure  
**Prepared:** October 29, 2026

## 1. Project Goal
We aim to identify patterns associated with injury-related traffic crashes in Chicago. We will investigate how time, weather/road conditions and location relate to whether a reported crash involves injuries.

The problem matters because understanding where and what conditions crashes area more likely to involve injuries can help Chicago residents get a deeper insight to crash patterns and valuable support for transportation safety planning.

Our core question is:
**Which combinations of temporal, environmental, roadway, and geographic factors are associated with injuries in Chicago traffic crashes?**

## 2. Data Sources
We select three related datasets published through the City of Chicago Data Portal. We will use the following data:

- Historical Chicago traffic crash records including crash dates, times and locations.

- Information about weather, road conditions and speed limits.

- Crash data that include whether injuries or fatalities occurred.

Dataset 1: ([Chicago Traffic Crashes - Crashes](https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if/about_data))

Dataset 2: ([Chicago Traffic Crashes - Vehicles](https://data.cityofchicago.org/Transportation/Traffic-Crashes-Vehicles/68nd-jvt3/about_data))

Dataset 3: ([Chicago Traffic Crashes - People](https://data.cityofchicago.org/Transportation/Traffic-Crashes-People/u6pd-qa9d/about_data))

Licensing and access: The data is publicly accessible for analysis via Chicago Data Portal API, the portal allows public viewing and CSV downloads, and the portal's dataset licensing information refers users to the City's Terms of Use.

## 3. Dataset Description

**Dataset 1: Chicago Traffic Crashes - Crashes**

- Information about traffic crashes on city streets within the City of Chicago limits and under the jurisdiction of the CPD.

- Records: 1,108,361

- Relevant Columns: Crash Date, Speed Limit, Weather and Lighting Conditions, Crash Type


**Dataset 2: Chicago Traffic Crashes - Vehicles**

- Information about vehicles involved in a traffic crash within the City of Chicago limits and under the jurisdiction of the CPD.

- Records: 2,259,617

- Relevant Columns: Crash Date, Number of Passengers, Vehicle Make, Vehicle Model, License Plate State


**Dataset 3: Chicago Traffic Crashes - People**

- Information about people involved in a crash and if any injuries were sustained within the City of Chicago limits and under the jurisdiction of the CPD.

- Records: 2,432,251

- Relevant Columns: Crash Date, City, Zipcode, Sex, Age, Driver’s License State, Airbag Deployed, Ejection, Injury Classification


We intend to integrate data from all 3 datasets.


## 4. Analysis of the 5 Vs


## 5. Data Preparation
1. **Export the three CSV files;** record date, published metadata, exact row counts, file sizes and sources.
2. **Inspect schemas and quality.** Confirm types, missing values, identifier uniqueness and categories.
3. **Clean and standardize.** Standardize missing or unknown categorial values; check numeral ranges.
4. **Choose the analysis window.** Start with records from 2018, then set a fixed cutoff and document any exclusion.
5. **Define the target.** At the crash level, set injury_crash = 1 when the total injury is more than zero, otherwise 0.
6. **Build joins.** Aggregate Vehicles and People records by crash_record_id before combining them in the Crashes table.
7. **Create predictors.** Derive hour, day, month, location grouping and weather/road categories. 
8. **Write analysis-ready data.** Store a reproducible table, potentially partitioned by crash year, with a brief data dictionary.
9. **Prepare evaluation data.** Establish training or validation data and address class imbalance using methods that do not contaminate the test set.

**Expected challenges**: The initial CSV exports may be substantial; the three tables have different row granularities, and missing values need careful treatment, and incorrect joins or outcome leakage could result in analyses and models being misleading. 
