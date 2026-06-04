---
title: "Project - Phase 2"
date: 2026-05-27
draft: false
description: "Phase 2"
slug: "phase2post"
tags: ["project", "DataSets"]
authors:
  - "rishi_ponnapalli"
  - "alyssa_diwale"
  - "mihika_mehta"
  - "manav_mahida"
showAuthorsBadges: false
---

# Blog Post 2

## Data Curation and Cleaning

Our final dataset merges two sources. The first is LobbyFacts, which contains 16,862 registered EU lobbying organizations with fields including lobbying cost, EP meeting counts, EP access passes, number of staff, country of headquarters, and whether the org lobbies for itself or on behalf of clients. The second is the World Bank API, from which we pulled GDP, population, and inflation rate for every country in the world.

We joined the two datasets on country name, achieving a 97.9% match rate. Before merging we cleaned the LobbyFacts data by standardizing country names from ALL CAPS to Title Case, filling missing meeting and pass counts with 0 (since absence of recorded data means no documented access, not bad data), and removing the 5,333 organizations that had zero lobbying cost recorded since these provide no useful financial signal. Our final working dataset contained 11,529 organizations.

## Exploratory Data Analysis

### EDA Plot 1 — Distribution of Lobbying Spend

<img src="eda_plot1_lobbying_cost.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Our first visualization is looking at how lobbying expenditure is distributed across all organizations. We plotted it two ways which were raw and log-transformed.

The raw distribution is very right-skewed. The vast majority of organizations spend under €50,000 on lobbying while a small amount spent millions. Some examples are how Meta spends €10M, Fleishman-Hillard spends €12.7M. All lobbying costs are reported in euros which is provided by LobbyFacts. Also, the x-axis on the raw plot is in scientific notation, the label 1e7 means the values are multiplied by 10,000,000, so 1.2 on the axis represents €12,700,000. So on a normal scale these outliers make every other organization invisible.

The log-transformed plot uses log(x+1), which is also written as log1p. It compresses the scale so that the smaller organizations become visible. Another thing to note is that organizations with zero lobbying cost recorded stay at 0 on the log scale even after transformation, since log(0 + 1) = 0. This accounts for the spike at 0 on the right plot, it shows the organizations with no reported lobbying spend and is not a data error. 

This finding matters because it shows how lobbying power is not evenly distributed. A tiny number of well-funded organizations dominate access to EU institutions. Also, it confirmed to us that we needed to use a log scale when displaying spending data in the app, and that we needed to log-transform the lobbying cost feature before putting it into our ML model so that the extreme values dont distort predictions, which we implemented in our final model.


### EDA Plot 2 — Lobbying Spend vs EP Meetings

<img src="eda_plot2_spend_vs_meetings.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />


Our second visualization asked an important question for our project which was if spending more money actually gets you more meetings. 
The answer is yes and our EDA suggests a positive relationship. We found a correlation of 0.565 between lobbying cost and meeting count. That is a moderately strong positive relationship, meaning organizations that spend more do tend to secure more documented parliamentary access. This scatter plot only includes organizations with both a recorded lobbying cost and at least one recorded meeting, which is why the spike at log(lobbying cost) = 0 from the distribution plot is no longer visible here. Those zero-cost organizations were filtered out before plotting since they provide no useful signal for the relationship between spending and access. 


### Model Verification

<img src="image copy.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

<img src="image copy 2.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

<img src="feature_relationships.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

We added a few plots to verify that our model was working well and to tune our model parameters. While the plots don't demonstrate that our model is perfect, it demonstrates that the model has room to grow, and we hope to imrpove it over the next few weeks.

## Machine Learning — Predicting Parliamentary Influence

### Why We Predict Meetings

57% of organizations in our dataset,9,686 orgs, have no recorded meeting data. Without predictions, we cannot rank or score these organizations at all, which means our app can only meaningfully serve users for less than half the dataset. The ML model solves this by learning patterns from the 43% of orgs that do have meeting data and applying those patterns to predict meetings for the rest.

### Model — Linear Regression

For our Phase II proof-of-concept we implemented a Linear Regression model. We chose Linear Regression because it is interpretable, we can directly see how much each feature contributes to the prediction.

We trained on 7,176 organizations that have recorded meeting data, using an 80/20 train/test split — meaning 5,664 organizations were used to train the model and 1,417 were held out to test how accurately it predicts on data it has never seen. The features we fed the model were lobbying cost, EP access passes, interest type (whether the org lobbies for itself or on behalf of clients), and the lobbying-to-GDP ratio from our World Bank merge.

### Results

Our model achieved an R² score of 0.41 and an RMSE of approximately 20 meetings. The R² score means the model explains 41% of the variance in meeting counts. 0.41 isn't the most ideal score so for Phase III we plan to look for more datasets, particularly ones that include the specific policy areas each organization lobbies on and also explore more sophisticated modeling approach that could better capture these non-linear relationships and improve predictive accuracy for our users.

## ER Diagram

