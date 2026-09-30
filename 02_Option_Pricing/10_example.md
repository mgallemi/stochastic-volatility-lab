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


## Asset Management — Portfolio Allocation

Asset management involves managing investments on behalf of clients, such as individuals, pension funds or institutions.

A quantitative approach can be used to decide how much of a portfolio to allocate to different assets.

For example, suppose we have three assets:

* Stock A
* Stock B
* Bond C

We can calculate the expected return of a portfolio as:

$$
E[R_p] = \sum_{i=1}^{n} w_i E[R_i]
$$

where:

* $w_i$ is the weight of asset $i$
* $E[R_i]$ is the expected return of asset $i$

For example:

```python
import numpy as np

# Portfolio weights
weights = np.array([0.40, 0.35, 0.25])

# Expected annual returns
expected_returns = np.array([0.08, 0.10, 0.04])

portfolio_return = np.dot(weights, expected_returns)

print(f"Expected portfolio return: {portfolio_return:.2%}")
```

The portfolio's expected return is:

$$
0.40(0.08) + 0.35(0.10) + 0.25(0.04)
= 0.078
$$

so the expected return is **7.8%**.

In practice, an asset manager would consider more than expected return. They may also analyse:

* risk and volatility
* correlations between assets
* diversification
* liquidity
* market conditions
* client objectives and constraints

AI and machine learning can also be used to analyse large datasets and identify patterns that may help inform portfolio construction or risk analysis.

The general idea is:

$$
\text{Data}
\rightarrow
\text{Risk \& Return Analysis}
\rightarrow
\text{Portfolio Construction}
\rightarrow
\text{Portfolio Management}
$$

This is one way that **mathematics, statistics, Python and AI** can connect with asset management.


## Options and Securities

An **option** is a financial derivative whose value depends on an underlying security, such as a stock.

For example, consider a European call option on a stock:

* Stock price: $100
* Strike price: $105
* Maturity: 1 year
* Volatility: 20%
* Risk-free rate: 5%

The call gives the holder the **right, but not the obligation**, to buy the stock for $105 at maturity.

Its payoff is:

$$
C_T = \max(S_T-K,0)
$$

For example, if the stock finishes at $120:

$$
C_T = \max(120-105,0)=15
$$

If the stock finishes at $90:

$$
C_T = \max(90-105,0)=0
$$

We can calculate the payoff for different possible stock prices using Python:

```python
import numpy as np

strike = 105

stock_prices = np.array([80, 90, 100, 105, 120, 130])

call_payoffs = np.maximum(stock_prices - strike, 0)

for S, payoff in zip(stock_prices, call_payoffs):
    print(f"Stock: ${S:.0f} → Call payoff: ${payoff:.0f}")
```

The important idea is that the option payoff is **non-linear**:

$$
C_T = \max(S_T-K,0)
$$

This non-linearity is why options require different pricing and risk-management techniques from simply holding the underlying security.

In quantitative finance, models such as **Black-Scholes** can be used to estimate the option's fair price, while Greeks such as **Delta, Gamma and Vega** help measure its sensitivity to different risk factors.

$$
\text{Security}
\rightarrow
\text{Option}
\rightarrow
\text{Payoff}
\rightarrow
\text{Pricing}
\rightarrow
\text{Risk Management}
$$



## Black-Scholes Example

The **Black-Scholes model** provides a theoretical price for a European option.

Consider a European call option with:

* Stock price: $S_0 = 100$
* Strike price: $K = 100$
* Risk-free interest rate: $r = 5%$
* Volatility: $\sigma = 20%$
* Time to maturity: $T = 1$ year

For a European call, the Black-Scholes formula is:

$$
C = S_0N(d_1)-Ke^{-rT}N(d_2)
$$

where

$$
d_1 =
\frac{
\ln(S_0/K)+(r+\frac{1}{2}\sigma^2)T
}{
\sigma\sqrt{T}
}
$$

and

$$
d_2=d_1-\sigma\sqrt{T}
$$

Using Python:

```python
import numpy as np
from scipy.stats import norm

S = 100
K = 100
r = 0.05
sigma = 0.20
T = 1

d1 = (np.log(S / K) + (r + 0.5 * sigma**2) * T) / (
    sigma * np.sqrt(T)
)

d2 = d1 - sigma * np.sqrt(T)

call_price = S * norm.cdf(d1) - K * np.exp(-r * T) * norm.cdf(d2)

print(f"d1 = {d1:.4f}")
print(f"d2 = {d2:.4f}")
print(f"Call price = ${call_price:.2f}")
```

