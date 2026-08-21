# Finance Foundations — Derivatives, Risk & Return

Asset (activo): anything with economic value that you own or invest in.

### Underlying Asset (activo subyacente)

The **underlying asset** is the asset whose value determines the price of a derivative, to put it differently, an asset that a derivative is based on.

**Example:** In a call option on Apple stock, the **Apple stock is the underlying asset** and the option is the derivative.

## Derivatives

A **derivative** is a financial contract whose value depends on an underlying asset, such as a stock, bond, commodity or interest rate.

**Example:** An option on Apple stock derives its value from the price of Apple stock.

### Forwards (Contrato a plazo)

A **forward** is a customized agreement between two parties to buy or sell an asset at a specified price on a future date.
Both parties are obligated to complete the transaction at the agreed price and date.

**Example:** You agree with a bank today to buy €100,000 worth of USD in 3 months at a fixed exchange rate.

### Futures (Contrato de futuros)

A **future** is a standardized forward contract traded on an organized exchange.
Both parties are also obligated to fulfill the contract.

**Example:** You buy a standardized oil futures contract that obliges you to buy oil at a specified price in 3 months.

**Key difference:** Forwards are usually **OTC and customized**, while futures are **exchange-traded and standardized**.

### Options

An **option** gives the buyer the **right, but not the obligation**, to buy or sell an asset at a specified price.

**Example:** You pay **€5** for the right to buy a stock at **€100**. If the stock rises to €120, you can exercise the option; if it falls to €80, you can let it expire.

### Swaps

A **swap** is an agreement to exchange one stream of payments for another.

**Example:** Two companies exchange **fixed interest payments for floating interest payments** on a loan.

## Exchange-Traded vs OTC

- **Exchange-traded:** Standardized contracts traded on an organized exchange.
- **OTC (Over-the-Counter):** Contracts negotiated directly between two parties and customized to their needs.

## Risk & Return

**Return** measures how much an investment gains or loses.

$$
R = \frac{P_1 - P_0}{P_0}
$$

**Example:** You buy a stock for **€100** and sell it for **€110**:

$$
R = \frac{110-100}{100} = 10\%
$$

Higher potential returns generally come with higher risk.

## Volatility

**Volatility** measures how much an asset's returns fluctuate over time.

**Example:** A stock that moves between €80 and €120 is more volatile than one that stays between €98 and €102.

## Capital Asset Pricing Model (CAPM)

The **Capital Asset Pricing Model (CAPM)** estimates the expected return of an asset based on its **systematic risk** relative to the market.

$$
E[R_i] = R_f + \beta_i \left(E[R_m] - R_f\right)
$$

where:

- $E[R_i]$ = expected return of the asset
- $R_f$ = risk-free rate
- $\beta_i$ = sensitivity of the asset to the market
- $E[R_m]$ = expected market return

**Example:** Suppose the risk-free rate is **3%**, the expected market return is **8%**, and a stock has a beta of **1.2**.

The CAPM formula is:

$$
E[R_i] = R_f + \beta_i(E[R_m] - R_f)
$$

Substituting the values:

E[R_i] = 3% + 1.2 × (8% − 3%)

E[R_i] = 3% + 1.2 × 5% = 9%

The CAPM therefore predicts an expected return of **9%** for the stock.

**Key idea:** Higher systematic risk ($\beta$) requires a higher expected return..

## Sharpe Ratio

The **Sharpe ratio** measures return relative to risk.

$$
\text{Sharpe Ratio} = \frac{R_p - R_f}{\sigma_p}
$$

where:

- $R_p$ = portfolio return
- $R_f$ = risk-free rate
- $\sigma_p$ = portfolio volatility

**Example:** A portfolio returns **10%**, the risk-free rate is **3%**, and volatility is **14%**:

$$
\text{Sharpe} = \frac{10\%-3\%}{14\%} = 0.50
$$

A higher Sharpe ratio generally means better risk-adjusted performance.

## Diversification

**Diversification** means spreading investments across different assets to reduce portfolio risk.

**Example:** Investing in **10 different stocks** is generally less risky than putting all your money into one stock, because poor performance in one asset can be offset by others.

## Realised Variance

**Realised variance** measures how much an asset's price actually fluctuated over a period, using observed returns.

Suppose you observe daily returns:

- Day 1: +2%
- Day 2: −1%
- Day 3: +3%
- Day 4: −2%

You calculate the squared returns and add them:

$$
RV = (0.02)^2 + (-0.01)^2 + (0.03)^2 + (-0.02)^2
$$

$$
RV = 0.0004 + 0.0001 + 0.0009 + 0.0004 = 0.0018
$$

So the **realised variance = 0.0018** for that period.

The corresponding realised volatility is:

$$
\sqrt{0.0018} \approx 4.24\%
$$