Tintin ER Diagram:
<img src="erdiagram_tintin.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Clousaeu ER Diagram:
<img src="erdiagram_clousaeu.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Stromae ER Diagram:
<img src="erdiagram_stromae.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />


Global ER Diagram:
<img src="erglobal.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

## Relational Models

Tintin Relational Model:
<img src="tintinrelational.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Clousaeu Relational Model:
<img src="clouseaurelational.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Stromae Relational Model:
<img src="stromaerelational.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Global Relational Model:
<img src="globalrelational.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />


## Wireframes

Main Page:
<img src="wireframe_main.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Tintin:
<img src="tintinwireframe_1.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

<img src="tintinwireframe_2.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Clouseau Page:
<img src="clouseauwireframe_1.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

<img src="clouseauwireframe_2.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

Stromae Page:
<img src="stromaewireframe_1.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

<img src="stromaewireframe_2.png" style="transform: rotate(0deg); display: block; max-width: 100%;" />

## DDL for Global Data Model

<details>
<summary>View full code</summary>
CREATE TABLE country (
country_code VARCHAR(10) PRIMARY KEY,
name VARCHAR(100) NOT NULL,
region VARCHAR(100),
income_group VARCHAR(100)
);

CREATE TABLE country_indicator (
indicator_id INTEGER PRIMARY KEY,
country_code VARCHAR(10) NOT NULL,
year INTEGER NOT NULL,
gdp_usd FLOAT,
population INTEGER,
inflation_rate FLOAT,
source VARCHAR(255),
FOREIGN KEY (country_code) REFERENCES country(country_code)
);

CREATE TABLE industry (
industry_id INTEGER PRIMARY KEY,
name VARCHAR(100) NOT NULL
);

CREATE TABLE policy_area (
policy_area_id INTEGER PRIMARY KEY,
name VARCHAR(100) NOT NULL,
description VARCHAR(10000),
tags VARCHAR(255)
);

CREATE TABLE organization (
org_id INTEGER PRIMARY KEY,
name VARCHAR(255) NOT NULL,
lobbyfacts_url VARCHAR(500),
members_eu INTEGER,
lobbying_cost FLOAT,
interest_represented VARCHAR(255),
country_code VARCHAR(10),
industry_id INTEGER,
FOREIGN KEY (country_code) REFERENCES country(country_code),
FOREIGN KEY (industry_id) REFERENCES industry(industry_id)
);

CREATE TABLE lobbying_activity (
activity_id INTEGER PRIMARY KEY,
org_id INTEGER NOT NULL,
policy_area_id INTEGER NOT NULL,
eu_institution VARCHAR(255),
activity_type VARCHAR(100),
description VARCHAR(10000),
source VARCHAR(255),
start_date DATE,
end_date DATE,
FOREIGN KEY (org_id) REFERENCES organization(org_id),
FOREIGN KEY (policy_area_id) REFERENCES policy_area(policy_area_id)
);

CREATE TABLE expenditure_record (
expenditure_id INTEGER PRIMARY KEY,
org_id INTEGER NOT NULL,
policy_area_id INTEGER,
year INTEGER NOT NULL,
amount_eur FLOAT,
amount_range_min_eur FLOAT,
amount_range_max_eur FLOAT,
currency VARCHAR(20),
source VARCHAR(255),
FOREIGN KEY (org_id) REFERENCES organization(org_id),
FOREIGN KEY (policy_area_id) REFERENCES policy_area(policy_area_id)
);

CREATE TABLE meeting (
meeting_id INTEGER PRIMARY KEY,
org_id INTEGER NOT NULL,
eu_body VARCHAR(255),
meeting_date DATE,
subject VARCHAR(10000),
attendees_count INTEGER,
source VARCHAR(255),
FOREIGN KEY (org_id) REFERENCES organization(org_id)
);

CREATE TABLE access_pass (
pass_id INTEGER PRIMARY KEY,
org_id INTEGER NOT NULL,
person_name VARCHAR(255),
role_title VARCHAR(255),
eu_body VARCHAR(255),
issue_date DATE,
expiry_date DATE,
source VARCHAR(255),
FOREIGN KEY (org_id) REFERENCES organization(org_id)
);

CREATE TABLE influence_prediction (
prediction_id INTEGER PRIMARY KEY,
org_id INTEGER NOT NULL,
policy_area_id INTEGER,
model_version VARCHAR(100),
run_date DATE,
influence_score FLOAT,
influence_class VARCHAR(100),
top_features_json VARCHAR(10000),
FOREIGN KEY (org_id) REFERENCES organization(org_id),
FOREIGN KEY (policy_area_id) REFERENCES policy_area(policy_area_id)
);

CREATE TABLE app_user (
user_id INTEGER PRIMARY KEY,
role VARCHAR(100),
email VARCHAR(255) NOT NULL,
created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE saved_query_export (
export_id INTEGER PRIMARY KEY,
user_id INTEGER NOT NULL,
query_json VARCHAR(10000),
created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
file_format VARCHAR(50),
FOREIGN KEY (user_id) REFERENCES app_user(user_id)
);


</details>