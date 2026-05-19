---
title: "Project - Phase I"
date: 2026-05-18
draft: false
description: "Our Idea"
slug: "phase1post"
tags: ["project", "Setup"]
authors:
  - "rishi_ponnapalli"
  - "alyssa_diwale"
  - "mihika_mehta"
  - "manav_mahida"
showAuthorsBadges: false
---

# Project Description! 

Understanding who influences EU policy, and how much they spend doing it, is really hard to figure out. Lobbying data is technically available to the public, but it’s scattered, confusing, and pretty inaccessible for most people. We are building a web application to address the lack of lobbying transparency by letting users search any policy area and immediately see which organizations are lobbying on it, how much money they are spending, where they’re from, and what industry they represent. 

To make the analysis more meaningful, we’re combining lobbying data from [LobbyFacts.eu](https://www.lobbyfacts.eu/) with World Bank API data including GDP, population, and government transparency scores, to add economic and political context to the lobbying patterns we’re uncovering. 



Our app is designed for three types of users: investigative journalists following the money, political science researchers looking for patterns, and everyday citizens who just want to understand who is shaping the policies that affect them.

## Data Sources 
1 https://datahelpdesk.worldbank.org/knowledgebase/topics/125589


2 https://api.worldbank.org/v2/country/all/indicator/NY.GDP.MKTP.CD?format=json&per_page=300&mrv=1


3 https://api.worldbank.org/v2/country/all/indicator/SP.POP.TOTL?format=json&per_page=300&mrv=1




# User Personas

## Tintin
Bio: International Independent Investigative Journalist
A sharp and fiercely independent investigative journalist and reporter who travels the world exposing corruption, political schemes, and works informing citizens on hard facts using raw data. He has recently shifted his focus to Europe, and more specifically Belgium, as he is researching lobbyists and European Politics.

1. As an investigative journalist, I want to research policy topics and see a breakdown of the various top organizations involved so that I can report which sectors are most influential.
2. As an investigative journalist, I want to view chart comparisons of lobbying organizations and compare factors like budgeting, European Parliament meetings, and financials so that I will be able the groups with the most financial weight behind decisions
3. As an investigative journalist, I want to view which organizations have “high influential” predictions so that I know where to prioritize the critical aspects of policy research.
4. As an investigative journalist, I want to be able to export my filtered search queries and the associated ML influence prediction models into a clean Excel file so I can back my research up with hard data.
5. As an investigative journalist, I want to view historical data of an organization's data like expenditures and meeting timelines so that I can see specific activity and major policy shifts.

## Jacques Clouseau:
Bio: A world-class political science researcher
Originally a crime detective, Clouseau now focuses his detective skills on political groups in the EU. He is very attentive to detail, and likes to collect as much evidence as he can in order to make accurate conclusions. Recently, he has changed his focus to analyzing lobbying groups and determining their goals and effectiveness.

User stories
1. As a poly-sci researcher, I want to search for a policy area like "AI" or "Climate" and immediately see which organizations that have historically lobbied on it, so that I can map the landscape of interest groups involved in a given policy debate.

2. As a poly-sci researcher, I want to see how much money each organization is spending on lobbying for a specific topic, so that I can compare relative financial power across interest groups and identify which groups are likely driving the policy agenda.

3. As a poly-sci researcher, I want to filter lobbying activity by industry and country of origin, so that I can understand whether domestic or foreign corporate interests are dominating a particular policy area.

4. As a poly-sci researcher, I want an ML-generated prediction of which lobbyists are most likely to be influential, so that I can prioritize my case studies on organizations who are most likely shaping actual policy outcomes.

## Stromae:
Bio: A 19 year old, a first-year university student in Leuven who just voted in his first European election. He follows politics loosely through TikTok and Instagram and is curious about how the EU actually works, but the institutional websites feel impenetrable and dry. He has a short attention span for dense text but loves visual explainers and "wait, what?" moments that he can screenshot and send to his group chat. He's not looking for a research tool; he wants to learn enough to feel informed and form opinions.


User stories
1. As a curious newcomer, I want a one-screen explanation of what the European Parliament does and who can access it, so that I can build a basic mental model without reading a textbook.

2. As a curious newcomer, I want to see an influence score visualized as a simple chart or ranking, so that I can grasp who matters without interpreting raw numbers.

3. As a curious newcomer, I want short, plain-language definitions when I hover over or tap on jargon, so that I don't feel lost or stupid while exploring.

4. As a curious newcomer, I want to screenshot a clean visual and share it on my Instagram story, so that I can show friends what I learned without writing a caption.

5. As a curious newcomer, I want a "did you know?" or "surprising fact" section, so that I can discover interesting things without knowing what to search for.





