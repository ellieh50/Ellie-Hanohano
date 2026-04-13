# FX Hedge Final Analysis & Recommendation
## Stage 4: Final Memo & Structured AI Prompt

**Created by:** Ellie Hanohano  
**Updated by:** Ellie Hanohano  
**Date Created:** April 12, 2026  
**Date Updated:** April 12, 2026  
**Version:**  1.0
**LLM Used:**  Claude Sonnet 4.6

**Role:** Financial Analyst / Treasury Analyst  
**Audience:** CFO or Director of Treasury 

---

## A. Exposure Summary

Our firm holds a confirmed **€2,500,000 foreign-currency receivable** due on **2026-06-25** (90 calendar days from inception date 2026-03-27), arising from a completed export sale invoiced in euros to a European customer.

Because we report revenues and cash flows in USD, this receivable is fully exposed to EUR/USD exchange rate risk. At the inception spot rate of **1.0850 USD/EUR**, the position carries an implied USD value of **$2,712,500**. Between now and settlement, every 1% move in EUR/USD shifts that value by approximately **$27,125**. There is no natural offset — no EUR-denominated payable, no EUR cost base — and our treasury policy requires formal review of all transaction exposures above $500,000.

The core risk is EUR depreciation: if the euro weakens, we receive fewer dollars than we planned, directly compressing reported revenue and operating cash flow for the quarter. A 5% adverse move would reduce proceeds by approximately $135,600; a 10% move by $271,250. Both outcomes are well within the historical volatility range for EUR/USD, which has moved more than 12% in a single calendar year as recently as 2022.

The hedging decision must therefore weigh certainty of USD proceeds against the cost and constraints of each available instrument, within the context of our near-term cash flow planning and CFO budget commitments.

---

## B. Summary of Hedge Outcomes

All figures below are derived from the Stage 2 Excel model at the base-case spot rate of **S_T = S₀ = 1.0850 USD/EUR**, using the CIP-derived forward rate of **1.08876 USD/EUR**.

| Strategy | USD Proceeds (Base Case) | Key Characteristic |
|---|---|---|
| **Unhedged** | $2,712,500 | Full EUR/USD exposure; best case if EUR strengthens |
| **Forward Hedge** | $2,721,902 | Locked in; zero sensitivity to S_T; forward premium to spot |
| **Money Market Hedge** | $2,721,902 | Identical to forward (CIP parity confirmed, Δ = $0.00) |
| **Put Hedge (K = 1.0800)** | $2,658,750 | Floor protection with upside; premium cost = $53,750 |
| **Call Hedge (K = 1.1000)** | $2,691,250 | Upside amplification above K_CALL; premium cost = $21,250 |

**Forward Hedge:** Locks in $2,721,902 today regardless of where EUR/USD settles. This is the only strategy that completely eliminates exchange-rate uncertainty. The tradeoff is that it also eliminates any benefit from EUR appreciation — if the euro strengthens to 1.15, we still only receive $2,721,902. For a firm whose primary concern is cash flow predictability and budget accuracy, this is typically the dominant consideration.

**Money Market Hedge:** Produces an outcome mathematically identical to the forward ($2,721,902) when rates are consistent with Covered Interest Rate Parity — as they are in our model. In practice, a firm must have available EUR borrowing capacity and accept the operational complexity of managing a short EUR loan position. For most mid-sized corporates, the forward is simpler to execute and achieves the same economic result.

**Put Option Hedge (K_PUT = 1.0800):** Provides a floor of approximately $2,658,750 net of the $53,750 upfront put premium, while allowing full participation if EUR appreciates. At the base case (S_T = S₀), the put expires slightly in-the-money but the net proceeds are below the forward due to premium cost. The put becomes superior to the unhedged position for any S_T below approximately **1.0585 USD/EUR** (the break-even where premium cost is offset by put payoff). Above that level, the unhedged position outperforms, and above the forward rate, the put outperforms the forward.

**Call Option (K_CALL = 1.1000):** The long EUR call, as modeled, is a benchmark rather than a standalone receivable hedge. A firm holding a EUR receivable naturally benefits from EUR appreciation, so paying a premium for additional EUR call exposure creates redundant upside at a cost of $21,250. The call's primary utility would be as the long leg of a zero-cost collar — selling a call to offset the cost of a put. That structure is outside this model's scope but is a logical next-step analysis.

**Unhedged:** Maximizes proceeds if EUR strengthens but offers no protection on the downside. At S_T = 0.95 × S₀ (1.0308), the unhedged position yields approximately $2,576,875 — roughly $145,000 below the forward hedge and more than $81,000 below the put floor net of premium. Accepting this risk requires high conviction in EUR appreciation or a firm-level policy that tolerates FX volatility in reported results.

