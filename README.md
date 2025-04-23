# Power Outage Analysis

by Michael Rubio (marubio@umich.edu)

---

## Introduction

Power outages can disrupt daily life, threaten safety, and cause significant economic damage—especially when they last for extended periods. For communities and utilities alike, being able to estimate how long an outage will last is a crucial step toward better preparation, faster recovery, and smarter resource allocation.

In this project, we aim to explore: **What factors influence the duration of a power outage?**  
We’ll use historical data on major outages to analyze trends and relationships, and ultimately build a predictive model that estimates how long an outage might last based on information available at the time it begins.

### Dataset Overview

We’re working with a dataset that includes major U.S. power outages from **January 2000 to July 2016**, totaling **1,534 entries**. For our analysis, we’ll focus on the following key features:

| Column              | Description                                                                                           |
|---------------------|-------------------------------------------------------------------------------------------------------|
| `CUSTOMERS.AFFECTED`| Number of customers impacted by the outage                                                            |
| `CLIMATE.CATEGORY`  | Climate classification of the year based on the Oceanic Niño Index (ONI) — *Warm, Cold, or Normal*    |
| `CAUSE.CATEGORY`    | General cause of the outage (e.g., severe weather, equipment failure)                                 |
| `OUTAGE.DURATION`   | Duration of the outage in minutes                                                                     |
| `TOTAL.SALES`       | Total electricity usage in the affected state (in megawatt-hours)                                     |
| `MONTH`             | Calendar month when the outage occurred                                                               |

These features provide a mix of environmental, economic, and temporal factors that we hypothesize may influence outage duration. Our goal is to understand these relationships and use them to improve outage planning and resilience.

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The original dataset was stored in an Excel spreadsheet and required several preprocessing steps before it could be analyzed:

- We removed the first five rows, which contained metadata and empty cells, not actual data.
- We then set the 6th row as the header row, which provided the actual column names.
- The 7th row (used for units) was discarded.
- The first column, which didn’t contain useful data, was also dropped.

After loading the cleaned data into a DataFrame, we selected the following columns for our analysis:

- `MONTH`
- `CLIMATE.CATEGORY`
- `CAUSE.CATEGORY`
- `CUSTOMERS.AFFECTED`
- `TOTAL.SALES`
- `OUTAGE.DURATION`
- `CLIMATE.REGION`

To make the data more interpretable, we renamed each column to be more human-readable and converted the `MONTH` column from numerical values (e.g., 1, 2, ..., 12) to full month names (e.g., "January", "February", etc.).

We also calculated the percentage of missing values in each column:

- **Month**: 0.59%  
- **Climate Category**: 0.59%  
- **Cause Category**: 0%  
- **Customers Affected**: 28.88%  
- **Total Sales**: 1.43%  
- **Outage Duration**: 3.78%  
- **Climate Region**: 0.39%

---

## Imputation

The `Customers Affected` column had the highest percentage of missing values (nearly 29%). Simply dropping these rows would result in a substantial loss of data. Upon inspection, we saw that missing values appeared to be random — they occurred across both long and short outages.

Because of this, we decided to **impute missing values with the median** of the non-missing values. Median imputation is robust to outliers and was appropriate given the wide variation in customer counts across outages.

After imputing the missing values in `Customers Affected`, we dropped rows with missing values in the remaining columns. These columns had relatively low percentages of missing data (each under 4%), so removing them did not significantly reduce the dataset size and ensured that the data was complete and clean for modeling.

---

### Distribution Before and After Imputation

We visualized the distribution of `Customers Affected` both **before** and **after** imputation to understand its effect:

#### Before Imputation

<iframe src="assets/customers_affected_before_imputation.html" width="800" height="500" frameborder="0"></iframe>

#### After Imputation

<iframe src="assets/customers_affected_after_imputation.html" width="800" height="500" frameborder="0"></iframe>

The original distribution was strongly **right-skewed**, with most outages affecting a relatively small number of customers and a long tail representing rare but very large-scale outages.

After applying **median imputation**, the overall shape of the distribution remained the same, preserving its skew. The only notable change is a visible spike at the median value — the location where missing values were filled in. This ensures consistency while maintaining the integrity of the data’s structure.

---
