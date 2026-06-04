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

### Routes
import logging
logger = logging.getLogger(__name__)

import streamlit as st
from modules.nav import SideBarLinks

st.set_page_config(page_title="REST API Matrix", layout='wide')

SideBarLinks()

st.markdown("# Routes")
st.sidebar.header("REST API Matrix")

st.markdown("## REST API Matrix")
st.write(
    "To start planning our REST API, we used the functionality planned for in our wireframes "
    "to devise a list of routes needed in our app. Then listed these in this REST API Matrix "
    "along with the scenarios in which these routes will be used within the app."
)

routes = [
    {
        "command": "/organizations",
        "get":     "Clouseau and Stromae want to search and filter organizations by country, interest type, and lobbying cost range.",
        "post":    "Admin wants to add a new organization to the database with lobbyfacts fields.",
        "put":     "N/A",
        "delete":  "N/A",
    },
    {
        "command": "/organizations/{org_id}",
        "get":     "Clouseau wants to see a full org profile including lobbying spend, EP passes, meetings, expenditure history, and activities.",
        "post":    "N/A",
        "put":     "Admin wants to update an existing organization's lobbyfacts data fields.",
        "delete":  "Admin wants to remove an organization from the database.",
    },
    {
        "command": "/organizations/top-spenders",
        "get":     "Clouseau wants to see the top N organizations ranked by lobbying cost, optionally filtered by country.",
        "post":    "N/A",
        "put":     "N/A",
        "delete":  "N/A",
    },
    {
        "command": "/country-indicators/{country_name}",
        "get":     "Clouseau wants to see GDP, fossil fuels, CO2 emissions, and urban population from World Bank data for a given country, plus total lobbying spend from that country.",
        "post":    "N/A",
        "put":     "N/A",
        "delete":  "N/A",
    },
    {
        "command": "/preferences",
        "get":     "Stromae wants to retrieve their saved policy area and country preferences for their personalized feed.",
        "post":    "Stromae wants to submit onboarding preferences — selected policy areas and countries.",
        "put":     "N/A",
        "delete":  "N/A",
    },
    {
        "command": "/organizations/{org_id}/influence-prediction",
        "get":     "Clouseau wants to run the ML model to get an influence score for a specific org. Joins lobbyfacts features (lobbying cost, FTE, meetings, EP passes) with World Bank features (GDP, fossil fuels) for the org's country. ✦ Calls ML model.",
        "post":    "N/A",
        "put":     "N/A",
        "delete":  "N/A",
    },
]

st.markdown("""
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
  .ml-tag {
    display: inline-block;
    background: #2563EB;
    color: white;
    font-size: 11px;
    padding: 2px 6px;
    border-radius: 4px;
    margin-top: 4px;
  }
</style>
""", unsafe_allow_html=True)

def cell(text):
    if text == "N/A":
        return f'<td><span class="na">N/A</span></td>'
    return f'<td>{text}</td>'

rows_html = ""
for r in routes:
    rows_html += f"""
    <tr>
      <td><span class="route-cmd">{r['command']}</span></td>
      {cell(r['get'])}
      {cell(r['post'])}
      {cell(r['put'])}
      {cell(r['delete'])}
    </tr>
    """

st.markdown(f"""
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
    {rows_html}
  </tbody>
</table>
""", unsafe_allow_html=True)

### Route Syntax Details
In creating our routes, we considered how to label them to make them most understandable for those working with them. This included labeling routes first by what was being accessed — organizations, country-indicators, and preferences — and then narrowing to more specific info. When specific entries needed to be accessed, such as a specific organization or country, we simply included their identifier in the route (e.g. /organizations/{org_id} or /country-indicators/{country_name}). This differs from the syntax used when filtering multiple entries, such as searching organizations by lobbying cost range or country, which was done using query parameters (e.g. /organizations?country=Belgium&min_cost=1000000). This keeps our routes clean and RESTful — path parameters identify a specific resource, while query parameters filter a collection.

### Implementing Routes
After planning the routes in the REST API Matrix, we implemented them in Python in our project's backend using Flask blueprints. This involved following the format of cursor commands and try/except blocks to catch database errors, and thinking carefully about how the frontend would interact with the API and how data needed to be presented. One specific design decision was Route 10, the influence prediction route. Rather than having the frontend pass raw feature values, the API itself fetches the organization's lobbyfacts features, lobbying cost, FTE members, meetings, EP passes, and joins them with the most recent World Bank indicators for that organization's country, including GDP and fossil fuel usage. This means the Streamlit frontend only needs to pass an org ID, and the API handles all the feature assembly under the hood before calling the ML model. This keeps the frontend simple and ensures the model always gets a consistent, well-structured feature set regardless of which page calls it.

