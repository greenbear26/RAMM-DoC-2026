---
title: "Project - Phase 3"
date: 2026-06-04
draft: false
description: "Phase 3"
slug: "phase3post"
tags: ["project", "DataSets"]
authors:
  - "rishi_ponnapalli"
  - "alyssa_diwale"
  - "mihika_mehta"
  - "manav_mahida"
showAuthorsBadges: false
---

# Blog Post 3

## ML 1 - Predicting EP Meetings

### Model Refinements

Based on the feedback we got for Phase 2, we made several improvements to our first ML model. We kept linear Regression (for now) because it's interpretable and works for this type of supervised learning problem. 

The main changes we made were fixing a data leakage issue where our StandardScaler was incorrectly fit on the full dataset before splitting. We corrected this so the scaler only learns from training data. We also log-transformed our skewed input features( lobbying cost and EP access passes) and switched to predicting log(meetings) instead of raw meetings, which better reflected the linear relationship we identified in our EDA. We added a Members FTE Squared term so we could see non-linear effects of org size, and encoded interest type as separate variables. 

We also experimented with lobbying_gdp_ratio and inflation_rate and found out our r-squared went to 0.46(higher) when this was removed but with those values also being part of the World Bank API we need to see how we can still implement lobbying_gdp_ratio and inflation_rate maybe with different machine learning algorithms so going into phase 4 we plan to continue experimenting with different feature combinations and potentially explore other algorithms to see if we can still push the R^2 value higher. 


## ML 2 - Predicting Party Parliamentary Presence 

### What We're Predicting 

Our first visualization is looking at how lobbying expenditure is distributed across all organizations. We plotted it two ways which were raw and log-transformed.

For our second model we shifted from regression to classification. We are predicting whether a European political party is currently in parliament or not. The reason we chose this as our target is that being in parliament is a strong signal of relevance and influence. Parties in parliament have direct legislative power, meaning lobbyists and citizens alike should pay more attention to them. 


### Data Sources 

We used two new datasets. The first is the Populist dataset which labeled the European political parties across ideological dimensions including populists, far-right, far-left, and eurosceptic. The second is the ParlFacts dataset which contains part characteristics like political family, left-right positioning, state-market orientation, liberty-authority score, and EU stance.  


### Data Cleaning and Merging


We cleaned both datasets by keeping the columns we needed and filtering to EU member countries. We then merged them on party ID to create a combined dataset of 191 parties with 17 features. NaN values were filled with 0 and country was one-hot encoded giving us 41 total features for training. 


### Model 

We used logistic regression via SGDClassifier with a 70/30 train-test split, predicting in_parliament as our target variable.


### Results

The model achieved an accuracy of 55.2% with an F1-score of 0.54. This is a proof of concept that we plan to continue tuning in Phase 4.


