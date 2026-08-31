## European Option Pricing as a Risk-Neutral Expectation

Consider a European option with payoff:

$$
h(S_T)
$$

where $h$ is a function describing how much the option pays at maturity.

### Why can't we use simple static replication?

A portfolio made of:

- $\alpha$ bonds
- $\lambda$ units of the stock

has value:

$$
\alpha B_t+\lambda S_t
$$

At maturity:

$$
\alpha B_T+\lambda S_T
$$

For this portfolio to replicate the option, we would need:

$$
\alpha B_T+\lambda S_T=h(S_T)
$$

But a stock + bond produces a **linear** payoff in $S_T$.

Many European options have **nonlinear** payoffs.

For example, a call option has:

$$
h(S_T)=(S_T-K)^+
$$

Its payoff is zero below $K$ and increases above $K$, so it cannot generally be replicated by simply holding a fixed number of stocks and bonds.

Therefore, we need a different pricing approach.

This leads to the idea of pricing a European option as a **risk-neutral expectation**.

## Dynamic Replication of a European Option

At maturity $T$, a portfolio containing $\alpha$ bonds and $\lambda$ units of the stock has value:

$$
V_T(\Phi)=\alpha B_T+\lambda S_T
$$

To replicate a European option with payoff $h(S_T)$, we would need:

$$
\alpha B_T+\lambda S_T=h(S_T)
$$

for **every possible value of $S_T$**.

The problem is that the left-hand side is **linear** in $S_T$, while a vanilla option payoff such as a call is **non-linear**.

Therefore, a vanilla option generally cannot be replicated using a **static portfolio** with fixed $\alpha$ and $\lambda$.

### Dynamic replication

Instead, we allow the portfolio to change over time.

Choose time points:

$$
0=t_0<t_1<\cdots<t_n=T
$$

At each time $t$, the portfolio contains:

- $\alpha_t$ units of the bond
- $\lambda_t$ units of the stock

Its value is:

$$
V_t(\Phi)=\alpha_tB_t+\lambda_tS_t
$$

At each rebalancing time, we change $\alpha_t$ and $\lambda_t$.

This is called **rebalancing**.

Importantly, rebalancing does not mean adding or removing money from the portfolio. We simply move wealth between the stock and the bond:

$$
\alpha_{t_i}B_{t_i}+\lambda_{t_i}S_{t_i}
=
\alpha_{t_{i+1}}B_{t_i}+\lambda_{t_{i+1}}S_{t_i}
$$

The total portfolio value stays the same immediately before and after the rebalancing.

### From discrete to continuous time

If we make the rebalancing intervals smaller and smaller, eventually we obtain continuous-time replication.

The change in the portfolio value is:

$$
dV_t(\Phi)=\alpha_t\,dB_t+\lambda_t\,dS_t
$$

Since the bond satisfies:

$$
dB_t=rB_t\,dt
$$

we get:

$$
\boxed{dV_t(\Phi)=r\alpha_tB_t\,dt+\lambda_t\,dS_t}
$$

### Key idea

**Static replication:** fixed portfolio.

**Dynamic replication:** continuously adjust the amount of stock and bonds to replicate the option payoff.

## Risk-Neutral Pricing

### 1. Discounting the portfolio

We can measure the portfolio value in **today's money** by dividing it by the bond:

$$
\tilde V_t(\Phi)=\frac{V_t(\Phi)}{B_t}
$$

where $B_t=e^{rt}$.

For a self-financing portfolio, the discounted value satisfies:

$$
d\tilde V_t(\Phi)=\lambda_t\,d\tilde S_t
$$

where $\tilde S_t$ is the **discounted stock price**:

$$
\tilde S_t=\frac{S_t}{B_t}
$$

Therefore:

$$
\boxed{
\tilde V_t(\Phi)
=
\tilde V_0(\Phi)
+
\int_0^t \lambda_u\,d\tilde S_u
}
$$

The important idea is that, after discounting, the portfolio's changes come from trading the discounted stock.

---

### 2. Complete markets

Our goal is to construct a **self-financing portfolio** that has exactly the same payoff as the option:

$$
V_T(\Phi)=h(S_T)
$$

If we can do this for **every possible payoff function $h$**, the market is called **complete**.

In a complete market, every derivative can theoretically be replicated by trading the available assets.

For now, we assume the market is complete.

---

### 3. Introducing a new probability

If the discounted stock price $\tilde S_t$ is a **martingale**, then the discounted portfolio value is also a martingale.

But under the real-world probability $\mathbb P$, we do not necessarily have this property.

So imagine there exists another probability measure $\mathbb P^*$ under which:

$$
\tilde S_t
$$

is a martingale.

This probability measure is called the **risk-neutral measure**.

Under this measure, the discounted option value is also a martingale.

Therefore:

$$
\tilde V_t(\Phi)
=
\mathbb E_t^*
\left[
\tilde V_T(\Phi)
\right]
$$

Since the portfolio replicates the option:

$$
V_T(\Phi)=h(S_T)
$$

we obtain:

$$
\tilde V_t(\Phi)
=
\mathbb E_t^*
\left[
e^{-rT}h(S_T)
\right]
$$

Multiplying by $B_t=e^{rt}$:

$$
\boxed{
V_t
=
e^{-r(T-t)}
\mathbb E_t^*
\left[
h(S_T)
\right]
}
$$

### Key idea

The price of a European derivative is:

> **The expected payoff under the risk-neutral measure, discounted back to today.**

$$
\boxed{
V_t=e^{-r(T-t)}\mathbb E_t^*[h(S_T)]
}
$$

This is the foundation of **risk-neutral pricing** and leads directly to the **Black-Scholes formula**.

---

### Intuition

Think of $\mathbb P^*$ as a **special pricing world**.

We are NOT saying:

> "This is how Apple will actually behave."

Instead, we use a probability measure where **discounted asset prices have no drift**.

This makes derivative pricing much easier:

$$
\text{Future payoff}
\rightarrow
\text{Risk-neutral expectation}
\rightarrow
\text{Discount back}
\rightarrow
\text{Today’s price}
$$