---

## C. Sensitivity Interpretation

The Stage 2 sensitivity table varies S_T from **1.0308 (−5%)** to **1.1393 (+5%)** in 1% increments. The analysis reveals distinct behavioral zones for each strategy.

**Under EUR depreciation (S_T < S₀):**

The forward and money market hedges are unambiguously superior to the unhedged position — and the gap widens as EUR weakens. At S_T = 1.0308 (−5%), the forward delivers $2,721,902 versus $2,576,875 unhedged, a protection value of **$145,027**. The put hedge also outperforms the unhedged position once S_T falls below ~1.0585, with the gap widening below the K_PUT strike of 1.0800. In a severe depreciation scenario (−5%), the put generates approximately $2,625,500 net of premium — roughly $48,600 below the forward, reflecting the premium cost.

The forward is the clear winner on downside protection: it delivers the highest USD proceeds in every depreciation scenario because it captures the forward premium while the put incurs premium cost.

**Under EUR appreciation (S_T > S₀):**

The unhedged position outperforms all hedged strategies once S_T exceeds the forward rate (~1.0888). At S_T = 1.1393 (+5%), the unhedged position returns approximately $2,848,125 — $126,223 above the locked-in forward. The put hedge closely tracks the unhedged position in appreciation scenarios (the put expires worthless; proceeds = S_T × FC_AMT less premium), meaning the put holder participates in approximately **96% of EUR upside** after accounting for the premium cost. The forward holder captures **none** of this upside.

**Key strategic insight from sensitivity analysis:**

The forward and money market hedges are optimal for firms prioritizing certainty; the put is optimal for firms that believe EUR may appreciate materially but want a defined downside floor. The cost of that optionality ($53,750 upfront, or approximately **2.0% of notional**) is the direct tradeoff. At current interest rate levels where the USD rate exceeds the EUR rate, the forward trades at a slight **premium to spot** (1.08876 vs. 1.0850), which meaningfully offsets the put's theoretical floor advantage.

---

## D. Strategic Recommendation

**Recommended strategy: Forward Hedge**

We recommend entering a 90-day EUR/USD forward sale contract to lock in **$2,721,902** on the full €2,500,000 receivable.

The forward is superior to the alternatives for four concrete reasons:

1. **It produces the highest guaranteed floor.** The forward rate of 1.08876 exceeds the put's effective floor (1.0800 strike less 2.15 cents premium = ~1.0585 effective floor) by approximately 300 basis points on a USD-per-EUR basis. The firm receives $2,721,902 with certainty versus a put floor of approximately $2,646,250 — a $75,652 guaranteed advantage.

2. **It requires no upfront cash outlay.** The put hedge requires a $53,750 premium payment at inception, creating an immediate cash flow impact. The forward requires no premium, no margin, and no capital deployment.

3. **Our risk profile favors certainty over optionality.** This receivable represents a discrete transaction exposure, not an ongoing or recurring FX risk stream. For a one-time exposure with a defined settlement date, locking in a known USD outcome allows the CFO to budget with full precision and eliminates any EUR/USD variance from the period's reported financials.

4. **The forward premium to spot reduces the opportunity cost.** Because the 90-day forward trades at a premium to spot (reflecting the USD > EUR interest rate differential), the firm receives slightly *more* than the current spot-implied value of the receivable. This is an unusual circumstance that makes the forward more attractive relative to historical norms where USD rates were near zero.

The money market hedge achieves the same economic result but requires EUR borrowing capacity and additional operational steps. Unless there is a specific reason to prefer the money market structure (e.g., to offset an existing EUR cash balance), the forward is the simpler and preferred execution path.

---

## E. Executive Justification

**Cash flow stability and budget certainty** are the overriding considerations for this decision. Our CFO has committed to USD-denominated revenue targets for Q2 2026. Accepting unhedged EUR exposure creates variance in reported results that cannot be explained by operational performance — it is purely financial. A forward contract converts that variance to zero, allowing management and investors to evaluate the period's results on their merits.

**Liquidity impact is minimal.** A standard EUR/USD forward with a bank counterparty requires no upfront cash. The firm delivers euros at settlement and receives dollars — the same cash flow that would occur in the unhedged case, but at a predetermined rate. There is no margin requirement, no premium outlay, and no balance sheet impact beyond a standard off-balance-sheet hedge disclosure.

