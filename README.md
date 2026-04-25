# 🪙 Gold Price Prediction System

A machine learning-powered Streamlit web application that forecasts monthly gold prices using market indicators and scenario-based analysis.

## 📋 Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Configuration](#configuration)

## ✨ Features

- **Real-time Market Data**: Automatically downloads daily gold prices, stock indices, VIX, USD proxy, oil, Treasury yields, and Bitcoin data
- **Machine Learning Model**: Random Forest Regressor trained on historical data (2000-present)
- **Scenario Analysis**: Three predefined scenarios:
  - **Baseline**: Unchanged market assumptions
  - **Crisis**: Stress and fear market assumptions
  - **Risk-On**: Calmer market conditions
- **Custom Macro Shocks**: Adjust forecasts with custom VIX, oil, Treasury yield, and USD bumps
- **Confidence Intervals**: 95% prediction intervals for uncertainty quantification
- **Feature Importance**: View top predictive features driving the forecast
- **Interactive Dashboard**: Real-time parameter adjustments and instant forecasts
- **Download Results**: Export forecasts, metrics, and feature importance as CSV files

## 📦 Prerequisites

- Python 3.7+
- pip (Python package manager)

## 🚀 Installation

### 1. Clone or Navigate to Project Directory
```bash
cd C:\Users\ACER\Videos\ANN_Project
```

### 2. Create Virtual Environment (Optional but Recommended)
```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies
```bash
pip install --upgrade pip
pip install streamlit nbformat pandas numpy matplotlib seaborn scikit-learn yfinance
```

## 📁 Project Structure

```
ANN_Project/
├── App.py                      # Main Streamlit application
├── gold_forcast_core.ipynb     # Jupyter notebook with ML logic and data pipeline
├── gld_price_data.csv          # Sample gold price data
├── gold_price_1970_2026_daily.csv
├── gold_prices_10y.csv
├── GPP.ipynb                   # Additional analysis notebook
└── README.md                   # This file
```

## 🎯 Usage

### Start the Application
```bash
streamlit run App.py
```

### Access the Web Interface
- **Local URL**: http://localhost:8501
- **Network URL**: http://192.168.8.101:8501 (if accessible from other devices)

### Using the Dashboard

1. **Forecast Settings (Sidebar)**
   - **Forecast Start**: Enter date in YYYY-MM-01 format (default: 2026-05-01)
   - **Forecast Horizon**: Select number of months to forecast (1-48, default: 20)
   - **Scenario**: Choose baseline, crisis, or risk_on

2. **Custom Macro Shocks (Sidebar)**
   - **VIX bump %**: Adjust volatility index (-50% to +200%)
   - **Oil bump %**: Adjust crude oil prices (-50% to +200%)
   - **10Y Yield bump %**: Adjust Treasury yields (-50% to +200%)
   - **USD proxy bump %**: Adjust currency strength (-50% to +200%)

3. **Generate Forecast**
   - Click **"Run Forecast"** button
   - System will download data, train model, and generate predictions

4. **View Results**
   - **KPI Metrics**: MAE, RMSE, MAPE%
   - **Forecast Chart**: Historical vs. predicted prices with 95% confidence bands
   - **Forecast Table**: Monthly predictions with lower/upper bounds
   - **Feature Importance**: Top 20 predictive features
   - **Download**: Export all results as CSV files

## 🔧 How It Works

### Data Pipeline
1. **Download Market Data** (2000-present)
   - Gold: GC=F or GLD
   - S&P 500: ^GSPC or SPY
   - VIX: ^VIX
   - USD Proxy: UUP or DX-Y.NYB
   - Oil: CL=F or BZ=F
   - Treasury 10Y: ^TNX
   - Bitcoin: BTC-USD

2. **Feature Engineering**
   - Daily returns and realized volatility
   - Monthly aggregations (mean, momentum, fear flags)
   - Lag features (1, 2, 3, 6, 12-month)
   - Manual event flags (crisis periods, wars, inflation shocks)

3. **Model Training**
   - Algorithm: Random Forest Regressor
   - Train/test split: 85/15
   - Hyperparameters:
     - n_estimators: 500
     - max_depth: 8
     - min_samples_leaf: 2

4. **Forecasting**
   - Recursive prediction loop using lag features
   - Scenario multipliers applied to base predictions
   - Macro shock factors combined additively
   - Confidence intervals computed from residual std deviation

## ⚙️ Configuration

### DEFAULT_CONFIG (in gold_forcast_core.ipynb)
```python
DEFAULT_CONFIG = {
    "start_date": "2000-01-01",      # Historical data start
    "end_date": None,                 # None = today
    "random_state": 42,               # Reproducibility
    "n_estimators": 500,              # Tree count
    "max_depth": 8,                   # Tree depth
    "min_samples_leaf": 2,            # Min samples per leaf
    "use_manual_event_flags": True    # Include crisis flags
}
```

### Scenario Multipliers
- **baseline**: 1.00 (no adjustment)
- **crisis**: 1.04 (4% upward bias, gold as safe haven)
- **risk_on**: 0.98 (2% downward bias, risk appetite)

### Macro Shock Sensitivity
- VIX: 0.0008 coefficient (gold inversely correlated)
- Oil: 0.0005 coefficient
- Treasury 10Y: -0.0007 coefficient (inverse)
- USD: -0.0006 coefficient (inverse)

## 📊 Model Performance

Default model metrics (on test set):
- **MAE**: ~1,219 USD per ounce
- **RMSE**: ~1,453 USD per ounce
- **MAPE%**: Varies by forecast period

## 🔗 Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| streamlit | 1.56.0+ | Web UI framework |
| pandas | 3.0.2+ | Data manipulation |
| numpy | 2.4.4+ | Numerical computing |
| scikit-learn | 1.8.0+ | Machine learning |
| yfinance | 1.3.0+ | Market data download |
| matplotlib | 3.10.9+ | Charting |
| seaborn | 0.13.2+ | Statistical visualization |
| nbformat | 5.10.4+ | Jupyter notebook handling |

## 🐛 Troubleshooting

### Issue: "Module not found" error
**Solution**: Ensure all dependencies are installed:
```bash
pip install -r requirements.txt
```

### Issue: Port 8501 already in use
**Solution**: Run on different port:
```bash
streamlit run App.py --server.port 8502
```

### Issue: Data download fails
**Solution**: Check internet connection and yfinance availability. The app will retry on next run.

### Issue: Stale UI after changes
**Solution**: Hard refresh browser (Ctrl+Shift+R or clear cache) and restart Streamlit.

## 📝 License

This project is provided as-is for educational and forecasting purposes.

## 👤 Author

Created as an AI/ML forecasting demonstration.

---

**Last Updated**: April 25, 2026
