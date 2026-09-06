## From Replication to the Black-Scholes Formula

We now connect **dynamic replication, delta hedging, stochastic calculus and option pricing**.

The key idea is simple:

> If an option can be perfectly replicated by a portfolio, then the price of the option must equal the value of that replicating portfolio.

### 1. The Option Price as a Function

Let the option price at time $t$ be

$$
V_t=f(t,S_t),
$$

where $S_t$ is the price of the underlying asset and $f$ is the unknown **pricing function**.

If $\Phi_t$ denotes the value of the replicating portfolio, then perfect replication implies

$$
V_t=\Phi_t.
$$

Therefore,

$$
V_t-\Phi_t=0.
$$

This equality gives us information about the function $f$ and about the composition of the replicating portfolio.

---

### 2. Why Do We Need Itô's Formula?

Under the risk-neutral measure $P^*$, the discounted stock price is a martingale. The stock price itself is therefore a **semimartingale**.

Assume that $S_t$ is continuous.

Since the option value is a function of the stochastic process $S_t$,

$$
V_t=f(t,S_t),
$$

we need a way to describe how $V_t$ changes when $S_t$ changes.

For an ordinary deterministic function, we would use the chain rule. For a stochastic process, we use **Itô's formula**.

For a continuous semimartingale:

$$
df(t,S_t)
=
f_t(t,S_t)\,dt
+
f_S(t,S_t)\,dS_t
+
\frac{1}{2}f_{SS}(t,S_t)\,d\langle S\rangle_t.
$$

The last term is the important difference from the ordinary chain rule. It appears because stochastic processes have **non-zero quadratic variation**.

---

### 3. Delta Appears Naturally

The term

$$
f_S(t,S_t)=\frac{\partial f}{\partial S}(t,S_t)
$$

measures how sensitive the option price is to a small change in the underlying price.

This quantity is called the **delta**:

$$
\boxed{\Delta_t=f_S(t,S_t)}
$$

For example, if $\Delta_t=0.6$, then a small increase of €1 in the stock price produces approximately a €0.60 increase in the option price.

More importantly, delta tells us how many units of the underlying asset should be held in the replicating portfolio.

Thus:

$$
\text{stock position}=\Delta_t.
$$

Because delta changes with $t$ and $S_t$, the portfolio must be continuously **rebalanced**. This is dynamic delta hedging.

---

### 4. From Delta Hedging to the PDE

The replicating portfolio consists of the underlying asset and the risk-free asset.

By choosing the stock holding to be

$$
\Delta_t=f_S(t,S_t),
$$

the $dS_t$ risk terms in the option and in the replicating portfolio match and can be cancelled.

What remains is a deterministic relationship that the pricing function $f$ must satisfy.

This relationship is a **partial differential equation (PDE)**.

For the standard Black-Scholes model, the PDE is

$$
f_t+\frac{1}{2}\sigma^2S^2f_{SS}+rSf_S-rf=0.
$$

The important point is that we did not guess this equation.

It comes from:

$$
\text{replication}
+
\text{Itô's formula}
+
\text{delta hedging}
+
\text{no-arbitrage}.
$$

---

### 5. Where Does Volatility Come From?

The PDE contains the parameter $\sigma$, the **volatility** of the underlying asset.

In the Black-Scholes model, the stock is assumed to follow

$$
dS_t=\mu S_t\,dt+\sigma S_t\,dW_t.
$$

Its quadratic variation satisfies

$$
d\langle S\rangle_t=\sigma^2S_t^2\,dt.
$$

This is precisely the term that appears in Itô's formula.

Empirically, volatility can be estimated from observed asset returns. In the model, $\sigma$ represents the instantaneous magnitude of the random movements of the stock.

---

### 6. The Terminal Condition

The PDE alone is not enough to determine the option price.

We also know what the option is worth **at maturity**.

If the option has payoff $h(S_T)$, then

$$
f(T,S_T)=h(S_T).
$$

For a European call with strike $K$:

$$
h(S_T)=(S_T-K)^+.
$$

Therefore,

$$
f(T,S_T)=(S_T-K)^+.
$$

We now have a **PDE together with a terminal condition**.

---

### 7. The Black-Scholes Equation

The pricing function $f$ is the solution of

$$
\boxed{
f_t+\frac{1}{2}\sigma^2S^2f_{SS}+rSf_S-rf=0
}
$$

with the appropriate terminal condition.

For a European call:

$$
f(T,S_T)=(S_T-K)^+.
$$

Solving this PDE gives the **Black-Scholes price**.

---

### 8. Black-Scholes Price of a European Call

The solution for a European call is

$$
\boxed{
C=S_0N(d_1)-Ke^{-rT}N(d_2)
}
$$

where

$$
d_1=
\frac{
\ln(S_0/K)+(r+\frac{1}{2}\sigma^2)T
}{
\sigma\sqrt{T}
}
$$

and

$$
d_2=d_1-\sigma\sqrt{T}.
$$

Here, $N(\cdot)$ denotes the **cumulative distribution function of the standard normal distribution**.

---

### 9. Black-Scholes Price of a European Put

The corresponding European put price is

$$
\boxed{
P=Ke^{-rT}N(-d_2)-S_0N(-d_1)
}
$$

These prices are consistent with the **put-call parity** relationship:

$$
C-P=S_0-Ke^{-rT}.
$$

---

### The Whole Story

The Black-Scholes formula is therefore the final result of a chain of ideas:

$$
\boxed{
\begin{array}{c}
\text{No-arbitrage}\\
\downarrow\\
\text{Replication}\\
\downarrow\\
V_t=f(t,S_t)\\
\downarrow\\
\text{Itô's formula}\\
\downarrow\\
\Delta_t=f_S(t,S_t)\\
\downarrow\\
\text{Dynamic hedging}\\
\downarrow\\
\text{Black-Scholes PDE}\\
\downarrow\\
\text{Terminal payoff}\\
\downarrow\\
\text{Black-Scholes formula}
\end{array}
}
$$

The central insight is:

> **We do not start by guessing the price of the option. We start by asking what portfolio can replicate it. Delta tells us how to construct that portfolio dynamically, Itô's formula describes how the option value evolves, and no-arbitrage forces the pricing function to satisfy the Black-Scholes PDE. Solving that PDE gives the option price.**