**Optionality is worth paying for when there is conviction in appreciation.** Our current base case does not carry such conviction. The 90-day EUR/USD rate is driven primarily by the US-EU interest rate differential, which at present favors the USD. Paying 200 basis points of notional for a put option that provides upside participation is reasonable in a high-conviction EUR-bullish environment — but not as a default treasury policy for a routine receivable.

**Operational simplicity matters.** Treasury bandwidth is a real constraint. A forward contract is executed in a single instruction to the firm's relationship bank, documented with a standard trade confirmation, and requires no ongoing monitoring until settlement. The money market hedge requires EUR borrowing, USD investment, and ongoing reconciliation of two open positions. The option requires premium settlement, payoff monitoring, and exercise decisions. For a $2.7 million single-settlement exposure, that operational overhead is not warranted.

**Accounting treatment is straightforward.** A forward designated as a cash flow hedge under ASC 815 qualifies for hedge accounting, with changes in fair value recorded in OCI until the hedged item (the receivable collection) affects earnings. This keeps P&L clean and avoids mark-to-market volatility during the interim period.

---

## F. Structured AI Prompt

*The following section is a standalone, reusable AI prompt. It can be submitted directly to Claude, ChatGPT, or any capable AI coding assistant to regenerate the Stage 2 Excel model from scratch.*

---

