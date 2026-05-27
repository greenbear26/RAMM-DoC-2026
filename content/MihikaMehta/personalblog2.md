---
title: "Phase II – Individual Reflection"
date: 2026-05-27
draft: false
description: "My contributions to Phase II of the RAMM lobbying transparency project"
authors:
  - "mihika_mehta"
---

### Data Cleaning and Merging

For the Data Cleaning and Merging I helped clean the LobbyFacts dataset and merging it with the World Bank API data. This involved standardizing country names from ALL CAPS to Title Case so they could match World Bank records, filling missing meetings and EP pass counts with 0, and removing organizations with no recorded lobbying spend. After cleaning we went from 16,862 organizations down to 11,529 usable records. 


### Exploratory Data Analysis

I built both EDA visualizations for the team blog post. The first plot showed the distribution of lobbying expenditure — which turned out to be extremely right-skewed, with most organizations spending under €50K while a handful spend millions. This was actually a really important finding because it told us we needed to log-transform the data for both visualization and ML purposes.

The second plot was my personal favorite. It was a scatter plot of lobbying spend vs EP meetings that showed a correlation of 0.565. That number basically proved the core premise of our entire app which was that money does buy parliamentary access. Seeing that come out of real data was actually an exciting moment.  


### ML Model

I contributed to training the Linear Regression model that predicts EP meeting counts for organizations with no recorded data. We used an 80/20 train/test split and achieved an R² of 0.41. While there's definitely room to improve, Linear Regression can't capture the non-linear complexity of lobbying behavior, it's a solid proof of concept for Phase II and gives us a clear direction for Phase III.

The biggest difficulty was that 57% of our organizations had no meeting data at all. This meant our training set was smaller than ideal and the model had to generalize a lot. For Phase III I'd like to explore whether adding policy area data could help the model make better predictions by giving it more context about what each org is actually lobbying on.

## Looking Ahead

Phase II pushed me to think more carefully about what "meaningful" data analysis actually means, not just running code but asking whether the outputs actually answer real questions for real users. The 0.565 correlation between spend and meetings is the kind of finding that makes Tintin's user story feel achievable. Phase III is where we get to actually build that.
