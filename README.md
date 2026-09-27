# Time Series Analysis and Forecasting of Adani Ports Stock Prices

> **Author:** Seyed Reza Zibjou  
> **Dataset:** Adani Ports (NSE India)  

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

---

## 4. Data Preparation
- Implemented via a custom `DataPrepration` class.
- Stock split adjustments and outlier management.
- Variance stabilization using Box-Cox transformation.
- Achieving stationarity through seasonal differencing and Augmented Dickey-Fuller (ADF) tests.

---

## 5. Modeling
- Implemented via a custom `Modeling` class.
- Dataset splitting, window generation for sequence models, and building RNN architectures.
- ARIMA, Auto-ARIMA, and Grid Search for optimal parameters ($p, d, q$).
- Model evaluation metrics and validation plotting.
