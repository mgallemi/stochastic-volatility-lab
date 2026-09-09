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

4. Why Is Delta Important for Hedging?

This is where the previous mathematical theory becomes concrete.

Suppose we have sold one call option.

The option has delta:

$$
\Delta=0.637.
$$

To replicate the option locally, we would hold approximately 0.637 shares of the stock.

If the stock moves, however, the option's delta changes.

For example:

Stock price        Call delta
€80                lower
€90                lower
€100               ≈ 0.637
€110               higher
€120               higher

Therefore, we cannot simply buy 0.637 shares once and leave the portfolio unchanged.

We need to recalculate delta and adjust the number of shares.

That is dynamic delta hedging.

5. Seeing Delta Change

We can let Python calculate the option price and delta for different stock prices:

stock_prices = np.linspace(80, 120, 9)

for S in stock_prices:
    price, delta = black_scholes_call(S, K, r, sigma, T)

    print(
        f"S = €{S:.0f} | "
        f"Call = €{price:.2f} | "
        f"Delta = {delta:.3f}"
    )

You should see that:

when the stock price is low, the call is less sensitive to the stock;
as the stock price increases, the delta increases;
the option price also increases.

The important relationship is:

$$
\boxed{\Delta=\frac{\partial C}{\partial S}}
$$

Delta is therefore the sensitivity of the option price to the underlying price.

It is also the number of shares used in the replicating portfolio.

These are two ways of looking at exactly the same quantity.

6. Connecting Everything

We can now see the full story:

$$
V_t=f(t,S_t)
$$

The option price depends on the current state of the market.

Using Itô's formula, we can describe how $V_t$ changes as $S_t$ moves.

The derivative

$$
\frac{\partial f}{\partial S}
$$

gives the delta.

Delta tells us how much of the underlying asset is needed in the replicating portfolio.

Because delta changes over time, we dynamically rebalance the portfolio.

The no-arbitrage condition then forces $f$ to satisfy the Black-Scholes PDE.

Solving that PDE gives the Black-Scholes formula, which we can finally implement in Python.

$$
\boxed{
\text{Stock model}
\rightarrow
\text{Itô}
\rightarrow
\text{Delta}
\rightarrow
\text{Dynamic replication}
\rightarrow
\text{PDE}
\rightarrow
\text{Black-Scholes price}
}
$$

The Python implementation lets us evaluate the final formula and experiment with how the option price and delta change when we change $S$, $K$, $r$, $\sigma$, or $T$.

The next natural step is to simulate the stock price through time and actually update the delta of the option at each time step. This turns the mathematical idea of dynamic replication into an actual Python simulation.
