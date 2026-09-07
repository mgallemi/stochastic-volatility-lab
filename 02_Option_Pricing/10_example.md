# Python Example — Black-Scholes Pricing and Delta

We can implement the Black-Scholes formula in Python to see how the theoretical option price and its delta depend on the underlying asset price.

For a European call:

$$
C=S_0N(d_1)-Ke^{-rT}N(d_2)
$$

and its delta is

$$
\Delta=N(d_1).
$$

```python
import numpy as np
from scipy.stats import norm


def black_scholes_call(S, K, r, sigma, T):
    d1 = (
        np.log(S / K) + (r + 0.5 * sigma**2) * T
    ) / (sigma * np.sqrt(T))

    d2 = d1 - sigma * np.sqrt(T)

    price = S * norm.cdf(d1) - K * np.exp(-r * T) * norm.cdf(d2)
    delta = norm.cdf(d1)

    return price, delta


# Example parameters
S = 100       # current stock price
K = 100       # strike price
r = 0.05      # risk-free interest rate
sigma = 0.20  # volatility
T = 1         # time to maturity

price, delta = black_scholes_call(S, K, r, sigma, T)

print(f"Call price: €{price:.2f}")
print(f"Delta: {delta:.4f}")
```

For these parameters, the call is worth approximately **€10.45**, and its delta is approximately **0.64**.

This means that around $S=100$, a €1 increase in the stock price corresponds approximately to a €0.64 increase in the option price.

### Seeing Delta Change

Delta is not constant. As the underlying price changes, the option's sensitivity changes too.

```python
stock_prices = np.linspace(80, 120, 9)

for S in stock_prices:
    price, delta = black_scholes_call(S, K, r, sigma, T)
    print(f"S = €{S:.0f} | Call = €{price:.2f} | Delta = {delta:.3f}")
```

This illustrates the idea of **dynamic hedging**:

$$
\Delta_t=f_S(t,S_t)
$$

As $S_t$ changes, $\Delta_t$ changes, so the replicating portfolio must be rebalanced.

### Key Takeaway

The Python implementation connects the mathematical theory to computation:

$$
\text{Black-Scholes PDE}
\rightarrow
\text{Black-Scholes formula}
\rightarrow
\text{Option price + Delta}
\rightarrow
\text{Dynamic hedging}
$$

Later, we can go one step further and **simulate stock paths and actually rebalance the delta hedge in Python**. That would make the replication idea much more tangible.