```
# Route 1 — GET /organizations
# Search / filter all organizations by policy area, country, or industry.
@organizations_bp.route("/organizations", methods=["GET"])
def get_all_organizations():
    current_app.logger.info("GET /organizations")
    try:
        policy_area = request.args.get("policy_area")
        country     = request.args.get("country")
        industry    = request.args.get("industry")

       
        query  = "SELECT * FROM organization WHERE 1=1"
        params = []

        if policy_area:
            query += """
                AND org_id IN (
                    SELECT org_id FROM lobbying_activity la
                    JOIN policy_area pa ON la.policy_area_id = pa.policy_area_id
                    WHERE pa.name = %s
                )"""
            params.append(policy_area)
        if country:
            query += " AND country_code = %s"
            params.append(country)
        if industry:
            query += """
                AND industry_id IN (
                    SELECT industry_id FROM industry WHERE name = %s
                )"""
            params.append(industry)

        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute(query, params)
            orgs = cursor.fetchall()

        current_app.logger.info(f"Retrieved {len(orgs)} organizations")
        return jsonify(orgs), 200
    except Error as e:
        current_app.logger.error(f"Database error in get_all_organizations: {e}")
        return error_response(str(e))


# Route 2 — GET /organizations/<org_id>
# Full org profile: base info + lobbying activities + expenditures.
@organizations_bp.route("/organizations/<int:org_id>", methods=["GET"])
def get_organization(org_id):
    current_app.logger.info(f"GET /organizations/{org_id}")
    try:
        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute("SELECT * FROM organization WHERE org_id = %s", (org_id,))
            org = cursor.fetchone()

            if not org:
                return error_response("Organization not found", 404)

            # Attach lobbying activities
            cursor.execute(
                "SELECT * FROM lobbying_activity WHERE org_id = %s", (org_id,)
            )
            org["lobbying_activities"] = cursor.fetchall()

            # Attach expenditure records
            cursor.execute(
                "SELECT * FROM expenditure_record WHERE org_id = %s", (org_id,)
            )
            org["expenditures"] = cursor.fetchall()

        return jsonify(org), 200
    except Error as e:
        current_app.logger.error(f"Database error in get_organization: {e}")
        return error_response(str(e))


# Route 3 — POST /organizations
# Add a new organization.
@organizations_bp.route("/organizations", methods=["POST"])
def create_organization():
    current_app.logger.info("POST /organizations")
    try:
        data = request.get_json()

        required_fields = ["name", "country_code", "industry_id", "lobbying_cost"]
        for field in required_fields:
            if field not in data:
                return error_response(f"Missing required field: {field}", 400)

        query = """
            INSERT INTO organization
                (name, lobbyfacts_url, members_eu, members_fte,
                 lobbying_cost, interest_represented, country_code, industry_id)
            VALUES (%s, %s, %s, %s, %s, %s, %s, %s)
        """
        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute(query, (
                data["name"],
                data.get("lobbyfacts_url"),
                data.get("members_eu"),
                data.get("members_fte"),
                data["lobbying_cost"],
                data.get("interest_represented"),
                data["country_code"],
                data["industry_id"],
            ))
            new_id = cursor.lastrowid

        get_db().commit()
        current_app.logger.info(f"Created organization id={new_id}")
        return jsonify({"message": "Organization created successfully", "org_id": new_id}), 201
    except Error as e:
        current_app.logger.error(f"Database error in create_organization: {e}")
        return error_response(str(e))


# Route 4 — PUT /organizations/<org_id>
# Update any fields on an existing organization.
@organizations_bp.route("/organizations/<int:org_id>", methods=["PUT"])
def update_organization(org_id):
    current_app.logger.info(f"PUT /organizations/{org_id}")
    try:
        data = request.get_json()

        allowed_fields = [
            "name", "lobbyfacts_url", "members_eu", "members_fte",
            "lobbying_cost", "interest_represented", "country_code", "industry_id"
        ]
        update_fields = [f"{f} = %s" for f in allowed_fields if f in data]
        params        = [data[f] for f in allowed_fields if f in data]

        if not update_fields:
            return error_response("No valid fields to update", 400)

        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute("SELECT org_id FROM organization WHERE org_id = %s", (org_id,))
            if not cursor.fetchone():
                return error_response("Organization not found", 404)

            params.append(org_id)
            query = f"UPDATE organization SET {', '.join(update_fields)} WHERE org_id = %s"
            cursor.execute(query, params)

        get_db().commit()
        return jsonify({"message": "Organization updated successfully"}), 200
    except Error as e:
        current_app.logger.error(f"Database error in update_organization: {e}")
        return error_response(str(e))


# Route 5 — DELETE /organizations/<org_id>
# Remove an organization from the database.
@organizations_bp.route("/organizations/<int:org_id>", methods=["DELETE"])
def delete_organization(org_id):
    current_app.logger.info(f"DELETE /organizations/{org_id}")
    try:
        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute("SELECT org_id FROM organization WHERE org_id = %s", (org_id,))
            if not cursor.fetchone():
                return error_response("Organization not found", 404)

            cursor.execute("DELETE FROM organization WHERE org_id = %s", (org_id,))

        get_db().commit()
        current_app.logger.info(f"Deleted organization id={org_id}")
        return jsonify({"message": "Organization deleted successfully"}), 200
    except Error as e:
        current_app.logger.error(f"Database error in delete_organization: {e}")
        return error_response(str(e))



# Route 6 — GET /policy-areas
# Fetch all policy areas to populate the search dropdown.
@policy_bp.route("/policy-areas", methods=["GET"])
def get_policy_areas():
    current_app.logger.info("GET /policy-areas")
    try:
        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute("SELECT * FROM policy_area ORDER BY name ASC")
            areas = cursor.fetchall()

        current_app.logger.info(f"Retrieved {len(areas)} policy areas")
        return jsonify(areas), 200
    except Error as e:
        current_app.logger.error(f"Database error in get_policy_areas: {e}")
        return error_response(str(e))



# Route 7 — GET /country-indicators/<country_code>
# Fetch GDP, population, and inflation for a given country (Clouseau detail cards).
@countries_bp.route("/country-indicators/<string:country_code>", methods=["GET"])
def get_country_indicators(country_code):
    current_app.logger.info(f"GET /country-indicators/{country_code}")
    try:
        with get_db().cursor(dictionary=True) as cursor:
            # Confirm the country exists
            cursor.execute(
                "SELECT * FROM country WHERE country_code = %s", (country_code,)
            )
            country = cursor.fetchone()
            if not country:
                return error_response("Country not found", 404)

            # Get the most recent indicator row for this country
            cursor.execute(
                """SELECT * FROM country_indicator
                   WHERE country_code = %s
                   ORDER BY year DESC""",
                (country_code,)
            )
            country["indicators"] = cursor.fetchall()

        return jsonify(country), 200
    except Error as e:
        current_app.logger.error(f"Database error in get_country_indicators: {e}")
        return error_response(str(e))


# Route 8 — GET /preferences
# Get the current user's saved policy + country preferences (Stromae feed).
@users_bp.route("/preferences", methods=["GET"])
def get_preferences():
    current_app.logger.info("GET /preferences")
    try:
        user_id = request.args.get("user_id")
        if not user_id:
            return error_response("Missing required parameter: user_id", 400)

        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute(
                "SELECT * FROM app_user WHERE user_id = %s", (user_id,)
            )
            user = cursor.fetchone()
            if not user:
                return error_response("User not found", 404)

            # Return saved queries / preferences for this user
            cursor.execute(
                "SELECT * FROM saved_query_export WHERE user_id = %s ORDER BY created_at DESC",
                (user_id,)
            )
            user["saved_queries"] = cursor.fetchall()

        # Don't return password hash to the client
        user.pop("password_hash", None)
        return jsonify(user), 200
    except Error as e:
        current_app.logger.error(f"Database error in get_preferences: {e}")
        return error_response(str(e))


# Route 9 — POST /preferences
# Submit onboarding preferences — policy areas & countries (Stromae onboarding).
@users_bp.route("/preferences", methods=["POST"])
def save_preferences():
    current_app.logger.info("POST /preferences")
    try:
        data = request.get_json()

        required_fields = ["user_id", "query_json"]
        for field in required_fields:
            if field not in data:
                return error_response(f"Missing required field: {field}", 400)

        with get_db().cursor(dictionary=True) as cursor:
            cursor.execute(
                "SELECT user_id FROM app_user WHERE user_id = %s", (data["user_id"],)
            )
            if not cursor.fetchone():
                return error_response("User not found", 404)

            cursor.execute(
                """INSERT INTO saved_query_export (user_id, query_json, file_format)
                   VALUES (%s, %s, %s)""",
                (data["user_id"], data["query_json"], data.get("file_format", "json"))
            )
            new_id = cursor.lastrowid

        get_db().commit()
        current_app.logger.info(f"Saved preferences export_id={new_id}")
        return jsonify({"message": "Preferences saved successfully", "export_id": new_id}), 201
    except Error as e:
        current_app.logger.error(f"Database error in save_preferences: {e}")
        return error_response(str(e))



# Route 10 — POST /lobby/prediction
# Submit lobbying inputs and return the model prediction.
@ml_bp.route("/lobby/prediction", methods=["POST"])
def get_lobby_prediction():
    current_app.logger.info("POST /lobby/prediction")
    try:
        data = request.get_json(silent=True) or {}

        required_fields = ["lobbying_cost", "ep_passes", "members_fte", "country", "interest"]
        missing_fields = [field for field in required_fields if field not in data]
        if missing_fields:
            return jsonify({"error": f"Missing required fields: {', '.join(missing_fields)}"}), 400

        prediction = lobby_model.predict(
            data["lobbying_cost"],
            data["ep_passes"],
            data["members_fte"],
            data["country"],
            data["interest"],
        )

        current_app.logger.info(f"lobby prediction returned {prediction:.2f}")
        return jsonify({
            "prediction": round(prediction, 2),
            "input_variables": {
                "lobbying_cost": float(data["lobbying_cost"]),
                "ep_passes": float(data["ep_passes"]),
                "members_fte": float(data["members_fte"]),
                "country": data["country"],
                "interest": data["interest"],
            },
        }), 200

    except ValueError as e:
        current_app.logger.error(f"lobby prediction input error: {e}")
        return jsonify({"error": str(e)}), 400
    except Exception as e:
        current_app.logger.error(f"lobby prediction error: {e}")
        return jsonify({"error": "Error processing prediction request"}), 500
``` 

