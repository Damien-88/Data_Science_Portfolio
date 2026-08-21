# Systems & Quantitative Analytics Portfolio

A collection of data science projects focused on complex systems, spatial-temporal modeling, quantitative finance, and \
computational linguistics. 

Rather than standard machine learning classification tasks, these projects emphasize backend database architecture, \
formal statistical modeling, dynamic systems, and interactive analytical dashboards.

---

## Portfolio Architecture & Tech Stack

All four projects are built on a unified core pipeline, using domain-specific libraries to address distinct analytical \
problems:

$$\text{PostgreSQL} \longrightarrow \text{Python (Pandas / SciPy)} \longrightarrow \text{Statistical \& Algorithmic Modeling} \longrightarrow \text{Interactive Dashboards}$$

| Project | Primary Domain | Core Algorithms & Methods | Key Libraries |
| :--- | :--- | :--- | :--- |
| **1. Urban Mobility** | Spatial Data Engineering | Moran's I, DBSCAN, Time-Series Forecasting | `GeoPandas`, `PySAL`, `Prophet` |
| **2. Financial Risk** | Quantitative Finance | GARCH(1,1), Monte Carlo, Value at Risk (VaR), PCA | `SciPy`, `statsmodels`, `arch` |
| **3. Linguistics** | Computational NLP | Word2Vec, Cosine Drift, KL Divergence, Zipf's Law | `spaCy`, `Gensim`, `NLTK` |
| **4. Agent Simulation** | Complex Systems | Mesa Agent-Based Framework, Sensitivity Analysis | `Mesa`, `NetworkX`, `NumPy` |

---

## Projects Overview

### 1. Urban Mobility & Transit Reliability
* **Objective:** Analyze public transit data to understand spatio-temporal bottlenecks and schedule reliability.
* **Key Findings:** Identified spatial autocorrelation in delay clusters using Moran's I and isolated key route \
                    bottlenecks during peak congestion hours.
* **Pipeline Highlights:**
  * PostGIS spatial indexing (`GIST`) for rapid geographic filtering.
  * Time-series delay forecasting using `Prophet` conditioned on weather and temporal features.
  * Interactive route choropleth maps built with `Folium`/`Plotly`.

### 2. Financial Markets & Risk Analytics
* **Objective:** Model asset volatility, evaluate portfolio risk metrics, and project stress-test scenarios.
* **Key Findings:** Constructed a dynamic risk model showing time-varying volatility dynamics and portfolio drawdowns \
                    under simulated tail-risk conditions.
* **Pipeline Highlights:**
  * Time-series PostgreSQL schemas leveraging SQL window functions (`LAG`, `LEAD`).
  * GARCH(1,1) volatility clustering and 10,000-path Monte Carlo simulations.
  * Interactive portfolio risk dashboards tracking parametric vs. historical Value at Risk (VaR).

### 3. Computational Linguistics (Historical Corpus Analysis)
* **Objective:** Measure semantic drift, vocabulary shifts, and syntactic changes across historical text corpora.
* **Key Findings:** Quantified directional shift vectors for target terminology across a century of text data using \
                    distributional semantics.
* **Pipeline Highlights:**
  * Full-text search `GIN` indexes in PostgreSQL for corpus storage.
  * Word2Vec model alignment to calculate cosine similarity shifts over time.
  * Dimensionality reduction using UMAP/t-SNE to map semantic trajectories.

### 4. Agent-Based Simulation (Dynamic Systems)
* **Objective:** Simulate emergent macro-level behaviors from micro-level agent interactions.
* **Key Findings:** Mapped phase transitions and tipping points where small shifts in individual behavior rules yield \
                    system-wide equilibrium shifts.
* **Pipeline Highlights:**
  * Object-oriented simulation models constructed with `Mesa`.
  * Batch logging of tick-by-tick simulation states directly into PostgreSQL.
  * Network topology analysis using `NetworkX` alongside animated spatial visualizations.

---

## Repository Structure

```text
├── 01_urban_mobility/
│   ├── 01_data_ingestion.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   ├── 03_statistical_modeling.ipynb
│   └── 04_visualization_dashboard.ipynb
├── 02_financial_risk/
├── 03_computational_linguistics/
├── 04_agent_simulation/
├── src/                        # Modular helper functions across projects
│   ├── db_utils.py             # PostgreSQL connection and query wrappers
│   ├── stats_utils.py          # Custom statistical functions
│   └── visualization.py        # Shared Plotly/Matplotlib formatting
├── environment.yml             # Conda environment specifications
└── README.md