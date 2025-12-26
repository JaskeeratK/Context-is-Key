# Context-is-Key
# Context Is Key
### Implementing *Context Is Key: Leveraging Contextual Information for Time Series Forecasting with LLMs*

This repository contains my implementation of the research paper **“Context Is Key” (arXiv:2410.18959)**.

The central idea of the paper is straightforward but important:  
**Large Language Models perform significantly better at time-series forecasting when relevant contextual information is explicitly provided.**

In this project, I compare three forecasting approaches:
1. ARIMA (traditional statistical baseline)
2. LLM without context
3. LLM with context (the main contribution of the paper)

The results clearly show why context plays a critical role when using LLMs for forecasting tasks.

---

## What this project does

- Uses real retail time-series data (weekly sales)
- Forecasts future values using:
  - ARIMA
  - LLM with a plain prompt
  - LLM with structured contextual information (holidays, store metadata, seasonality)
- Compares predictions visually and quantitatively using MAE
- Reproduces the key comparison plot used in the paper

This repository is meant to be educational, reproducible, and easy to extend.

---

## Core idea

Traditional forecasting models like ARIMA operate purely on numerical patterns in the data.  
LLMs, on the other hand, are capable of using descriptive information and reasoning over it.

Instead of providing the LLM with only a sequence of historical values, additional information is included, such as:
- Store-level metadata
- Whether holidays occur in the forecast window
- Seasonal characteristics

This added context allows the LLM to reason about the data rather than simply extrapolate past trends.

---

## Tech Stack

- Python
- Statsmodels (ARIMA)
- Pandas and NumPy
- Matplotlib
- Groq API for LLM inference
- Colab Notebook

---

### Large Language Model

All LLM-based forecasts are generated using **LLaMA-based models accessed via the Groq API**.

- The model receives historical sales values as text
- In the context-aware setup, additional dataset-derived metadata is included in the prompt
- The model outputs numerical forecasts for the specified future horizon

The same LLM is used for both the context-free and context-aware experiments, ensuring that performance differences arise solely from the presence or absence of contextual information.
## Dataset

This project uses the **Walmart Store Sales Forecasting dataset**, a publicly available retail time-series dataset commonly used for demand forecasting.

---
### Data sources

The dataset consists of three CSV files:

- **sales.csv**  
  Contains historical weekly sales data at the **store–department level**.
  - Key columns: `Store`, `Dept`, `Date`, `Weekly_Sales`, `IsHoliday`

- **features.csv**  
  Contains external factors aligned by store and date.
  - Includes holiday indicators and additional temporal features

- **stores.csv**  
  Contains store-level metadata.
  - Includes store `Type` and `Size`

These files are merged on `Store`, `Date`, and `IsHoliday` to form a unified dataset.

### Temporal characteristics

- **Frequency:** Weekly  
- **Time index:** `Date` (converted to datetime)
- **Target variable:** `Weekly_Sales`
- **Granularity:** Store–department level time series

### Forecasting setup

For each experiment, a **single store–department pair** is selected from the merged dataset.

- **History window:** 52 weeks (used as model input)
- **Forecast horizon:** 8 weeks (used as the test period)

The data is split chronologically:
- Past 52 weeks → input to ARIMA and LLMs
- Next 8 weeks → ground truth for evaluation

### Use of contextual information

- **ARIMA** and **LLM without context** use only historical `Weekly_Sales` values.
- **LLM with context** additionally receives:
  - Store type and store size
  - Holiday indicators from the dataset
  - Calendar-based seasonal information

This mirrors the approach described in the paper, where contextual signals are injected as natural language rather than engineered numerical features.

## Workflow Overview

### Data Preparation

- Load weekly sales data
- Split the data into:
  - A historical window used as model input
  - A future window used as the forecasting target

---

### ARIMA Baseline

ARIMA is used as a classical baseline model:
- Trained only on historical sales values
- Does not incorporate any external or contextual information
- Serves as a reference point for comparison

---

### LLM without Context

In this setup, the LLM is prompted with:
- Past sales values only
- A request to predict the next few time steps

This evaluates how well an LLM performs without any additional reasoning cues.

---

### LLM with Context

In the context-aware setup, the prompt includes:
- Historical sales values
- Store-related metadata
- Holiday indicators
- Seasonal information

This setup reflects the main claim of the paper: providing contextual signals significantly improves forecasting quality.

---

## Visualization

The final output includes a time-series comparison plot showing:
- Ground truth values
- ARIMA predictions
- LLM predictions without context
- LLM predictions with context

This mirrors the comparison presented in the paper and makes the performance differences clear.

---

## Evaluation

Models are evaluated using **Mean Absolute Error (MAE)**.

The expected performance trend is:

LLM with context < LLM without context < ARIMA

This ordering is consistent with the findings reported in the original paper.

---

## Future Improvements

Possible extensions to this work include:
- Multi-store forecasting
- Longer forecast horizons
- Automated context generation
- Confidence interval estimation
- Comparison with additional statistical baselines

---

## Reference

If you use or build upon this work, please refer to the original paper:

**Context Is Key: Leveraging Contextual Information for Time Series Forecasting with LLMs**  
arXiv:2410.18959

---


