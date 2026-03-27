# FX Receivable Exposure: EUR Hedging Decision Memo

**Created by:** Ellie Hanohano  
**Updated by:** Ellie Hanohano  
**Date Created:** March 27, 2026  
**Date Updated:** March 27, 2026  
**Version:**  1.0
**LLM Used:**  Claude Sonnet 4.6


---

## Executive Summary (≤150 words)

Our firm is owed €2,500,000 from a European customer, payable in 90 days (approximately June 25, 2026). At today's spot rate of approximately 1.0850 USD/EUR, the receivable is worth roughly $2,712,500. However, if the euro weakens against the dollar over the next three months, our realized USD proceeds could fall materially short of that figure — a risk we cannot ignore given current volatility in EUR/USD driven by diverging Federal Reserve and ECB policy paths. This memo defines the exposure, explains the downside risk, and introduces three hedging strategies — forward contracts, options, and a money market hedge — for the CFO's consideration. A full quantitative comparison will be built in Stages 2 through 4, culminating in a formal recommendation.

---

## Background & Objectives

**Exposure Details**

Our firm holds an FX transaction exposure arising from a confirmed export sale invoiced in euros:

- **Currency:** Euro (EUR)
- **Amount:** €2,500,000
- **Settlement Date:** June 25, 2026 (~90 days from today)
- **Current Spot Rate:** ~1.0850 USD/EUR
- **Implied USD Value at Spot:** ~$2,712,500

**Why This Is Risky**

Because we invoice in EUR but report in USD, any depreciation of the euro before the settlement date directly reduces our USD proceeds. EUR/USD has historically exhibited annualized volatility of 6–9%. A 5% adverse move — well within historical norms — would cost approximately $135,000. A 10% adverse move would reduce proceeds by roughly $270,000. Neither outcome is hypothetical; EUR/USD moved more than 12% in calendar year 2022 alone.

The risk is asymmetric from an operational standpoint: a weaker euro hurts our revenue line, while a stronger euro only partially offsets existing margin pressures. Our treasury policy requires that transaction exposures above $500,000 be formally reviewed for hedging eligibility.

**Primary Objective**

Evaluate available hedging instruments and select a strategy that locks in an acceptable minimum USD outcome while preserving reasonable upside, subject to cost and operational constraints.

---

## Methods

Three hedge families are under consideration. Each will be modeled in Stage 2 using live market data and scenario analysis.

**1. Forward Contract**

A bank agrees today to purchase our €2,500,000 at a fixed forward rate (typically spot adjusted for the interest rate differential) on June 25, 2026.

- *Pro:* Eliminates exchange rate uncertainty entirely; zero upfront premium; simple to execute.
- *Con:* We forfeit any benefit if the euro strengthens; obligates us to deliver the euros regardless of circumstances (counterparty risk if receivable is delayed or defaults).

**2. Currency Options (Purchased Put)**

We purchase the right — but not the obligation — to sell €2,500,000 at a specified strike price (e.g., 1.0800 USD/EUR) on or before the settlement date.

- *Pro:* Provides a guaranteed floor while allowing full participation in euro appreciation; flexibility if receivable timing shifts.
- *Con:* Requires an upfront premium (typically 1–2% of notional, or ~$27,000–$54,000); adds cost to the hedge.

**3. Money Market Hedge**

We borrow euros today (against the future receivable), convert to USD at spot, and invest the USD proceeds for 90 days. When the receivable arrives, we repay the EUR loan.

- *Pro:* Replicates a forward synthetically; effective when money market rates are favorable; no counterparty premium.
- *Con:* Requires credit capacity to borrow in EUR; operationally more complex than a forward; may not be available or cost-efficient given current EUR lending spreads.

---

## Limitations & Next Steps

**Limitations**

This memo relies on estimated spot and forward rates and does not yet incorporate bid-ask spreads, option implied volatility surfaces, or our actual borrowing rate in EUR. The receivable is assumed to arrive exactly on the settlement date; any delay would affect the hedge's effectiveness. Additionally, no scenario weighting or probability distribution has been applied at this stage.

**Next Steps**

- **Stage 2 — Excel Model Build:** Construct a working .xlsx model that computes and compares USD proceeds under each of the three hedging strategies across a range of future spot rate scenarios (e.g., 0.95 to 1.20 USD/EUR). Include an unhedged baseline for comparison.
- **Stage 3 — Technical Specification:** Document the model's logic, inputs, and assumptions in sufficient detail for independent replication or AI-assisted reconstruction. Identify areas for enhancement (e.g., stochastic rate simulation, break-even analysis).
- **Stage 4 — Final Analysis & Recommendation:** Select the optimal hedge strategy based on model outputs, draft a structured AI prompt used in the analysis, and present the final recommendation to the CFO with supporting data.

*Responsibility: Finance / Treasury team. Target completion for Stage 2: two weeks.*

---

## References

- Board of Governors of the Federal Reserve System. (2026). *H.10 Foreign Exchange Rates*. https://www.federalreserve.gov/releases/h10/
- European Central Bank. (2026). *Euro foreign exchange reference rates*. https://www.ecb.europa.eu/stats/exchange/eurofxref/html/index.en.html
- Madura, J. (2021). *International Financial Management* (14th ed.). Cengage Learning.
- CFA Institute. (2023). *Currency Risk Management* in *CFA Program Curriculum, Level 2*. CFA Institute.
