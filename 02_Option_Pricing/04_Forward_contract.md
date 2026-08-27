## Forward Contract

A **long forward** is an agreement to buy an underlying asset at a fixed price $F$ at maturity $T$.

At maturity, the payoff is:

$$
S_T-F
$$

### Replicating the forward

We can replicate this payoff by:

- **Buying 1 unit of the asset** → payoff $S_T$
- **Borrowing $e^{-rT}F$ today** → debt of $F$ at maturity

Therefore, the portfolio payoff is:

$$
S_T-F
$$

which is exactly the same as the forward.

The value of this portfolio today is:

$$
S_0-e^{-rT}F
$$

A new forward has zero value at inception, so:

$$
S_0-e^{-rT}F=0
$$

Therefore, the fair forward price is:

$$
\boxed{F=S_0e^{rT}}
$$

**Example:** Suppose an Apple share is worth €100 today, the interest rate is 5%, and the maturity is 1 year:

$$
F=100e^{0.05}\approx €105.13
$$

So the fair forward price is approximately **€105.13**.

**Key idea:** The forward price is determined by the current asset price and the cost of financing it until maturity.