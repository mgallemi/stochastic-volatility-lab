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


## Python Example — Black-Scholes with the S&P 500

So far, we used an abstract stock with $S=100$. Now let's use a more realistic underlying: the **S&P 500 index**.

Suppose we want to estimate the theoretical price of a European call option on the S&P 500.

The Black-Scholes model does not require the underlying to be an individual company. We can use an index level as $S$ as well.

### 1. Example Setup

Suppose the S&P 500 is currently at:

$$
S_0=5,500
$$

and we consider a European call with:

$$
K=5,500
$$

so the option is **at-the-money**.

Assume:

* Risk-free rate: $r=4%$
* Volatility: $\sigma=18%$
* Time to maturity: $T=0.5$ years

We can use the same Black-Scholes function:

```python
import numpy as np
from scipy.stats import norm


def black_scholes_call(S, K, r, sigma, T):

    d1 = (
        np.log(S / K) + (r + 0.5 * sigma**2) * T
    ) / (sigma * np.sqrt(T))

    d2 = d1 - sigma * np.sqrt(T)

    price = (
        S * norm.cdf(d1)
        - K * np.exp(-r * T) * norm.cdf(d2)
    )

    delta = norm.cdf(d1)

    return price, delta


# S&P 500 example
S = 5500
K = 5500
r = 0.04
sigma = 0.18
T = 0.5

price, delta = black_scholes_call(S, K, r, sigma, T)

print(f"Call price: {price:.2f} index points")
print(f"Delta: {delta:.3f}")
```

The result is approximately:

```text
Call price: 294.85 index points
Delta: 0.584
```

### 2. What Does This Mean?

The theoretical Black-Scholes price is approximately:

$$
C_0\approx294.85
$$

**S&P 500 index points**.

The delta is approximately:

$$
\Delta\approx0.584.
$$

So, locally, if the S&P 500 increases by 1 index point, the option price increases by approximately:

$$
0.584
$$

index points.

For example, if the S&P 500 moves from

$$
5500\rightarrow5501,
$$

the option price would increase by approximately:

$$
\Delta C\approx0.584\times1=0.584.
$$

Again, this is only a **local approximation** because delta changes as the index moves.

### 3. What Happens If the S&P 500 Moves?

We can see how both the option price and delta change:

```python
sp500_levels = np.arange(4500, 6501, 250)

for S in sp500_levels:

    price, delta = black_scholes_call(
        S, K, r, sigma, T
    )

    print(
        f"S&P 500: {S:.0f} | "
        f"Call: {price:.2f} | "
        f"Delta: {delta:.3f}"
    )
```

Conceptually, we expect:

$$
S\uparrow
\quad\Rightarrow\quad
C\uparrow
$$

and

$$
S\uparrow
\quad\Rightarrow\quad
\Delta\uparrow.
$$

When the S&P 500 is far below the strike, the call is unlikely to finish in-the-money, so its delta is relatively small.

When the S&P 500 is far above the strike, the call behaves more like the underlying itself, so its delta approaches 1.

Therefore:

$$
0<\Delta<1
$$

for a standard European call.

### 4. A Financial Interpretation

Imagine a trader has **sold the S&P 500 call**.

At the beginning:

$$
\Delta\approx0.584.
$$

The trader therefore needs exposure equivalent to approximately **0.584 units of the underlying** to hedge the option's local sensitivity.

If the S&P 500 rises, delta increases.

If the S&P 500 falls, delta decreases.

So the trader repeatedly adjusts the hedge:

$$
\boxed{
\text{S\&P 500 moves}
\rightarrow
\text{delta changes}
\rightarrow
\text{hedge is adjusted}
}
$$

This is the same dynamic replication idea we saw before, but now applied to a familiar market index.

### 5. One Important Real-World Detail

The example above is a **simplified Black-Scholes model**.

For an actual S&P 500 option, we would need to consider details such as:

* dividends paid by the companies in the index;
* the actual risk-free rate;
* the market-implied volatility;
* the exact maturity;
* whether the option is European or American.

In particular, for an index that pays dividends, the standard formula is adjusted using the dividend yield $q$:

$$
C=S_0e^{-qT}N(d_1)-Ke^{-rT}N(d_2).
$$

So the simple example above is mainly useful for understanding **how the model works**, rather than producing a real market quote.

