# What Effects How Long a Power Outage Lasts?

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

These features provide a mix of environmental, economic, and temporal factors that we hypothesize may influence outage duration. Our goal is to understand these relationships and use them to improve outage planning.

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

To make the data more interpretable, we renamed each column to be more readable and converted the `MONTH` column from numerical values (e.g., 1, 2, ..., 12) to full month names (e.g., "January", "February", etc.).

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

 <iframe
 src="assets/affected_before.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

#### After Imputation

<iframe
 src="assets/affected_after.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
The original distribution was strongly **right-skewed**, with most outages affecting a relatively small number of customers and a long tail representing rare but very large-scale outages.

After applying **median imputation**, the overall shape of the distribution remained the same, preserving its skew. The only notable change is a visible spike at the median value — the location where missing values were filled in. This ensures consistency while maintaining the integrity of the data’s structure.

## Univariate Analysis 

Here, we're going to look at the distribution of the **Cause Category**.

<iframe
 src="assets/cause_distribution.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
We see from the histogram that the majority of major power outages are caused by severe weather and intentional attack (such as vandalism). This shows that power outages are mostly caused by weather, and leads us to think about ways to protect our power from severe weather conditions.

Now we're going to look at the distribution of the **Climate Category**.

<iframe
 src="assets/climate_category.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
We see from the histogram that majority of power outages occur during normal climate conditions, followed by cold then warm episodes. This shows that power outages aren't necessecarily driven by deviations in the ONI (Oceanic Nino Index).

## Bivariate Analysis 

Now we're going to look at the relationship between **Climate Region vs Outage Duration**.

<iframe
 src="assets/region_duration.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
From the boxplot, we can see that the East North Central region has the highest median outage duration of 3120 minutes and the West North Central region has the lowest median outage duration of 59.5 minutes. We can also see that other than the East North Central region and the West North Central region, all the regions do have a large number of outliers.

Now we're going to look at the relationship between **Total Sales vs Outage Duration**.

<iframe
 src="assets/sales_box.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
The box plots show that lower total sale ranges (0-15M) tend to have wider spread and higher median outage duration times, while higher total sale ranges exhibit shorter and more consistent outage duration times. This suggests that higher-selling regions have better recovery times.

## Aggregates

To better understand how power outages vary throughout the year, I created a pivot table that groups the data by month and summarizes the average number of customers affected and the average outage duration.

### Average Outage Statistics by Month

| Month      | Customers Affected | Outage Duration (minutes) |
|------------|--------------------|----------------------------|
| January    | 132,934.647        | 3,387.947                  |
| February   | 110,949.386        | 2,497.144                  |
| March      | 88,996.085         | 3,265.894                  |
| April      | 85,433.224         | 1,493.860                  |
| May        | 100,688.754        | 2,093.424                  |
| June       | 108,803.649        | 1,957.654                  |
| July       | 94,754.036         | 2,315.897                  |
| August     | 172,845.053        | 2,428.477                  |
| September  | 214,190.804        | 4,294.522                  |
| October    | 153,130.198        | 3,645.774                  |
| November   | 90,828.000         | 1,728.159                  |
| December   | 134,032.402        | 3,311.794                  |


The pivot table shows that September had the highest average number of customers affected, with over 200k customers affected.

We also generally see that late-summer and fall months tend to show more severe outages, having higher average number of customers affected and higher average outage duration than other times of the year.

---

## Framing a Prediction Problem

For this project, I chose to predict the **duration of a power outage (in minutes)**. This is a regression problem because the target variable, `OUTAGE.DURATION`, is a continuous numerical value. We are trying to estimate how long an outage will last based on available features, rather than classifying it into a discrete category.

This prediction is meaningful because outage duration directly impacts **customer experience**, **emergency preparedness**, and **operational planning**. An accurate estimate of how long a power outage might last can help communities make more informed decisions about food storage, medical equipment, evacuation, and infrastructure deployment.

To evaluate model performance, I chose **Mean Squared Error (MSE)** as the evaluation metric. MSE penalizes larger errors more than smaller ones, which is ideal in this context where **large mispredictions (underestimates especially)** can have serious consequences.

### Features Used for Prediction

To train my model, I used the following features:

