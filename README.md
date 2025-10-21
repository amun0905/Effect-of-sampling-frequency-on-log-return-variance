# Effect of Sampling Frequency on Log Return Variance

This notebook investigates how the sampling frequency of log returns affects the standard deviation (variance) of those returns.  
The topic is particularly relevant for Value-at-Risk (VaR)** modeling over longer horizons (e.g., biweekly, monthly), where returns can be sampled at different frequencies (daily, weekly, etc.).

---

##  Research Question

> What happens to the VaR estimate if biweekly returns are sampled on a daily basis instead of on a weekly basis?

Since parametric VaR depends on the standard deviation of returns, this becomes a question of how the sampling frequency affects the sample variance.

---

##  Model Setup

The notebook generates a synthetic asset price series using a Geometric Brownian Motion (GBM) model:

$$
dS_t = \mu S_t\,dt + \sigma S_t\,dW_t
$$

where  
- \( S_t \): asset price  
- \( \mu \): drift (expected return)  
- \( \sigma \): volatility  
- \( W_t \): standard Wiener process  

The discrete-time approximation over \( N \) steps and horizon \( T \) is:

$$
S_{t+\Delta t} = S_t \exp\left((\mu - \tfrac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t}\,Z_t\right), \quad Z_t \sim \mathcal{N}(0,1)
$$

Log returns are computed as:

$$
r_t = \log\left(\frac{S_t}{S_{t-1}}\right)
$$

---

##  Sampling Scenarios

Two sampling schemes for biweekly (10-day) returns are compared:

1. **Weekly sampling** — sample prices once per week (e.g., every Wednesday)  
   → fewer, non-overlapping observations

2. **Daily sampling** — compute biweekly returns every day  
   → more, overlapping observations

Although both represent the same 10-day horizon, their sample sizes and autocorrelation structures differ.

---

##  Results and Insights

- Both sampling methods yield similar distributions of log returns (same mean and variance in theory).  
- However, daily sampling produces many more overlapping observations, leading to serial correlation and smaller effective variance estimates.  
- As a result, **VaR estimates** derived from high-frequency (overlapping) samples may be **biased downward**, underestimating long-term risk.

---

##  Key Takeaways

- The **frequency of sampling** log returns directly influences the **estimated variance** and therefore VaR.  
- **Overlapping samples** artificially smooth the return distribution and reduce the apparent volatility.  
- For multi-day VaR estimation, it’s preferable to use **non-overlapping samples** (e.g., true weekly or biweekly returns).