### Key Idea

The important thing is that the underlying can be an **index**, not only an individual stock.

The same mathematical structure remains:

$$
\boxed{
S_0
\rightarrow
\text{Black-Scholes}
\rightarrow
C_0,\Delta
\rightarrow
\text{Dynamic hedge}
}
$$

This is one reason the Black-Scholes framework is so useful: once we understand the mathematical structure, we can apply it to many different underlying assets.


## Morgan Stanley Example

A practical example of where quantitative finance can be applied is **Morgan Stanley's Quantitative Finance division**.

For example, quantitative analysts may work on problems involving:

* derivative pricing
* risk modelling
* statistical analysis
* mathematical modelling
* computational methods
* trading and financial markets

This connects directly with concepts such as **option pricing, stochastic processes, simulation and volatility modelling**.

For example, a quantitative model could be used to estimate the value of an option:

```python
S = 100       # Current stock price
K = 100       # Strike price
r = 0.05      # Risk-free interest rate
sigma = 0.20  # Volatility
T = 1         # Time to maturity

price, delta = black_scholes_call(S, K, r, sigma, T)

print(f"Option price: {price:.2f}")
print(f"Delta: {delta:.3f}")
```

This gives a simple example of how **mathematical finance + Python** can be used in a real financial institution such as Morgan Stanley.


## Morgan Stanley Example — Dynamic Hedging

A quantitative finance team at a bank such as **Morgan Stanley** may use mathematical models to help price and hedge derivatives.

For example, suppose a trader has sold a European call option. The option's value changes when the underlying stock price changes.

The **delta** measures how sensitive the option is to the stock price:

$$
\Delta = \frac{\partial C}{\partial S}
$$

If the option has a delta of 0.60, the trader can initially hedge the position by holding approximately 0.60 shares for each option sold.

```python
S = 100
delta = 0.60

hedge = delta

print(f"Stock hedge: {hedge:.2f} shares")
```

If the stock price changes, the option's delta also changes. The hedge therefore needs to be adjusted:

```python
old_delta = 0.60
new_delta = 0.72

additional_shares = new_delta - old_delta

print(f"Additional shares needed: {additional_shares:.2f}")
```

The trader would need to buy an additional **0.12 shares** per option to update the hedge.

This illustrates the idea of **dynamic hedging**:

$$
\text{Calculate delta}
\rightarrow
\text{Hold the hedge}
\rightarrow
\text{Stock moves}
\rightarrow
\text{Recalculate delta}
\rightarrow
\text{Rebalance}
$$

This is one of the practical connections between **Black-Scholes, derivatives, stochastic processes and quantitative finance**.



## Morgan Stanley — AI in Quantitative Finance

AI and machine learning can also be applied to quantitative finance.

For example, a financial institution such as **Morgan Stanley** could use machine learning to identify patterns in large financial datasets and build models for tasks such as:

* predicting market variables
* detecting unusual trading activity
* estimating risk
* analysing financial data
* supporting trading and investment decisions

A simple example is using a machine learning model to predict whether the next stock return will be positive or negative.

```python
import numpy as np
from sklearn.linear_model import LogisticRegression

# Example features:
# previous day's return and volatility
X = np.array([
    [0.01, 0.02],
    [-0.02, 0.03],
    [0.015, 0.018],
    [-0.01, 0.025],
    [0.02, 0.019]
])

# 1 = positive return, 0 = negative return
y = np.array([1, 0, 1, 0, 1])

model = LogisticRegression()
model.fit(X, y)

# New observation
new_data = np.array([[0.01, 0.02]])

prediction = model.predict(new_data)

print("Predicted direction:", prediction[0])
```

The model learns a relationship between the input variables and the historical outcomes.

However, this does **not** mean that AI can reliably predict financial markets. Financial data is noisy, relationships can change over time, and a model that performs well on historical data may fail on unseen data.

This creates an important connection between **AI and quantitative finance**:

$$
\text{Financial Data}
\rightarrow
\text{Machine Learning}
\rightarrow
\text{Prediction}
\rightarrow
\text{Risk Analysis}
\rightarrow
\text{Trading Decision}
$$

In quantitative finance, the challenge is therefore not simply to build a more complex model, but to determine whether the model captures a **real and robust relationship** rather than noise in the data.
