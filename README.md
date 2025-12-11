# Rolling CAPM Alpha Analysis (Daily) — TOPIX × Japanese Stocks

This repository provides a quantitative analysis of **daily CAPM alpha**  for Japanese equities using **rolling OLS regression** with the **TOPIX** index  as the market factor.  The project compares **large-cap** and **small-cap** stocks to highlight differences in alpha stability, persistence, and idiosyncratic risk.

---

##  Overview

The goal of this project is to estimate **daily CAPM alpha**:

\[
R_{i,t} = \alpha_i + \beta_i R_{m,t} + \varepsilon_t
\]

using a **rolling 250-day (1 year) window**, and evaluate:

- How alpha evolves over time  
- How stable alpha is for large-cap vs small-cap stocks  
- Whether alpha shows persistence (AR(1))  
- Differences in idiosyncratic volatility across market caps  

This approach is widely used in quantitative equity research  
and hedge-fund style performance attribution.

---

##  Features

###  Daily rolling CAPM alpha  
- Uses `statsmodels` OLS  
- 250-day rolling window  
- Requires 120+ observations to estimate alpha  

###  Market model with **TOPIX**  
TOPIX data is downloaded reliably using Stooq.

###  Comparison:  
- **Large-cap stocks** (Toyota, Sony, KDDI, etc.)  
- **Small-cap stocks** (~30–300 billion yen market cap)

###  Alpha persistence (AR(1))  
\[
\alpha_t = c + \phi \alpha_{t-1} + e_t
\]

- φ close to 1 → persistent alpha  
- φ near 0 → noisy alpha (typical for small caps)

###  Visualization  
- Rolling alpha charts for large & small caps  
- Shows structural differences in alpha behavior

---

##  Example Output

### Large-cap stocks  
- Alpha is near zero (efficient market behavior)  
- Low volatility, smooth curves  
- Little persistent abnormal performance  

### Small-cap stocks  
- Higher alpha variance  
- Large idiosyncratic shocks  
- Occasional positive spikes (SMB factor behavior)  
- Lower stability vs large caps

- small_candidates = [
- "4443.T",  # Sansan小型 SaaS
- "7359.T",  # 東京通信
- "4192.T",  # スパイダープラス
- "4375.T",  # セーフィー
- "2934.T",  # ジャパンフーズ
- "6613.T",  # QDレーザ
- "3491.T",  # GA technologies（中小型）
- "6081.T",  # アライドアーキテクツ
- "3182.T",  # オイシックス（中小型）
- "3134.T",  # Hamee
- "4385.T",  # メルカリ子会社不使用→クラウド小型
- "7095.T",  # Macbee Planet
- "4884.T",  # クリングルファーマ
- "4593.T",  # ヘリオス
]


---

##  Methodology

### 1. Download prices  
- `yfinance` for individual stocks  
- `pandas_datareader` (Stooq) for TOPIX  
- Adjusted close used for all equities  

### 2. Compute daily returns  
\[
R_{t} = \frac{P_t}{P_{t-1}} - 1
\]

### 3. Rolling OLS  
Performed for each stock independently:

```python
model = sm.OLS(y_window, X_window).fit()
alpha = model.params["const"]
````

### 4. Alpha persistence

AR(1):

```python
model = sm.OLS(y, sm.add_constant(x)).fit()
phi = model.params[1]
```

### 5. Visualization

Matplotlib-generated alpha curves highlight structural differences.

---

##  Technologies Used

* Python 3.10+
* pandas
* numpy
* yfinance
* pandas_datareader
* statsmodels
* matplotlib

---

##  How to Run

Install dependencies:

```bash
pip install yfinance pandas numpy statsmodels pandas_datareader matplotlib
```

Run the analysis:

```bash
python rolling_alpha_topix.py
```

---

##  Notes

* Some small-cap stocks are frequently delisted; the script handles missing tickers safely.
* Daily alpha is typically small:

  * Large caps: ±0.0003
  * Small caps: ±0.002〜0.01
* Annualized alpha ≈ daily alpha × 250

---

##  Author

Created by **Haruka**
Economics student / Quant finance enthusiast
