# Chapter Recap

This chapter introduced the main ideas behind **derivatives pricing and mathematical finance**.

### 1. Financial Instruments

* **Asset:** a financial instrument with value, such as a stock or bond.
* **Derivative:** a contract whose value depends on an underlying asset.
* **Forward/Future:** obligation to buy or sell an asset at a future date for a specified price.
* **Option:** gives the buyer the **right, but not the obligation**, to buy or sell an asset.

### 2. No-Arbitrage

**Arbitrage** is a risk-free profit opportunity.

The **no-arbitrage principle** says that two portfolios with the same future payoff must have the same price today.

This is the fundamental idea behind derivative pricing.

### 3. Replication

A **replicating portfolio** is a portfolio of simpler assets that produces the same payoff as a derivative.

Therefore:

$$
\text{Derivative price}
=
\text{Replicating portfolio price}
$$

This allows us to price derivatives without directly predicting their future value.

### 4. Forward Pricing

For a non-dividend-paying stock:

$$
F_0=S_0e^{rT}
$$

The forward price is determined by **no-arbitrage**, not by a prediction of the future stock price.

### 5. Static vs Dynamic Replication

* **Static replication:** the portfolio is constructed once and not changed.
* **Dynamic replication:** the portfolio is continuously adjusted as market conditions change.

Options generally require **dynamic replication** because their payoffs are nonlinear.

### 6. Put-Call Parity

For European call and put options with the same strike $K$ and maturity $T$:

$$
C-P=S_0-Ke^{-rT}
$$

Put-call parity is another consequence of **no-arbitrage**.

### 7. Martingales and Risk-Neutral Pricing

Under a **risk-neutral probability measure** $P^*$, discounted asset prices behave as martingales.

For a derivative with payoff $h(S_T)$:

$$
V_t=e^{-r(T-t)}E_t^*[h(S_T)]
$$

The risk-neutral measure is a **pricing measure**, not a description of the real-world probabilities.

### 8. Complete and Incomplete Markets

A market is **complete** if every contingent payoff can be perfectly replicated.

* **Complete market:** the risk-neutral measure is unique.
* **Incomplete market:** multiple risk-neutral measures may exist because some sources of risk cannot be perfectly hedged.

### 9. Dynamic Hedging and Delta

If the option price is

$$
V_t=f(t,S_t),
$$

then

$$
\Delta_t=\frac{\partial f}{\partial S}(t,S_t)
$$

is the option's **delta**.

Delta determines how many units of the underlying asset are held in the replicating portfolio.

Because delta changes as the stock price changes, the portfolio must be **rebalanced dynamically**.

### 10. Itô's Formula

**Itô's formula** is the stochastic version of the chain rule.

It describes how a function such as

$$
V_t=f(t,S_t)
$$

changes when $S_t$ follows a stochastic process.

It is essential for deriving the **Black-Scholes PDE**.

### 11. Black-Scholes

The Black-Scholes derivation follows:

$$
\text{Replication}
\rightarrow
\text{Itô's formula}
\rightarrow
\text{Delta hedging}
\rightarrow
\text{PDE}
\rightarrow
\text{Option price}
$$

For a European vanilla option, the terminal condition is

$$
f(T,S_T)=h(S_T).
$$

For a call:

$$
h(S_T)=(S_T-K)^+.
$$

### Big Picture

The central idea of the chapter is:

$$
\boxed{
\text{No-arbitrage}
\rightarrow
\text{Replication}
\rightarrow
\text{Hedging}
\rightarrow
\text{Pricing}
}
$$

Mathematical finance uses **mathematics, probability and stochastic processes** to construct portfolios that replicate derivative payoffs and therefore determine their prices.