## User Interface


# Figure 1
st.image("assets/fig1_shaping_eu.png", use_container_width=True)
st.markdown("*Figure 1 - Shaping EU Policies (Citizen Home)*")
st.markdown("**Figure 1 Description**")
st.write("We created this page for the Stromae persona — a curious everyday citizen who wants to understand who is shaping EU policy without being overwhelmed by data. The page presents two card-based selection grids: one for picking policy areas of interest such as AI, Climate & Energy, or Healthcare, and one for picking EU member countries. Selections are tracked in session state and highlighted as the user clicks. When they submit, the choices are sent to the POST /preferences route and saved to the database for their personalized feed.")
st.markdown("---")

# Figure 2
st.image("assets/fig2_org_comparison.png", use_container_width=True)
st.markdown("*Figure 2 - Organization Comparison (Researcher Home)*")
st.markdown("**Figure 2 Description**")
st.write("This page is built for the Clouseau persona — a political science researcher who wants to compare organizations in depth. The layout uses a main content area with a Score Comparison tab alongside a persistent Saved Comparisons column on the right. Once two organizations are saved and selected, the page renders a side-by-side comparison showing lobbying spend, policy areas, EU meeting counts, EP access passes, and the ML influence score returned by the influence prediction route.")
st.markdown("---")

