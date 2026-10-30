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

**Volume**

Dataset 1: 1,108,361 records

Dataset 2: 2,259,617 records

Dataset 3: 2,432,251 records


The datasets contain millions of records, which make data integration and filtering more demanding than a smaller dataset. Combining matching data across all datasets will require matching crash identifiers and accounting for factors such as crashes involving multiple vehicles carrying multiple people. We intend to use the full datasets initially, evaluating file sizes during data preparation and then cleaning and analyzing the records.


**Velocity**

Our project uses historical traffic crash records provided by the City of Chicago through the City of Chicago Data Portal. This information is not updated in real-time, and will instead serve as a historical dataset to be analyzed to identify historical patterns in vehicle crashes. Batch-processing will be sufficient for this project.


**Variety**

Our project utilizes three datasets sourced from the City of Chicago Data Portal. These datasets primarily contain tabular data such as CSV files. The main issue we expect to encounter when integrating these datasets is excessive information across multiple datasets. The crashes datasets identifies one crash at a time, while the vehicles and people datasets may contain multiple related records. We will standardize columns and handle missing values before continuing with analysis of the datasets.

**Veracity**

The datasets utilized in this project are sourced through the City of Chicago Data Portal. These datasets contain records on crashes under the jurisdiction of both the City of Chicago and the Chicago Police Department. Because these datasets are sourced through an official city portal, they provide a consistent source for data analysis. Individual records may contain missing information such as weather conditions and number of passengers in a given vehicle crash, which may interfere with combining datasets with joint information. We will account for columns with excessive missing data and determine which incomplete columns should be excluded.

**Value**

This project will analyze traffic crash data from multiple datasets detailing varying levels of information regarding recorded traffic crashes. By cleaning and combining crash-level, vehicle-level, and person-level information, we hope to uncover which combinations of environmental, geographic, and roadway factors are associated with injury-related crashes. Interpreting these findings will provide a clearer picture of injury-related crashes and improve roadway safety.

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

## 6. Limitations and Risks
1. **Not all crashes are recorded:** Coverage is limited to crashes reported through the CPD so crashes handled by other agencies may be absent.

**Possible effect on the project:** Counts may underrepresent all real-world crashes.

2. **Uneven early historical coverage:** Citywide coverage does not begin until September 2017.

**Possible effect on the project:** Earlier years may look artificially low or geographically uneven.

3. **Estimated conditions and reporting differences:** Weather/road conditions and posted speed limits may be imperfect.

**Possible effect on the project:** Recorded predictors can contain measurement error or bias.

4. **Missing or inconsistent data:** Some records may lack coordinates or important categories.

**Possible effect on the project:** Location analyses may exclude crashes and become less representative.

5. **Outcome timing and amendments:** Injury information may be corrected after initial reporting.

**Possible effect on the project:** Repeated exports may produce different totals.

6. **Class imbalance and leakage:** The injury class may be less common.

**Possible effect on the project:** Model evaluation may be inflated or misleading.

7. **Data access or schema changes:** Public data and API behavior can change.

**Possible effect on the project:** Re-running the pipeline may give different results.

8. **Computational constraints:** Free cloud resources may impose runtime, memory or usage limits.

**Possible effect on the project:** Full-data joins or training may be slow or restricted.

