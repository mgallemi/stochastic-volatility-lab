# Derivatives

## 1. Forward Contract

A **forward** is an agreement to buy or sell an underlying asset at a specified price on a future date.

### Long position

The **long** position agrees to buy the asset at maturity.

### Short position

The **short** position agrees to sell the asset at maturity.

### Example

Suppose you agree to buy a stock for **€100** in 3 months.

At maturity:

- If the stock is worth **€120** → long position gains **€20**.
- If the stock is worth **€80** → long position loses **€20**.

---

## 2. Payoff

The **payoff** is the amount received or lost from a financial contract at maturity.

For a long forward:

$$
\text{Payoff} = S_T - K
$$

where:

- $S_T$ = asset price at maturity
- $K$ = agreed forward price

### Example

If $K = €100$:

| Stock price at maturity | Long forward payoff |
|---:|---:|
| €80 | −€20 |
| €100 | €0 |
| €120 | +€20 |

---

## 3. Maturity

**Maturity** is the date when the contract expires and its final payoff is determined.

Example:  
A forward contract with a maturity of **3 months** settles after 3 months.

---

## 4. Forward Price

The **forward price** is the price agreed today for buying or selling the underlying asset at maturity.

For a simple non-dividend-paying asset:

$$
F_0 = S_0e^{rT}
$$

---

## 5. Futures Contract

A **future** is a standardized forward contract traded on an organized exchange.

Unlike forwards, futures are typically **marked to market daily**.

---

## 6. Futures Price

The **futures price** is the price at which the underlying asset can be bought or sold through the futures contract.

---

## 7. Variance Swap

A **variance swap** is a derivative whose payoff depends on the difference between **realised variance** and a predetermined variance level.

Simplified:

$$
\text{Payoff} \propto
\text{Realised Variance} - \text{Strike Variance}
$$

### Example

If the realised variance is higher than the agreed variance level, the long variance position receives a positive payoff.

---

## 8. Volatility Swap

A **volatility swap** is similar to a variance swap, but its payoff depends directly on **realised volatility** rather than realised variance.

Simplified:

$$
\text{Payoff} \propto
\text{Realised Volatility} - \text{Strike Volatility}
$$

### Key difference

- **Variance swap** → based on variance
- **Volatility swap** → based on volatility