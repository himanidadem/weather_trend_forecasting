# 🌍 Weather Trend Forecasting

A complete data science pipeline for analyzing global weather data, detecting anomalies, and forecasting daily temperature for diverse cities.

**Submission for:** PM Accelerator Data Scientist / Analyst Technical Assessment
**Track:** Advanced
**Dataset:** [Global Weather Repository (Kaggle)](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository)

---

## 📋 Project Overview

This project takes 138,000+ daily weather records from **257 capital cities across 211 countries** (May 2024 → May 2026) and produces:

- A **clean, analysis-ready dataset** with engineered time and geographic features
- An **ensemble forecasting pipeline** (ARIMA, Seasonal Naive, XGBoost) for 3 climatically-diverse cities
- **Multivariate anomaly detection** identifying real weather events (heatwaves, storms, pollution spikes)
- **Climate trend analysis** revealing patterns by hemisphere, season, and continent
- **Air quality correlation analysis** linking weather conditions to pollution
- **Spatial visualizations** mapping temperature and pollution globally

---

## 🎯 Key Findings

### Forecasting Performance (Mean Absolute Error, °C)

| City | ARIMA | Seasonal Naive | XGBoost | Ensemble |
|------|-------|---------------|---------|----------|
| **London** | 3.40 | 3.49 | 3.36 | **2.99** ✅ |
| **New Delhi** | 6.93 | 4.12 | **3.52** ✅ | 3.64 |
| **Canberra** | 5.80 | 7.65 | 7.07 | **5.24** ✅ |

**Insights:**
- **Ensemble averaging** improved or matched the best single model in 2 of 3 cities
- **XGBoost** excels for cities with strong seasonal transitions (New Delhi monsoon)
- **ARIMA** struggles with seasonal change because it lacks calendar awareness
- Forecast accuracy correlates strongly with the *climate volatility* of each city

### Climate & Pollution Insights
- **Asian cities** lead global PM2.5 (median 28.65 µg/m³) — 4× higher than Oceania
- **Hemisphere reversal** in seasonality is clearly visible (Northern peaks in July, Southern in January)
- **Latitude alone** explains a large part of temperature variation — confirmed both visually and via Random Forest feature importance
- **Temperature ↔ feels-like (+0.98)** and **PM2.5 ↔ NO₂ (+0.53)** are the strongest correlations — useful for both feature selection and pollution-source identification

---

## 🗂️ Repository Structure

```
weather-trend-forecasting/
│
├── data/
│   ├── GlobalWeatherRepository.csv       # Raw dataset (from Kaggle)
│   ├── weather_cleaned.parquet           # Cleaned + feature-engineered dataset
│   └── forecast_results.csv              # Final model performance metrics
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb            # Cleaning, outlier handling, feature engineering
│   ├── 02_eda.ipynb                      # Exploratory analysis & correlations
│   ├── 03_anomaly_detection.ipynb        # Z-score + Isolation Forest anomalies
│   ├── 04_forecasting.ipynb              # ARIMA, Seasonal Naive, XGBoost, Ensemble
│   └── 05_advanced_analyses.ipynb        # Climate trends, AQI, feature importance, maps
│
├── figures/                              # All saved visualizations (15+ PNGs)
│
├── README.md                             # This file
├── requirements.txt                      # Python dependencies
└── .gitignore
```

---

## 🚀 How to Run This Project

### Prerequisites
- Python 3.10+
- ~500 MB free disk space
- Windows / macOS / Linux

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/himanidadem/weather_trend_forecasting.git
cd weather_trend_forecasting

# 2. Create a virtual environment
python -m venv venv

# Activate it:
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Run the notebooks

Run them in order using **VS Code with the Jupyter extension** or any Jupyter environment:

```
notebooks/01_data_cleaning.ipynb     →  Cleans raw data, saves weather_cleaned.parquet
notebooks/02_eda.ipynb               →  Explores patterns and correlations
notebooks/03_anomaly_detection.ipynb →  Flags weather anomalies
notebooks/04_forecasting.ipynb       →  Trains and evaluates forecasting models
notebooks/05_advanced_analyses.ipynb →  Climate, AQI, feature importance, maps
```

> Each notebook is self-contained and reproducible. The cleaned `parquet` file is included so notebooks 02–05 can be run independently of notebook 01.

---

## 🔬 Methodology Summary

### Phase 1: Data Cleaning & Feature Engineering
- Dropped 7 redundant imperial-unit columns (Fahrenheit, mph, etc.)
- Capped physically-impossible values (e.g., wind > 410 km/h, temp > 60°C)
- Removed duplicate (location, timestamp) rows
- Engineered features: `year`, `month`, `day_of_year`, `season` (hemisphere-aware), `continent`
- Saved as compressed `.parquet` for fast downstream loading

### Phase 2: Exploratory Data Analysis
- Correlation heatmap revealed redundant features (`feels_like` vs `temp`, `wind` vs `gust`)
- Boxplots by latitude zone confirmed climate physics
- Time series of target cities showed distinct climate signatures
- Normalized inconsistent capitalization in `condition_text`

### Phase 3: Anomaly Detection
- **Univariate Z-score** (>3 SD per city) — caught individual extreme readings
- **Isolation Forest** (multivariate, contamination=2%) — detected combined-feature anomalies
- Anomaly profile reveals storm signatures: low pressure, high wind, more rain, 5× higher PM2.5

### Phase 4: Forecasting
- 80/20 train/test split (chronological — no shuffling!)
- Three diverse models: ARIMA(5,1,0), Seasonal Naive (1-year lag), XGBoost with cyclical and lag features
- Ensemble = simple unweighted average
- Evaluated using MAE and RMSE on the held-out 20%

### Phase 5: Advanced Analyses
- Monthly and year-over-year temperature trends
- 4-panel scatter plots: PM2.5 vs (wind, humidity, pressure, temperature)
- Random Forest feature importance for temperature prediction
- Global scatter maps of temperature and PM2.5

---

## 🛠️ Tech Stack

- **Python** 3.10+
- **pandas, numpy** — data wrangling
- **matplotlib, seaborn** — visualization
- **scikit-learn** — Random Forest, Isolation Forest, metrics
- **statsmodels** — ARIMA modeling
- **XGBoost** — gradient boosting
- **PyArrow** — Parquet I/O

See `requirements.txt` for exact versions.

---

## 🌟 About PM Accelerator

> **PM Accelerator Mission:** "By making industry-leading tools and education available to individuals from all backgrounds, we level the playing field for future PM leaders."

Learn more: [pmaccelerator.io](https://www.pmaccelerator.io/)

---

## 👤 Author

**Himani Dadem**
GitHub: [@himanidadem](https://github.com/himanidadem)

---

## 📜 License

This project is for educational and assessment purposes only. The Global Weather Repository dataset is sourced from [WeatherAPI](https://www.weatherapi.com/) via Kaggle.
