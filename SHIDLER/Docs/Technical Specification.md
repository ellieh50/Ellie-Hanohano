# EUR Receivable FX Hedge Analysis: Technical Specification

**Created by:** Ellie Hanohano  
**Updated by:** Ellie Hanohano  
**Date Created:** April 4, 2026  
**Date Updated:** April 4, 2026  
**Version:**  1.0
**LLM Used:**  Claude Sonnet 4.6

**Role:** Financial Analyst / Treasury Analyst  
**Audience:** CFO or Director of Treasury  

**Purpose:** Provide a professional, quantitative specification documenting the Stage 2 Excel hedge model and articulating a refined, reproducible design for comparative FX hedge analysis.

---

## 1. Problem Statement

Our firm expects to receive €2,500,000 from a European customer on **2026-06-25** (90 calendar days from inception date 2026-03-27), arising from a confirmed export sale invoiced in euros. Because we report in USD, every basis-point move in EUR/USD between today and settlement directly affects realized revenue. At the inception spot rate of 1.0850 USD/EUR, the receivable is worth approximately **$2,712,500**. A 5% adverse EUR depreciation would reduce proceeds by roughly **$135,600**; a 10% move would cost approximately **$271,250** — material against any operating margin benchmark.

The objective is to evaluate three hedging strategies — forward contract, money market hedge, and purchased EUR put option — against an unhedged baseline, quantify their respective USD outcomes across a ±5% exchange-rate range, and produce decision-ready analytics for senior management review in Stage 4.


---


## 2. Inputs (Known Variables)

All variables below appear as labeled, yellow-highlighted input cells in the Stage 2 Excel model. Named ranges use the standardized convention established in the assignment.

| Named Range   | Description                              | Unit          | Value      | Source                                    |
|---------------|------------------------------------------|---------------|------------|-------------------------------------------|
| `FC_AMT`      | Foreign-currency receivable amount       | EUR           | 2,500,000  | Company export contract                   |
| `S0_in`       | EUR/USD spot rate at inception           | USD per EUR   | 1.08500    | Federal Reserve H.10, 2026-03-27          |
| `F0_in`       | 90-day EUR/USD forward rate              | USD per EUR   | 1.08876    | Derived via CIP (see §3)                  |
| `R_USD`       | USD risk-free rate (annualized)          | Annual %      | 5.30%      | 90-day T-bill yield, 2026-03-27           |
| `R_FC`        | EUR risk-free rate (annualized)          | Annual %      | 3.90%      | ECB deposit rate approximation, 2026-03-27|
| `K_PUT`       | EUR put option strike                    | USD per EUR   | 1.08000    | Analyst selection — slightly OTM put      |
| `K_CALL`      | EUR call option strike (benchmark)       | USD per EUR   | 1.10000    | Analyst selection — OTM call              |
| `PREM_PUT`    | Put option premium per unit of EUR       | USD per EUR   | 0.02150    | Indicative dealer quote                   |
| `PREM_CALL`   | Call option premium per unit of EUR      | USD per EUR   | 0.00850    | Indicative dealer quote                   |
| `T_DAYS`      | Days from inception to settlement        | Calendar days | 90         | Derived: 2026-03-27 → 2026-06-25          |

---

## 3. Assumptions & Constraints

The following conventions were applied uniformly across all calculations. Any deviation in a rebuilt version must be explicitly noted.

