## Forward Contract

A **long forward** is an agreement to buy an underlying asset at a fixed price $F$ at maturity $T$.
"The forward price must equal the cost of obtaining the asset at maturity without taking market risk."

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

### Why do the payoffs cancel?

Suppose the candy is worth **€300** at maturity.

The **long forward** gives:

$$
S_T-F=300-90=€210
$$

The **short replicating portfolio** has the opposite payoff:

$$
-(S_T-F)=-210
$$

Together:

$$
210-210=\boxed{0}
$$

The same happens whatever the final price of the candy is:

- If $S_T=€50$: $-40+40=0$
- If $S_T=€300$: $210-210=0$
- If $S_T=€1,000$: $910-910=0$

The two positions always move in **exactly opposite directions**.

Mathematically:

$$
\boxed{(S_T-90)-(S_T-90)=0}
$$

The $S_T$ terms cancel, so the final result does **not depend on the future price**.

**That's why the strategy has zero market risk.**

The arbitrage opportunity exists because the forward and the replicating portfolio have **the same future payoff but different prices today**.

## Model-Free Pricing and Replication

### Model-Free Forward Price

For a non-dividend-paying stock, the forward price is **model-free**.

This means we do not need to assume how the stock price will evolve. We only use **no-arbitrage arguments** and observable quantities such as the current stock price and interest rate.

The formula changes if the underlying asset provides income (e.g. **dividends**) or has costs (e.g. **commodity storage costs**).

---

### Static vs Dynamic Replication

**Static replication:** the replicating portfolio is constructed at $t=0$ and is **not changed** before maturity.

**Dynamic replication:** the portfolio must be **rebalanced over time**, changing the amounts of the underlying asset and bonds.

For example, Black-Scholes uses **dynamic replication** because the number of shares needed to hedge an option changes over time.

---

### Put-Call Parity

For a European call and put with the same strike $K$ and maturity $T$:

$$
C-P=S_0-Ke^{-rT}
$$

Why?

A **long call + short put** has payoff:

$$
(S_T-K)^+-(K-S_T)^+=S_T-K
$$

This is the same payoff as:

- Long the stock → $S_T$
- Borrow $Ke^{-rT}$ today → $-K$ at maturity

Therefore, by no-arbitrage, they must have the same price.

**Key idea:** Put-call parity is another **model-free relationship** based on identical payoffs.