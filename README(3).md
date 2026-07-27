# Retail Sales Forecasting with SARIMA

<p align="center">
  <img src="assets/sarima_forecast.png" alt="SARIMA retail sales forecast" width="900">
</p>

<p align="center">
  <strong>Time series analysis and 24-month forecasting for monthly retail sales data</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue" alt="Python">
  <img src="https://img.shields.io/badge/Model-SARIMA-orange" alt="SARIMA">
  <img src="https://img.shields.io/badge/Forecast-24%20Months-success" alt="24-Month Forecast">
  <img src="https://img.shields.io/badge/License-MIT-lightgrey" alt="MIT License">
</p>

---

## Project Overview

This project applies time series analysis to monthly retail sales data from a global supermarket. Historical observations from **2015 to 2018** are used to build a seasonal forecasting model and generate sales forecasts for the following **24 months**.

The project covers the complete forecasting workflow:

- Data inspection and preprocessing
- Trend and seasonality analysis
- Augmented Dickey-Fuller stationarity testing
- Automated SARIMA model selection
- Akaike Information Criterion evaluation
- Residual diagnostic testing
- 24-month forecasting
- 95% confidence interval estimation

> This is a historical forecasting study. Forecasts for 2019–2020 were generated using data available through 2018 and should not be interpreted as current market forecasts.

---

## Project Objectives

The main objectives of this project are to:

- Analyze the trend and seasonal structure of monthly retail sales
- Test whether the sales series is stationary
- Select an appropriate SARIMA model automatically
- Evaluate model assumptions through residual diagnostics
- Produce a 24-month sales forecast
- Visualize forecast uncertainty using 95% confidence intervals
- Demonstrate how forecasting can support inventory and budget planning

---

## Dataset

| Property | Description |
|---|---|
| Domain | Retail sales |
| Frequency | Monthly |
| Training period | 2015–2018 |
| Forecast period | 2019–2020 |
| Forecast horizon | 24 months |
| Model | SARIMA |
| Confidence level | 95% |

The dataset was obtained from an external public source.

Dataset link:

```text
https://lnkd.in/dzN9-CZ8
```

Before redistributing the dataset in this repository, its original license and usage terms should be reviewed.

---

## Methodology

### 1. Data Preprocessing

The preprocessing stage includes:

- Converting the date column into a time series index
- Checking for missing or invalid observations
- Confirming monthly data frequency
- Converting sales values into numeric format
- Sorting observations chronologically

### 2. Exploratory Time Series Analysis

The sales series is examined to identify:

- Long-term trend
- Seasonal patterns
- Sudden changes
- Potential outliers
- Changes in variance

### 3. Stationarity Testing

The **Augmented Dickey-Fuller test** is used to evaluate whether the time series is stationary.

When necessary, differencing is applied as part of the ARIMA modeling process.

### 4. Automated Model Selection

The `pmdarima` library is used to evaluate different ARIMA and SARIMA parameter combinations.

The primary model selection criterion is:

```text
AIC — Akaike Information Criterion
```

A lower AIC value indicates a better balance between model fit and model complexity.

### 5. Forecast Generation

The selected SARIMA model is used to calculate:

- 24-month point forecasts
- Lower confidence bounds
- Upper confidence bounds
- 95% forecast intervals

---

## Residual Diagnostics

Residual diagnostic tests are used to determine whether the model errors behave approximately like white noise.

| Diagnostic Test | p-value | Interpretation |
|---|---:|---|
| Ljung-Box | 0.53 | No statistically significant residual autocorrelation |
| Jarque-Bera | 0.84 | Residuals are compatible with normality |
| Heteroskedasticity test | 0.52 | No statistically significant variance instability |
| Skewness and kurtosis | Within a reasonable range | Residual distribution is approximately balanced |

All reported p-values are greater than `0.05`. Therefore, the diagnostic results do not provide sufficient evidence against the tested residual assumptions.

> Successful residual diagnostics support model adequacy, but they do not guarantee future forecasting accuracy.

---

## Project Outputs

The project produces the following outputs:

- Monthly retail sales visualization
- Stationarity test results
- Selected SARIMA model summary
- Residual diagnostic plots
- Ljung-Box test results
- Jarque-Bera test results
- Heteroskedasticity test results
- 24-month forecast table
- Forecast chart with 95% confidence intervals

---

## Repository Structure

```text
retail-sales-forecasting-sarima/
│
├── assets/
│   └── sarima_forecast.png
│
├── data/
│   └── README.md
│
├── notebooks/
│   └── retail_sales_forecasting.ipynb
│
├── results/
│   ├── forecast_24_months.csv
│   └── model_summary.txt
│
├── src/
│   └── forecast.py
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/oguzhanbekmezci6/retail-sales-forecasting-sarima.git
cd retail-sales-forecasting-sarima
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate the environment on Windows:

```bash
venv\Scripts\activate
```

Activate the environment on macOS or Linux:

```bash
source venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

---

## Usage

Run the Jupyter Notebook:

```bash
jupyter notebook notebooks/retail_sales_forecasting.ipynb
```

Or run the Python script:

```bash
python src/forecast.py
```

Forecast outputs can be stored in the `results/` directory.

---

## Technologies

- Python
- pandas
- NumPy
- Matplotlib
- SciPy
- statsmodels
- pmdarima
- Jupyter Notebook

---

## Example Modeling Workflow

```python
import pandas as pd
import pmdarima as pm

data = pd.read_csv(
    "data/retail_sales.csv",
    parse_dates=["date"],
    index_col="date"
)

sales = data["sales"].asfreq("MS")

model = pm.auto_arima(
    sales,
    seasonal=True,
    m=12,
    test="adf",
    information_criterion="aic",
    stepwise=True,
    suppress_warnings=True
)

forecast, confidence_intervals = model.predict(
    n_periods=24,
    return_conf_int=True,
    alpha=0.05
)

print(model.summary())
```

> Column names should be adjusted according to the dataset used in the project.

---

## Business Value

This forecasting workflow can support:

- Demand forecasting
- Inventory planning
- Procurement planning
- Sales target setting
- Budget planning
- Cash-flow forecasting
- Seasonal demand analysis

---

## Limitations

- The dataset contains only four years of monthly observations.
- Promotions, holidays, prices, inflation, and other external variables are not included.
- Structural breaks are not modeled separately.
- Forecast accuracy should be evaluated on holdout data using metrics such as MAE, RMSE, MAPE, or sMAPE.
- Good residual diagnostics do not necessarily mean the selected model will outperform every alternative model.

---

## Future Improvements

- Add rolling-origin cross-validation
- Perform backtesting on holdout observations
- Report MAE, RMSE, MAPE, and sMAPE
- Compare SARIMA with ETS and Prophet
- Build XGBoost and LightGBM forecasting models
- Add holiday and promotion variables
- Develop an interactive Streamlit dashboard
- Add automated model monitoring and retraining

---

## Author

**Oğuzhan Bekmezci**

Statistics Graduate  
Data Science · Machine Learning · Time Series Analysis

[GitHub Profile](https://github.com/oguzhanbekmezci6)

---

## License

The source code in this project is available under the MIT License.

The external dataset remains subject to its original license and terms of use.
