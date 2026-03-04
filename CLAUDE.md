# IQVIA Holdings (IQV) — DCF Model Context

This file is automatically loaded by Claude Code at the start of every session.

## Project Overview

Full DCF model for IQVIA Holdings Inc. (ticker: IQV), a healthcare data and clinical research company.
Data sourced from 10-Ks via BAMSEC. Historical period: 2020A–2025A. Projection period: 2026E–2040E.

## Company Background

IQVIA has three reportable segments:
- **TAS** (Technology & Analytics Solutions) — SaaS platforms, data subscriptions, RWE, consulting
- **R&DS** (Research & Development Solutions) — CRO / clinical trial outsourcing, backlog-driven
- **CSMS** (Contract Sales & Medical Solutions) — outsourced pharma sales force, headcount-driven

## Historical Data Status

All historical tabs are **hardcoded blue** (no formulas). All projection tabs will be **formula-driven black**, pulling from schedules and assumptions.

Completed historical data tabs:
- Balance Sheet (2020A–2025A) — includes 2020A as anchor for 2021 deltas
- Income Statement (2021A–2025A)
- Cash Flow Statement (2021A–2025A) — hardcoded from reported 10-Ks; projected SCF will be formula-driven

## Color Coding Convention

- **Blue** = hardcoded input
- **Black** = formula (derived)
- **Green** = cross-tab reference (linking to another sheet)
- **Red** = error / check flag

## Architecture

```
Historical tabs (hardcoded blue)
  └── BS, IS, SCF

Supporting Schedules (formula-driven, feed into projected BS)
  └── Each schedule has historical columns verifying against actual BS,
      projection columns driven by assumptions tab

Assumptions Tab
  └── All projection drivers live here; schedules reference this tab

Projected Statements (formula-driven)
  └── Projected IS → Projected BS → Projected SCF (via BS deltas)

DCF
```

### Key Principle: Schedules → BS → SCF
Schedules drive the projected BS. The projected SCF is derived from BS deltas (not linked directly to schedules). Historical SCF is hardcoded.

## Completed Schedules

| Schedule | Tab Name | Status | Notes |
|----------|----------|--------|-------|
| Goodwill | Goodwill | ✅ Done | By segment (TRS, R&DS, CSMS), FX plug |
| Shareholders' Equity | SE Roll | ✅ Done | Separate roll per component: APIC, RE, Treasury, AOCI, NCI |
| Investments | Investments | ✅ Done | 3 rolls: current securities, non-current securities, unconsolidated affiliates |
| Accounts Receivable | AR Schedule | ✅ Done | Split: billed AR (DBO), unbilled (DUO), allowance % gross AR |
| Unearned Income | WC (part) | ✅ Done | % of revenue (majority of BoP balance recognized in year per footnote) |
| AP & Accrued Expenses | AP Schedule | ✅ Done | Split into: AP (DPO), client contract accruals, compensation, professional fees, contingent consideration, interest, restructuring, other |
| Prepaid Expenses | WC (part) | ✅ Done | % of revenue (balance too stable for days-based driver) |
| Other Current Assets | WC (part) | ✅ Done | % of revenue (no footnote breakdown available) |
| Deferred Tax Assets | DTA Schedule | ✅ Done | Superset of all DTA/DTL components 2021–2025; net ties to BS |

## Remaining Schedules (Build Order)

### Round 2 — Working Capital (remaining)
- [ ] Income taxes receivable — % revenue placeholder; replace with tax schedule output later
- [ ] Deposits and other assets — % revenue placeholder; replace with lease schedule output later
- [ ] Income taxes payable — % revenue placeholder; replace with tax schedule output later
- [ ] Other current liabilities — pull footnote; likely % COGS
- [ ] Other liabilities — pull footnote; may contain pension/LT deferred revenue

### Round 3 — Complex Schedules (require 10-K footnote data)
- [ ] PP&E & Depreciation — vintage waterfall (same structure as Ford precedent)
- [ ] Intangibles & Amortization — vintage waterfall by category (customer relationships, technology, trade names have different useful lives)
- [ ] Operating Lease — ROU asset roll + lease liability roll; weighted avg discount rate and term from footnote
- [ ] Debt & Interest — by tranche (pull from debt footnote); gross proceeds vs. repayments
- [ ] Tax Schedule (Levered & Unlevered) — NOL schedule, interest carryforward (163j), effective rate reconciliation

### Round 4
- [ ] Acquisition Schedule — drives goodwill additions, intangibles additions, and SCF investing line