# Figure 3
st.image("assets/fig3_add_org_search.png", use_container_width=True)
st.markdown("*Figure 3 - Add Organization: Search & Save Tab*")
st.markdown("**Figure 3 Description**")
st.write("The Add Organization page has two tabs. The Search & Save tab allows the researcher to search existing organizations from our lobbyfacts dataset by filtering on policy area, country, or industry. Results are returned from the GET /organizations route and displayed as a list with a Save button next to each entry. Saved organizations are stored in session state and immediately appear in the Saved Comparisons column on the Organization Comparison page.")
st.markdown("---")

# Figure 4
st.image("assets/fig4_add_org_create.png", use_container_width=True)
st.markdown("*Figure 4 - Add Organization: Create New Tab*")
st.markdown("**Figure 4 Description**")
st.write("The Create New tab provides a form for adding a brand new organization to the database. Fields map directly to the lobbyfacts dataset columns — organization name, country, lobbying cost, FTE members, EU members, interest represented, and LobbyFacts URL. On submission the form calls POST /organizations, and if successful the new organization is automatically added to the researcher's saved comparison list.")
st.markdown("---")

# Figure 5
st.image("assets/fig5_prediction.png", use_container_width=True)
st.markdown("*Figure 5 - Lobby Prediction Demo*")
st.markdown("**Figure 5 Description**")
st.write("The Lobby Prediction Demo page allows a researcher to manually input lobbying features and call the ML influence prediction endpoint directly. Inputs include lobbying cost, EP passes, Members FTE, country, and interest representation type — all features drawn from our lobbyfacts and World Bank datasets. Clicking Predict calls the influence prediction route, which joins the lobbyfacts features with the most recent World Bank indicators for the selected country and returns a predicted influence score and class.")
st.markdown("---")
