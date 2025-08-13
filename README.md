# Solar Forecasting with SARIMAX + Fourier Terms

This project builds a **solar energy forecasting pipeline** using:
- **PVGIS API** for historical **Plane of Array (POA)** irradiance data
- **SARIMAX models** enhanced with **Fourier terms** to capture annual seasonality
- Conversion of forecasted energy into **monetary value** using electricity rates

This project demonstrates:
1. Statistical time series modeling 
2. Integrating external APIs, data cleaning, and feature engineering
3. Interpreting economic impact by translating kWh into $ impact

---

## Key Features

- Historical solar irradiance pulled from **PVGIS API**
- Validation against real-world **NREL** solar farm production data
- **SARIMAX + Fourier terms** to capture yearly seasonality
- Forecasts **5 years ahead** with confidence intervals
- Conversion of energy into **$ value** using historical electricity prices
- Fully reproducible pipeline in Python

---

## Objectives

1. Forecast daily solar energy output from historical data only  
2. Capture strong yearly seasonality using Fourier terms instead of large seasonal lags  
3. Tune SARIMAX parameters and evaluate accuracy  
4. Convert kWh to $ using electricity price data  
5. Package into a clean, repeatable pipeline  

---

## Methodology

1. **Data Acquisition**
   - Modeled POA irradiance from [PVGIS API](https://re.jrc.ec.europa.eu/pvgis.html) for Walled Lake, MI (2014–2020)
   - Real MI solar farm production data from [NREL Solar Power Data](https://www.nrel.gov/grid/solar-power-data.html) for validation

2. **Data Processing**
   - Hourly POA → daily averages  
   - Scaled by:
     - Panel area: **26.5 m²**
     - Panel efficiency: **18%**
     - System size: **5 kW**
   - Expressed in **kWh/day**
   - Validation R² score: **0.607** to see how close modeled POA irradiance data resembeled real NREL solar farm data scaled to size

3. **Exploratory Analysis**
   - Year-over-year overlays
   - Autocorrelation & PACF inspection
   - Stationarity checks (ADF test)
   - Seasonal differencing (lag = 365 days)

4. **Modeling**
   - SARIMAX `(1, 0, 1)` with **Fourier seasonal terms** (annual seasonality, order=4)
   - 5-year forecast horizon (2021–2025)
   - Confidence intervals for uncertainty estimation

5. **Economic Conversion**
   - State electricity rates from EIA data (¢/kWh)
   - Mapped daily forecasts to yearly average rates
   - Calculated daily $ value for historical + forecast data

6. **Results**
   - **Total estimated value (2014–2025)** for a 5 kW system in Walled Lake, MI:
     ```
     $13,724.26
     ```
     
---

## Tech Stack

- **Python**: Pandas, NumPy, Matplotlib, Seaborn
- **Statsmodels**: SARIMAX, Fourier terms, diagnostics
- **Scikit-learn**: metrics
- **Data Sources**:
  - [PVGIS API](https://re.jrc.ec.europa.eu/pvgis.html) – modeled irradiance
  - [NREL Solar Data](https://www.nrel.gov/grid/solar-power-data.html) – real-world validation
  - [EIA](https://www.eia.gov/electricity/data/state/) – electricity rates