The result is approximately:

```text
d1 = 0.3500
d2 = 0.1500
Call price = $10.45
```

So, under the Black-Scholes assumptions, the theoretical price of the call is approximately **$10.45**.

The model also gives the option's **Delta**:

$$
\Delta = N(d_1)
$$

```python
delta = norm.cdf(d1)

print(f"Delta = {delta:.3f}")
```

which gives approximately:

```text
Delta = 0.637
```

This means that, locally, a $1 increase in the stock price corresponds to approximately a **$0.637 increase in the option price**, assuming the other variables remain unchanged.

### From theory to practice

Black-Scholes connects several ideas in quantitative finance:

$$
\text{Stochastic Process}
\rightarrow
\text{Itô's Formula}
\rightarrow
\text{Dynamic Hedging}
\rightarrow
\text{Black-Scholes PDE}
\rightarrow
\text{Option Price}
$$

The model therefore provides both a **pricing framework** and a way to understand how an option can be **hedged dynamically**.



## Machine Learning Example

**Machine learning** allows a computer to learn patterns from data instead of being explicitly programmed with every rule.

For example, we can train a simple model to predict whether a stock's return will be **positive or negative** based on previous market information.

We first create some example data:

```python
import numpy as np
from sklearn.linear_model import LogisticRegression

# Features:
# [previous return, volatility]
X = np.array([
    [0.02, 0.15],
    [-0.01, 0.20],
    [0.03, 0.12],
    [-0.02, 0.25],
    [0.01, 0.18],
    [0.04, 0.10]
])

# Target:
# 1 = positive return
# 0 = negative return
y = np.array([1, 0, 1, 0, 1, 1])
```

We can then train a **logistic regression** model:

```python
model = LogisticRegression()

model.fit(X, y)
```

The model learns a relationship between the features and the target.

We can use it to make a prediction for new data:

```python
new_data = np.array([[0.02, 0.16]])

prediction = model.predict(new_data)

print("Predicted class:", prediction[0])
```

If the output is:

```text
Predicted class: 1
```

the model predicts a **positive return**.

We can also obtain the probability of each class:

```python
probability = model.predict_proba(new_data)

print(probability)
```

### Important idea

The model is not given a rule such as:

> "If volatil


## Itô's Formula Applied to Black-Scholes

In the Black-Scholes model, the stock price follows a stochastic process:

$$
dS_t = \mu S_t\,dt + \sigma S_t\,dW_t
$$

where:

* $S_t$ is the stock price
* $\mu$ is the expected return
* $\sigma$ is the volatility
* $W_t$ is a Brownian motion

An option price depends on both **time** and the stock price:

$$
V_t = f(t,S_t)
$$

Because $S_t$ is stochastic, the ordinary chain rule is not sufficient. We use **Itô's Formula**.

For a function $f(t,S_t)$:

$$
df =
f_t\,dt
+
f_S\,dS_t
+
\frac{1}{2}f_{SS}(dS_t)^2
$$

Substituting the stock process:

$$
dS_t = \mu S_t\,dt+\sigma S_t\,dW_t
$$

and using

$$
(dW_t)^2=dt
$$

gives:

$$
df =
\left(
f_t
+
\mu S_t f_S
+
\frac{1}{2}\sigma^2S_t^2f_{SS}
\right)dt
+
\sigma S_t f_S\,dW_t
$$

This is important because it describes how the option price changes as the stock price evolves.

### Connection with Delta

The coefficient

$$
f_S
$$

is the **Delta** of the option:

$$
\Delta = \frac{\partial V}{\partial S}
$$

Therefore, Delta tells us how much the option price changes locally when the underlying stock changes.

In Black-Scholes, we can construct a portfolio containing the option and the underlying stock. By choosing the number of shares appropriately, the stochastic $dW_t$ component can be eliminated.

This is the key idea behind **dynamic hedging**:

$$
\text{Itô's Formula}
\rightarrow
\text{Delta}
\rightarrow
\text{Hedging}
\rightarrow
\text{Black-Scholes PDE}
\rightarrow
\text{Option Price}
$$

The resulting Black-Scholes PDE is:

$$
f_t
+
\frac{1}{2}\sigma^2S^2f_{SS}
+
rSf_S
-rf
=0
$$

with the terminal condition for a European call:

$$
f(T,S_T)=\max(S_T-K,0)
$$

Solving this PDE gives the Black-Scholes option pricing formula.

## Black-Scholes as a Quantitative Finance Algorithm

Black-Scholes can be viewed as a mathematical machine that transforms market inputs into an option price and its sensitivities.

For a European call option, we provide:

* Stock price $S$
* Strike price $K$
* Risk-free rate $r$
* Volatility $\sigma$
* Time to maturity $T$

The model first calculates:

$$
d_1 =
\frac{
\ln(S/K)+(r+\frac{1}{2}\sigma^2)T
}{
\sigma\sqrt{T}
}
$$

and

$$
d_2=d_1-\sigma\sqrt{T}
$$

Then it produces the option price:

$$
C=S N(d_1)-Ke^{-rT}N(d_2)
$$

and the Delta:

$$
\Delta=N(d_1)
$$

We can represent the process like a computational pipeline:

$$
\boxed{
(S,K,r,\sigma,T)
\rightarrow
(d_1,d_2)
\rightarrow
(C,\Delta)
}
$$

In Python:

```python
import numpy as np
from scipy.stats import norm

def black_scholes(S, K, r, sigma, T):

    d1 = (
        np.log(S / K)
        + (r + 0.5 * sigma**2) * T
    ) / (sigma * np.sqrt(T))

    d2 = d1 - sigma * np.sqrt(T)

    price = (
        S * norm.cdf(d1)
        - K * np.exp(-r * T) * norm.cdf(d2)
    )

    delta = norm.cdf(d1)

    return price, delta
```

We can then feed the market information into the model:

```python
S = 100
K = 100
r = 0.05
sigma = 0.20
T = 1

price, delta = black_scholes(S, K, r, sigma, T)

print(f"Option price: ${price:.2f}")
print(f"Delta: {delta:.3f}")
```

Output:

```text
Option price: $10.45
Delta: 0.637
```

The important idea is that the model is not simply calculating a number. It provides information that can be used for **risk management and hedging**.

For example, if the Delta is $0.637$, a trader hedging one short call would initially hold approximately $0.637$ shares of the underlying.

If the stock price changes, the Delta changes as well:

$$
S_t \uparrow
\quad\Rightarrow\quad
\Delta_t \text{ changes}
$$

The trader can therefore repeatedly recalculate the Delta and adjust the hedge.

This creates a feedback loop:

$$
\text{Market Data}
\rightarrow
\text{Black-Scholes}
\rightarrow
\text{Price + Greeks}
\rightarrow
\text{Hedge}
\rightarrow
\text{New Market Data}
\rightarrow
\text{Recalculate}
$$

This is the **quantitative and computational side of derivatives trading**: mathematical models are turned into algorithms that continuously process market information and support pricing and risk management.


# Chapter Recap — From No-Arbitrage to Black-Scholes

This chapter introduced the main ideas behind **derivatives pricing and quantitative finance**, starting from basic financial instruments and gradually connecting them to mathematical modelling, stochastic calculus and computation.

## 1. Financial Instruments

A financial market contains different types of instruments:

* **Stocks** represent ownership of a company.
* **Bonds** represent lending money to an issuer.
* **Forwards and futures** create obligations to buy or sell an asset in the future.
* **Options** give the buyer the right, but not the obligation, to buy or sell an underlying asset.

For a European call option, the payoff at maturity is:

$$
C_T = \max(S_T-K,0)
$$

where $S_T$ is the underlying price at maturity and $K$ is the strike price.

---

## 2. No-Arbitrage

One of the fundamental principles of financial mathematics is **no-arbitrage**.

An arbitrage opportunity is a strategy that produces a positive profit with no risk and no initial investment.

The no-arbitrage principle allows us to determine relationships between financial instruments.

For example, the forward price of a non-dividend-paying stock is:

$$
F_0=S_0e^{rT}
$$

The forward price is therefore determined by a **no-arbitrage relationship**, rather than being a prediction of the future stock price.

---

## 3. Replication

The central idea is that if two portfolios produce the same future payoff, they must have the same price in a no-arbitrage market.

This leads to the idea of a **replicating portfolio**.

$$
\text{Derivative Payoff}
=
\text{Replicating Portfolio Payoff}
$$

Therefore:

$$
\text{Derivative Price}
=
\text{Replicating Portfolio Price}
$$

This idea is one of the foundations of derivative pricing.

---

## 4. Static and Dynamic Replication

A **static portfolio** is constructed once and then left unchanged.

A **dynamic portfolio** must be continuously or repeatedly rebalanced.

Options have nonlinear payoffs, so a simple fixed combination of stocks and bonds generally cannot reproduce their payoff for every possible stock price.

Instead, the hedge can be adjusted over time.

This leads to **dynamic replication**.

---

## 5. Put-Call Parity

For European call and put options with the same strike $K$ and maturity $T$:

$$
C-P=S_0-Ke^{-rT}
$$

This relationship is another consequence of no-arbitrage.

It connects the prices of calls, puts, the underlying asset and a risk-free bond.

---

## 6. Risk-Neutral Pricing

Instead of explicitly constructing a replicating portfolio every time, we can express derivative prices using a special probability measure called the **risk-neutral measure**.

Under the risk-neutral measure $\mathbb{P}^*$, discounted asset prices are martingales.

The general pricing equation is:

$$
V_t
=
e^{-r(T-t)}
\mathbb{E}^{*}
\left[
h(S_T)
\mid \mathcal{F}_t
\right]
$$

where $h(S_T)$ is the derivative payoff.

Importantly, $\mathbb{P}^*$ is a **pricing measure**, not necessarily the real-world probability distribution of future prices.

---

## 7. Complete and Incomplete Markets

A market is **complete** if every relevant contingent claim can be replicated.

In a complete market, there is a unique risk-neutral measure.

In an incomplete market, some risks cannot be perfectly hedged, so there can be multiple risk-neutral measures.

This becomes particularly relevant for models with additional sources of uncertainty, such as **stochastic volatility**.

---

## 8. Dynamic Hedging and Delta

Suppose an option has value:

$$
V_t=f(t,S_t)
$$

Its **Delta** is:

$$
\Delta_t
=
\frac{\partial f}{\partial S}
$$

Delta measures the sensitivity of the option price to the underlying asset.

It also determines the number of shares in the local replicating portfolio.

For example, if:

$$
\Delta=0.637
$$

then a small increase of $1 in the stock price corresponds approximately to an increase of $0.637 in the option price.

Because Delta changes as the underlying changes, the hedge must be **rebalanced dynamically**.

---

## 9. Itô's Formula

Because the underlying asset follows a stochastic process, the ordinary chain rule from calculus is not enough.

If:

$$
V_t=f(t,S_t)
$$

then Itô's Formula gives:

$$
df
=
f_tdt
+
f_SdS_t
+
\frac{1}{2}f_{SS}(dS_t)^2
$$

For a stock following:

$$
dS_t
=
\mu S_tdt+\sigma S_tdW_t
$$

we use:

$$
(dW_t)^2=dt
$$

which produces the additional second-order term.

This is the mathematical bridge between **stochastic processes and option pricing**.

---

## 10. Black-Scholes

The Black-Scholes model combines the previous ideas.

Starting with a stochastic model for the stock price and applying Itô's Formula to the option value, we can construct a dynamically hedged portfolio.

Eliminating the stochastic component leads to the **Black-Scholes PDE**:

$$
f_t
+
\frac{1}{2}\sigma^2S^2f_{SS}
+
rSf_S
-rf
=0
$$

with the appropriate terminal payoff.

For a European call:

$$
f(T,S_T)=\max(S_T-K,0)
$$

Solving the PDE gives the Black-Scholes formula:

$$
C
=
S_0N(d_1)
-
Ke^{-rT}N(d_2)
$$

where:

$$
d_1
=
\frac{
\ln(S_0/K)
+
(r+\frac{1}{2}\sigma^2)T
}{
\sigma\sqrt{T}
}
$$

and:

$$
d_2=d_1-\sigma\sqrt{T}
$$

---

## 11. From Mathematics to Python

These mathematical models can be implemented computationally.

For example:

```python
price, delta = black_scholes(
    S=100,
    K=100,
    r=0.05,
    sigma=0.20,
    T=1
)
```

For these parameters:

$$
C\approx10.45
$$

and:

$$
\Delta\approx0.637
$$

Python therefore becomes a way to turn the mathematical model into a computational tool for **pricing and risk analysis**.

---

## 12. The Bigger Picture

The ideas in this chapter are connected:

$$
\boxed{
\text{No-Arbitrage}
\rightarrow
\text{Replication}
\rightarrow
\text{Dynamic Hedging}
\rightarrow
\text{Itô's Formula}
\rightarrow
\text{Black-Scholes}
}
$$

The main story is that **derivative pricing is not just about guessing what an option is worth**.

We start with the principle that arbitrage should not exist. From this, we construct replicating strategies. When the replication must change over time, we use dynamic hedging. Because prices evolve stochastically, we need Itô's calculus. This ultimately leads to the Black-Scholes equation and, in the classical model, an explicit option-pricing formula.

This provides the foundation for more advanced quantitative finance topics such as **Monte Carlo simulation, stochastic volatility, Heston models, volatility surfaces and numerical option pricing**.
