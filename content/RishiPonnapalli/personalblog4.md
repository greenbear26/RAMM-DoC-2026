---
title: "Personal Blog 4"
date: 2026-06-12
draft: false
description: "Blog for phase 4 of the project"
slug: "blog4"
tags: ["authors", "config", "docs"]
authors:
  - "rishi_ponnapalli"
showAuthorsBadges : true
---

# Phase 4 blog

### Contributions
This week, we finalized our project's structure and implemented all features. I worked on a bit of everything, from inserting our data into the database to finalizing our ml models to iterating on the frontend. On the database side, we had to modify our tables to actually align with the data we had, so I worked specifically on modifying and inserting the data for the European parties we had. I also implemented a few routes for this data so that the frontend could access this data. Our ML models were mostly done, but I did work a bit on modifying the page layout for our second ML model related to parties. Also, in general, I was debugging lots of problems we had with implementation from all of my teammates, and working to fix the issues so that the app functioned properly. This was actually the hardest part, as the issues I was debugging ranged from streamlit issues to API issues, and it was really challenging to solve them all.

### ML Models
We have 2 ML models in our project. One is a linear regression model that predicts the amount of European Commission meetings for a lobbying organization based on their profile, like their lobbying spend and amount of members. We trained the model and stored the weights in the database, as well as the scaling parameters. Then, we wrote a route that accepts the paramaters needed to use the model, which then queries the database, gets the weights and scaling, and performs the calculation to return a predicted value. A note here is that our model actually predicts the log(meetings), so we have to undo this operation when actually running the model. We verified the assumptions of this by plotting the y predicted vs y actual and plotting the residuals. These plots showed a relatively linear relationship and not too much pattern on the residuals, however, it also shows that there is room for improvement.

Our second model is a logistic regression model that predicts whether a European party will be in parliamnent or not based the party's ideology, such as if they're eurosceptic or populist. We trained this model with stochastic gradient descent with the log-loss function, and after running Leave One Out Cross Validation, we had an F1 score 0.6. The confusion matrix we generated showed pretty even accuracy throughout, and the correlation between all of input parameters wasn't too high, which promote the validity of this model. This model also has weights and scaling stored in the database, and a similar route was written so that we could use it.

### Architecture
The database has many tables related to our app. We have an app user table that contains identification information for any user logging in. Then, we have 4 tables used for various functionality on the site:

1. **country_indicator**: This stores gdp and inflation data for all countries in the EU, coming from World Bank. It's used in our lobbying model to infer parameters from a country selection.
2. **lobbying_organization**: This stores various information about organzations, including their lobbying cost. This is used in our exploring and comparisons of organizations to provide the data necessary to do it.
3. **party_info**: This stores data pertaining to parties in the EU, like whether they're populist or not. This is used when we explore data pertaining to national parties in the EU.
4. **party_to_lobby_info**: This stores data related to lobbying organizations interacting with European Parliament parties. This is used in the page where we display this data to the user.

There are routes that enable the usage of all of these datasets. They mostly consist of GET requests that allow for filtering, allowing the frontend to easily get this data. Also, there are the tables for the ML models that are mentioned above.

### Reflection
This entire program has been a lot of fun. I learned many new things, both technical skills and also the skills required to live in a different country. While there were many challenges, especially with actually creating the project properly, I learned a lot about how to conquer those challenges, and I am proud of the app we ended up with. Thank you so much for organizing this program and allowing me to have such a great experience!