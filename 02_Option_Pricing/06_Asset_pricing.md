## Fundamental Theorems of Asset Pricing

The two Fundamental Theorems of Asset Pricing connect **no-arbitrage, martingales, risk-neutral pricing and market completeness**.

### First Fundamental Theorem

A market is **viable** (no arbitrage) if and only if there exists a probability measure $P^*$, equivalent to the real-world probability $P$, such that **discounted asset prices are martingales** under $P^*$.

In simple terms:

$$
\boxed{
\text{No arbitrage}
\iff
\text{a risk-neutral measure }P^*\text{ exists}
}
$$

So if we can find a probability measure under which discounted asset prices behave like fair games, the market is consistent with no-arbitrage.

---

### Second Fundamental Theorem

A viable market is **complete** if and only if the risk-neutral probability measure $P^*$ is **unique**.

$$
\boxed{
\text{Complete market}
\iff
\text{unique risk-neutral measure }P^*
}
$$

This connects two ideas:

* **No arbitrage** → at least one risk-neutral measure exists.
* **Completeness** → exactly one risk-neutral measure exists.

If the market is complete, every derivative payoff can be replicated by trading the available assets, so there is a unique arbitrage-free price.

---

## Risk-Neutral Probability

The probability measure $P^*$ is called the **risk-neutral measure** because, under $P^*$, discounted asset prices behave like martingales.

This allows derivative prices to be computed as:

$$
V_t=e^{-r(T-t)}\mathbb{E}_t^*[h(S_T)]
$$

The important point is that $P^*$ is **not the real-world probability**.

It is a pricing probability constructed so that the market is consistent with no-arbitrage.

---

## Incomplete Markets

A market can be viable (no arbitrage) but **incomplete**.

In this case, there may be several different risk-neutral measures:

$$
P_1^*,P_2^*,P_3^*,\ldots
$$

This happens when there are sources of randomness that cannot be fully hedged using the available traded assets.

For example, suppose the asset price depends on another random process $Y_t$:

$$
S_t=S_t(Y_t)
$$

If $Y_t$ contains its own randomness and there is no traded asset that allows us to hedge it, we cannot perfectly replicate every derivative.

Therefore:

> **Incomplete market → derivatives cannot always be perfectly replicated → risk-neutral measure may not be unique.**

---

## How This Relates to Stochastic Volatility

This becomes particularly important in **stochastic volatility models**.

In a simple Black-Scholes model, volatility $\sigma$ is deterministic, so the model has one main source of randomness: the stock.

In a stochastic volatility model, volatility itself is random:

$$
dS_t=\cdots
$$

$$
dY_t=\cdots
$$

where $Y_t$ can represent a random volatility factor.

Now there can be more sources of randomness than traded assets, making the market potentially incomplete.

---

## Practical Approach to Incomplete Markets

In practice, we often assume that the market selects a particular risk-neutral measure $P^*$ for pricing.

A key practical idea is that **model parameters are often calibrated to market derivative prices rather than estimated only from historical asset data**.

For example:

1. Observe market prices of vanilla options.
2. Choose a model, such as a stochastic volatility model.
3. Calibrate the model parameters so that it reproduces the observed vanilla option prices.
4. Use the calibrated model to price more complicated **exotic derivatives**.

So:

$$
\text{Market vanilla prices}
\rightarrow
\text{Calibration}
\rightarrow
\text{Model}
\rightarrow
\text{Exotic option pricing}
$$

### Key Idea

The Fundamental Theorems give the theoretical foundation:

$$
\boxed{
\text{No arbitrage}
\rightarrow
\text{Risk-neutral measure}
}
$$

and

$$
\boxed{
\text{Complete market}
\rightarrow
\text{Unique risk-neutral measure}
}
$$

In incomplete markets, the risk-neutral measure may not be unique, so additional modelling or market information is needed to determine derivative prices.
