# Mortality Forecasting Model

This project builds a Lee-Carter mortality forecasting model using U.S. age-specific mortality data. The goal is to estimate historical mortality patterns, forecast future mortality rates, and interpret the implications for life expectancy and longevity risk.

## Project Overview

The Lee-Carter model represents log mortality as:

$$
\log m_{x,t} = a_x + b_x k_t + \epsilon_{x,t}
$$

where:

* $m_{x,t}$ is the central death rate at age $x$ and year $t$
* $a_x$ represents the average age-specific mortality level
* $b_x$ measures how sensitive each age is to changes in the mortality trend
* $k_t$ represents the overall mortality index over time

The model is estimated using singular value decomposition on centered log mortality rates.

## Data

The project uses U.S. period 1x1 age-specific death rates from the Human Mortality Database. The raw data includes mortality rates by year, age, and sex. This project focuses on total population mortality and uses ages 0 through 100 for the refined Lee-Carter model.

The raw data file is not included in this repository. Processed outputs and model results are generated from the notebooks.

## Methodology

The project is organized into four main steps:

1. **Data Cleaning**
   The raw mortality file is loaded, cleaned, and transformed into an age-by-year mortality matrix.

2. **Lee-Carter Parameter Estimation**
   The model estimates $a_x$, $b_x$, and $k_t$ using singular value decomposition. The first SVD component explains approximately 89.7% of the centered log-mortality variation.

3. **Mortality Forecasting**
   The mortality index $k_t$ is forecasted using a random-walk-with-drift model. Future age-specific mortality rates are then reconstructed through the Lee-Carter equation.

4. **Life Expectancy Projection**
   Forecasted mortality rates are converted into approximate period life tables to project life expectancy at birth and remaining life expectancy at age 65 through 2050.

## Jump-Off Adjustment

The project applies a jump-off adjustment so that projected mortality rates begin from the latest observed mortality rates rather than the fitted Lee-Carter values. This avoids discontinuity between historical and projected life expectancy.

## Sensitivity Analysis

Several drift assumptions are compared:

* Full sample: 1933–2024
* Recent period: 1970–2024
* Modern period: 1980–2024
* Pre-COVID modern period: 1980–2019

The full-sample drift produces the most optimistic projection, while the modern 1980–2024 drift produces the most conservative projection. Under the baseline jump-off adjusted forecast, life expectancy at birth is projected to increase from approximately 79.3 years in 2024 to approximately 82.6 years in 2050. Remaining life expectancy at age 65 is projected to increase to approximately 22.0 years by 2050.

## Key Results

* Estimated Lee-Carter mortality parameters using singular value decomposition
* Explained approximately 89.7% of centered log-mortality variation with the first component
* Forecasted the mortality index through 2050 using random walk with drift
* Projected U.S. life expectancy at birth to approximately 82.6 years by 2050 under the baseline forecast
* Conducted sensitivity analysis under alternative mortality improvement assumptions

## Actuarial Interpretation

The results suggest continued mortality improvement over time. This has different implications across insurance products:

* For life insurance, lower mortality rates may reduce expected death claims in the near term.
* For annuities, longer lifetimes increase the expected duration of benefit payments.
* For pension systems, increasing life expectancy raises longevity risk and may increase long-term liabilities.

This project demonstrates how mortality forecasting models can support actuarial analysis of longevity risk.

## Project Structure

```text
mortality-forecasting-model/
├── data/
│   ├── raw/
│   │   └── Mx_1x1.txt
│   └── processed/
│       ├── us_total_mortality_matrix.csv
│       ├── us_total_log_mortality_matrix.csv
│       ├── a_x_100.csv
│       ├── b_x_100.csv
│       ├── k_t_100.csv
│       ├── adjusted_forecast_mx_2050.csv
│       └── sensitivity_life_expectancy_results.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_lee_carter_estimation.ipynb
│   ├── 03_time_series_forecast.ipynb
│   └── 04_life_expectancy_projection.ipynb
└── README.md
```

## Tools Used

* Python
* pandas
* numpy
* matplotlib
* singular value decomposition
* time series forecasting
* life table construction