```
# FX HEDGE MODEL — STRUCTURED AI PROMPT
# Stage 4 | FIN-321 International Finance & Securities
# Version 1.0 | 2026-04-12

# ─────────────────────────────────────────────────────────────────────────────
# GOAL
# ─────────────────────────────────────────────────────────────────────────────

Build a fully functional Excel workbook (.xlsx) that models forward, money
market, and option hedging strategies for a USD-reporting firm with a
foreign-currency receivable. The model must be auditable, color-coded, and
produce a sensitivity table and chart comparing all strategies side-by-side.
Deliver the completed .xlsx file as a downloadable output.

# ─────────────────────────────────────────────────────────────────────────────
# INPUT VARIABLES
# ─────────────────────────────────────────────────────────────────────────────

Use the following named ranges and values as the model's input parameters.
All inputs must appear in a clearly labeled yellow-highlighted input section.
These are the ONLY hardcoded values in the model; all other cells must use
Excel formulas referencing these inputs.

  FC_AMT      = 2,500,000      # EUR receivable amount
  S0_in       = 1.08500        # Spot rate (USD per EUR) at inception
  R_USD       = 0.0530         # USD risk-free rate, annualized, simple, ACT/360
  R_FC        = 0.0390         # EUR risk-free rate, annualized, simple, ACT/360
  K_PUT       = 1.08000        # EUR put option strike (USD per EUR)
  K_CALL      = 1.10000        # EUR call option strike (USD per EUR)
  PREM_PUT    = 0.02150        # Put premium per EUR (USD)
  PREM_CALL   = 0.00850        # Call premium per EUR (USD)
  T_DAYS      = 90             # Days to settlement (calendar, ACT/360 basis)

NOTE: Do NOT hardcode F0_in. Derive it as a formula:
  F0_in = S0_in × (1 + R_USD × T_DAYS/360) / (1 + R_FC × T_DAYS/360)
This ensures mathematical parity between the forward and money market hedges.

# ─────────────────────────────────────────────────────────────────────────────
# COLOR-CODING CONVENTION
# ─────────────────────────────────────────────────────────────────────────────

Apply the following background fills throughout the model:
  Yellow  (#FFFF00) — Input cells (editable by user)
  Blue    (#BDD7EE) — Assumption labels and parity check rows
  Green   (#E2EFDA) — Formula/intermediate calculation cells
  Gray    (#D9D9D9) — Output and summary KPI cells

Input cell text: blue (RGB 0,0,255) to signal hardcoded values
Formula cell text: black (RGB 0,0,0)
All cells: Arial font, 11pt

# ─────────────────────────────────────────────────────────────────────────────
# SHEET 1: FX HEDGE MODEL — REQUIRED SECTIONS
# ─────────────────────────────────────────────────────────────────────────────

## SECTION A — INPUTS

  - Create a labeled input block with all variables listed above.
  - Highlight every editable cell yellow with blue text.
  - Include a column of notes identifying the named range, source, and units
    for each input (e.g., "FC_AMT | Company export contract | EUR").
  - Place F0_in as a green formula cell directly below the other inputs,
    labeled "Forward Rate F0 (CIP-derived)" with the formula visible.

## SECTION B — FORWARD HEDGE

  - Calculate: USD_forward = FC_AMT × F0_in
  - Display result as a gray output cell.
  - Label clearly as "Locked-in USD Proceeds (Forward Hedge)".

## SECTION C — MONEY MARKET HEDGE (3 explicit steps)

  Step 1 — EUR to borrow today:
    EUR_borrow = FC_AMT / (1 + R_FC × T_DAYS/360)

  Step 2 — Convert EUR to USD at spot:
    USD_spot = EUR_borrow × S0_in

  Step 3 — Invest USD for T_DAYS:
    USD_mm = USD_spot × (1 + R_USD × T_DAYS/360)

  - Show each step as a labeled green formula row.
  - Display USD_mm as a gray output cell.
  - Add a parity check row (blue): USD_forward − USD_mm
    Label it "Parity Check (should = $0.00)".
    If the result is nonzero beyond rounding ($0.01), flag it.

## SECTION D — OPTION HEDGES

  Put hedge:
  - Total put premium (USD): FC_AMT × PREM_PUT  [green row]
  - Put payoff at S_T:        MAX(K_PUT − S_T, 0) × FC_AMT  [green row]
  - EUR receivable at S_T:    FC_AMT × S_T  [green row]
  - Net USD proceeds (put):   (FC_AMT × S_T) + put payoff − total put premium
    [gray output row]

  Call hedge (benchmark):
  - Total call premium (USD): FC_AMT × PREM_CALL  [green row]
  - Call payoff at S_T:        MAX(S_T − K_CALL, 0) × FC_AMT  [green row]
  - Net USD proceeds (call):   (FC_AMT × S_T) + call payoff − total call premium
    [gray output row]

  - Include one user-editable S_T input cell (yellow) in this section,
    labeled "Base-case ending spot S_T (set to S0_in by default)".
  - Default formula: =S0_in

## SECTION E — SENSITIVITY TABLE

  - Construct an 11-row table varying S_T from 0.95×S0_in to 1.05×S0_in
    in 1% increments (−5%, −4%, −3%, −2%, −1%, 0%, +1%, +2%, +3%, +4%, +5%).
  - For each S_T row, compute USD proceeds for all five strategies:
      Column 1: S_T value (formula: =S0_in × (1 + pct))
      Column 2: % vs S0 (label the step)
      Column 3: Unhedged (=FC_AMT × S_T)
      Column 4: Forward Hedge (=USD_forward, fixed)
      Column 5: Money Market Hedge (=USD_mm, fixed)
      Column 6: Put Hedge (=FC_AMT×S_T + MAX(K_PUT−S_T,0)×FC_AMT − FC_AMT×PREM_PUT)
      Column 7: Call Hedge (=FC_AMT×S_T + MAX(S_T−K_CALL,0)×FC_AMT − FC_AMT×PREM_CALL)
  - Highlight the 0% row (base case) in a distinct color (#FFF2CC amber).
  - Apply $#,##0 USD formatting to all proceeds columns.
  - Add a header row with navy fill and white bold text.

## SECTION E (continued) — SENSITIVITY CHART

  - Insert a line chart immediately below the sensitivity table.
  - X-axis: S_T values (column 1 of sensitivity table)
  - Y-axis: USD Proceeds
  - Series: One line per strategy (5 total), each in a distinct color:
      Unhedged: gray
      Forward: blue
      Money Market: green
      Put Hedge: red
      Call Hedge: orange/gold
  - Title: "USD Proceeds by Hedge Strategy vs. Ending Spot Rate"
  - Chart height: ~12cm; width: ~22cm

## SECTION F — SUMMARY KPI DASHBOARD

  - Gray output box listing base-case results for all five strategies.
  - Columns: Strategy name | USD Proceeds (base case)
  - Final row: "Hedge Recommendation" with placeholder text:
    "[ Forward Hedge — Lock in $2,721,902 at F0 = 1.08876 USD/EUR ]"
  - Bold the recommendation row.

# ─────────────────────────────────────────────────────────────────────────────
# SHEET 2: NOTES & ASSUMPTIONS
# ─────────────────────────────────────────────────────────────────────────────

Create a second worksheet titled "Notes & Assumptions" containing:

  1. A header matching the main model title style.
  2. A two-column table (Topic | Assumption/Note) covering:
      - Day count convention: ACT/360, simple interest
      - Forward rate: CIP-derived, not a market quote
      - R_USD source: 90-day T-bill yield, 2026-03-27
      - R_FC source: ECB deposit rate approximation
      - Option style: European (payoff at expiry only)
      - Option premiums: paid upfront, no time-value adjustment
      - Bid-ask spreads: excluded
      - Counterparty risk: excluded
      - Settlement timing: receivable assumed to arrive exactly on T_DAYS
      - Tax: excluded
      - Scope: single static hedge, no dynamic rebalancing
  3. Color-coding legend matching the main model.

# ─────────────────────────────────────────────────────────────────────────────
# VERIFICATION REQUIREMENTS
# ─────────────────────────────────────────────────────────────────────────────

Before delivering the file, confirm:

  [ ] F0_in formula produces ~1.08876 (±0.00001) given the above inputs
  [ ] USD_forward ≈ $2,721,902
  [ ] USD_mm ≈ $2,721,902
  [ ] Parity check ≈ $0.00
  [ ] Put net proceeds at S_T = S0_in ≈ $2,658,750
  [ ] Call net proceeds at S_T = S0_in ≈ $2,691,250
  [ ] Sensitivity table contains exactly 11 data rows
  [ ] Zero formula errors (#REF!, #DIV/0!, #VALUE!, #NAME?)
  [ ] Chart renders with all 5 series visible

# ─────────────────────────────────────────────────────────────────────────────
# EXPORT
# ─────────────────────────────────────────────────────────────────────────────

  - File name: stauffer-adam-stage2-model.xlsx
  - US Letter page size (if printing); 1-inch margins
  - Font: Arial throughout
  - Freeze top rows and first column on Sheet 1 for scrollability
  - Deliver as a downloadable .xlsx file
```