### After All Schedules
- [ ] Assumptions Tab
- [ ] Projected IS
- [ ] Projected BS (link all schedules)
- [ ] Projected SCF (BS deltas)
- [ ] DCF

## Balance Sheet Line Items

### Assets
- Cash and cash equivalents
- Trade accounts receivable and unbilled services, net
- Prepaid expenses
- Income taxes receivable
- Investments in debt, equity and other securities (current)
- Other current assets and receivables
- Total current assets
- Property and equipment, net
- Operating lease right-of-use assets
- Investments in debt, equity and other securities (non-current)
- Investments in unconsolidated affiliates
- Goodwill
- Other identifiable intangibles, net
- Deferred income taxes
- Deposits and other assets, net
- Total assets

### Liabilities & Equity
- Accounts payable and accrued expenses
- Unearned income
- Income taxes payable
- Current portion of long-term debt
- Other current liabilities
- Total current liabilities
- Long-term debt, less current portion
- Deferred income taxes
- Operating lease liabilities
- Other liabilities
- Total liabilities
- Common stock and additional paid-in capital
- Retained earnings
- Treasury stock, at cost
- Accumulated other comprehensive loss
- Equity attributable to IQVIA Holdings Inc.'s stockholders
- Noncontrolling interests
- Total stockholders' equity
- Total liabilities and stockholders' equity
- Memo: Shares Outstanding

## SCF Line Items (Reported Format — Keep As-Is)

### Operating
Net income / D&A / Amortization of debt issuance costs and discount / Stock-based compensation /
Gain on disposals of property and equipment, net / (Earnings) losses from unconsolidated affiliates /
(Gain) loss on investments, net / Benefit from deferred income taxes /
Accounts receivable and unbilled services / Prepaid expenses and other assets /
Accounts payable and accrued expenses / Unearned income / Income taxes payable and other liabilities /
**Net cash provided by operating activities**

### Investing
Acquisition of property, equipment and software / Acquisition of businesses, net of cash acquired /
Sales (Purchases) of marketable securities, net / Investments in unconsolidated affiliates, net /
(Investments in) proceeds from sale of debt and equity securities / Proceeds from sale of property, equipment and software /
Other / **Net cash used in investing activities**

### Financing
Proceeds from issuance of debt / Payment of debt issuance costs / Repayment of debt and principal payments on finance leases /
Proceeds from revolving credit facility / Repayment of revolving credit facility /
Payments related to employee stock incentive plans, net / Repurchase of common stock /
Acquisition of Quest's non-controlling interest / Contingent consideration and deferred purchase price payments /
Other / **Net cash used in financing activities**

Effect of foreign currency exchange rate changes on cash /
Increase (decrease) in cash and cash equivalents /
Cash and cash equivalents at beginning of period /
Cash and cash equivalents at end of period

## SCF Projection Logic (BS Delta Mapping)

| SCF Line | BS Source |
|----------|-----------|
| Benefit from deferred income taxes | Δ DTA (asset) − Δ DTL (liability), sign flipped |
| Accounts receivable | Δ Trade AR — NOTE: includes acquired WC in historicals |
| Prepaid and other assets | Δ prepaid + Δ other current assets + Δ income taxes receivable + Δ deposits |
| AP and accrued expenses | Δ AP/accrued |
| Unearned income | Δ unearned income |
| Income taxes payable and other liabilities | Δ ITP + Δ other current liabilities + Δ other liabilities + Δ operating lease liabilities (from lease schedule) |
| Repurchase of common stock | Δ treasury stock (from SE schedule) |
| Marketable securities | Δ current investments (from investments schedule) |
| Investments in unconsolidated affiliates | Δ affiliates (from investments schedule) |
| Debt/equity securities | Δ non-current investments (from investments schedule) |
| Capex | From PP&E schedule |
| Acquisitions | From acquisition schedule |
| All debt lines | From debt schedule |
| SBC (non-cash) | From SE schedule |

**Important**: Historical working capital SCF lines ≠ BS deltas due to acquired working capital from M&A and FX translation on BS balances. Historical SCF stays hardcoded. Only projection years are formula-driven via BS deltas.

## Key Projection Drivers (To Be Set in Assumptions Tab)

### Working Capital
- DSO (days sales outstanding) — billed AR
- DUO (days unbilled outstanding) — unbilled services
- Allowance % of gross AR
- DPO (days payable outstanding) — accounts payable
- Unearned income % revenue
- Prepaid % revenue
- Other current assets % revenue
- Other current liabilities % COGS
- Other liabilities % revenue
- Income taxes receivable / payable — link to tax schedule

