# ✈️ Flight Delay Forecasting — Time Series Analysis
 
Forecasting daily average departure delays for three major US airlines (American, Delta, Southwest) using classical time series methods. Built as a group project for MGSC 697 – Time Series Forecasting.
 
---
 
## 📌 Problem Statement
 
Flight delays cost the US airline industry billions annually and frustrate millions of passengers. This project asks: **can historical delay patterns reliably forecast future delays?** We model daily average departure delays at the airline level to support operational planning for staffing, gate scheduling, and customer communication.
 
---
 
## 📦 Dataset
 
- **Source:** Bureau of Transportation Statistics (BTS) via Kaggle
- **Coverage:** January 2019 – August 2023 (4.5 years)
- **Scale:** ~3 million individual flight records across 32 attributes
- **Target variable:** Average daily departure delay per airline (minutes)
- **Airlines modeled:** American Airlines, Delta Air Lines, Southwest Airlines
- **Frequency:** Daily (~1,704 observations per airline after aggregation)
---
 
## 🔍 Methodology
 
### 1. Exploratory Data Analysis
- Time series decomposition and rolling mean/std to identify trend and volatility shifts (notably the COVID-19 disruption in 2020)
- Day-of-week analysis: Fridays consistently show the highest delays; Tuesdays the lowest
- Month-of-year analysis: Summer (June–August) and December are peak delay periods
- ACF/PACF analysis confirming weekly seasonality (spikes at lags 7, 14, 21, 28) and short-term autocorrelation
- Stationarity testing via Augmented Dickey-Fuller test
### 2. Models Evaluated
 
| Model | Notes |
|---|---|
| Historical Mean | Baseline |
| Naïve | Baseline |
| Drift | Baseline |
| Seasonal Naïve (m=7) | Best benchmark |
| ETS(A,N,A) | Additive error, no trend, additive weekly seasonality |
| ARIMA | Non-seasonal, auto-selected |
| SARIMA | Seasonal ARIMA with weekly period (m=7) |
 
### 3. Model Selection
- Information criteria (AIC, AICc, BIC) used for ETS and SARIMA candidate selection
- 7-day rolling time series cross-validation on training data
- Final evaluation on a 12-month holdout test set (Sep 2022 – Aug 2023)
---
 
## 📊 Key Results
 
| Airline | Best Model | Test RMSE | vs. Benchmark |
|---|---|---|---|
| American Airlines | SARIMA(1,1,2)(0,1,1,7) | 10.30 | −20.8% vs. Seasonal Naïve |
| Delta Air Lines | SARIMA(3,0,0)(1,0,1,7) | 10.83 | −13.8% vs. Naïve |
| Southwest Airlines | ETS(A,N,A) | 8.50 | −3.6% vs. Seasonal Naïve |
 
> **Note:** All three models outperformed benchmark methods. For Southwest, ETS generalized better than SARIMA on the test set, indicating SARIMA overfit the training data in that case.
 
---
 
## 💡 Key Takeaways
 
- Weekly seasonality (day-of-week effects) is the dominant pattern across all three airlines — a seasonal period of 7 is appropriate for all models
- COVID-19 created a structural break in delay levels; models trained post-2020 would likely perform differently
- Extreme spikes (e.g., the January 2023 winter storm disruptions) are not predictable by univariate models — external variables like weather and airport congestion are needed
- SARIMA vs. ETS winner varies by airline: SARIMA wins for American and Delta, ETS wins for Southwest — model selection cannot be one-size-fits-all
---
 
## 🛠️ Tech Stack
 
- **Python** (pandas, numpy, statsmodels, matplotlib, seaborn)
- **Models:** `statsmodels.tsa.ETSModel`, `statsmodels.tsa.SARIMAX`
- **Validation:** Custom rolling cross-validation implementation
---
 
## 🚀 Running the Notebook
 
1. Clone the repository
2. Install dependencies:
```bash
   pip install pandas numpy matplotlib seaborn statsmodels
```
3. Open `Time_Series_Project_Flight_Delay.ipynb` in Jupyter and run all cells
> The notebook expects the BTS flight delay dataset. You can download it from [(https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023)]
 
---
 
## 🔭 Future Work
 
- Incorporate exogenous variables (weather, holidays, airport congestion) via **SARIMAX**
- Explore deep learning approaches (LSTM, Temporal Fusion Transformer) for capturing non-linear patterns
- Extend to all 18 carriers in the dataset
- Build a real-time forecasting pipeline using live BTS data feeds
---
 
## 👥 Authors
 
Cindy Hong, Yawen Wang, Syed Shaheryar Haider