- **Month**: The month when the power outage occurred  
- **Cause Category**: The high-level reason for the outage (e.g., severe weather, equipment failure)  
- **Climate Category**: Annual climate condition based on the Oceanic Niño Index (ONI) — *Warm, Cold, or Normal*  
- **Climate Region**: The general geographic region of the U.S. in which the outage occurred  
- **Total Sales**: The total electricity usage in the state (in megawatt-hours)  
- **Customers Affected**: The number of customers impacted by the outage  

All of these features are known at the time the outage is reported, ensuring that the model is trained only on **real-time accessible inputs**, making it suitable for deployment in emergency forecasting or outage response tools.

## Baseline Model

To establish a baseline for prediction, I used a **Linear Regression** model to predict power outage duration based on six features available at the time an outage begins.

### Features Used

**Quantitative:**
- `Customers Affected` — number of customers impacted by the outage
- `Total Sales` — total electricity usage in the state (in megawatt-hours)

**Nominal Categorical:**
- `Cause Category` — reason for the outage
- `Month` — month when the outage occurred
- `Climate Category` — annual climate condition based on the Oceanic Niño Index
- `Climate Region` — general geographic region of the U.S.

All categorical variables were **one-hot encoded**, with the first category dropped to prevent multicollinearity.

---

### Model Implementation

The model was implemented using a `scikit-learn` pipeline. I split the dataset into an 80/20 train-test split and evaluated the model using **Mean Squared Error (MSE)** on the test set.

- **Baseline Test MSE**: `41,761,280`

This relatively high error suggests that the linear model struggles to generalize well to unseen data. Outage durations may depend on more complex relationships (e.g., nonlinear effects, interactions) that linear regression is not equipped to capture.

---

### Residual Analysis

To better understand the model’s behavior, I visualized the **residuals** (prediction errors). Residuals are the differences between the actual and predicted durations:

```py
residuals = y_test - y_pred
```
Visualizing the residuals helps show us whether the model under-predicts or over-predicts.

<iframe
 src="assets/residual_plot.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
The histogram shows that most residuals are small, clustering around 0. The distribution is right-skewed, meaning that there are a large number of cases where the model underestimates the outage duration. These outliers may represent long outages that our linear model doesn't capture, showing that the baseline model is limited in handling extreme cases.

## Final Model

To improve on the baseline Linear Regression, I built a **Random Forest Regressor** model, which is better suited for capturing **nonlinear relationships** and **complex interactions** between features.

### New Engineered Features

To enhance model performance, I engineered two additional features:

#### 1. Log-Transformed `Customers Affected`
The number of affected customers spans several orders of magnitude. Applying a **logarithmic transformation** reduces skew and allows the model to better distinguish between small-scale and large-scale outages.

> *“Since the number of affected customers varies drastically, the log transformation helps reduce the influence of extreme values and improves the model’s ability to learn meaningful patterns.”*

#### 2. `Season` Feature (from `Month`)
Months were grouped into **four seasons** (Winter, Spring, Summer, Fall), which provide more generalizable weather-related context.

> *“I created a `Season` feature because seasonal conditions strongly influence outage patterns. This helped reduce noise from month-to-month variability and allowed the model to capture broader climate effects.”*

---

### Hyperparameter Tuning

I used `GridSearchCV` to tune key hyperparameters of the `RandomForestRegressor`, evaluating each combination using **5-fold cross-validation** and **negative MSE** as the scoring metric. The best-performing parameters were:

- `n_estimators = 200`  
- `max_depth = 10`  
- `min_samples_split = 2`

---

### Final Model Performance

The final model achieved a **test MSE of 32,717,926**, compared to the baseline MSE of **41,761,280**.

This ~9 million-point reduction in MSE suggests that the final model:
- **Generalizes better to unseen data**
- **Handles nonlinearities more effectively**
- **Captures more nuanced outage relationships**

---

## Conclusion 

Our analysis shows that outage duration can be reasonably estimated using factors like cause category, customer scale, and time of the year. While the baseline **Linear Regression** model provided a starting point, the **Random Forest Regressor** model with feature engineering significantly improved predictive accuracy. With further refinement, this kind of model could help utilities make faster decisions and allocate resources more effectively during emergencies.