### PP&E & D&A
- Capex % revenue (or split maintenance vs. growth)
- Average useful life (from footnotes)
- Depreciation waterfall by vintage year

### Intangibles
- Acquired intangibles per deal (from acquisition schedule)
- Useful life by category (customer relationships, technology, trade names)
- Amortization waterfall by category and vintage

### Debt & Interest
- Tranche-by-tranche from footnotes (maturity, coupon, amortization schedule)
- Revolver balance assumption
- Interest rate per tranche

### Tax
- Effective tax rate reconciliation by component
- NOL carryforward utilization
- 163(j) interest carryforward utilization

### Equity
- SBC % revenue
- Share repurchases ($ or % of CFO)
- Dividends — IQVIA pays none
- OCI assumption — zero or small FX

### Acquisitions
- Annual M&A spend assumption
- Goodwill % of purchase price (from historical PPA footnotes)
- Intangibles % of purchase price

## Revenue Model (Separate Build)

### R&DS — Backlog-Driven by Sponsor Cohort
Extremely granular model already partially built. Four cohorts:
1. **EBP** (Emerging BioPharma, <$500M revenue) — VC funding lagged 1.25 years drives trial starts → bookings → backlog → revenue
2. **Small/Mid** ($500M–$5B) — preferred-provider framework, R&D budget growth
3. **Large Pharma** (top 20 sponsors) — MSA renewals, patent cliff pull-forward, backlog burn rate
4. **Gov/Academic** — NIH/BARDA funding, BIOSECURE Act

Each cohort: gross bookings → cancellation rate → net new bookings → backlog roll (BoP + net bookings - revenue recognized = EoP) → revenue = burn rate × BoP backlog

Passthrough revenue modeled separately (0% contribution margin).

### TAS — SaaS Unit Economics by Sub-Segment
Four sub-segments:
1. **Technology platforms** (OCE, CRM, MIDAS, Analytics Link) — Full ARR cohort waterfall by customer size (large/mid/EBP): beginning ARR + new logo ARR + expansion ARR - churned ARR = ending ARR → NRR, NDR, LTV:CAC
2. **Real world solutions** (RWE, patient data networks) — study volume × ASV per customer cohort (biopharma/payer/provider/gov); FDA/IRA mandate toggles
3. **Analytics and consulting** — engagement volume × ACV; attach rate to technology platforms; pharma R&D strategy spend driver
4. **Information offerings** (national/sub-national data subscriptions) — contract count × ACV × renewal rate by geography (North America / Europe / emerging markets)

Macro inputs: pharma commercial spend, pharma R&D strategy spend, global RWE study volume, FDA/IRA mandates, global pharma sales force size, global VC biotech funding (feeds into platform demand).

Alt data to incorporate: ClinicalTrials.gov, FDA approvals database, Veeva 10-K (NRR/churn benchmarks), IQVIA Institute reports, LinkedIn job postings, World Bank GDP API.

### CSMS — Simple
% growth assumption or headcount × revenue per FTE.

## Key Analytical Flags / Notes

- **Reclassifications across years**: IQVIA uses "Deferred revenue" vs "Contract liabilities" terminology inconsistently. Also "Non-controlling interests" vs "Noncontrolling interests." Standardized in model.
- **Investments dual label**: "Investments in debt, equity and other securities" appears twice on BS (current and non-current). Disambiguated with (current) / (non-current) suffixes.
- **Acquisition contamination of WC**: BS deltas ≠ SCF operating cash flows historically due to acquired working capital. Historical SCF is hardcoded. Only projections use formula-driven BS deltas.
- **FX translation**: BS uses period-end rates, SCF uses average rates. This gap also causes BS deltas to differ from SCF. FX effect zeroed in projections.
- **Net income in SCF**: Uses consolidated NI (IQVIA + NCI), not NI attributable to IQVIA only. This is correct — SCF is a consolidated statement.
- **DTA footnote vs BS**: Footnote shows gross DTA/DTL globally; BS shows net by jurisdiction. Gross footnote will never directly reconcile to BS DTA line. Use BS lines as anchors; use footnote components to understand drivers.
- **163(j) interest limitation**: IQVIA has disallowed interest carryforwards. 2025 implied gross balance ~$310M ($65M DTA / 21%). Track in interest carryforward schedule.
- **ROU assets**: Depreciation embedded in D&A line. No separate SCF line (consistent with IQVIA's reported format). Drive ROU asset BS balance via lease schedule.
- **Shares**: Issued vs. outstanding difference = treasury shares. 2025: 259.1M issued, 169.6M outstanding, 89.5M treasury.
