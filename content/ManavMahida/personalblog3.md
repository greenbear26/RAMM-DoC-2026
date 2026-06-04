---
title: "Personal Blog 3"
date: 2026-05-18
draft: false
description: "Blog for assignment 3 of the project"
slug: "blog3"
tags: ["authors", "config", "docs"]
authors:
  - "manav_mahida"
showAuthorsBadges : true
---

# Phase 3 Blog
For Phase III, my responsibilities were on the CS side covering the backend REST API, database schema, and frontend pages. I revised the SQL DDL based on Phase II reviewer feedback, adding CREATE DATABASE IF NOT EXISTS, audit trail columns to every table, expanding the app_user table, and adding ml_model_weights and updated influence_prediction tables to support storing trained model parameters so the model doesn't retrain on every prediction call.

I designed and implemented 10 Flask routes across 4 blueprints, grounded in our actual datasets from lobbyfacts and the World Bank. The routes cover searching and filtering organizations, fetching full org profiles with expenditure history, creating, updating, and deleting organizations, retrieving top spenders by lobbying cost, fetching World Bank country indicators including GDP and fossil fuels, saving user preferences, and running ML influence predictions that join lobbyfacts and World Bank features together.

On the frontend I built the Home page with the hero search bar and persona login buttons, the Citizen Home page for Stromae with the policy and country card picker that submits preferences to the API, and the Organization Comparison page for Clouseau with side-by-side spend, policy areas, and ML influence score comparison.

Our visit to Eurostat was a highlight of the week. Walking into the institution whose data we have been pulling into our app the entire program made everything feel real. Hearing about how they standardize data submissions across all 27 EU member states gave me a much deeper appreciation for the cleaning decisions we made in Phase II.