1. **Day count:** Actual/360 for both USD and EUR interest rates, consistent with money market convention. `T_DAYS = 90` is used as the accrual period.
2. **Simple interest:** All rate calculations use the simple interest form `(1 + r × T/360)`, not compound. This is standard for sub-one-year money market instruments.
3. **Forward rate via CIP:** `F0_in` is not a market input — it is derived from Covered Interest Rate Parity: `F0 = S0 × (1 + R_USD × T/360) / (1 + R_FC × T/360)`. This ensures mathematical parity between the forward hedge and the money market hedge. A separately quoted bank forward may differ by bid-ask spread; this model uses the theoretical no-arbitrage rate for clean comparability.
4. **Option premiums paid upfront:** Both `PREM_PUT` and `PREM_CALL` are denominated in USD per EUR and are deducted from proceeds at inception. No time-value adjustment is made for investing/financing the premium over `T_DAYS`.
5. **European-style options assumed:** Option payoffs are computed only at settlement (expiry), not on an early-exercise basis.
6. **No transaction costs or bid-ask spreads:** Spot, forward, and option quotes are treated as mid-market. Live execution will add cost to every strategy.
7. **Receivable assumed to settle exactly on T_DAYS:** No slippage, counterparty default, or timing mismatch is modeled.
8. **No credit or counterparty risk:** All hedge counterparties are assumed to perform in full.
9. **No tax or accounting treatment:** Hedge P&L is presented on a gross USD-proceeds basis only.
10. **Exchange rate convention:** All rates expressed as USD per EUR (direct quote from a US firm's perspective). A higher rate means a stronger euro.

---

## 4. Calculation Flow

The following sequence describes the logical order of operations for each hedge type. Steps are expressed conceptually so any analyst or AI can implement them in Excel, Python, or any other tool.

### 4.1 Forward Hedge

1. Confirm `F0_in` using the CIP formula: divide `(1 + R_USD × T_DAYS/360)` by `(1 + R_FC × T_DAYS/360)` and multiply by `S0_in`.
2. Multiply `FC_AMT` by `F0_in` to obtain locked-in USD proceeds.
3. Record as `USD_forward`. This is the certainty benchmark — the firm knows this number today.

### 4.2 Money Market Hedge

The money market hedge synthetically replicates the forward by using spot FX and borrowing/lending in the two currencies.

1. **Borrow EUR today:** Divide `FC_AMT` by `(1 + R_FC × T_DAYS/360)`. This gives the EUR amount to borrow now such that, at maturity, the EUR receivable exactly repays the loan. Call this `EUR_borrow`.
2. **Convert to USD at spot:** Multiply `EUR_borrow` by `S0_in` to obtain `USD_spot`.
3. **Invest USD for T_DAYS:** Multiply `USD_spot` by `(1 + R_USD × T_DAYS/360)` to obtain `USD_mm` — the maturity value of the USD investment.
4. **Parity check:** Compute `USD_forward − USD_mm`. Under CIP, this difference must equal zero (within floating-point rounding). A nonzero result signals a rate input error and must be investigated before the model is used for decisions.

### 4.3 Option Hedge — EUR Put (Downside Protection)

1. Compute total put premium in USD: multiply `FC_AMT` by `PREM_PUT`. This is a cash outflow at inception.
2. At any given ending spot rate `S_T`, compute the put payoff: `MAX(K_PUT − S_T, 0) × FC_AMT`. If `S_T` is above the strike, the put expires worthless and payoff is zero.
3. Compute gross EUR-converted proceeds: `FC_AMT × S_T`.
4. Net USD proceeds under the put hedge: `(FC_AMT × S_T) + put payoff − total put premium`.
5. The effective floor (minimum guaranteed proceeds before premium) is `FC_AMT × K_PUT`. After subtracting the premium, the effective floor is `FC_AMT × K_PUT − (FC_AMT × PREM_PUT)`.

### 4.4 Option Hedge — EUR Call (Benchmark / Collar Leg)

1. Compute total call premium in USD: multiply `FC_AMT` by `PREM_CALL`.
2. At any given `S_T`, compute call payoff: `MAX(S_T − K_CALL, 0) × FC_AMT`. If `S_T` is below `K_CALL`, the call expires worthless.
3. Net USD proceeds under the call hedge: `(FC_AMT × S_T) + call payoff − total call premium`.
4. Note: A long call on EUR combined with a short position in EUR would form a different structure. In this model, the call is presented as a standalone benchmark comparison — it is not a receivable hedge in isolation.

### 4.5 Unhedged Baseline

Multiply `FC_AMT` by the realized ending spot rate `S_T`. No premium cost. Full exposure to EUR/USD movement in both directions.

---

## 5. Outputs

The Stage 2 model produces the following named outputs. Each corresponds to a clearly labeled section of the Excel workbook.

| Output Name          | Description                                                              | Format          | Purpose                                     |
|----------------------|--------------------------------------------------------------------------|-----------------|---------------------------------------------|
| `USD_forward`        | Locked-in proceeds from forward hedge at `F0_in`                         | Scalar (USD)    | Certainty benchmark; zero exchange-rate risk|
| `USD_mm`             | Maturity value of money market hedge investment                           | Scalar (USD)    | Parity cross-check against `USD_forward`    |
| `Parity_check`       | `USD_forward − USD_mm`; should equal $0.00                               | Scalar (USD)    | Model integrity verification                |
| `USD_put_base`       | Net put hedge proceeds at base-case `S_T = S0_in`                        | Scalar (USD)    | Base-case option outcome                    |
| `USD_call_base`      | Net call hedge proceeds at base-case `S_T = S0_in`                       | Scalar (USD)    | Benchmark call outcome at base case         |
| `Sensitivity_Table`  | USD proceeds for all five strategies at `S_T` ranging ±5% of `S0_in`    | 11-row table    | Full scenario comparison                    |
| `Chart_1`            | Line chart of `Sensitivity_Table`; strategy proceeds vs. `S_T`           | Line chart      | Visual decision aid for CFO presentation    |
| `Summary_KPI`        | Side-by-side base-case results for all strategies                         | Gray output box | Executive dashboard; feeds Stage 4 memo     |
| `Recommendation_Row` | Placeholder for Stage 4 hedge selection                                   | Text cell       | Final deliverable anchor                    |

---

## 6. Model Review: What Worked & What to Improve

### What Worked

- **CIP-derived forward rate** ensures exact mathematical parity between `USD_forward` and `USD_mm`, removing any ambiguity from a manually entered forward rate and making the parity check a genuine integrity test rather than a trivially satisfied identity.
- **Sensitivity table structure** (±5% in 1% increments, 11 rows) is appropriately granular for a 90-day horizon and produces a chart that cleanly separates the five strategy lines without visual clutter.
- **Color-coding convention** (yellow inputs / green formulas / gray outputs) follows industry-standard financial modeling practice and makes the model auditable in under five minutes.
- **Notes & Assumptions tab** centralizes all simplifications in one place, making it easy for a reviewer to identify what the model does and does not capture.

### What Should Be Improved

1. **Named ranges not formally registered:** The variable names listed in §2 are used as column labels and comments but are not registered as Excel named ranges with `Ctrl+F3`. A production model should formally define `FC_AMT`, `S0_in`, etc. as named ranges so formulas self-document (e.g., `=FC_AMT * F0_in` rather than `=C7*C9`).
2. **Option premium time-value adjustment:** The current model deducts the premium as a lump sum without reflecting the opportunity cost of that cash outflow over `T_DAYS`. A refined model would add `PREM_PUT × FC_AMT × (1 + R_USD × T_DAYS/360)` as the effective premium cost, or alternatively discount option proceeds back to an NPV basis.
3. **Static `S_T` in Section D:** The base-case option payoff section (Section D) uses a manually entered `S_T` cell. While functional, it is disconnected from the sensitivity table. A cleaner design would pull Section D directly from the 0% row of the sensitivity table to avoid maintaining two separate `S_T` references.
4. **No break-even analysis:** The model does not explicitly compute the break-even `S_T` at which the put hedge outperforms the unhedged position, or the rate at which the forward hedge outperforms the put. These crossover points are high-value outputs for CFO communication.
5. **No cost-of-hedge metric:** Adding a row that expresses each hedge's cost relative to the unhedged outcome (e.g., forward hedge discount as a percentage of notional) would sharpen the tradeoff summary.
6. **Sensitivity range could be asymmetric:** EUR/USD downside moves are the primary risk driver for a USD-reporting exporter. A refined sensitivity table might range from −10% to +3% rather than a symmetric ±5%, better reflecting the actual risk distribution.

---

## 7. Sensitivity Plan

The sensitivity analysis varies `S_T` from `0.95 × S0_in` to `1.05 × S0_in` in eleven discrete steps of 1% (i.e., −5%, −4%, −3%, −2%, −1%, 0%, +1%, +2%, +3%, +4%, +5%). For reference, at `S0_in = 1.0850`, this range spans **1.0308 to 1.1393 USD/EUR**.

At each `S_T`, the table computes USD proceeds for five strategies:

| Column | Strategy | Key behavior across the range |
|--------|----------|-------------------------------|
| Unhedged | `FC_AMT × S_T` | Linear — falls 1-for-1 with EUR weakness |
| Forward | `FC_AMT × F0_in` | Flat — completely insensitive to `S_T` |
| Money Market | `USD_mm` | Flat — identical to forward under CIP |
| Put Hedge | Unhedged + MAX(K_PUT − S_T, 0) × FC_AMT − Premium | Kinked at K_PUT; floor below, tracks unhedged above |
| Call Hedge | Unhedged + MAX(S_T − K_CALL, 0) × FC_AMT − Premium | Kinked at K_CALL; linear with upside above strike |

The line chart presents all five series simultaneously against `S_T` on the x-axis. The most analytically useful visual comparisons are: (a) the gap between the put hedge floor and the unhedged line at low `S_T` values, which quantifies downside protection; and (b) the crossover between the forward and put hedges in the upper range, which shows the `S_T` at which paying for optionality becomes worthwhile.

---

## 8. Limitations & Next Steps

**Analytical exclusions:** This model does not incorporate implied volatility pricing for options (premiums are treated as given inputs), dynamic hedging rebalancing, transaction costs, bid-ask spreads, credit valuation adjustment (CVA) on forward contracts, or any accounting or tax treatment of hedge gains and losses. The money market hedge assumes the firm can borrow EUR at the risk-free rate, which understates real-world corporate borrowing costs.

**Scope boundary:** The analysis covers a single, static hedge initiated at inception and held to maturity. Rolling hedges, partial hedges, and collar structures are outside this model's scope.

**Next steps — Stage 4:** This specification will be distilled into a structured AI prompt for Stage 4. The prompt will supply `FC_AMT`, `S0_in`, `F0_in`, `R_USD`, `R_FC`, `K_PUT`, `PREM_PUT`, `T_DAYS`, and the sensitivity range as explicit parameters, and will instruct the model to reproduce the calculation flow in §4, generate the outputs in §5, and deliver a written hedge recommendation. The improvement items identified in §6 — particularly break-even crossover analysis and named ranges — will be incorporated into the Stage 4 prompt design.

---

## References

- Federal Reserve. (2026). *H.10 Foreign Exchange Rates*. https://www.federalreserve.gov/releases/h10/
- European Central Bank. (2026). *Euro foreign exchange reference rates*. https://www.ecb.europa.eu/stats/exchange/eurofxref/
- Madura, J. (2021). *International Financial Management* (14th ed.). Cengage Learning. Ch. 11–12.
- McDonald, R. L. (2013). *Derivatives Markets* (3rd ed.). Pearson. Ch. 2–3 (Forward pricing, CIP).
