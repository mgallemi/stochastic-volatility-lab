Chapter 1: Derivatives & Option Pricing


What is a derivative?

A derivative is a financial contract whose value depends on an underlying asset or variable.

The underlying can be: a stock, a commodity, an interest rate, a currency, an index

Example: An option on Apple stock derives its value from the price of Apple stock.


Futures

A future is an agreement to buy or sell an asset at a predetermined price on a predetermined future date.

Both parties are obligated to complete the transaction.

Example:

Today: You agree to buy 1 barrel of oil for $80 in 3 months.

After 3 months:
Oil = $100 → you benefit because you buy for $80.
Oil = $60 → you lose because you are still obligated to buy for $80.


Options

An option gives the buyer the right, but not the obligation, to buy or sell an asset at a predetermined price.

The buyer normally pays a premium for this right.

Two basic types:

Call → right to BUY

Put → right to SELL

Example — Call:

You buy a call giving you the right to buy a stock for €100.

If the stock goes to €130 → you can exercise the option.

If the stock falls to €80 → you simply don't exercise it. Your maximum loss is generally the premium you paid.


Future vs Option

Key idea:
Future = obligation.
Option = right, not obligation.

This distinction is very important for option pricing.


Swaps

A swap is an agreement between two parties to exchange cash flows according to predetermined rules.

A common example is an interest-rate swap.

Example:

Company A has a loan with a variable interest rate but wants more certainty.

It agrees with Company B to:

pay a fixed interest rate
receive a variable interest rate

They exchange the corresponding cash flows.


Maturity

Maturity is the date when a derivative contract expires or reaches its final settlement.

We often denote the time remaining until maturity by:

T

Example:

Today = January 1
Option expires = July 1

Then:

T=6 months

For an option, time to maturity is one of the variables that affects its price.


Exchange-traded vs OTC
Organised market / Exchange

Derivatives are traded through an organised exchange with standardised contracts.

Examples include standardised futures and options.

Characteristics:

standardised contract specifications
central clearing
transparent prices
exchange rules

Think:

"Everyone trades the same standardised contract."

OTC — Over The Counter

The contract is negotiated directly between two parties rather than being traded on a central exchange.

The parties can customise:

maturity
notional
underlying
payment structure
other conditions

Example:

A company and a bank negotiate a customised interest-rate swap specifically for the company's financing needs.

Think:

Exchange = standardised.
OTC = customised.



2. What is the "option pricing problem"?

This is the question your project will eventually investigate:

How much should an option be worth today?

Suppose:

Stock price = €100
Strike = €100
Maturity = 1 year
Volatility = 20%
Risk-free rate = 3%

What should the call option cost?

That's the option pricing problem.

And this is where Black-Scholes comes in.