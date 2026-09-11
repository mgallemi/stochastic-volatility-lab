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

## Python Example — Delta Hedging a Call

The previous example used Black-Scholes to answer:

> **“How much is this option worth?”**

Now we look at the same model from a different perspective:

> **“If I sell this option, how many shares of the stock should I hold to hedge my position?”**

This is where **delta hedging** becomes practical.

### 1. The Situation

Suppose we sell one European call with:

$$
S_0=100,\qquad K=100,\qquad r=5\%,\qquad
\sigma=20\%,\qquad T=1.
$$

The Black-Scholes model gives approximately:

$$
C_0=10.45
$$

and

$$
\Delta_0\approx0.637.
$$

If we have sold the call, our position has a negative delta:

$$
-\Delta_0=-0.637.
$$

To hedge this exposure, we buy approximately **0.637 shares**.

The idea is:

$$
\text{short call}+\text{long }0.637\text{ shares}
$$

The stock position offsets the option's sensitivity to movements in the stock price.

---

### 2. But the Hedge Does Not Stay the Same

Imagine that the stock price increases from €100 to €110.

The option becomes more sensitive to the stock price, so its delta increases.

We therefore need to buy **more shares** to remain hedged.

If the stock falls, delta decreases and we need fewer shares.

This is why the hedge is **dynamic**.

We can visualize this by calculating the delta for different stock prices:

```python
stock_prices = np.arange(80, 121, 5)

for S in stock_prices:
    price, delta = black_scholes_call(S, K, r, sigma, T)

    print(
        f"Stock: €{S:.0f} | "
        f"Option: €{price:.2f} | "
        f"Hedge: {delta:.3f} shares"
    )
```

The output might look roughly like:

```text
Stock: €80  | Option: €1.86 | Hedge: 0.221 shares
Stock: €85  | Option: €3.26 | Hedge: 0.337 shares
Stock: €90  | Option: €5.09 | Hedge: 0.430 shares
Stock: €95  | Option: €7.26 | Hedge: 0.535 shares
Stock: €100 | Option: €10.45 | Hedge: 0.637 shares
Stock: €105 | Option: €14.00 | Hedge: 0.728 shares
Stock: €110 | Option: €18.00 | Hedge: 0.812 shares
Stock: €115 | Option: €22.28 | Hedge: 0.874 shares
Stock: €120 | Option: €26.17 | Hedge: 0.916 shares
```

The important thing is not the exact numbers. It is the pattern:

$$
S\uparrow
\quad\Rightarrow\quad
\Delta\uparrow
$$

and therefore

$$
\text{shares in hedge}\uparrow.
$$

---

### 3. Thinking Like a Trader

Suppose the stock starts at €100.

We initially hold:

$$
0.637\text{ shares}.
$$

Then the stock rises to €110 and the delta becomes approximately:

$$
0.812.
$$

Our hedge is now too small.

We need to increase our stock position:

$$
0.812-0.637=0.175
$$

so we buy another **0.175 shares**.

If the stock later falls and delta decreases, we would sell some shares.

So the process looks like:

$$
\boxed{
\text{Calculate delta}
\rightarrow
\text{Hold delta shares}
\rightarrow
\text{Stock moves}
\rightarrow
\text{Recalculate delta}
\rightarrow
\text{Rebalance}
}
$$

This is the practical meaning of

$$
\Delta_t=f_S(t,S_t).
$$

---

### 4. Why This Connects to the Theory

The important insight is that **delta is not just a mathematical derivative**.

It has a direct financial interpretation:

$$
\boxed{
\Delta
=
\frac{\partial C}{\partial S}
=
\text{number of shares in the local replicating portfolio}
}
$$

The derivative tells us the option's sensitivity to the stock.

That same number tells us how much stock we need to hold to replicate that sensitivity.

This is why the derivative appears naturally in the Black-Scholes replication argument.

### The Two Views of Black-Scholes

We can therefore think about Black-Scholes in two complementary ways:

**Pricing view**

$$
\text{Market assumptions}
\rightarrow
\text{PDE}
\rightarrow
\text{Black-Scholes formula}
\rightarrow
\text{Option price}
$$

**Hedging view**

$$
\text{Option}
\rightarrow
\text{Calculate delta}
\rightarrow
\text{Hold stock}
\rightarrow
\text{Rebalance}
\rightarrow
\text{Replicate}
$$

These are not two different theories.

They are **two perspectives on the same no-arbitrage model**.

> The Black-Scholes price tells us what the option should cost, while the delta tells us how to construct the stock position needed to hedge or replicate it.

