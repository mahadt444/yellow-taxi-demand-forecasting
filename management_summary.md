#  Management Summary NYC Yellow Taxi Trip Demand Analysis and Forecasting

##  Objective
The goal of this project was to analyze NYC Yellow Taxi trip data to uncover trends. For the analysis I focused on trends related to **Time**, **Distance**, and **Revenue**, and then in the second part i focused on  building a **predictive model** to forecast **daily trip demand**. The insights and forecasts aim to support taxi drivers and fleet managers in optimizing operations, maximizing earnings, and aligning supply with demand.

## Key Findings from Exploratory Data Analysis (EDA)

###  Time-Based Trends
- **Peak trip volumes** occur between **2 PM and 6 PM**, with the highest demand at **5 PM**.
- **Fridays** have the highest trip counts (~47,000), followed by **Thursdays** and **Wednesdays**.
- **Weekends** show delayed demand start, with higher volumes in the afternoon and evening.
- **Heatmaps** reveal consistent high demand during weekday rush hours.

###  Distance Patterns
- Most trips are **short-distance**, with over **220,000 trips under 3 miles**.
- Popular routes are highly concentrated in **Manhattan**, especially between adjacent neighborhoods in the **Upper East Side** and **Upper West Side**.
- **Longer trips** are more common during late-night and early-morning hours, often linked to **airport runs**.

###  Revenue Insights
- **Highest average revenue per trip** occurs between **4–5 AM** (~$24), likely due to longer trips with less traffic.
- **EWR (Newark Airport)** and **Staten Island** pickups yield the highest average fares (~$90 and $76 respectively).
- **Top revenue-generating routes** involve trips **outside NYC** or between **boroughs and airports**, often exceeding **$200 per trip**.


##  Demand Forecasting with XGBoost

###  Why I selected XGBoost?
- Handles **non-linear relationships** and **time-based feature interactions**
- Flexible and scalable for time series regression
- Performs well with engineered features which i added in the data to capture seasonal trends

###  Feature Engineering
- Created **lag features** (`lag_1` to `lag_7`) to capture short-term patterns
- Added **calendar-based features**: `day_of_week`, `is_weekend`

###  Model Performance
- **MAE**: **5.84 trips/day**
- **RMSE**: **8.09 trips/day**
- **R² Score**: **0.77**

 The model explains **77% of the variance** in daily trip demand, with **low prediction error**, making it reliable for operational use.

### Forecasting
- The model was used to successfully **predict next-day trip demand** by updating lag and calendar features.


##  Conclusion & Recommendations

- **Short, high-frequency trips** dominate the NYC taxi landscape, especially in Manhattan.
- **Revenue potential** is higher in **early mornings**, **late nights**, and for **airport or inter-borough trips**.
- The **XGBoost model** provides accurate daily demand forecasts and can be deployed to:
  - Guide **driver shift planning**
  - Optimize **fleet distribution**
  - Support **dynamic pricing strategies**


