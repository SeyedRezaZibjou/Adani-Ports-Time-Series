

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Methodology](https://img.shields.io/badge/Methodology-CRISP--DM-orange.svg)](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)
[![Deep Learning](https://img.shields.io/badge/Deep%20Learning-SimpleRNN%20%7C%20Keras-red.svg)](https://keras.io/api/layers/recurrent_layers/simple_rnn/)
[![Time Series](https://img.shields.io/badge/Model-ARIMA%20(AutoARIMA)-green.svg)](https://alkaline-ml.com/pmdarima/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# Time Series Analysis & Forecasting: Adani Ports Stock Prices

An end-to-end quantitative financial time series forecasting project applied to **ADANIPORTS** (NSE India) equity data. Following the **CRISP-DM** methodology, this study integrates statistical time-series decomposition, transformation, and structural modeling (**ARIMA**) alongside deep recurrent neural networks (**SimpleRNN**).

The entire system is architected as an automated, reusable, and modular pipeline using Python Object-Oriented Programming (OOP).

---

## Project Overview
> **Author:** Seyed Reza Zibjou <br>
> **Asset:** Adani Ports and Special Economic Zone Ltd (`ADANIPORTS.csv`) <br>
> **Time Horizon:** November 27, 2007 – April 30, 2021 (3,322 trading days) <br>
> **Target Feature:** Volume Weighted Average Price (`VWAP`)


---

## End-to-End Pipeline Architecture

The pipeline orchestrates data flow sequentially from raw market logs to final economic forecasts:

```text
[Raw Stock Data: ADANIPORTS.csv]
              │
              ▼
    ┌───────────────────┐
    │   DataExplorer    │ ──► [EDA, Box-Hist, Rolling Volatility, ACF/PACF, Missing Audit]
    └─────────┬─────────┘
              │ (Inherited pipeline)
              ▼
    ┌───────────────────┐
    │  DataPrepration   │ ──► [Split Adj (5:1) ➔ IQR Clipping ➔ Box-Cox (λ=0.1430) ➔ Differencing ➔ ADF Test]
    └─────────┬─────────┘
              │ (Transformed clean series injected)
              ▼
    ┌───────────────────┐
    │     Modeling      │ ──► 1. ARIMA Pipeline: [AutoARIMA / GridSearch ➔ Fit ARIMA(0,0,1) ➔ Eval]
    └─────────┬─────────┘     2. RNN Pipeline:   [10-day Window Framing (3D) ➔ SimpleRNN ➔ Eval]
              │
              ▼
[Inverse Transform Engine] ──► Projects predictions back to original stock valuation (INR)
```  

---

## Table of Contents
1. Preface & Abstract
2. Business Understanding
3. Data Understanding
4. Data Preparation
5. Modeling

---

## 1. Preface & Abstract
This project implements a complete pipeline for time series analysis, preprocessing, and price forecasting of Adani Ports stock data from the National Stock Exchange of India (NSE). 

Following the CRISP-DM methodology, the project covers exploratory data analysis (EDA), handling anomalies and stock splits, variance stabilization, stationarity checks (ADF test), and building predictive models including ARIMA and RNNs.

---

## 2. Business Understanding
- **Introduction:** Adani Ports and Special Economic Zone Limited is a major port infrastructure company.
- **Project Objective:** Accurate stock price trend forecasting to support trading decisions using quantitative analysis.
- **CRISP-DM Framework:** Applied structured phases from business requirements definition to evaluation.

---

## 3. Data Understanding
- Exploratory Data Analysis (EDA) implemented via a custom `DataExplorer` class.
- Visualizations including time series plots, box plots, histograms, rolling statistics, and ACF/PACF analysis.
- Target column selection (e.g., VWAP - Volume Weighted Average Price).
- Missing value detection and analysis of sudden market drops.
- Explored financial trends, volume metrics, and high-frequency volatility.
- Identified an apparent **80.01% price drop** on **2010-09-23** caused by an unadjusted **5:1 Stock Split**.

---

## 4. Data Preparation
- Implemented via a custom `DataPrepration` class.
- Stock split adjustments and outlier management.
  - **Stock Split Adjustment:** Divided pre-split historical prices (before September 23, 2010) by 5 to maintain price series continuity.
  - **Outlier Clipping:** Handled extreme market tail risks without distorting underlying variance.
- Variance stabilization using Box-Cox transformation.
  - **Box-Cox Power Transform:** Stabilized heteroscedasticity ($\lambda = 0.1430$).
- Achieving stationarity through seasonal differencing and Augmented Dickey-Fuller (ADF) tests.
  - **Raw series: Augmented Dickey-Fuller (ADF):** $p\text{-value} = 0.9843$ (Non-stationary).
  - **First-order Differencing ($d=1$):** ADF Statistic $= -50.0329$ ($p\text{-value} = 0.0000$), confirming strict stationarity.

---

## 5. Modeling
- Implemented via a custom `Modeling` class.
- Dataset splitting, window generation for sequence models, and building RNN architectures.
- ARIMA, Auto-ARIMA, and Grid Search for optimal parameters ($p, d, q$).
- Model evaluation metrics and validation plotting.
  
**Baseline Statistical Modeling (ARIMA):**
  * Tuned using **AutoARIMA** (AIC minimization) and custom **Grid Search**.
  * Optimal Model: **ARIMA(0, 0, 1)** ($\text{AIC} = -8322.189$).
    
**Deep Sequence Modeling (SimpleRNN):**
  * Sliding Window / Lookback: `step = 10` trading days.
  * Train shape: `(2646, 10, 1)`, Test shape: `(655, 10, 1)`.
  * Architecture: `Input(10, 1)` $\rightarrow$ `SimpleRNN(units)` $\rightarrow$ `Dense(1)`.
  * Training callbacks: `EarlyStopping`, `ReduceLROnPlateau`, and `TensorBoard` integration.

---

## Benchmark Results

Both models were evaluated on the transformed stationary series (80% Train / 20% Test split):

| Model Architecture | Selected Order / Specs | MAE | RMSE | Status |
| :--- | :---: | :---: | :---: | :---: |
| **ARIMA** | $(0, 0, 1)$ | **0.031616** | **0.048265** | 🏆 **Best Performer** |
| **SimpleRNN** | Lookback = 10, Epochs w/ Callbacks | 0.032154 | 0.049313 | Competitive |

> **Key Takeaway:** While SimpleRNN successfully learned temporal dynamics and local autocorrelation, classical ARIMA(0,0,1) delivered slightly superior predictive accuracy with significantly lower computational overhead.
> 


---


## 📁 Repository Structure
```text
Adani-Ports-Time-Series/
├── ADANIPORTS.csv                     # Historical equity dataset (NSE India, 2007–2021)
├── Adaniports_Zibou_SeyedReza.ipynb   # Main Jupyter Notebook (Complete analysis, OOP pipeline & model training)
├── LICENSE                            # Project license (MIT License)
├── README-fa.md                       # Persian documentation (مستندات فارسی پروژه)
├── README.md                          # Main project documentation (English)
└── requirements.txt                   # Essential Libraries

```

---

## Getting Started

Follow these steps to set up the project environment and run the analysis locally.

### 1. Prerequisites
Ensure you have [Python 3.8+](https://www.python.org/downloads/) installed. We recommend using a virtual environment (venv or conda) to manage dependencies cleanly.

### 2. Installation
Clone this repository and install the required libraries:
```bash
# Clone the repository
git clone https://github.com/SeyedRezaZibjou/Adani-Ports-Time-Series.git
cd Adani-Ports-Time-Series

# Install dependencies
pip install -r requirements.txt
