# Options

An **option** is a derivative that gives the buyer the **right, but not the obligation**, to buy or sell an underlying asset at a specified price.

## Strike Price

The **strike price** ($K$) is the price at which the option can be exercised.

**Example:**  
A call option on **Apple (AAPL)** has a strike price of **$200**.

The holder has the right to buy Apple shares for **$200**, regardless of the market price.

---

## Call Option

A **call option** gives the buyer the **right to buy** the underlying asset at the strike price.

**Example:**  
You buy a call option on **Tesla (TSLA)** with:

- Strike price: **$300**
- Current stock price: **$280**

If Tesla rises to **$350**, you can buy it for $300 and potentially profit from the difference.

### Call payoff

$$
\text{Payoff} = \max(S_T-K,0)
$$

---

## Put Option

A **put option** gives the buyer the **right to sell** the underlying asset at the strike price.

**Example:**  
You buy a put option on **NVIDIA (NVDA)** with:

- Strike price: **$180**
- Current stock price: **$200**

If NVIDIA falls to **$150**, you can sell it for $180.

### Put payoff

$$
\text{Payoff} = \max(K-S_T,0)
$$

---

## European Options

A **European option** can only be exercised **at maturity**.

**Example:**  
A European call on **Microsoft (MSFT)** with maturity in 6 months can only be exercised on that maturity date.

---

## American Options

An **American option** can be exercised **at any time before or at maturity**.

**Example:**  
An American put on **Amazon (AMZN)** expiring in 6 months could be exercised today, next month, or at maturity.

### Key difference

- **European** → exercise only at maturity
- **American** → exercise at any time before maturity

---

## Vanilla Options

A **vanilla option** is a standard call or put with conventional terms.

**Example:**  
A standard call option on **Apple** with a fixed strike price and maturity.

---

## Exotic Options

An **exotic option** has more complex features than a standard vanilla option.

**Example:**  
A bank could create an option on **EUR/USD** whose payoff depends on whether the exchange rate reaches a certain level during the option's lifetime.

---

## Forward-Start Options

A **forward-start option** is an option whose terms are determined at a future date.

**Example:**  
Today, you agree to receive a call option on **Amazon** starting in 6 months. The strike price is determined when the option starts.

---

## Path-Dependent Options

A **path-dependent option** depends not only on the final asset price but also on the **path taken by the asset**.

**Example:**  
An option on **Tesla** may depend on whether its stock price ever reaches $400 during the life of the option, rather than only on its price at maturity.

---

## Spread Options

A **spread option** has a payoff based on the difference between two underlying assets.

**Example:**  
An option based on the difference between the prices of **Brent crude oil** and **WTI crude oil**.

$$
\text{Payoff} = \max(S_1-S_2-K,0)
$$

---

## Basket Options

A **basket option** is based on a portfolio (basket) of several assets.

**Example:**  
A basket option could be based on a portfolio containing **Apple, Microsoft and NVIDIA**.

The payoff depends on the combined performance of these stocks.

---

# Option Moneyness

**Moneyness** describes the relationship between the underlying asset price and the strike price.

## In the Money (ITM)

An option is **in the money** when exercising it would produce a positive payoff.

### Call

If:

- Apple stock: **$220**
- Strike: **$200**

The call is **ITM** because:

$$
220-200=20
$$

### Put

If:

- NVIDIA stock: **$150**
- Strike: **$180**

The put is **ITM** because:

$$
180-150=30
$$

---

## Out of the Money (OTM)

An option is **out of the money** when exercising it would produce zero payoff.

### Call

Apple stock = **$180**  
Strike = **$200**

$$
\max(180-200,0)=0
$$

The call is **OTM**.

### Put

NVIDIA stock = **$200**  
Strike = **$180**

$$
\max(180-200,0)=0
$$

The put is **OTM**.

---

## At the Money (ATM)

An option is **at the money** when the underlying price is approximately equal to the strike.

**Example:**

Apple stock = **$200**  
Strike = **$200**

The option is **ATM**.

---

## Spot ATM

**Spot ATM** means the strike price is approximately equal to the **current spot price**.

$$
K \approx S_0
$$

**Example:**

Apple spot price = **$200**  
Strike = **$200**

→ Spot ATM.

---

## Forward ATM

**Forward ATM** means the strike price is approximately equal to the **forward price**.

$$
K \approx F_0
$$

This is different from spot ATM because the **forward price** can differ from the current spot price due to factors such as interest rates and dividends.

---

## Intrinsic Value

The **intrinsic value** is the payoff the option would have if it were exercised **right now**.

### Call

$$
\text{Intrinsic Value}=\max(S_0-K,0)
$$

### Put

$$
\text{Intrinsic Value}=\max(K-S_0,0)
$$

**Example:**  
Apple is trading at **$220** and a call has a strike of **$200**.

$$
\text{Intrinsic Value}=220-200=\$20
$$

So the call has **$20 of intrinsic value**.