---
title: "Personal Blog 4"
date: 2026-05-18
draft: false
description: "Blog for assignment 4 of the project"
slug: "blog4"
tags: ["authors", "config", "docs"]
authors:
  - "manav_mahida"
showAuthorsBadges : true
---

# Phase 4 Blog

For Phase IV, my work centered on completing the frontend for two of our three personas, the European Citizen and the Political Science Researcher.

For the Citizen persona, I built out the full UI flow starting from the Citizen Home page, which greets Stromae and lets them pick policy areas and countries they care about through an interactive card picker that submits preferences to the API and surfaces relevant lobbying organizations influencing those policies. This persona is designed around a regular European citizen who wants to understand which lobbying organizations are influencing the policies that affect their daily life, so the interface prioritizes simplicity and discoverability over raw data.

For the Political Science Researcher persona, I built the full suite of pages, the Researcher Home, and the Organization Comparison page which lets researchers search for organizations, save them, and select two to compare side by side with lobbying spend, policy areas, and ML influence scores. In Phase IV I refined this page significantly, removing the Score Comparison tab that cluttered the layout and replacing it with policy area and country filter dropdowns so researchers can narrow results by the 9 policy categories derived directly from our lobbyfacts dataset. I also wired up an Apply button flow to keep the interface clean.

Beyond the UI work I fixed the policy area dropdown being empty on fresh clones by seeding the `policy_area` table in `02-inserts.sql`, and added `pandas` to `api/requirements.txt` so the data load works out of the box for anyone cloning the repo.

Coming into this program I knew almost nothing about European politics or even coding. This experience has definitely been difficult at times but I had so much fun and learned more than I expected about a topic that was completely new to me. I am going to miss everyone I met here, the people made this whole thing worth it.

