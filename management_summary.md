#  Management Summary NYC Yellow Taxi Trip Demand Analysis and Forecasting

##  Objective
The goal of this project was to analyze NYC Yellow Taxi trip data to uncover trends. For the analysis I focused on trends related to **Time**, **Distance**, and **Revenue**, and then in the second part i focused on  building a **predictive model** to forecast **daily trip demand**. The insights and forecasts aim to support taxi drivers and fleet managers in optimizing operations, maximizing earnings, and aligning supply with demand.


## Step-by-Step Process

### 1.  Data Cleaning
- Parsed datetime fields and converted them to proper `datetime` objects.
- Handled missing values:
  - Filled missing values in `passenger_count`, `RatecodeID`, and `store_and_fwd_flag` using mode.
  - Replaced missing values in `congestion_surcharge` and `airport_fee` with 0.
- Removed invalid trips (e.g., zero or negative duration/distance).
- Ensured consistent data types for categorical and numerical columns.


### 2.  Exploratory Data Analysis (TDR Framework)
To structure my work and make sure the insights are easy to follow and well connected I bifurcated my EDA into Time, Distance and Revenue based analyis

#### ** Time Based Analysis **

- **Trips per Hour of Day**:
  - Peak hours are between **2 PM and 6 PM**, with the highest volume at **5 PM (~19,600 trips)**.
  - Morning demand increases from **6 AM**, peaking again between **9–11 AM**.
  - Low demand between **12 AM–5 AM**, with the lowest at **4 AM (2,000 trips)**.

- **Trips per Day of the Week**:
  - **Friday** has the highest trip volume (~47,000), followed by **Thursday** and **Wednesday**.
  - **Sunday** has the lowest demand (~35,000), indicating reduced weekend usage.
  - Weekdays are generally busier than weekends.

- **Heatmap: Trips by Hour and Day**:
  - Consistent morning and evening demand during weekdays.
  - Friday shows the **highest full-day activity**.
  - Weekend demand starts later in the day and peaks in the evening, suggesting more leisure-related travel.

#### ** Distance Based Analysis **

- **Trip Distance Distribution**:
  - Most trips are **short**, with over **220,000 trips under 3 miles**.
  - **Very short trips (<1 mile)** are also common (~71,000+), indicating hyper-local travel.

- **Top Pickup-Dropoff Pairs**:
  - High-frequency routes are concentrated in **Manhattan**, especially between adjacent zones like:
    - **Upper East Side South → Upper East Side North**
    - **Midtown Center → Upper East Side South**
  - These routes reflect **dense urban mobility patterns**.

- **Average Trip Distance by Hour**:
  - **Longer trips** occur during early morning hours (2–6 AM) with average distances >30 miles (e.g., airport runs).
  - **Shorter trips** dominate during the day (10 AM–6 PM), averaging 2.8–3.2 miles.

- **Distance Categories**:
  - **Short (1–3 miles)**:  150,000 trips
  - **Very Short (<1 mile)**: 71,000 trips
  - **Medium (3–7 miles)**: 51,000 trips
  - **Long (7–15 miles)**: 21,000 trips
  - **Very Long (>15 miles)**: 11,000 trips

#### ** Revenue Based Analysis **

- **Average Revenue per Trip by Hour**:
  - Highest average revenue occurs at **4–5 AM (~$24)** due to longer trips (e.g., airport).
  - Daytime revenue stabilizes around **$17–$20**.
  - Late-night trips (12–3 AM) also show elevated fares (~$19–$20).

- **Average Revenue by Pickup Borough**:
  - **EWR (~$90)** and **Staten Island (~$76)** have the highest fares.
  - **Queens (~$47)** and **Bronx (~$32)** follow.
  - **Manhattan (~$16)** has the lowest average fare due to short, frequent trips.

- **Top High-Revenue Routes**:
  - Routes involving **"Outside NYC"** and **airport zones** (e.g., **JFK → Arden Heights**) generate the highest average fares ($170–$240+).
  - These often represent **inter-county or airport transfers**.


## Demand Forecasting with XGBoost

### Why I chose XGBoost?
XGBoost was selected for its:
- Ability to model **non-linear relationships** and **complex feature interactions**
- Scalability and fast training
- Good performance with **feature-rich time series data**


###  Feature Engineering

- Created **lag features** (`lag_1` to `lag_7`) to capture short-term temporal dependencies.
- Added **calendar features**:
  - `day_of_week` (0–6)
  - `is_weekend` (True/False)


###  Train-Test Split

- Used the **last 14 days** as a test set to simulate real-world forecasting performance.


###  Model Tuning

- Performed **GridSearchCV** over:
  - `n_estimators`: [50, 100, 200]
  - `learning_rate`: [0.01, 0.05, 0.1, 0.2]
  - `max_depth`: [3, 5, 7]
- Used **5-fold cross-validation** with **negative RMSE** as the scoring metric.


### model Evaluation

| Metric | Value |
|--------|-------|
| **MAE** | 5.84 trips |
| **RMSE** | 8.09 trips |
| **R² Score** | 0.77 |

The model explains **77% of the variance** in daily trip demand with low prediction error, making it suitable for operational use.


###  Forecasting

- The model was used to **predict next-day demand** using the most recent 7 days of data.
- Calendar features were dynamically updated for the forecast date.


##  Conclusion & Recommendations

- **Short-distance, high-frequency trips** dominate NYC taxi operations, especially in **Manhattan**.
- **Revenue potential is highest** during early mornings, late nights, and on **airport or inter-county routes**.
- The **XGBoost model** provides:
  - Accurate daily demand forecasts
  - can be used for **driver shift optimization**, **fleet distribution**, and **dynamic pricing**



