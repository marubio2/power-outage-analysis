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
