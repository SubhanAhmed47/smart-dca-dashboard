# Smart Decline Curve Analysis (DCA) Dashboard

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![DuckDB](https://img.shields.io/badge/Database-DuckDB-orange.svg)](https://duckdb.org/)
[![Streamlit](https://img.shields.io/badge/Frontend-Streamlit-red.svg)](https://streamlit.io/)

A hybrid petroleum engineering and data science tool that supercharges traditional production forecasting with modern data engineering, machine learning, and automated anomaly detection. 

Instead of evaluating wells in total isolation or blindly replacing domain physics with AI, this application implements a **hybrid framework**. It streams a multi-well, noisy simulated SCADA dataset through a structured ETL pipeline, fits classical physics-based decline curves, and benchmarks them head-to-head against a global Machine Learning model.

---

## Dashboard Preview

| Well-Level KPIs & Dynamic Forecast Benchmarking | Field-Wide Holdout Error Analysis (RMSE) |
|---|---|
| ![Dashboard UI](assets/dashboard_ui.png) | ![Field-Wide Accuracy](assets/accuracy_chart.png) |

---

## Core Architecture & Features

### 1. Data Engineering Pipeline (DuckDB Medallion Architecture)
Built an in-memory, high-performance ETL pipeline using **DuckDB** to process ~2.5 years of daily production data across 15 wells:
*   **Bronze Layer:** Raw, untouched SCADA/production accounting data ingest containing realistic sensor noise, data gaps, and operational shut-ins.
*   **Silver Layer:** Automated data quality and cleaning routines—nulling outlier spikes, interpolating missing steps, and reindexing production calendars per well.
*   **Gold Layer:** Feature engineering store optimized for analytics, generating rolling averages, cumulative production metrics, water cut/GOR slopes, and time-lagged variables.

### 2. The Forecast Battle: Arps Physics vs. XGBoost ML
*   **Classical Baseline:** Automatically fits industry-standard **Arps Hyperbolic Decline Curves** using non-linear least squares (`SciPy`) to compute Estimated Ultimate Recovery (EUR) down to an economic limit.
*   **Machine Learning Layer:** Trains a global **XGBoost** model across the entire field simultaneously to capture complex, multi-well cross-dynamics that single-well Arps curves miss in isolation.
*   **Rigorous Validation:** Both methodologies are evaluated head-to-head on a strict 90-day holdout window using Root Mean Squared Error (RMSE). The system dynamically flags the winning model (e.g., highlighting that Arps excels on stable wells while XGBoost adapts better to complex, volatile fluctuations).

### 3. AI-Powered Anomaly Detection
*   Implements an **Isolation Forest** unsupervised algorithm to monitor well streams and instantly flag deviations from their natural decline trends.
*   Features an automated rule-engine to classify anomalies into actionable operational causes (e.g., mechanical lifting failure, water breakthrough, or surface metering errors).

### 4. Interactive Streamlit Interface
*   Designed a tailored UI using an industry-specific color palette (oil-black/rust/amber).
*   Equipped with dynamic KPI cards, interactive Plotly decline plots, log-log diagnostic views for type-curve matching, and an executive field-wide model performance dashboard.

---

##  Tech Stack

*   **Data Warehouse & ETL:** DuckDB
*   **Core Data Stack:** Pandas, NumPy
*   **Physics Modeling:** SciPy (Optimization & Integration modules)
*   **Machine Learning:** XGBoost, Scikit-learn (Isolation Forest)
*   **Visualization:** Plotly Express / Graph Objects
*   **Application Framework:** Streamlit, Pyngrok

---

##  Getting Started

### Prerequisites
Make sure you have Python 3.10+ installed. Clone the repository and install the dependencies:

```bash
git clone [https://github.com/YOUR_USERNAME/smart-dca-dashboard.git](https://github.com/YOUR_USERNAME/smart-dca-dashboard.git)
cd smart-dca-dashboard
pip install -r requirements.txt
