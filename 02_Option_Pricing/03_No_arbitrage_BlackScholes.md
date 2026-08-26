# No-Arbitrage Pricing and Black-Scholes

## Market Assumptions

For simplicity, we consider a market with two assets:

- **Bond $B$** → risk-free asset
- **Stock $S$** → risky asset

We assume the bond has:

- No dividends
- Constant interest rate $r$
- Initial value:

$$
B_0 = 1
$$

The bond grows continuously at the risk-free rate:

$$
dB_t = rB_t\,dt
$$

Solving this equation gives:

$$
B_t = e^{rt}
$$

### Numerical example

Suppose:

- Initial bond value: $B_0=1$
- Interest rate: $r=5\%$
- Time: $t=2$ years

Then:

$$
B_2=e^{0.05\times2}
$$

$$
B_2\approx1.105
$$

So **€1 invested in the risk-free bond becomes approximately €1.105 after 2 years**.

### Key idea

The bond represents the **risk-free asset**: its value grows continuously at the constant interest rate $r$.