# Financial_Intelligence
AI/ML GDG Induction Tasks
# 📈 Predictive Market Intelligence System (PS1)

> *A statistically grounded market analysis system built to forecast prices, quantify uncertainty, and explain market behavior using real financial data.*

This repository implements **PS1 (The Application Layer)** and fully addresses **Task 1 (Predictive Core)** and **Task 2 (Analytical Chatbot)** as defined in the problem statement.

The project is intentionally designed to reflect **real learning, real failures, and real reasoning**, rather than just polished outputs.

---

## 🧠 Project Motivation

Financial markets are noisy, non-stationary, and probabilistic by nature. Any serious predictive system must:

* Learn from historical data
* Avoid data leakage and unrealistic validation
* Quantify uncertainty instead of claiming certainty
* Explain market behavior using both numbers and context

This project was built with these principles in mind.

---

## 📊 Task 1 – Predictive Core (Static Time Series Forecasting)

### 🎯 Objective

To forecast short-term stock price movements while explicitly modeling **volatility and uncertainty**, not just point predictions.

---

### 📥 Data Source

* **Ticker**: `AAPL` (Apple Inc.)
* **Source**: `yfinance`
* **Date Range**: 2019-01-01 to 2026-01-01
* **Frequency**: Daily

The `yfinance` library was chosen because it provides clean, reproducible, and industry-standard market data.

---

### 🔍 Exploratory Analysis

* Visualized historical closing prices
* Observed long-term upward trend and short-term volatility
* Verified data integrity using `data.info()`

This step helped identify **non-stationarity**, a key property of financial time series.

---

### 🧮 Baseline Model: Moving Average (MA)

A short-window Moving Average (window = 2) was implemented as a **baseline**, not as a final forecasting model.

**Purpose of MA:**

* Establish a naive benchmark
* Understand price smoothness
* Compare against more formal models

This baseline achieved a very high apparent accuracy (~99%), which led to an important realization:

> High accuracy does not imply predictive power in financial time series.

---

### 📏 Evaluation Metrics

Metrics computed on the MA baseline:

* Mean Absolute Error (MAE)
* Root Mean Squared Error (RMSE)
* Mean Absolute Percentage Error (MAPE)

The inflated accuracy was caused by:

* Smooth day-to-day price changes
* MA using nearby values rather than true future prediction

This insight directly influenced the transition to ARIMA.

---

### 🧠 Forecasting Model: ARIMA

The core forecasting model is **ARIMA (AutoRegressive Integrated Moving Average)**.

#### Why ARIMA?

| Reason               | Explanation                        |
| -------------------- | ---------------------------------- |
| Financial relevance  | Designed for non-stationary series |
| Interpretability     | Clear statistical meaning          |
| Low data requirement | Works well with limited history    |
| Uncertainty modeling | Native confidence intervals        |

Rather than treating the market as a black box, ARIMA enables **reasoned forecasting**.

---

### 🔁 Train–Test Strategy

* 80% data used for training
* 20% reserved for testing

This split allowed comparison between historical behavior and unseen data.

---

### 📈 Forecasting Output

* 7-day forward forecast
* Confidence intervals capturing volatility
* Combined visualization of:

  * Training data
  * Testing data
  * ARIMA forecast
  * Uncertainty bands

The confidence interval is treated as a **first-class output**, not an afterthought.

---

## 🤖 Task 2 – Analytical Chatbot (RAG-Based Explanation)

### 🎯 Objective

To allow users to query the market in natural language and receive **data-grounded, analyst-style explanations**.

This task focuses on understanding *why* a stock moved, not predicting *where* it will go.

---

### 🧠 Pipeline Overview

```
User Query
   ↓
Ticker Extraction (LLM + Regex Guardrails)
   ↓
Financial News Retrieval (Tavily)
   ↓
Vector Storage & Search (ChromaDB)
   ↓
Quantitative Grounding (yfinance)
   ↓
LLM-Based Reasoned Explanation
```

---

### 🧾 Vector Database: ChromaDB

* News snippets embedded using **HuggingFace MiniLM**
* Stored in **ChromaDB** for semantic similarity search
* Enables retrieval of contextually relevant financial news

This ensures explanations are **grounded in retrieved evidence**, not hallucination.

---

### 📊 Quantitative Grounding

Live numerical metrics fetched via `yfinance`:

* Current price
* 52-week change
* Market capitalization

These values act as factual anchors during reasoning.

---

### 🧠 Explanation Strategy

The LLM (Gemini) synthesizes:

* Retrieved news context
* Live market statistics
* User intent

into a professional, data-driven explanation.

---

## ⚠️ Challenges, Failures & Key Learnings

### Task 1 – Forecasting

**Non-stationary prices**
Raw closing prices violated ARIMA assumptions → learned importance of differencing.

**Inflated accuracy from MA baseline**
High accuracy revealed metric misuse rather than model superiority.

**Risk of data leakage**
Training on future data highlighted the need for strict validation discipline.

**Overconfidence in point predictions**
Led to prioritizing confidence intervals over single values.

---

### Task 2 – Chatbot

**Ticker extraction errors**
Solved using LLM parsing with regex guardrails.

**Irrelevant news retrieval**
Improved through semantic search with ChromaDB.

**LLM hallucinations**
Mitigated by enforcing quantitative grounding via yfinance.

---

## 🧠 Final Reflection

This project was built through experimentation, failure, and iteration.

More than forecasting prices, it demonstrates:

* Statistical thinking
* Awareness of market uncertainty
* Understanding of AI system limitations
* Ability to reason about failures

> *The strongest learning came not from high accuracy, but from understanding why accuracy can be misleading in financial markets.*

---

📌 *This repository reflects time invested in learning how markets and models actually behave — not just how to make them run.*
