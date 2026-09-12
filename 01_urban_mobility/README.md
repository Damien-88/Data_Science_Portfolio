# Project 01: Urban Mobility & Transit Delay Analytics

## Executive Overview
This project delivers an end-to-end spatial-temporal analytics pipeline designed to identify, quantify, and forecast \
systemic public transit delays. By combining a **PostGIS** spatial database with **Global/Local Moran's $I$** spatial \
statistics and an **XGBoost Regressor**, the system transitions raw transit schedule data into actionable operational \
insights and real-time delay forecasts.

---

## Architecture & Data Pipeline
The analytics pipeline is structured into four sequential modules:

```text
01_urban_mobility/
├── 01_data_ingestion.ipynb             # GTFS parsing, PostgreSQL/PostGIS loading, synthetic delays
├── 02_spatial_stats.ipynb              # Spatial EDA, Global Moran's I, LISA clusters, Folium map
├── 03_predictive_modeling.ipynb        # Time-series resampling, lag features, XGBoost model
├── 04_visualization_dashboard.ipynb    # Composite Plotly dashboard & HTML export
├── delay_hotspots_map.html             # Exported interactive spatial cluster map
└── executive_dashboard.html            # Exported interactive Plotly summary dashboard
```

---

## Key Methodology & Analytical Results

### 1. Spatial Ingestion & PostGIS Geometry
* Ingested raw GTFS feed feeds (`stops`, `routes`, `trips`, `stop_times`) into PostgreSQL/PostGIS using chunked database \
transactions.
* Constructed spatial point geometries (`EPSG:4326`) and transformed coordinates to planar projections (`EPSG:3857`) for \
accurate spatial distance calculations.
* Built **GiST spatial indices** on transit stop locations to accelerate spatial join queries.

### 2. Spatial Autocorrelation & Bottleneck Identification (LISA)
To determine whether transit delays occur randomly or form geographic bottlenecks, spatial autocorrelation was evaluated \
across the transit network using a Spatial Weights Matrix ($k$-nearest neighbors).

* **Global Moran's $I$ Statistic:** `0.7954` ($p < 0.0001$)
  * *Verdict:* High positive spatial autocorrelation. Transit delays are heavily clustered in specific corridors rather \
  than distributed randomly.
* **Local Indicators of Spatial Association (LISA):**
  * **High-High (Hotspots):** Identified **379 critical bottleneck stops** where high delays are surrounded by high delays.
  * **Low-Low (Coldspots):** Identified **2,811 high-reliability stops** forming smooth transit corridors.
  * Generated an interactive spatial map (`delay_hotspots_map.html`) using Folium.

### 3. Predictive Delay Forecasting (XGBoost)
Delay events were aggregated into **15-minute system-wide network intervals** to form a continuous time series. Features \
engineered include cyclical time signals (`hour`, `dayofweek`, `is_peak_hour`), historical lags (`15m`, `30m`, `1h`), and \
1-hour rolling window statistics.

* **Validation Strategy:** 80/20 Chronological Train/Test Split (preventing temporal data leakage).
* **Model:** XGBoost Regressor (`n_estimators=100`, `learning_rate=0.05`, `max_depth=5`).
* **Performance Metrics:**
  * **Root Mean Squared Error (RMSE):** Evaluated against baseline delay variance.
  * **Mean Absolute Error (MAE):** Low average seconds off per 15-minute prediction window.
  * **Key Predictor:** Historical 15-minute delay lag (`lag_15m`) and rolling averages emerged as top feature importances.

### 4. Interactive Executive Dashboard
Synthesized spatial and temporal findings into a multi-panel Plotly interactive dashboard (`executive_dashboard.html`), \
displaying:
1. System-wide hourly delay risk profiles (peak hour identification).
2. Top 15 highest-delay transit stop corridors for targeted infrastructure interventions.

---

## Tech Stack
* **Database & Geospatial:** PostgreSQL, PostGIS, GeoPandas, PySAL (`esda`), Shapely, SQLAlchemy
* **Machine Learning & Time Series:** XGBoost, Scikit-Learn, Pandas, NumPy
* **Visualization:** Folium, Plotly, Matplotlib