## Vanilla Option Pricing and the Black-Scholes Formula

Under a viable and complete market, we can price a vanilla option by constructing a **replicating portfolio** of bonds and the underlying asset.

The option price can be written as a function of time and the asset price:

$$
V_t=f(t,S_t)
$$

where $f$ is the **pricing function**.

### Dynamic Replication

Because the option payoff is generally nonlinear, the replicating portfolio must be adjusted over time.

The number of shares held in the portfolio is chosen as:

$$
\Delta_t=\frac{\partial f}{\partial S}(t,S_t)
$$

This is called the **delta** of the option.

> **Delta measures the sensitivity of the option price to changes in the underlying asset price and determines the number of shares used in the replicating portfolio.**

As the asset price changes, delta changes, so the portfolio must be continuously rebalanced. This is **dynamic hedging**.

### From Replication to Black-Scholes

Applying **Itô's formula** to $f(t,S_t)$ describes how the option price changes as the underlying asset evolves.

By choosing the stock holding equal to the option's delta, the first-order stock-risk terms can be cancelled.

This leads to a **partial differential equation (PDE)** for the pricing function $f$.

For a European option, the terminal condition is:

$$
f(T,S_T)=h(S_T)
$$

For a European call:

$$
f(T,S_T)=(S_T-K)^+
$$

Solving the resulting PDE gives the **Black-Scholes formula** for the price of a European vanilla option.

### Key Idea

The derivation follows:

$$
\text{Replicating portfolio}
\rightarrow
\text{Pricing function }f(t,S_t)
\rightarrow
\text{Itô's formula}
\rightarrow
\text{Delta hedging}
\rightarrow
\text{Black-Scholes PDE}
\rightarrow
\text{Black-Scholes formula}
$$

The important intuition is that **Black-Scholes prices an option by dynamically replicating its payoff using the underlying asset and the risk-free asset.**
