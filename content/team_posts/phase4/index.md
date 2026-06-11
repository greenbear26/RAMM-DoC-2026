---
title: "Project - Phase 4"
date: 2026-06-11
draft: false
description: "Phase 4"
slug: "phase4post"
tags: ["project", "DataSets"]
authors:
  - "rishi_ponnapalli"
  - "alyssa_diwale"
  - "mihika_mehta"
  - "manav_mahida"
showAuthorsBadges: false

---

# Blog Post 4

## ML 1 - Predicting EP Meetings

### UI and Frontend Improvements

After getting feedback in Phase 3, we went back and fixed some issues with how the prediction page was displaying results. The biggest problem was that the output was just showing a raw number like 0.44 with no label, which isn't going to make sense to anyone who looks at it. We fixed this by updating the label to say "Predicted EP meetings" and displaying it as a clean metric card alongside lobbying cost and EP passes so users can actually understand what they're looking at.

We also got rid of the raw JSON block that was showing up under the prediction result. It was just dumping all the input variables as unformatted JSON on the screen which didn't look good and wasn't useful for the user. We replaced it with a simple caption that summarized the inputs in English.

One thing worth noting is that the country dropdown on the prediction page does not currently affect the model output. This is because country is not one of our feature columns — the model uses lobbying cost, EP passes, members FTE, and interest type as inputs. We kept the country field in the UI since it provides useful context, but it is a known limitation of the current model that geographic variation is not captured in the prediction.

### Interactive Scatter Plot

One of the bigger additions to this phase was an interactive scatter plot on the prediction page. After a user runs a prediction, they can now see a chart that plots all lobbying organizations by lobbying cost vs EP meetings on a log scale, with their predicted organization shown as a red star. This lets users actually see where they land compared to everyone else in the dataset, which is a lot more meaningful than just seeing a number. We also added a "Similar Organizations" section below the chart that shows up to 5 organizations with a similar lobbying cost range, each with a Save button so users can send them straight to the Organization Comparison page.

---

## ML 2 - Party Parliamentary Presence

### Connecting to the Lobbying Theme

One of the main pieces of feedback we got for ML 2 was that it felt disconnected from the lobbying side of the project. We were predicting whether a political party is in parliament based on ideology scores, but there was no obvious link to lobbying organizations. In Phase 4 we tried to fix this by bringing in the Integrity WatchMEP meetings dataset. This dataset has over 65,000 published lobbying meetings between MEPs and lobbying organizations, tagged by EP political group. We then mapped the national parties in our Populist and ParlFacts datasets to their corresponding EP groups using a party family mapping. This lets us connect the ideology features we use for the model to real lobbying activity that's actually happening in the European Parliament.

We also built a new page for the journalist persona called "Which Lobbyists Are Influencing EU Party Groups?" where users can pick an EP party group and see which lobbying organizations have met with them the most based on the Integrity Watch data. This pairs with the Party Parliament Prediction page to give journalists a more complete picture. They can now predict how likely a party is to hold seats, and then see which lobbyists are actively targeting the same party group.

### ML 2

ML 2 is still logistic regression using SGDClassifier, predicting in_parliament as the target. We use ideology features from Populist and ParlFacts including populist scores, far-right, far-left, eurosceptic flags, left-right positioning, and EU stance. We are currently sitting at 55.2% LOO-CV accuracy with an F1-score of 0.54.

---

## User Interface

# Figure 1
<img src="fig1.png" width="100%">

*Figure 1 - [Page Title]*

**Figure 1 Description**

[Description of the updated or new UI page shown in Figure 1.]

---

# Figure 2
<img src="fig2.png" width="100%">

*Figure 2 - [Page Title]*

**Figure 2 Description**

[Description of the updated or new UI page shown in Figure 2.]

---

## Routes

### New / Updated Routes

<style>
  .api-table {
    width: 100%;
    border-collapse: collapse;
    font-family: monospace;
    font-size: 14px;
    margin-top: 16px;
  }
  .api-table th {
    background-color: #2E2E2E;
    color: #F5F0E8;
    padding: 12px 16px;
    text-align: left;
    font-weight: 600;
    border: 1px solid #444;
  }
  .api-table td {
    padding: 12px 16px;
    border: 1px solid #D4C9B0;
    vertical-align: top;
    line-height: 1.5;
    background-color: #FAFAF7;
    color: #1A1A1A;
  }
  .api-table tr:hover td {
    background-color: #F0EBE0;
  }
  .route-cmd {
    font-family: monospace;
    font-weight: 600;
    color: #1A1A1A;
    white-space: nowrap;
  }
  .na {
    color: #A89F8C;
    font-style: italic;
  }
</style>

<table class="api-table">
  <thead>
    <tr>
      <th>Command</th>
      <th>GET</th>
      <th>POST</th>
      <th>PUT</th>
      <th>DELETE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><span class="route-cmd">/organizations</span></td>
      <td>Clouseau and Stromae want to search and filter organizations by country, interest type, and lobbying cost range.</td>
      <td>Admin wants to add a new organization to the database with lobbyfacts fields.</td>
      <td><span class="na">N/A</span></td>
      <td><span class="na">N/A</span></td>
    </tr>
    <tr>
      <td><span class="route-cmd">/organizations/{org_id}</span></td>
      <td>Clouseau wants to see a full org profile including lobbying spend, EP passes, meetings, expenditure history, and activities.</td>
      <td><span class="na">N/A</span></td>
      <td>Admin wants to update an existing organization's lobbyfacts data fields.</td>
      <td>Admin wants to remove an organization from the database.</td>
    </tr>
    <tr>
      <td><span class="route-cmd">/party/prediction</span></td>
      <td>Clouseau wants to run the ML model to get a parliamentary presence prediction for a specific party.</td>
      <td><span class="na">N/A</span></td>
      <td><span class="na">N/A</span></td>
      <td><span class="na">N/A</span></td>
    </tr>
  </tbody>
</table>