---

## G. Extra Credit: Areas for Further Study & Improvement

### 1. AI Skills & Automation: From Static Model to Live Decision Tool

The Stage 2 model is a static snapshot — it prices hedges against a single set of market inputs locked in on 2026-03-27. A natural and powerful extension would be to connect it to live market data using AI tools. An AI agent with web search capabilities (such as Claude with search enabled) could retrieve the current EUR/USD spot rate, 90-day T-bill yield, and ECB policy rate on demand, populate the model's input cells programmatically, and re-run all hedge calculations without any manual intervention. Taken further, a Monte Carlo simulation layer could replace the discrete ±5% sensitivity table with a probability-weighted distribution of USD outcomes under 10,000 simulated EUR/USD paths, producing expected-value and value-at-risk metrics that give the CFO a far richer picture of downside risk than a deterministic table. Claude's Code Execution environment can run such simulations in seconds within a conversation, meaning the analyst could iterate on strike prices, notional amounts, and settlement dates interactively before finalizing the hedge terms with the bank.

### 2. Multi-File Reasoning: Maintaining Consistency Across a Project Lifecycle

One of the most challenging aspects of a multi-stage project — especially one that spans weeks and involves both Excel and Markdown deliverables — is keeping all artifacts internally consistent. If the spot rate assumption changes between Stage 1 and Stage 2, does the memo still reference the correct value? If the forward rate formula is updated, does the Stage 3 specification reflect that change? An AI with access to all three files simultaneously (the Stage 1 memo, Stage 3 spec, and Stage 2 workbook) could scan for inconsistencies — mismatched rate values, outdated variable names, or calculation flows that no longer match the implemented formulas — and flag them before submission. GitHub provides the version history to make this tractable: an AI agent could compare the current commit against the prior version, identify what changed, and automatically update dependent documents. For a production treasury model that is rebuilt quarterly, this kind of multi-file coherence checking would eliminate a major source of model error and audit risk.

### 3. GitHub & Version Control: Audit-Ready Model Governance

Committing each stage deliverable to a GitHub repository — spec, model, prompt, and memo — creates a timestamped, immutable record of every decision made during the analysis. This is directly analogous to the audit trail requirements under ASC 815 hedge accounting documentation: the hedge must be formally designated and documented *before* the hedged transaction occurs, and that documentation must be retrievable on demand. A GitHub commit with a descriptive message ("Designated EUR forward hedge for Q2 receivable; F0 = 1.08876, T = 90 days, notional €2.5M") serves exactly that purpose. Auditors can inspect the entire decision history — which assumption was used, when it was changed, and who approved it — without relying on shared drives, email threads, or undated spreadsheet files. For a Big 4 audit client, a treasury team that maintains its hedge models in a structured GitHub repository with linked specs and prompts is demonstrating a level of process discipline that directly supports the hedge effectiveness documentation required for OCI deferral treatment.

---

## References

- Federal Reserve. (2026). *H.10 Foreign Exchange Rates*. https://www.federalreserve.gov/releases/h10/
- European Central Bank. (2026). *Euro foreign exchange reference rates*. https://www.ecb.europa.eu/stats/exchange/eurofxref/
- Madura, J. (2021). *International Financial Management* (14th ed.). Cengage Learning.
- McDonald, R. L. (2013). *Derivatives Markets* (3rd ed.). Pearson.
- FASB ASC 815. *Derivatives and Hedging*. Financial Accounting Standards Board.
