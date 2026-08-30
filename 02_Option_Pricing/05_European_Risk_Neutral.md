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